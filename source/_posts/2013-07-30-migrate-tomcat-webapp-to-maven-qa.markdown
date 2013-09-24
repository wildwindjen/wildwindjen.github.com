---
layout: post
title: "Migrate Tomcat Webapp to Maven QA"
date: 2013-07-30 09:22
comments: true
categories: 
- Maven
- Build Tool
---
## 總結
> 我有預感，這只是個開始。-- 毛利小五郎。

工作上做了點嘗試，把剛接手的專案，試著導入 Maven 。因為想要針對一團亂的外部函式庫做管理。而馬糞，是我第一個想到的。倉促之下，只能先做問題集的整理。

## 原本的專案編碼是 UTF-8
這個問題大概只有發生在開發平台是 windows 的吧。偏偏我就是其中一個受害者。目前已知有兩個作法，實際上是同一種解法，就是餵 `project.build.sourceEncoding` 系統屬性值給他。

### 作法一 Command Line
{% codeblock Cold Block %}
mvn -Dfile.encoding=UTF-8 -Dproject.build.sourceEncoding=UTF-8 package
{% endcodeblock %}

### 作法二 POM.xml
{% codeblock Cold Block %}
	<project  xmlns=....>
		...
		<properties>
			<project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
		</properties>
{% endcodeblock %}


## 去哪裡得知外部函式庫的 maven 設定
假設我是要加進來 log4j ，請 google maven log4j。你會找到[這一頁](http://www.mvnrepository.com/artifact/log4j/log4j)。請點選你想要找的版本，接著就是複製設定到 pom.xml 了。


## 自家的 jar ，怎麼兜進去
在專案目錄下新建一個 libs 子目錄(跟 pom.xml 同一層)。準備放自家的函式庫。

依照 maven 的版控結構規則，重新建立子目錄 `{gourpId}/{artifactId}/{version}` 以及重新命名 jar 為 `{artifactId}-{version}.jar` 。

舉例來說：原本叫做 wade.jar 的檔案，我就隨便給他個 groupId=wild 、 artifactId=wind 、 version=1.0。所以我就必須在 {PROJECT_DIR}/libs 下建立 wild/wind/1.0/wind-1.0.jar。

然後記得新增 repository 和 dependency 設定，請參考下方。

有個延伸題，我有兩個 jar 檔，都想要放在 wild/wind/1.0/ 下怎麼辦，請多設定個 `classifier` ，檔案的命名就變成要 `{artifactId}-{version}-{classifier}.jar`。

{% codeblock Cold Block %}
	<project  xmlns=....>
		...
		<repositories>
			<repository>
				<id>ProjectRepo</id>
				<name>ProjectRepo</name>
				<url>file:///${project.basedir}/libs</url>
			</repository>
		</repositories>
		....
		<dependencies>
			<dependency>
				<groupId>wild</groupId>
				<artifactId>wind</artifactId>
				<version>1.0</version>
				<classifier>encode</classifier>
			</dependency>
			<dependency>
				<groupId>wild</groupId>
				<artifactId>wind</artifactId>
				<version>1.0</version>
				<classifier>decode</classifier>
			</dependency>
		</dependencies>
{% endcodeblock %}


## The method getJspApplicationContext(ServletContext) is undefined for the type JspFactory
說穿了，就是原本 tomcat 就有 jsp-api.jar (servlet-api.jar 也一樣)，匯出來的專案又包含了 jsp-api.jar，所以造成用錯版本。

所以要達成一個目標：package 之後的動作都不要有 jsp-api.jar 。這時候就要靠這一味： scope 。
至於值的意義請參考 [Dependency Scope](http://maven.apache.org/guides/introduction/introduction-to-dependency-mechanism.html#Dependency_Scope Dependency_Scope)
{% codeblock Cold Block %}
	<dependency>
		<groupId>javax.servlet</groupId>
		<artifactId>jsp-api</artifactId>
		<version>2.0</version>
		<scope>provided</scope>
	</dependency>
{% endcodeblock %}


本來如果只是這樣就罷手，也就無事一身輕。偏偏想到目前開發都是使用 eclipse，就騷屁股(台語)地想要一氣呵成。所以又有下面的 m2eclipse 篇。

## m2eclipse 沒有 run on server
專案 > 屬性 > Facets
勾選 Dynamic Project Module，並且記得挑選版本。否則之後無法變更。

## lib 不會跟著 deploy 到 tomcat plugin
參考 [stackover](http://stackoverflow.com/questions/6356421/maven-tomcat-projects-in-eclipse-indigo-3-7) ，使用路徑定義的方式 Deployment Assemply。

* maven 專案目錄上按右鍵，properties。
* 新增對應(Folder)
	* Source: target/{your-project-name}/WEB-INF/lib
	* Deploy Path: WEB-INF/lib
* Run As：
	* Maven Clean
	* Maven Install
	* Run on Server
			
## 不同環境，不同的設定
我在我的主程式多了一層子目錄 `webconf`，裡面放置各種環境的設定檔。下面我模擬了 dev 和 online 兩個情境，`src/main/webconf/dev` 和 `src/main/webconf/online`，分別放置開發環境跟線上環境的設定檔。

{% codeblock Cold Block %}
		... ...
	</dependencies>
	<!-- -->
	<profiles>
		<profile>
			<id>dev</id>
			<activation>
				<activeByDefault>true</activeByDefault>
			</activation>
			<build>
				<plugins>
					<plugin>
						<groupId>org.apache.maven.plugins</groupId>
						<artifactId>maven-war-plugin</artifactId>
						<version>2.2</version>
						<configuration>
							<webResources>
								<resource>
									<directory>src/main/webconf/dev</directory>
									<targetPath>WEB-INF</targetPath>
								</resource>
							</webResources>
						</configuration>
					</plugin>
				</plugins>
			</build>
		</profile>
		<profile>
			<id>online</id>
			<build>
				<plugins>
					<plugin>
						<groupId>org.apache.maven.plugins</groupId>
						<artifactId>maven-war-plugin</artifactId>
						<version>2.4</version>
						<configuration>
							<webResources>
								<resource>
									<directory>src/main/webconf/online</directory>
									<targetPath>WEB-INF</targetPath>
								</resource>
							</webResources>
						</configuration>
					</plugin>
				</plugins>
			</build>
		</profile>
	</profiles>
	<build>
		... ...
{% endcodeblock %}

接著，輸入 
{% codeblock Cold Block %}
mvn package -P dev
{% endcodeblock %} 
或者 
{% codeblock Cold Block %}
mvn package -P online
{% endcodeblock %}
即可輸出不同情境的佈署檔案。是不是很方便啊？

`activeByDefault` 則是用來設定哪一個為預設的情境。所以 
{% codeblock Cold Block %}
mvn package -P dev
{% endcodeblock %}
跟 
{% codeblock Cold Block %}
mvn package
{% endcodeblock %}
是一樣的。