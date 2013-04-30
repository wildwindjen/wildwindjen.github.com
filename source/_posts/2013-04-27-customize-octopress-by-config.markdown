---
layout: post
title: "Customize Octopress by Configuration"
date: 2013-04-27 18:49
comments: true
categories: 
- Octopress
---
## 總結
<pre>
女為悅己者容。
</pre>

這一部份是記錄研究如何客製化成我想要的樣子。先搞定功能的部分。

外表部分，其實早在安裝 octopress 之前看過一輪網路上的參考，就知道預設版型都一樣，不像 wordpress，還有得挑。所以想要有點個人特色，就得靠自己樣樣來。不過看了官方文件，發現常用的東西要客製化也算簡單，真的深入一點可能就得要會 Ruby 打造術了吧。我不會 Ruby，暫時也還沒有計畫去研究，所以我想要先從內建的機制下手: _config.xml。再加上一些些基本 css 和圖片的調整，應該可以有個不太一樣的東西。

功能部分，看了 _config.xml 的設定項目，發現還內建了好幾個 plugin。最開心的是有 syntax highlighting 的部分。這真的是想要做程式相關分享的一大福音啊。


## 日期格式
我對日期格式有一種莫名的偏執。

+ 參考 [Ruby Doc](http://www.ruby-doc.org/core-1.9.2/Time.html#method-i-strftime "Ruby Doc datetime format")
+ 打開 _config.xml 修改成:
<pre>
date_format: "%Y-%m-%d %H:%M"
</pre>


## 留言討論 - 預設的 Disqus
內建的討論串機制是使用 disqus，要設定真的超級簡單的。假設是沒有 Disqus 帳號的人: 

+ 註冊帳號。
+ 去註冊信箱收信，並且啟動帳號。
+ 登入 Disqus。點選「Dashboard」功能。
+ 登入首頁左上方有個 「register a site」的連結。
+ 填入 Site URL 和 Site Name 就會自動產生 short name 資訊。點選 continue 之後，隨便選就好，只是要產生你要放的 code。可是我們用不到。
+ 把 short name 填入 _config.xml 裡:
<pre>
disqus_short_name: short name
disqus_show_comment_count: true
</pre>
+ 是的，這樣就行了，只要執行 deploy 就可以看到下面有 disqus comments 囉。


