---
layout: post
title: "get back octopress"
date: 2013-04-26 23:58
comments: true
categories: octopress
---
## 阿丞語錄
<pre>
天行健，君子以自戕不息。
有產出，就應該有回收，維持循環的地獄。
</pre>

## 總結
都放上去 GitHub 了，總得演練怎麼拿回來。下面補充上一篇沒有提到的備份(octopress 官網建議的方式)，並且示範怎麼把備份的東西重新拿回來。

##備份
+ stage octopress 裡面的檔案
<pre>
git add .
</pre>
+ commit 檔案到本地檔案庫，並且填寫描述訊息。
<pre>
git commit -m "備份 source"
</pre>
+ 從本地檔案庫推到遠端檔案庫，並且指名放在 source branch 裡面。
<pre>
git push origin source
</pre>

##取回
+ 假設現在換了一台電腦(先把原本的 octopress 更名)
+ 先從 source branch 複製一份回來，放到 octopress 目錄裡。
<pre>
git clone -b source git@github.com:wildwindjen/wildwindjen.github.com.git octopress
</pre>
+ 切換到 octopress 目錄裡面
<pre>
cd octopress
</pre>
+ 從 master branch 複製一份回來，放到 _deploy 目錄裡。(之前 rake deploy 就是把資料推到 master 裡。)
<pre>
git clone git@github.com:wildwindjen/wildwindjen.github.com.git _deploy 
</pre>
+ 重新安裝必要的環境及設定
<pre>
gem install bundler
bundle install
rake install
</pre>
+ 重設編碼 (如果跟我一樣是在 windows 上面測試的話。)
<pre>
set LC_ALL=zh_TW.UTF-8
set LANG=zh_TW.UTF-8
</pre>