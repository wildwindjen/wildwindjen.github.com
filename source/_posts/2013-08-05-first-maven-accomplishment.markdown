---
layout: post
title: "馬糞處女作"
date: 2013-08-05 09:30
comments: true
categories: 
- Maven
- Build Tool
---
## 總結
> 我是個現實主義者，我喜歡目前自己所從事的一切，並對此始終深信不疑。 -- 巴菲特。

從做中學累積經驗真的很快，但是沒記下來，就等於沒有。這段期間歷經了幾個問題：

0. 編碼議題 (properties)
0. 如何建立本地函示庫 (repository)
0. 特定函式庫只想要支援 compile 階段，但是 package、deploy 不包含。(scope)
0. FCKeditor 不想要用它依賴的 slf4j-api 版本。(exclusions)
0. 依照佈署環境產生不同的設定檔。(profiles)

有些問題之前已經在 [Migrate Tomcat Webapp to Maven QA](/blog/2013/07/30/migrate-tomcat-webapp-to-maven-qa/) 分享過了，所以直接看成品囉。

## 成品
{% codeblock google plus comments %}

<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
	<modelVersion>4.0.0</modelVersion>
	<groupId>wild.wind</groupId>
	<artifactId>WildTest</artifactId>
	<packaging>war</packaging>
	<version>1.0-SNAPSHOT</version>
	<name>WildTest Maven Webapp</name>
	<url>http://maven.apache.org</url>
	<!-- -->
	<properties>
		<project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
		<project.reporting.outputEncoding>UTF-8</project.reporting.outputEncoding>
	</properties>

	<repositories>
		<!-- 建立 Local 函示庫 -->
		<repository>
			<id>ProjectRepo</id>
			<name>ProjectRepo</name>
			<url>file:///${project.basedir}/libs</url>
		</repository>
	</repositories>

	<dependencies>
		<dependency>
			<groupId>junit</groupId>
			<artifactId>junit</artifactId>
			<version>3.8.1</version>
			<scope>test</scope>
		</dependency>

		<!-- servlet, tomcat 本身應該要有，所以指定 provided -->
		<dependency>
			<groupId>javax.servlet</groupId>
			<artifactId>servlet-api</artifactId>
			<version>2.5</version>
			<scope>provided</scope>
		</dependency>

		<!-- jsp, tomcat 本身應該要有，所以指定 provided -->
		<dependency>
			<groupId>javax.servlet</groupId>
			<artifactId>jsp-api</artifactId>
			<version>2.0</version>
			<scope>provided</scope>
		</dependency>

		<!-- MySQL JDBC, 模擬佈署環境 tomcat 自己放，所以指定 provided -->
		<dependency>
			<groupId>mysql</groupId>
			<artifactId>mysql-connector-java</artifactId>
			<version>5.2.18</version>
			<scope>provided</scope>
		</dependency>

		<!-- Oracle JDBC, 模擬佈署環境 tomcat 自己放，所以指定 provided -->
		<dependency>
			<groupId>oracle</groupId>
			<artifactId>ojdbc14</artifactId>
			<version>1.0</version>
			<scope>provided</scope>
		</dependency>

		<!-- Struts -->
		<dependency>
			<groupId>struts</groupId>
			<artifactId>struts</artifactId>
			<version>1.2.0</version>
		</dependency>

		<!-- FCKeditor, 改用比較新的 slf4j-api -->
		<dependency>
			<groupId>net.fckeditor</groupId>
			<artifactId>java-core</artifactId>
			<version>2.0</version>
			<exclusions>
				<exclusion>
					<groupId>org.slf4j</groupId>
					<artifactId>slf4j-api</artifactId>
				</exclusion>
			</exclusions>
		</dependency>
		
		<!-- slf -->
		<dependency>
			<groupId>org.slf4j</groupId>
			<artifactId>slf4j-api</artifactId>
			<version>1.5.8</version>
		</dependency>

		<dependency>
			<groupId>org.slf4j</groupId>
			<artifactId>slf4j-simple</artifactId>
			<version>1.5.8</version>
		</dependency>

		<!-- 去 Local 函示庫找 Jar 檔 -->
		<dependency>
			<groupId>wild</groupId>
			<artifactId>wind</artifactId>
			<version>1.0</version>
		</dependency>

		<dependency>
			<groupId>wild</groupId>
			<artifactId>wild</artifactId>
			<version>1.0</version>
			<classifier>encode</classifier>
		</dependency>

		<dependency>
			<groupId>wild</groupId>
			<artifactId>wild</artifactId>
			<version>1.0</version>
			<classifier>decode</classifier>
		</dependency>
	</dependencies>
	
	<profiles>
		<!-- 本機開發設定檔 -->
		<profile>
			<id>dev_local</id>
			<activation> <!-- 預設使用此設定檔 -->
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
									<directory>src/main/webconf/dev_local</directory>
									<targetPath>WEB-INF</targetPath>
								</resource>
							</webResources>
						</configuration>
					</plugin>
				</plugins>
			</build>
		</profile>
		
		<!-- 線上設定檔 -->
		<profile>
			<id>prod</id>
			<build>
				<plugins>
					<plugin>
						<groupId>org.apache.maven.plugins</groupId>
						<artifactId>maven-war-plugin</artifactId>
						<version>2.4</version>
						<configuration>
							<webResources>
								<resource>
									<directory>src/main/webconf/prod</directory>
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
		<finalName>WildTest</finalName>
	</build>
</project>

{% endcodeblock %}
