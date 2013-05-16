---
layout: post
title: "馬糞(Maven)初體驗"
date: 2013-05-16 22:42
comments: true
categories: 
---
## 總結
<pre>
工欲善其事，必先利其器。
</pre>

知道馬糞這個東西很久了，最近突然想起來，來玩看看。當然另一方面，是想要拿來管理一些專案。


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


## 基本觀念
### Build Lifecycle 和 Phases 和 Goals
和 Ant 最大的不同在於，Maven 是在慣例制的(Over Convension)，也就是說，很多東西可以不設定，會有預設值，而這個預設值，是大部分情況下適用的。不用像 Ant 一樣，每個動作都指定到位。只需要針對和慣例不同的特殊狀況，再去設定即可。

要使用 Maven 之前要先了解 `建構生命週期(Build Lifecycle)`。Maven 內建了三大 Build Lifecycle： Default、Clean、Site。Default 負責了專案的佈署流程。Clean 處理專案清空的任務。Site 則用來產生描述專案相關資訊的站台文件。

建構生命週期由許多建構階段(Build Phases)組成，建構階段代表的是生命週期裡的步驟。各生命週期組成的建構階段可以看[Lifecycle Reference](http://maven.apache.org/guides/introduction/introduction-to-the-lifecycle.html#Lifecycle_Reference  Lifecycle Reference)。

建構階段由建構目標(Goals)組成。看看官方說法：
<pre>
A goal represents a specific task (finer than a build phase) which contributes to the 
building and managing of a project. It may be bound to zero or more build phases. A 
goal not bound to any build phase could be executed outside of the build lifecycle by 
direct invocation. The order of execution depends on the order in which the goal(s) 
and the build phase(s) are invoked.
</pre>

例如：`mvn clean dependency:copy-dependencies package`。 clean 和 package 是建構階段，dependency:copy-dependencies 是建構目標。執行順序則依序為 clean、dependency:copy-dependencies、package。建構目標可以連結到零或多個建構階段。建構目標連結一到多個建構階段的，只要執行到這些建構階段，也會一併執行建構目標。連結到零個建構階段的建構目標，只能透過直接下命令才執行得到。什麼建構目標連結到什麼建構階段，跟所屬的 packaging 有關，[Built-in Lifecycle Bindings](http://maven.apache.org/guides/introduction/introduction-to-the-lifecycle.html#Built-in_Lifecycle_Bindings  Built-in Lifecycle Bindings)定義的很清楚。





所以下命令時，指定執行的生命週期及階段，例如：`mvn clean install`。只有 Default 生命週期可以直接指定階段即可，例如：`mvn compile`。階段有其執行順序，所以當下`mvn compile`指令時，從 validate 到 compile 都會被執行。

常用的階段如下：
+ compile: 編譯程式。
+ test: 測試已編譯的程式。
+ package: 將已編譯的程式打包。例如打包成 JAR 檔。
+ install: 將打包檔放進去本地的 Respository。
+ deploy: 將最後成品分享至遠端的 Respository。



### Repository