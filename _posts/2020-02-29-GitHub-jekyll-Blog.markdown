---
layout: post
title:  "從零開始的網誌生活－Windows+GitHub+jekyll架設"
date:   2020-02-29 13:40:00 +0800
categories:
- Program
tags: git GitHub jekyll blog
description: 從零開始的阿油鹹鹹事，以Windows10+GitHub+jekyll從頭開始架設。
---

###### 最後修改日期｜Mar 03, 2020

# DowYuu言

紀錄一下我的鹹鹹事的建立過程XD

本篇文章會用簡單的方式且 **不會說明詳細概念原理**，請良民小心服用，有需要的良民請自行上網看看巨巨們的詳細觀念分享。

# 從Git開始

還記得以前學生時代接觸過但懵懵懂懂，在那邊乖乖打指令不懂在打什麼鬼東西。

後來再次接觸後在前輩指導下才有種豁然開朗的感覺，又用了圖形化介面一整個舒舒服服！

## 安裝TortoiseGit

可愛的圖形化介面Git龜龜醬，到[TortoiseGit下載頁][TortoiseGitDownloadPage]去下載**程式**和**語言安裝包**，程式看自己電腦幾位元的就去載哪個，這部分應該不用多說。

![TortoiseGit下載頁圖片][1-1]

![TortoiseGit開始安裝][1-2-1]

基本上沒有要多做設定就是一直「Next」到天荒地老，安裝完畢後的畫面會預設勾起要啟動首次啟動精靈，就順勢繼續做下去吧！

![TortoiseGit安裝完成][1-2-2]

## TortoiseGit首次啟動設定

一開始進入首次啟動精靈，就會有選擇語言的部分，預設的語言只有英文，擴充語言包是要另外下載的。

![TortoiseGit首次啟動語言選擇][1-3-1]

不過剛剛我們已經有跟TortoiseGit一起下載好了，此時的首次啟動精靈可以先擱著，點開剛剛下載的繁體中文語言包。

![TortoiseGit中文語言包安裝][1-3-2]

![TortoiseGit中文語言包安裝完成][1-3-3]

按下一步後，安裝的速度非常的快就完成了，點擊完成回到剛剛的首次啟動精靈，就會發現語言中多了一個繁體中文的選項啦，可喜可賀！

![TortoiseGit可以選中文了][1-3-4]

接著點下一步。

![TortoiseGit首次啟動說明頁][1-3-5]

此時會跳出需要git執行檔的頁面，畢竟一開始我們只裝了圖形介面，最重要的本體還沒安裝XD

小精靈很貼心了附上你目前平台所適用的git下載連結，我是win10就貼了個Windows用的git給我，感動得痛哭流涕。

![TortoiseGit首次啟動git設定頁][1-3-6]

來到git for windows的頁面，首頁大大的Download按鈕給他點下去。

![git下載頁][1-3-7]

載完點開安裝程式，也是下一步到底。

![git安裝1][1-3-8]

![git安裝2][1-3-9]

安裝完git回到首次啟動精靈，點擊立即檢查，路徑就會自己跑出來囉！（沒自動跑出來就委屈自己手動連結吧）

![TortoiseGit首次啟動git設定頁已設定git路徑][1-3-10]

接著是設定使用者資訊，自行輸入後再按「下一步」。

![TortoiseGit首次啟動使用者資訊][1-3-11]

認證和憑證設定部分就預設就好，點擊「完成」。

![TortoiseGit首次啟動認證頁][1-3-12]

這樣初步的設定就完成了！在資料夾中點右鍵就會發現有git和TortoiseGit的選項囉！！

![TortoiseGit首次啟動設定完成，右鍵已有TortoiseGit設定][1-3-13]

# 接著是GitHub

## 辦個GitHub帳號吧

如題，要把網誌放在GitHub上，總是要辦個GitHub帳號吧，[去去去][github]。

## 建立新專案

辦完帳號認證登入後，點擊右上角的「＋」號，點擊「New repository」。

![點擊「New repository」][2-1]

到了新增頁面，這邊最主要就是要輸入Repository name（專案名稱）。

要GitHub當做靜態頁面伺服器，專案名稱要特別設定為「`GitHub帳號`.github.io」，像我的帳號是dowyuu，專案名稱就要設定為：dowyuu.github.io。

![專案名稱輸入GitHub帳號.github.io][2-2]

完成建立後會跳到此專案的主頁，這邊就會顯示你要上傳這個專案的鏈結。

![新增專案完成][2-3]

# 人家基於Ruby

這邊使用的網誌套件為`jekyll`，是個靜態站點生成器，如果有動態需求就要去另請高明了。

因為`jekyll`是基於`Ruby`，理所當然第一步要去安裝囉`Ruby`！

## Windows Ruby安裝

不同作業系統有不同的下載方式，油仔我Win10所以用Windows的安裝方法。

Windows安裝`Ruby`要用`RubyInstaller`，[網址在此][RubyInstaller]。

這邊有列出各種版本，原本我首次安裝的時候不信邪的裝了最新版，結果後續在「jekyll安裝主題樣式」的步驟遇到了一些問題，所以建議還是乖乖地選那個突出=>的版本齁Q____Q

![RubyInstaller下載頁][3-1]

沒有特別需求一樣就是當個Next狂人。

![開始安裝Ruby][3-2-1]

![安裝Ruby完成][3-2-2]

安裝完後，會跳到MSYS2的安裝畫面，就輸入「3」並按Enter，Ruby就會幫你安裝許多有的沒的東西。

![MSYS2安裝頁][3-3-1]

看到succeeded就可以關掉了！

![MSYS2安裝完成][3-3-2]

# jekyll颯爽登場

## 還是要安裝

第一步還是要來安裝囉！開啟你的命令提示字元，輸入RubyGems的安裝指令。

```
gem install jekyll
```

![輸入gem install jekyll安裝jekyll][4-1]

RubyGems是個套件管理系統，可以透過它安裝套件與其相關依附套件，像是我們透過它安裝了jekyll，它會自動將其本身與所使用到的其他套件一併安裝到好，舒舒服服。若是要查看目前gem有安裝了什麼與其版本，可以用下方指令：
```
gem list
```

![輸入gem list查看套件列表][4-2]

## jekyll的第一個家

用命令提示字元`cd`到你要存放jekyll檔案的目錄下，像我就是將檔案放在`E:\jekyll test`下。

輸入建立初始jekyll專案的指令：

```
jekyll new firstjekyll(檔案資料夾名稱)
```

![新增jekyll專案][4-3]

jekyll會在目前的資料夾下再建一個資料夾，名稱就是`jekyll new`後方輸入的名稱，並把`jekyll`的初始內容檔案放在裡面，所以要再進入到此資料夾中。

```
cd firstjekyll(檔案資料夾名稱)
```

此時開啟此資料夾，就可以看到裡面的初始內容檔案。

![初始jekyll專案內容][4-4]

## 啟動jekyll

這時候就可以在本機上啟動jekyll網站看看囉！！

```
jekyll serve
```

![輸入jekyll serve啟動jekyll][4-5]

這時打開瀏覽器，網址輸入<http://127.0.0.1:4000/>就可以看到初始網站囉！

![啟動jekyll成功][4-6]

## 文章怎麼建？

這時候再次打開這個資料夾，會發現資料夾中比原先的初始內容多了一些東西，~~不想懂原理的話就不用管它自己上網查~~，網誌中最重要的文章部分釋放在`_posts`中，打開`_posts`資料夾。

![啟動jekyll後的資料夾][4-7]

會看到裡面有一個`.markdown`檔案，這就是我們初始檔案中的那篇文章囉！

![posts為文章放置處][4-8]

`jekyll`的文章都是用`markdown`撰寫，往後要建立文章就要新增一個`markdown`檔案（副檔名為`.markdown`或`.md`），檔名格式為：

```
西元年-月份-日期-文章檔案名稱.markdown
```

文章檔案名稱建議還是乖乖用英文比較妥，且是文章檔案名稱 **並不是文章標題名稱**，文章檔案名稱是點進文章中會顯示在網址列的名稱，文章標題的設定是在檔案裡面。

## 來看看文章內容

打開預設建立的文章，會看到最上方標頭處會有些設定，`title`是要顯示的文章標題名稱，`date`是日期但實際還是會依據`markdown`檔案名稱的日期去編譯，`categories`為分類，不同主題樣式也可能會有不同的設定。

![文章內容][4-9]

文章內容的寫法就是`markdown`寫法，詳細用法可以參考我的另一篇文章：[MarkDown－優雅排版沒煩惱][markdownNote]。

## 幫網誌穿上漂漂衣

### 下載主題

前往[jekyllthemes][jekyllthemes]去找一個自己順眼的主題點進去，點擊頁面上的「Download」。

![找個順眼的jekyll主題][4-10]

### 安裝主題問題解決

下載下來的壓縮檔解壓縮後，一樣用命令提示字元進入此目錄，對它啟動看看`jekyll serve`，咦怎麼跟剛剛成功啟動的訊息不同呢？

![錯誤訊息顯示資源缺少][4-11]

看最後一行會發現它顯示沒有某個套件的資源，這是因為許多主題所搭配的功能套件，我們並沒有去一一下載。  
當然你要慢慢看它缺什麼就去下載也是可以，但畢竟功能越複雜的主題用的套件就越多，一一下載也太累了，這邊我們用`bundler`來輕鬆解決這個問題。

`bundler`也是`ruby`的套件，是用來解決gem套件相容性問題的管理器，預設`ruby`是會安裝的，如果沒安裝`bundler`同樣可以用`gem`下載：`gem install bundler`。

在使用`bundler`下載套件前，有沒有看到資料夾中有個`Gemfile.lock`，不要問，刪掉它就對了。

![刪除Gemfile.lock][4-12]

這邊用`bundler`的下載指令，它會依照資料夾內的設定檔去幫你一一下載需要的套件。

```
bundle install
```
![bundle安裝][4-13]

部分套件安裝時會跑出一堆訊息不用害怕，基本上看到`Bundle complete!`就代表完成囉（不一定在最後一行，找一下XD）！

![bundle安裝完成][4-14]

有時在安裝時會遇到錯誤訊息，其實他的錯誤訊息都寫得滿清楚的，並且也會放上建議語法。像是下圖是我遇到的錯誤，就是某個套件版本必須在2.6以下，但我的版本是2.6.5.114，它就顯示建議我安裝1.9.25的版本，並附上語法`gem install ffi -v '1.9.25' --source 'https://rubygems.org/'`，484超貼心的？

![bundle錯誤][4-15]

Bundle成功後，再一次`jekyll serve`啟動看看，嗯？怎麼又有問題了？

![套件版本不符][4-16]

看了一下訊息，他說我有某套件1.8.2版本，但我的`Gemfile`要0.9.5版，用`gem list`看一下那個套件，原來是安裝到兩個版本（不同版本會以`，`隔開）。

![安裝到兩個版本][4-17]

這時候可以把用不到的版本移除掉，使用`gem`移除某版本號的指令。

```
gem uninstall 套件名 -v 版本號
```

像我這裡要移除多的版本就用`gem uninstall i18n -v 1.8.2`，顯示成功即可。

![移除套件版本][4-18]

在刪除版本時有時會遇到刪除某版本的套件時，會問你它連帶的套件版本要不要一起刪除，基本上就是Y就好，反正頂多安裝回來，像是我用的主題使用3.8.5版本的`jekyll`，但當初安裝時沒輸入版號就會安裝到最新版（4.0.0），使的要刪掉4.0.0版本時會跳出問一堆連帶的套件要不要刪掉。

如果是完全不懂其原理的人~~像我~~，過程基本上就是循環：

`jekyll serve`看有何套件有版本衝突→`gem uninstall 套件名 -v 版本號`刪除多於版本→刪除`Gemfile.lock`→`bundle install`

解決了套件的問題，終於看起來啟動了，結果又來了個黃字紅字錯誤，看了紅字第一行，`GitHub Metadata: Error processing value 'description'`表示缺乏了`description`屬性，開啟你此專案中的`_config.yml`檔案，這個檔案中是專門放此部落格的各種設定，找到`description:`的部分並在後面加上文字即可，奇怪捏不寫也不行逆XD

![description缺乏錯誤][4-19]

再一次啟動，又有問題了。

```
Conversion error: Jekyll::Converters::Scss encountered an error while converting 'assets/css/style.scss':
       Invalid CP950 character "\xE2" on line 5
```

![Conversion缺乏錯誤][4-20]

看到錯誤訊息中的路徑了嗎？讓我們去看看這個`assets/css/style.scss`...啊？沒有這個檔案？

簡單簡單，沒有就自己建，在`assets/css`中建立`style.scss`，並在內容中打上下方內容即可。

```markdown
---
---

@charset "UTF-8";
```

最後的最後，終於運行起來啦！！！有木有感動RRRRR

![終於成功啟動][4-21]

![安裝完主題後啟動畫面][4-22]

除了基本頁面資訊設定，不同的主題有不同的設定內容，在這邊就不多說了，各位良民可以自行去看看該主題的說明敘述噢。

# 把網誌放上GitHub

原先我們已經[把TortoiseGit安裝好](#從git開始)並[辦了GitHub帳號](#接著是github)了，接著最後步驟就是把剛剛[建立好的jekyll專案](#jekyll颯爽登場)放上去GitHub。

**這邊只講單純一台電腦將專案放上`github`的指令而已**，並不會講到工作區(`workspace`)、暫存區(`index`)、檔案庫(`repository`)、分支(`branch`)...等等詳細觀念，有興趣的良民請自行餵狗，網路上的巨巨們會給你詳細且正確的觀念解答的。

不專業的從簡說，`git`在本機端會有個檔案庫，在遠端(以本篇例子遠端檔案庫就是指`github`)也會有個檔案庫。

我們要做的動作是將我們的專案提交(`commit`)給本機端檔案庫，再將本機端檔案庫的資料推送(`push`)給`github`。

順帶一提，做為公開靜態頁面的`GitHub`專案，**專案資料夾中的所有檔案都是公開的**，不要亂藏一些有的沒的在裡面喔人家都看的見喔XDDDD

## 先來建立本機端檔案庫

因為我們使用`TortoiseGit`相對好上手，在我們的`jekyll`專案資料夾中點右鍵，有個「`Git在此建立版本庫`」，用力給他點下去就對了。

![點擊「Git在此建立版本庫」][5-1]

之後會跳出Git初始化視窗，不須勾選點擊「確定」即可。

![Git初始化視窗，不須勾選點擊「確定」即可][5-2]

顯示「已在XX初始化Git版本庫」，就是建立成功囉！

![建立成功本機檔案庫成功][5-3]

此動作對應為`git`中的`git init`指令。

此時就會發現，在資料夾中多了一個「.git」的隱藏資料夾，也就等於開始了這個專案資料夾的版本控制囉！

![多了一個「.git」的隱藏資料夾][5-4]

## 將資料提交給本機端檔案庫

建立好後的本機端檔案庫目前只是一個空殼，因為並不是目前檔案就會自動跑進去做版本控制，而是會控制每一次你提交(`commit`)給本機端檔案庫的版本，所以你在此專案資料夾中做任何更動，包括新增檔案、刪除檔案、異動檔案都要提交(`commit`)給本機端檔案庫才會是一個新版本。

同樣是在我們的`jekyll`專案資料夾中點右鍵，有個「`Git提交 (C)->"master"`」，再次後方的"master"是預設的分支名稱，分支觀念這裡不多做說明，點擊後會出現`TortoiseGit`的提交視窗：

![TortoiseGit提交視窗][5-7]

上方區塊是撰寫此版本訊息的區塊，就是讓你備註這個版本相關資訊的地方，版本訊息為 **必填** 噢。

下方區塊是異動檔案區塊。

仔細看下方檔案列表最上方有一個小標題「未版本控制的檔案」，這是因為我們還沒將檔案加入版本控制，所以要告訴目前是空殼的本機端檔案庫這些檔案要做版本控制的，此動作對應為`git`中的`git add [檔案名稱]`指令。

![未版本控制的檔案][5-8]

到目前為止，是不是聽起來有點複雜快要受不了了？

開心的是，在`TortoiseGit`中不需要區分將檔案新增進版本控制、將檔案自版本控制中移除、異動檔案等等指令，**只要執行提交就會自動幫你執行相對應動作的指令** ，舒舒服服！

下方區塊會列出「目前檔案庫中的版本」和「目前目錄」的差異檔案，剛剛我有說過剛新增的本機端檔案庫只是一個空殼，所以才會全部的檔案都是差異檔案。

但你有沒有注意到下方檔案列表前有可勾選的小方框？

![檔案前有可勾選的小方框][5-9]

是的，要提交的檔案記得前面要是勾起來的，區塊上方也有提供許多類型的點選方式，像是全選、只選未受版本控制的檔案......等等，目前的情況沒有特別的需求，所以就點全選就好，這樣全部的檔案都會被勾起。

上方區塊有輸入訊息，下方區塊有勾選要提交的檔案，兩項都完成後右下角的「提交」按鈕就可以點擊了，用力地點下去。

![輸入訊息並勾選提交檔案後，即可點擊「提交」][5-10]

此時會出現提交訊息視窗，跑出藍字就是提交成功囉！

![提交中畫面][5-11]

![提交成功畫面][5-12]

## 最後放送上GitHub吧

剛剛說了提交給本機端檔案庫，現在就要來說最後步驟－推送(`push`)給遠端檔案庫。

其實剛剛「提交成功畫面」成功後，左下角會有個「推送」的按鈕，或是在專案資料夾中點「右鍵」→「TortoiseGit」→「推送」。

![提交成功後左下角有個「推送」按鈕][5-12]

![或在專案資料夾中點「右鍵」→「TortoiseGit」→「推送」][5-13]

此時會開啟推送介面，因為是第一次推送，我們需要設置中間區塊－目的目錄的遠端選項，點下右邊的「管理」按鈕。

![推送介面，第一次推送要設置遠端選項，點擊遠端右邊的管理按鈕][5-14]

此時會進到「設定」中的「遠端」頁面。

還記得我們在建立`GitHub`帳號時最後停留的頁面嗎？在URL處貼上我們建立在`GitHub`上的專案的那串鏈結吧！

![鏈結來自建立在GitHub上的專案][5-15]

![在URL處貼上鏈結][5-16]

貼上後會發現上方會幫你預設帶入一個遠端名稱「origin」，此時直接點擊確定即可，視窗關閉後會發現遠端的下拉選單處已經有選到剛剛你設置的「origin」了，接著就是用力的點下「確定」。

![設置後會直接選到origin][5-17]

點擊「確定」後，會跳出`GitHub`的登入視窗，輸入你`GitHub`的帳號密碼吧！

![跳出視窗後登入GitHub][5-18]

此時會出現推送訊息視窗，跑出藍字就是提交成功囉！

![推送訊息視窗][5-19]

## 看著放上GitHub的網誌

恭喜你，你已經完成了所有的步驟，可以在`GitHub`上看到自己放上去的網誌了。

所以我說，那個網址呢？

不要急不要慌，網址在這裡：

```
https://[GitHub帳號].github.io/
```

像我的帳號是`dowyuu`，所以網址就是`https://dowyuu.github.io/`囉。

![放上GitHub的網誌][5-20]

而此時你`GitHub`上的專案也可以看到上傳的檔案和你提交時設置的訊息喔，再次強調因為是公開的大家都看得到喔！

![GitHub上的專案看得到檔案和提交訊息][5-20]

往後要是有任何此專案資料夾中的修改，不論是修改了檔案內容、新增了檔案、刪除檔案、新增資料夾......等等，都要 **記得做「提交」加上「推送」的動作** ，這樣`GitHub`上的網誌內容才會是最新的。

灑花灑花，如果有任何問題都可以提出討論囉，我們下次見。

[TortoiseGitDownloadPage]:https://tortoisegit.org/download/
[github]:https://github.com/
[RubyInstaller]:https://rubyinstaller.org/downloads/
[markdownNote]:https://rubyinstaller.org/downloads/
[jekyllthemes]:http://jekyllthemes.org/

[1-1]:{{site.url}}/img/2020-02-29-GitHub-jekyll-Blog/1-1.png "TortoiseGit下載頁"
[1-2-1]:{{site.url}}/img/2020-02-29-GitHub-jekyll-Blog/1-2-1.png "TortoiseGit開始安裝"
[1-2-2]:{{site.url}}/img/2020-02-29-GitHub-jekyll-Blog/1-2-2.png "TortoiseGit安裝完成"
[1-3-1]:{{site.url}}/img/2020-02-29-GitHub-jekyll-Blog/1-3-1.png "TortoiseGit首次啟動語言選擇"
[1-3-2]:{{site.url}}/img/2020-02-29-GitHub-jekyll-Blog/1-3-2.png "TortoiseGit中文語言包安裝"
[1-3-3]:{{site.url}}/img/2020-02-29-GitHub-jekyll-Blog/1-3-3.png "TortoiseGit中文語言包安裝完成"
[1-3-4]:{{site.url}}/img/2020-02-29-GitHub-jekyll-Blog/1-3-4.png "TortoiseGit可以選中文了"
[1-3-5]:{{site.url}}/img/2020-02-29-GitHub-jekyll-Blog/1-3-5.png "TortoiseGit首次啟動說明頁"
[1-3-6]:{{site.url}}/img/2020-02-29-GitHub-jekyll-Blog/1-3-6.png "TortoiseGit首次啟動git設定頁"
[1-3-7]:{{site.url}}/img/2020-02-29-GitHub-jekyll-Blog/1-3-7.png "git下載頁"
[1-3-8]:{{site.url}}/img/2020-02-29-GitHub-jekyll-Blog/1-3-8.png "git開始安裝"
[1-3-9]:{{site.url}}/img/2020-02-29-GitHub-jekyll-Blog/1-3-9.png "git安裝完成"
[1-3-10]:{{site.url}}/img/2020-02-29-GitHub-jekyll-Blog/1-3-10.png "TortoiseGit首次啟動git設定頁已設定git路徑"
[1-3-11]:{{site.url}}/img/2020-02-29-GitHub-jekyll-Blog/1-3-11.png "TortoiseGit首次啟動使用者資訊"
[1-3-12]:{{site.url}}/img/2020-02-29-GitHub-jekyll-Blog/1-3-12.png "TortoiseGit首次啟動認證頁"
[1-3-13]:{{site.url}}/img/2020-02-29-GitHub-jekyll-Blog/1-3-13.png "TortoiseGit首次啟動設定完成，右鍵已有TortoiseGit設定"

[2-1]:{{site.url}}/img/2020-02-29-GitHub-jekyll-Blog/2-1.png "點擊「New repository」"
[2-2]:{{site.url}}/img/2020-02-29-GitHub-jekyll-Blog/2-2.png "專案名稱輸入GitHub帳號.github.io"
[2-3]:{{site.url}}/img/2020-02-29-GitHub-jekyll-Blog/2-3.png "新增專案完成"

[3-1]:{{site.url}}/img/2020-02-29-GitHub-jekyll-Blog/3-1.png "RubyInstaller下載頁"
[3-2-1]:{{site.url}}/img/2020-02-29-GitHub-jekyll-Blog/3-2-1.png "開始安裝Ruby"
[3-2-2]:{{site.url}}/img/2020-02-29-GitHub-jekyll-Blog/3-2-2.png "安裝Ruby完成"
[3-3-1]:{{site.url}}/img/2020-02-29-GitHub-jekyll-Blog/3-3-1.png "MSYS2安裝頁"
[3-3-2]:{{site.url}}/img/2020-02-29-GitHub-jekyll-Blog/3-3-2.png "MSYS2安裝完成"

[4-1]:{{site.url}}/img/2020-02-29-GitHub-jekyll-Blog/4-1.png "輸入gem install jekyll安裝jekyll"
[4-2]:{{site.url}}/img/2020-02-29-GitHub-jekyll-Blog/4-2.png "輸入gem list查看套件列表"
[4-3]:{{site.url}}/img/2020-02-29-GitHub-jekyll-Blog/4-3.png "新增jekyll專案"
[4-4]:{{site.url}}/img/2020-02-29-GitHub-jekyll-Blog/4-4.png "初始jekyll專案內容"
[4-5]:{{site.url}}/img/2020-02-29-GitHub-jekyll-Blog/4-5.png "輸入jekyll serve啟動jekyll"
[4-6]:{{site.url}}/img/2020-02-29-GitHub-jekyll-Blog/4-6.png "啟動jekyll成功"
[4-7]:{{site.url}}/img/2020-02-29-GitHub-jekyll-Blog/4-7.png "啟動jekyll後的資料夾"
[4-8]:{{site.url}}/img/2020-02-29-GitHub-jekyll-Blog/4-8.png "posts為文章放置處"
[4-9]:{{site.url}}/img/2020-02-29-GitHub-jekyll-Blog/4-9.png "文章內容"
[4-10]:{{site.url}}/img/2020-02-29-GitHub-jekyll-Blog/4-10.png "找個順眼的jekyll主題"
[4-11]:{{site.url}}/img/2020-02-29-GitHub-jekyll-Blog/4-11.png "主題設置2"
[4-12]:{{site.url}}/img/2020-02-29-GitHub-jekyll-Blog/4-12.png "刪除Gemfile.lock"
[4-13]:{{site.url}}/img/2020-02-29-GitHub-jekyll-Blog/4-13.png "bundle安裝"
[4-14]:{{site.url}}/img/2020-02-29-GitHub-jekyll-Blog/4-14.png "bundle安裝完成"
[4-15]:{{site.url}}/img/2020-02-29-GitHub-jekyll-Blog/4-15.png "bundle錯誤"
[4-16]:{{site.url}}/img/2020-02-29-GitHub-jekyll-Blog/4-16.png "套件版本不符"
[4-17]:{{site.url}}/img/2020-02-29-GitHub-jekyll-Blog/4-17.png "安裝到兩個版本"
[4-18]:{{site.url}}/img/2020-02-29-GitHub-jekyll-Blog/4-18.png "移除套件版本"
[4-19]:{{site.url}}/img/2020-02-29-GitHub-jekyll-Blog/4-19.png "description缺乏錯誤"
[4-20]:{{site.url}}/img/2020-02-29-GitHub-jekyll-Blog/4-20.png "Conversion缺乏錯誤"
[4-21]:{{site.url}}/img/2020-02-29-GitHub-jekyll-Blog/4-21.png "終於成功啟動"
[4-22]:{{site.url}}/img/2020-02-29-GitHub-jekyll-Blog/4-22.png "安裝完主題後啟動畫面"

[5-1]:{{site.url}}/img/2020-02-29-GitHub-jekyll-Blog/5-1.png "點擊「Git在此建立版本庫」"
[5-2]:{{site.url}}/img/2020-02-29-GitHub-jekyll-Blog/5-2.png "Git初始化視窗，不須勾選點擊「確定」即可"
[5-3]:{{site.url}}/img/2020-02-29-GitHub-jekyll-Blog/5-3.png "建立成功本機檔案庫成功"
[5-4]:{{site.url}}/img/2020-02-29-GitHub-jekyll-Blog/5-4.png "多了一個「.git」的隱藏資料夾"
[5-5]:{{site.url}}/img/2020-02-29-GitHub-jekyll-Blog/5-5.png "在專案資料夾中點「右鍵」→「TortoiseGit」→「設定」"
[5-6]:{{site.url}}/img/2020-02-29-GitHub-jekyll-Blog/5-6.png "設定中的Git頁面"
[5-7]:{{site.url}}/img/2020-02-29-GitHub-jekyll-Blog/5-7.png "TortoiseGit提交視窗"
[5-8]:{{site.url}}/img/2020-02-29-GitHub-jekyll-Blog/5-8.png "未版本控制的檔案"
[5-9]:{{site.url}}/img/2020-02-29-GitHub-jekyll-Blog/5-9.png "檔案前有可勾選的小方框"
[5-10]:{{site.url}}/img/2020-02-29-GitHub-jekyll-Blog/5-10.png "輸入訊息並勾選提交檔案後，即可點擊「提交」"
[5-11]:{{site.url}}/img/2020-02-29-GitHub-jekyll-Blog/5-11.png "提交中畫面"
[5-12]:{{site.url}}/img/2020-02-29-GitHub-jekyll-Blog/5-12.png "提交成功畫面"
[5-12]:{{site.url}}/img/2020-02-29-GitHub-jekyll-Blog/5-12.png "提交成功後左下角有個「推送」按鈕"
[5-13]:{{site.url}}/img/2020-02-29-GitHub-jekyll-Blog/5-13.png "或在專案資料夾中點「右鍵」→「TortoiseGit」→「推送」"
[5-14]:{{site.url}}/img/2020-02-29-GitHub-jekyll-Blog/5-14.png "推送介面，第一次推送要設置遠端選項，點擊遠端右邊的管理按鈕"
[5-15]:{{site.url}}/img/2020-02-29-GitHub-jekyll-Blog/5-15.png "鏈結來自建立在GitHub上的專案"
[5-16]:{{site.url}}/img/2020-02-29-GitHub-jekyll-Blog/5-16.png "在URL處貼上鏈結"
[5-17]:{{site.url}}/img/2020-02-29-GitHub-jekyll-Blog/5-17.png "設置後會直接選到「origin」"
[5-18]:{{site.url}}/img/2020-02-29-GitHub-jekyll-Blog/5-18.png "跳出視窗後登入GitHub"
[5-19]:{{site.url}}/img/2020-02-29-GitHub-jekyll-Blog/5-19.png "推送訊息視窗"
[5-20]:{{site.url}}/img/2020-02-29-GitHub-jekyll-Blog/5-20.png "放上GitHub的網誌"
[5-21]:{{site.url}}/img/2020-02-29-GitHub-jekyll-Blog/5-21.png "GitHub上的專案看得到檔案和提交訊息"
