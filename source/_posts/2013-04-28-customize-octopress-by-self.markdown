---
layout: post
title: "customize octopress by self"
date: 2013-04-28 11:06
comments: true
categories: octopress
---
## 阿丞語錄
<pre>
士為悅己者死。
</pre>


## 總結
有些東西不是單純靠 config 就能弄出來的。偏偏自己又想要有這些東西。所以搞死自己也是算活該吧。


## 留言討論 - 綁 Google 的留言
因為我跟 Disqus 沒那麼熟。雖然 Google Plus 好像也沒熟去哪裡，可是我至少有帳號。一開始我是參考這一篇的: [Google Plus Comments Link for Octopress](http://blog.justin.kelly.org.au/google-plus-octopress/ "Google Plus Comments Link for Octopress")。但這個作法只是出現連結，而且每篇產生後還得自己 +1。所以最後我沒採用。以下是我的經驗整理：

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
是的，這是最終我採取的做法。留言直接在下方出現，不用點連結，也不用每篇寫完後要自己先 +1。這個做法是踹上面那個解法過程中，發現的可能做法，然後自己在 octopress 上試出來的。請先看一下 [How to Add Google+ Comments to Any Webpage or Blog](http://dashburst.com/how-to-add-google-comments-to-any-webpage-or-blog-unofficially/ "How to Add Google+ Comments to Any Webpage or Blog")。以下是我自己的做法：

+ 一樣要設定 _config.xml 裡的 googleplus_user 。
+ 修改 `source/_layouts/post.html` ，在 Disqus 後面補上一段程式碼：
{% codeblock google comments html %}
	{% raw %}
	{% if site.disqus_short_name and page.comments == true %}
	  <section>
		<h1>Comments</h1>
		<div id="disqus_thread" aria-live="polite">{% include post/disqus_thread.html %}</div>
	  </section>
	{% endif %}

	<!-- Google Comments, by wildwindjen -->
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

+ 接著在 `_config.xml` 中新增一個 `googleplus_comments` 參數。如果不想加這個參數，可以把上一段的 if 判斷拿掉，這樣就會一直都出現。維持橡皮人的彈性只是職業病而已。
<pre>
# Facebook Like
facebook_like: false

# google comments, by wildwindjen
googleplus_comments: true
</pre>

+ 這樣就好了，步驟比上個做法簡短，結果更漂亮。只是得改 code 而已。話說 octopress 有提供客製化 post 的機制嗎？實在不是很喜歡這樣直接改 source code。 我喜歡客製化部分跟原生部分切開來。
+ 如果你留言送出時，遇到了 `Unsafe JavaScript attempt to access frame ... ` 的訊息，可以改成跟我一樣的作法。我的作法可以讓本機的 `preview` 也正常。這個問題讓我弄了大半天。原因只是 URL 有誤。可是訊息提示很容易誤導。我最後是在[唯一的線索](http://browsingthenet.blogspot.tw/2013/04/google-plus-comments-on-any-website.html "這邊")看到某篇回覆參透的。
<pre>
	data-href = "http://wildwindjen.github.io" + window.location.pathname
</pre>