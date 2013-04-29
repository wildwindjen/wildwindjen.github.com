---
layout: post
title: "first octopress at github"
date: 2013-04-25 12:30
comments: true
categories: octopress
---
## 阿丞語錄
<pre>
不經一試，不長一智。
結果是甜美的，過程是母親的。
</pre>

## 總結
也不知道在失心瘋什麼，沒碰過 Git，也沒碰過 Ruby，居然學人家搞 octopress。不管怎樣，就這樣一路跌跌撞撞，不懂什麼就補齊什麼。也拼拼湊湊出來一些東西。就順便把經驗整理一下。

在有 proxy 的環境下安裝(ex: 公司)，跟 github 有相關的操作，統一改用 HTTPS 會比較沒問題。在家的話，基本上隨便 google 參考都可以裝成功。因為之前找到的資料幾乎都是 SSH ，反而因為比較少 HTTPS 的方式。以下就直接拿 HTTPS 的建立方式作分享，因為這個方式在家也適用。

## 我的環境
+ OS: Window XP
+ Git: Git-1.8.1.2-preview20130201
+ Ruby: Ruby 1.93

## GitHub
+ 去 [GitHub](http://github.com "GitHub")
+ 沒帳號就申請帳號
+ 創建帳號後，登入，然後建立一個 Repository，名字叫做 your_github_id.github.com。 (例如我的就是 wildwindjen.github.com)

## 安裝 Git
+ 下載 [Git](http://git-scm.com/ "Git")
+ 直接執行安裝檔，中間除了我想讓 Path 找得到 git ，有改過這頁的設定(Run Git from the Windows Command Prompt)，其他步驟都是預設值。

## 設定 GitHub SSH Key (這一段是想走 SSH 的人要做的)
+ 參考 [generating-ssh-keys](https://help.github.com/articles/generating-ssh-keys "generating-ssh-keys")
+ 打開 Git Bash
<pre>
    cd ~/.ssh
</pre>
如果沒有的話就自己建立一個 .ssh
+ 產生 ssh key
<pre>
    ssh-keygen -t rsa -C "your_email@example.com"
</pre>
+ 輸入你想輸入的密碼 passphrase (有寫比較安全，但也可以留空)
+ 複製產生出來的 key，以下的語法我在 window 7 可以執行，但是 XP 不行。(所以直接去 C:\Documents and Settings\wildwindjen\.ssh\id_rsa.pub 用編輯器開檔，全選、複製就可以了。)
<pre>
    clip < ~/.ssh/id_rsa.pub
</pre>
+ 登入 GitHub > Account Setting > SSH Keys，點選 Add SSH Keys 按鈕，名字亂取，把剛剛複製的貼上就好。

## 安裝 Ruby
+ 參考這篇 [在windows上安裝octopress 新手教學 / 初學者指南](http://itspg.github.io/blog/2012/02/29/octopress-on-windows-tutorial/ "在windows上安裝octopress 新手教學 / 初學者指南")
+ 下載 Ruby 和 DevKit
+ 安裝 Ruby ，一路上都是預設值。
+ 新增一個 devkit 目錄，解壓縮 DevKit 到這個目錄 devkit 裡面
+ 打開命令提示字元，切換目錄到 devkit，執行以下目錄，安裝開發用的工具：
<pre>
    ruby dk.rb init
    ruby dk.rb install
</pre>
+ 更新 gem。
<pre>
    gem update --system
</pre>

## 安裝 octopress
+ 切換到你想要放 octopress 的目錄
+ 執行以下命令，注意到我這邊是使用 https。
<pre>
	git clone https://github.com/imathis/octopress.git octopress
	cd octopress
	gem install bundler
	bundle install
	rake install
</pre>

## 設定 octopress
+ 先設定 octopress 要放上去的 Repository
<pre>
    rake setup_github_pages
</pre>
+ 接著會問你要對應的 github 路徑是哪一個，輸入 your_github_id.github.com 路徑。例如我的是長這樣：
<pre>
    git@github.com:wildwindjen/wildwindjen.github.io
</pre>
+ 接著就可以打開 octopress/_config.xml 作其他的個人化設定。

## 撰寫 octopress
+ 先新增一篇文章：
<pre>
    rake new_post["文章名稱"]
</pre>
+ 去 octopress/source/_posts 就可以看到副檔名 .markdown 的文章。
+ 使用慣用的編輯器打開，使用 markdown 語法開始寫文章吧。
+ 完成的話，要執行以下命令，讓 markdown 檔案轉換成 html：
<pre>
    rake generate
</pre>
+ 想要在本機預覽:
<pre>
    rake preview
</pre>
+ 打開瀏覽器，輸入 [http://127.0.0.1:4000/]，就可以預覽結果。(要結束預覽回到命令模式請 ctrl+c)
+ 如果你跟我一樣，有遇到亂碼問題(YAML Exception reading xxx.markdown: invalid byte sequence in CP950 ...)，請：
    + 確認檔案編碼是不是使用 UTF-8 存檔。
    + 輸入以下指令
    <pre>
        set LC_ALL=zh_TW.UTF-8
        set LANG=zh_TW.UTF-8
    </pre>
    + 再重新 rake generate 一次
    
## 發佈 octopress
+ 預覽都沒問題，就是該放到 github 上囉。直接發佈看看：
<pre>
    rake deploy
</pre>
+ 假設你跟我一樣，因為某些緣故不能使用 SSH。先確認一下你的 origin，在 octopress/_deploy 目錄下輸入：
<pre>
    git remote -v
</pre>
+ 應該是會看到以下格式
<pre>
    origin  git@github.com:wildwindjen/wildwindjen.github.io (fetch)
    origin  git@github.com:wildwindjen/wildwindjen.github.io (push)
</pre>
+ 請輸入以下指令作修改，改走 HTTPS 的方式 deploy：
<pre>
git remote set-url origin https://github.com/wildwindjen/wildwindjen.github.com.git
</pre>
+ 重新 rake deploy 就好囉。
+ 因為之後會上傳 source ，所以我也在 octopress/ 下確認一次 origin，這邊也是得改走 HTTPS。

## 未驗證
寫文章至此，我猜想在做「rake setup_github_pages」步驟時，直接填入 https 格式，應該就沒有後面的麻煩了。之後有時間再踹踹看。

## 其他經驗
### SSH 的 Debug 方式
過程中遇到一堆 SSH 的問題，直到遇見這個語法，才解除了我盲目的人生。
<pre>
    ssh -vvv git@github.com
</pre>

### SSH 的客製化設定檔
.ssh 目錄下面可以自己新增一個檔名 config 來做一些客製化的動作，例如：假設有需要把預設的 port 22 改換成 443，可以透過這一招。可惜這次沒機會表現。
<pre>
    Host github.com
    User wildwind.jen@gmail.com
    Hostname ssh.github.com
    Port 443
</pre>