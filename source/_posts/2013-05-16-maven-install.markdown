---
layout: post
title: "馬糞(Maven)初體驗"
date: 2013-05-16 22:42
comments: true
categories: 
- Maven
- Build Tool
---
## 總結
<pre>
當個新時代的糞青。
</pre>

知道馬糞這個東西很久了，最近突然想起來，開始來玩看看。當然，會突然想起他，是因為想要拿來管理一些手上的專案。Maven 是一個軟體專案管理工具，除了軟體建構(Building)、測試、打包、佈署以外，還包含套件依賴性解決以及軟體分享的特性，定位其實跟 ANT 等 Building Tool 不太一樣。

在軟體建構部分，和 Ant 最大的不同在於，Maven 是採慣例制的(Convension Over Configuration)。也就是說，很多東西可以不設定，會有預設值，而這個預設值，是大部分情況下適用的(慣例)。不用像 Ant 一樣，每個動作都指定到位。只需要針對和慣例不同的特殊狀況，再去設定變更即可。輕鬆多了。


## 安裝

+ 先至[Maven官網](http://maven.apache.org/download.cgi Maven)下載
+ 解壓縮之後，放到想放的地方
+ 設定系統的環境變數 M2_HOME，指到剛剛的馬糞目錄，例如我的就是 `D:\PROGRAM\apache-maven-3.0.5`。
+ 接著在 path 變數新增 %M2_HOME%\bin。
+ 打開 console 介面，執行 `mvn --version`。應該要看到類似的訊息：
{% codeblock Cold Block %}
Apache Maven 3.0.5 (r01de14724cdef164cd33c7c8c2fe155faf9602da; 2013-02-19 21:51:
28+0800)
Maven home: D:\PROGRAM\apache-maven-3.0.5
Java version: 1.6.0_38, vendor: Sun Microsystems Inc.
Java home: C:\Program Files\Java\jdk1.6.0_38\jre
Default locale: zh_TW, platform encoding: MS950
OS name: "windows 7", version: "6.1", arch: "amd64", family: "windows"
{% endcodeblock %}


## 行前教育
從 `Maven: The Definitive Guide` 裡面得知，Maven 核心做的事情很少，除了解析 XML 和追蹤生命週期、外掛的能力以外，主要是透過外掛(Plugins)撐完全場。
<pre>
The core of Maven is pretty dumb, it doesn't know how to do much beyond parsing
a few XML documents and keeping track of a lifecycle and a few plugins.
</pre>

### Plugins & Goals
把 Plugins 想成一個個獨立的元件(或者分類，相關類型的功能總是會集中在一起)，而每個元件各自提供一或者多個功能，這些功能我們稱它為 Goals。每個 Goal 都是代表一個獨立的任務，是 Maven 執行工作的最小單位。

`compiler:compile` 代表的就是 compiler 這個 plugin 下的 compile goal。

由於透過網路眾人的力量以及分享，Plugins 函式庫日益成長茁壯，可以支援的能力包山包海，讓整個軟體的建構、管理，不太需要重頭到尾打造，最多只要微調。

### Build Lifecycle 和 Phases
剛剛提到 Goals 負責的是各自獨立的作業，彼此之前沒有任何關係，或者順序性上的依賴。如果直接拿 Goal 來用，設定會變得太瑣碎。所以需要有更宏觀的概念，也就是所謂的生命週期(Build Lifecycle)和階段(Phases)。

目前 Maven 內建了三大 Build Lifecycle： Default、Clean、Site。

+ Default 是專案建構流程的生命週期。
+ Clean 是處理專案清空任務時的生命週期。
+ Site 則用來產生描述專案相關資訊的站台文件時的生命週期。

每個生命週期都各自包含了多個階段，代表的是生命週期裡的步驟。拿最常用的 Default 來看，常用的階段如下：(詳細定義請參考 [Lifecycle Reference](http://maven.apache.org/guides/introduction/introduction-to-the-lifecycle.html#Lifecycle_Reference  Lifecycle Reference))

+ compile: 編譯程式。
+ test: 測試已編譯的程式。
+ package: 將已編譯的程式打包。例如打包成 JAR 檔。
+ install: 將打包檔放進去本地的 Respository。
+ deploy: 將最後成品分享至遠端的 Respository。

階段本身有順序上的依賴，也就是當命令下達執行 test 階段時，test 之前的階段，都會先依序被執行。那執行到底是執行哪些動作呢？

之前說了，Goal 才是執行工作的最小單位，所以 Phase 和 Goal 必須建立起連結關係。可以查看 [Built-in Lifecycle Bindings](http://maven.apache.org/guides/introduction/introduction-to-the-lifecycle.html#Built-in_Lifecycle_Bindings  Built-in Lifecycle Bindings)。左邊是 Phase ，右邊是對應到的 Goal(格式 Plugin:Goal)。

所以，許多 Phases 組成一個 Lifecycle，而每個 Phase 又是對應到(binding)一或多個 Goal。Lifecycle 代表的是想要執行的管理種類(建構、清理、還是產生站台文件)，Phase 負責順序上的依賴，以及簡化設定的使命，Goal 就是真正執行功能的小螺絲。

### Packaging
哪些 Goal 對應到哪些 Phase，依照 POM 內的 packaging 元素設定，會略有不同，請參考 [Built-in Lifecycle Bindings](http://maven.apache.org/guides/introduction/introduction-to-the-lifecycle.html#Built-in_Lifecycle_Bindings  Built-in Lifecycle Bindings)。

拿 jar 跟 ear 來比較，可以得知 package phase 所使用的 goal 就不一樣了。packaging 預設值是 jar。

因為 packaging 元素設定，支援的 Phase 似乎也略有差異。

### Plugin
透過 POM 的 plugin 設定，可以為各預設的 Phase 加上一些額外的 Goal，執行的順序依序為：內建、自行設定(自行設定的順序則以在 POM 上的順序)。以下例子是示範為 process-test-resources Phase 新增一個 time 的 Goal。
{% codeblock maven plugin setting %}
	{% raw %}
<plugin>
    <groupId>com.mycompany.example</groupId>
    <artifactId>display-maven-plugin</artifactId>
    <version>1.0</version>
    <executions>
        <execution>
            <phase>process-test-resources</phase>
            <goals>
                <goal>time</goal>
            </goals>
        </execution>
    </executions>
</plugin>
{% endraw %}
{% endcodeblock %}

話說 Goal 本身就有資訊來註記可以綁到哪些 Phase 上了，所以假設上面的 time 這個 Goal 在他裡面註記只能綁到 process-test-resources Phase，其實 phase 元素的設定是可以省略的。

### groupId, artifactId, version
Maven 使用這三個資訊來定位座標。講白話一點，就是這三個資訊加起來，是一個專案的識別碼。

+ groupId 代表的是公司、組織或團隊資訊。
+ arifactId 是單一專案名稱
+ version 是專案的版號

以上三個資訊在不同專案間，不能完全一模一樣。packaging 雖然也是重要的資訊，但不是用來定位的一份子。標記格式如下：
<pre>
groupId:artifactId:packaging:version
</pre>
這樣的標記方式應用於 Maven 的依賴機制上。

在下面我將會創建一個 wild.wind:wadetest:jar:1.0-SNAPSHOT 專案當範例。


## 開始第一個 Maven
+ 建立第一個 Maven
<pre>
mvn archetype:generate -DarchetypeGroupId=org.apache.maven.archetypes -DgroupId=wild.wind -DartifactId=wadetest
</pre>
會看到許多專案類型可以挑選，預設是 264 org.apache.maven.archetypes:maven-archetype-quickstart (An archetype which contains a sample Maven project.)。採用預設直接按 Enter 就好，想要挑別的，就輸入對應的編號，再按 Enter。
{% img center /images/wildwind/maven_install/first_maven.png 600 350 'Choose Maven Project' 'Choose Maven Project' %}

+ 接著會要你挑專案類型使用的版本(別懷疑，同一種類型不只一種版本可以挑)，小的習慣預設值。
+ 然後設定想要的版號，這個資訊是會出現在自己專案的 POM 裡。跟上面那個不一樣。
+ 然後要你確認一下資訊對不對。按下 Enter 就建立完成了。

想要直接指定建立的專案類型，不想在那邊挑，只要輸入時，直接改指定 archetypeArtifactId 的值：
<pre>
mvn archetype:generate -DarchetypeArtifactId=maven-archetype-quickstart -DgroupId=wild.wind -DartifactId=wadetest
</pre>
記得 maven-archetype-quickstart 換成你要使用的 artifactId 就好。如果不知道 artifactId 在哪裡，跑列表的時候就有看到囉，想辦法找你常用的吧。
<pre>
....
264: remote -> org.apache.maven.archetypes:maven-archetype-quickstart ....
265: remote -> org.apache.maven.archetypes:maven-archetype-site   ...
</pre>

回來現場。打開新建的 Maven 專案目錄，最簡單的 Java 專案可以看到裡面的目錄結構大致如下：
<pre>
wadetest
    POM.xml
    src
        main
            java
        test
</pre>
這是最簡單的樣子，Maven 專案的大原則就是要有 POM.xml ，以及 src 下有 main、test ，test 是用來放測試程式的。main 下面放的是 source code，依照你的專案類型 main 底下有不同的子目錄分類。Spring 類型的 Maven 專案就可能長這樣：
<pre>
wadetest
    POM.xml
    src
        main
            java
            resources
            webapp
                WEB-INF
        test
</pre>

compile 後 src 會多一個兄弟目錄 target。裡面就是放著成品，可能是 classes ，也有可能是打包後的 jar。

<pre>
wadetest
    POM.xml
    src
        main
            java
        test
    target
        classes
</pre>

就這樣，跟一般 java 專案目錄結構比起來，src 下多了 main、test 而已，而不是直接放 source code。

## 執行專案建構週期
試試依序以下的命令，並在每一次執行後，去看看目錄的變化

+ mvn compile
+ mvn test
+ mvn package
+ mvn clean

以後要直接打包，直接 `mvn package` 就好，前面的步驟會自動先做。

## 進階指令
### 想要知道這個 plugin 有哪些 goals
可以使用 help 這個 plugin 裡面的 describe。示範用來查詢 compiler 這個 plugin：
<pre>
mvn help:describe -Dplugin=compiler -Dmedium=true
</pre>

或者如果 plugin 有提供 help，就直接執行看看：
<pre>
mvn compiler:help
</pre>


## 待學習
如果只是單純的私下開發與管理，以上的觀念應該是夠用了，但是如果要跟外面世界合作應該還要搞懂 Remote Repository。另外 Local Repository & Remote Repository 負責了 Maven 的依賴性管理，詳細機制需要再研究。

再找時間踹看看。