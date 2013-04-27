---
layout: post
title: "customize octopress by config"
date: 2013-04-27 18:49
comments: true
categories: 
---
## 阿丞語錄
<pre>
為悅己者容。
</pre>

## 總結
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


## 留言討論 - 綁 Google 的留言
因為我跟 Disqus 沒那麼熟。雖然 Google Plus 好像也沒熟去哪裡，可是我至少有帳號。我是參考這一篇的: [Google Plus Comments Link for Octopress](http://blog.justin.kelly.org.au/google-plus-octopress/ "Google Plus Comments Link for Octopress")。以下是我的經驗整理：

+ 首先要設定 _config.xml 裡的 googleplus_user，這裡面的是 google plus id，純數字。這一關我有被騙到，一開始自以為聰明直接打 Google 帳號。不知道怎麼找自己 id 的人可以看 [How Do I Find My Google Plus User ID?](http://ansonalex.com/google-plus/how-do-i-find-my-google-plus-user-id-google/ "How Do I Find My Google Plus User ID?") 。
+ 因為會用到 Google API Key，所以先去 [API Console - Google Code](https://code.google.com/apis/console/ "API Console - Google Code")。然後參考 [Create Your Own Google API Key](http://www.designchemical.com/blog/index.php/faq/create-your-own-google-api-key/ "Create Your Own Google API Key") ，開啟你的 Google+ API 服務，讓他變成 ON 的狀態。
+ 切到 API Access，創建新的 Key(Create New Browser Key)，記住新的 key 值。
+ 接著就照著 [Google Plus Comments Link for Octopress](http://blog.justin.kelly.org.au/google-plus-octopress/ "Google Plus Comments Link for Octopress") 說的，修改 `source/_includes/custom/after_footer.html` 和 `source/_includes/custom/head.html` 兩個檔案。其中的 `{YOUR_GOOGLE_API_KEY}` 記得換成剛剛的 Google API Key。
+ 然後有個規則要特別注意的是，每發一篇新的文章，要自己先 +1 一下，而且得設定成_公開_，不然不會出現 comments 區塊。我被這一關卡超久的，因為我預設都是限定開放。後來發現 API 回傳的資料都是空，可是登入 Google Plus 又查得到，稍微發揮了點推理能力才解決。
+ 使用 preview 是看不到結果的，得 deploy 才行。除非...，跟我一樣，把 `after_footer.html` 做點修改：
<pre>
//$post_link = encodeURI(jQuery(location).attr('href'));
$post_link = window.location.pathname;
</pre>


## 留言討論 - 綁 Google 的留言(終極版)
是的，這是最終我採取的做法。好處是可以直接在下方留言，不用點連結。這個做法是踹上面那個解法過程中，發現的可能做法。請先看一下 [How to Add Google+ Comments to Any Webpage or Blog](http://dashburst.com/how-to-add-google-comments-to-any-webpage-or-blog-unofficially/ "How to Add Google+ Comments to Any Webpage or Blog")。以下是我自己的做法：

+ 修改 `source/_layouts/post.html` ，在 Disqus 後面補上一段程式碼：
{% codeblock google comments html %}
	{% raw %}
	{% if site.disqus_short_name and page.comments == true %}
	  <section>
		<h1>Comments</h1>
		<div id="disqus_thread" aria-live="polite">{% include post/disqus_thread.html %}</div>
	  </section>
	{% endif %}

	<!-- Google Comments -->
	{% if site.googleplus_comments == true %}
	  <section>
		<script src="https://apis.google.com/js/plusone.js">
		</script>
		<div class="g-comments"
			data-href=window.location
			data-width="789"
			data-first_party_property="BLOGGER"
			data-view_type="FILTERED_POSTMOD">
		</div>
	  </section>
	{% endif %}
	<!-- Google Comments end -->
	{% endraw %}
{% endcodeblock %}

+ 接著在 `_config.xml` 中加個參數。如果不想加這個參數，可以把上一段的 if 判斷拿掉，這樣就會一直都出現。維持橡皮人的彈性只是職業病而已。
<pre>
# Facebook Like
facebook_like: false

# google comments, by wildwindjen
googleplus_comments: true
</pre>

+ 這樣就好了，步驟比上個做法簡短，結果更漂亮。只是得改 code 而已。話說 octopress 有提供客製化 post 的機制嗎？實在不是很喜歡這樣直接改 source code。 我喜歡客製化部分跟原生部分切開來。
+ 如果你遇到了 unsafe javascript 的訊息，請改成跟我一樣的做法。
<pre>
	data-href = "http://wildwindjen.github.io" + window.location.pathname
</pre>