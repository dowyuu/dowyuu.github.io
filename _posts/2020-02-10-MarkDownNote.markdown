---
layout: post
title:  "MarkDown－優雅排版沒煩惱"
date:   2020-02-10 13:40:10 +0800
categories:
- Program
tags:
- Github
- Jekyll
description: 阿油的MarkDown語法筆記，讓你舒服排版的標記語言。
---

###### 最後修改日期｜Jul 07, 2020

# DowYuu言
因為Jekyll的關係而接觸了`Markdown`，因此想說稍微記錄一下`Markdown`的用法。  
用起來的感覺真的覺得寫出來的東西看起來很舒服XD  
文內有許多個人語氣、見解，如果有錯誤的話也歡迎提出討論。（雖然我還沒架好留言版功能哈哈哈哈）  
如果有新用到的語法會慢慢加上來，嘻嘻。

# MarkDown
一種基於純文字的輕量標記語言，目的是為了更易讀易寫，在Github中有良好的應用，副檔名可為`.markdown`或`.md`，看你高興囉。

**可完全兼容`html`寫法**，但在區塊元素中寫入`Markdown`語法將不會被進行處理。  
行內元素如`<span>`、`<cite>`、`<del>`則不受限制，可以任意使用`Markdown`語法。

示範：
<div>我在div元素中使用 **強調** 語法</div>
<span>我在span元素中使用 **強調** 語法</span>

總之在`.markdown`或`.md`檔中，看你高興要寫`Markdown`語法或是`html`寫法都可以。  
當然如果你要對元素進行很複雜的屬性設置，還是乖乖用`html`寫法比較實際啦 :v

# 區塊元素標籤與段落
**前後排必須皆為空行**，否則會失效。  
`Markdown`多數區塊都需要在兩個空行之間。

# &lt;br>｜換行
在行尾加上「兩個」以上的空白，然後斷行即可。

# <h1>～<h6>｜標題
`Markdown`支援兩種標題的語法：「Setext形式」和「atx形式」。

因為標題標籤會打亂頁面目錄所以就不示範了XD

## Setext寫法
是用在標題的下一排打上底線符號的形式，h1用`=`，h2用`-`，數量不拘想打幾個就幾個。因為區塊元素，前後排必須皆為空行，否則會失效。

### <h1>

輸入：
```ruby
This is an H1
=============
```

### <h2>

輸入：
```ruby
This is an H2
-------------
```

## Atx寫法
在標題前加上1到6個`#`，標題與`#`間須有一個空格，對應<h1>~<h6>，結尾`#`可加可不加，純粹是美觀用的，數量可與前方數量不同（階級是由前方數量決定）。

輸入：

    # This is an H1
    ## This is an H2 ##
    ### This is an H3 #
    #### This is an H4 ###
    ##### This is an H5 #
    ###### This is an H6
    ######This is an Wrong H6, no space between word and #

#  &lt;a>｜鏈結
分三種：「行內鏈結」（簡易）、「參照鏈結」（整齊）、「自動鏈結鏈結」（懶(X)）。

## 行內鏈結
格式為`[顯示的文字](鏈結網址 "title")`，title可省略，網址與title中間須有空格，鏈結網址也可以是相對路徑的本機檔案。

因為本站有裝套件所以鏈結都會另開新視窗，不然 **預設是不會另開新視窗的**。若要設置`target=_blank`，可在後方加上`{:target="_blank"}`或直接使用html寫法。

附上`jekyll`用之`<a>`元素設置`target=_blank`（另開新視窗）套件－[jekyll-target-blank](https://github.com/keithmifsud/jekyll-target-blank)，裝了一勞永逸，給你滿滿的新視窗。

輸入：
```ruby
[Google](https://www.google.com/)  
[DowYuu的照片]({{site.url}}/img/2020-02-10-MarkDownNote/DowYuu.png "這是DowYuu的照片")  
[GoogleBlank](https://www.google.com "This is Google"){:target="_blank"}
```

解析為：

[Google](https://www.google.com/)  
[DowYuu的照片]({{site.url}}/img/2020-02-10-MarkDownNote/DowYuu.png "這是DowYuu的照片")  
[GoogleBlank](https://www.google.com "This is Google")

## 參照鏈結
格式為`[顯示的文字][識別碼]`（中間可有空格），並在他處以`[識別碼]:網址 "title"`形式設定。

類似下圖這種**查表對照**的概念XD
<img src="{{site.url}}/img/2020-02-10-MarkDownNote/a.png" alt="參照鏈結" title="查表對照" style="border:1px #DDD solid;" />

- 識別碼**不分大小寫且可為空**（識別碼為空時，識別碼＝顯示的文字），識別碼可為多個文字。  
- 網址部分可用`<>`包覆，但`<`、`>`與網址間**不能有空隙**，否則新版`jekyll`會有URL解析錯誤：`bad URI(is not URI?)`。  
- title可用`""`、`''`包覆，且可摺到下一行去。

識別碼設置列須為獨立區塊，多個識別碼設置列可放一起。

輸入：
```ruby
[Google][google-web]  
[Google2][]  
[Google3][google-web3]  
[Google4][I am Google!]

[不要把識別碼設置列跟別的東西放在同列][doNotBeSameRow]
[doNotBeSameRow]:https://www.google.com/

[google-web]:https://www.google.com/ "This is Google"
[Google2]:<https://www.google.com/> 'This is Google2'
[google-web3]:<https://www.google.com/>
              "This is Google3"
[I am Google!]:https://www.google.com/ "This is Google"
```

解析為：  
[Google][google-web]  
[Google2][]  
[Google3][google-web3]  
[Google4][I am Google!]

[不要把識別碼設置列跟別的東西放在同列][doNotBeSameRow]
[doNotBeSameRow]:https://www.google.com/

[google-web]:https://www.google.com/ "This is Google"
[Google2]:<https://www.google.com/> 'This is Google2'
[google-web3]:<https://www.google.com/>
              "This is Google3"
[I am Google!]:https://www.google.com/ "This is Google"

## 自動鏈結
url網址或電子郵件以`<url網址|電子郵件>`形式可自動成為鏈結。

自動鏈結電子郵件會將字元轉換為16進位的HTML實體，這樣的格式可以混淆一些不好的信箱地址收集機器人。  
像是`<hello@gmail.com>`會被轉換為：
```html
<a href="&#x68;&#x65;&#x6C;&#x6C;&#x6F;&#x40;&#x67;&#x6D;&#x61;&#x69;&#x6C;&#x2E;&#x63;&#x6F;&#x6D;">&#x68;&#x65;&#x6C;&#x6C;&#x6F;&#x40;&#x67;&#x6D;&#x61;&#x69;&#x6C;&#x2E;&#x63;&#x6F;&#x6D;</a>
```

輸入：
```ruby
<http://www.google.com/>  
<hello@gmail.com>
```

解析為：  
<http://www.google.com/>  
<hello@gmail.com>

#  &lt;code>｜代碼
由左右各一個`` ` ``包覆而成。

如果代碼中想要有`` ` ``符號，可以使用左右各兩個`` ` ``。若是要代碼中只有`` ` ``符號，同樣使用左右各兩個`` ` ``並在中間用空格隔開。

`<code>`標籤內的代碼都會被轉為HTML實體（`<`轉為`&lt;`），顯示代碼更加方便。

輸入：
```ruby
`我的代碼`

``我的代碼中有`符號``

`` ` ``
```

解析為：

`我的代碼`

``我的代碼中有`符號``

`` ` ``

# &lt;pre>｜預格式化區塊
一般使用為與前段空行（因為區塊元素），並在*前方加上4個空格或一個tab*。

若要加上程式語言顏色標記（`jekyll`預設使用`Rouge`），則為由「&#123;% highlight <程式語言> [linenos] %}」與「&#123;% endhighlight %}」或「	&#96;&#96;&#96;<程式語言>」與「&#96;&#96;&#96;」包覆而成，「&#96;&#96;&#96;」的用法不支援[linenos]（行數顯示）。  

程式語言若此為ruby，則填入「&#123;% highlight ruby %}」，支援程式語言[在此](https://github.com/rouge-ruby/rouge/wiki/List-of-supported-languages-and-lexers)，線上示範[在此](http://rouge.jneen.net/v3.16.0/markdown/)。

若設置`linenos`參數則會顯示行數，**部分網誌主題會預設顯示行數**，像我自己使用的就有，所以你們看到我網誌內的`<pre>`是有行數的。

`<pre>`標籤內的代碼都會被轉為HTML實體（`<`轉為`&lt;`），顯示代碼更加方便。

輸入：
```ruby
    def print_hi(name)  
    puts "Hi, #{name}"  
    end  
    print_hi('Tom')  
    #=> 註解
```

&#123;% highlight ruby %}  
def print_hi(name)  
puts "Hi, #{name}"  
end  
print_hi('Tom')  
#=> 註解  
&#123;% endhighlight %}

    ```ruby
    def print_hi(name)  
    puts "Hi, #{name}"  
    end  
    print_hi('Tom')  
    #=> 註解  
    ```

解析為：

    def print_hi(name)  
    puts "Hi, #{name}"  
    end  
    print_hi('Tom')  
    #=> 註解  

{% highlight ruby %}
def print_hi(name)
  puts "Hi, #{name}"
end
print_hi('Tom')
#=> 註解
{% endhighlight %}

```ruby
def print_hi(name)  
puts "Hi, #{name}"  
end  
print_hi('Tom')  
#=> 註解  
```

# <blockquote>｜引言
在文字前方加上`>`，`>`與文字中間的空格可有可無，其中*可以使用`Markdown`語法*。  
同一區塊的文字可以只在首句前打上`>`，不需要每一句前面都加。  
引言中可再加入引言（可有階層），依據階層數在前方加上相對數量的`>`即可，不同階層的前後同樣要加上空行。  
因為區塊元素，前後排必須皆為空行，否則會失效。

輸入：
```ruby
> Blockquote, you **can use** Markdown inside.

>  No blank row between1
> No blank row between2

> Blockquote row1
Blockquote row2

> First step blockquote
>
> > Second step of blockquote, remember to add blank row to wrap it
>
> Back to first step
```

解析為：  
> Blockquote, you **can use** Markdown inside.

>  No blank row between1
> No blank row between2

> Blockquote row1
Blockquote row2

> First step blockquote
>
> > Second step of blockquote, remember to add blank row to wrap it
>
> Back to first step

# <ul>｜有序清單
在項目前加上`*`、`+`、`-`，符號與文字間須有空格或tab。  
可有階層，依據階層數在前方加上相對數量的「兩個空格」或「一個tab」即可，`<ul>`與`<ol>`可互相嵌套。

輸入：
```ruby
* DowYuu
1. 遊戲
1. 繪畫
+ Leon
  * 遊戲
    * Apex
- Oten
```

解析為：

* DowYuu
1. 遊戲
1. 繪畫
+ Leon
  * 遊戲
    * Apex
- Oten

# <ol>｜無序清單
在項目前加上`數字`+`.`，符號與文字間須有空格或tab。
若項目前一行為空行，則`<li>`內會被包上`<p>`元素，像是：`<li><p>被包了</p></li>`。  
數字並不代表順序，`Markdown`會依照「排列」順序幫你編上12345，所以懶的話可以全部都寫`1.`XD，不過建議第一項至少要從１開始。  
`<ul>`與`<ol>`可互相嵌套。
若不想要文字成為`<ol>`但剛好文字格式也是`數字`+`.`，可使用[跳脫字元](#跳脫字元)：`.`換成`\.`使其取消為列表。

輸入：
```ruby
1. DowYuu
+ 遊戲
+ 繪畫
9. Leon
3. Oten

5. 我上方有斷空行

88\. 我不當<ol>啦JOJO！
```

解析為：

1. DowYuu
+ 遊戲
+ 繪畫
9. Leon
3. Oten

5. 我上方有斷空行

88\. 我不當<ol>啦JOJO！

# &lt;hr>｜分隔線
在一行中用三個或以上的`*`、`-`、`_`，符號先可以有空格，同行中不能有其他文字。

輸入：
```ruby
***  
* * *  
- - - - - -  
_ _   _  
*** - 錯誤示範：插入了其他符號
```

解析為：

***  
* * *  
- - - - - -  
_ _   _  
*** - 錯誤示範：插入了其他符號

# &lt;em>與&lt;strong>｜強調
`<em>`格式為`*強調文字*`或`_強調文字_`。
`<strong>`格式為`**強調文字**`或`__強調文字__`。

若符號左右都有空白，則會被解析為一般符號，若想要直接是一般符號，可以使用`\*`或`\_`。

輸入：
```ruby
*em強調*

__strong強調__

我 * 左右 * 都有空白就會變回一般星號

\*我左邊的是一般星號
```

解析為：

*em強調*

__strong強調__

我 * 左右 * 都有空白就會變回一般星號

\*我左邊的是一般星號

# &lt;img>｜圖片
用法與`<a>`類似，只是在前方加上個`!`，分：「行內鏈結」（簡易）、「參照鏈結」（整齊）。
如果要對`<img>`做詳細設置（例如設置長寬、class），還是乖乖用HTML語法吧:v  
（是可以用擴充套件或CSS的選擇器搭配url設置做到啦，但是麻煩RRRRR）

## 行內圖片
格式為`![alt文字](圖片網址 "title")`，網址與title中間須有空格。  
細節去看[&lt;a>｜鏈結](#a鏈結)吧:v

輸入：
```ruby
![DowYuu]({{site.url}}/img/2020-02-10-MarkDownNote/DowYuu.png "這是DowYuu的照片！")
```

解析為：

![DowYuu]({{site.url}}/img/2020-02-10-MarkDownNote/DowYuu.png "這是DowYuu的照片！")

## 參照圖片
格式為`![alt文字][識別碼]`（中間可有空格），並在他處以`[識別碼]:網址 "title"`形式設定。  
細節去看[&lt;a>｜鏈結](#a鏈結)吧:v

輸入：
```ruby
![DowYuu][DowYuu Photo]

[DowYuu Photo]:{{site.url}}/img/2020-02-10-MarkDownNote/DowYuu.png "這是用參照的DowYuu的照片！"
```

解析為：

![DowYuu][DowYuu Photo]

[DowYuu Photo]:{{site.url}}/img/2020-02-10-MarkDownNote/DowYuu.png "這是用參照的DowYuu的照片！"

# &lt;del>｜刪除線
在文字的左右加上各2個`~`。

輸入：
```ruby
~~我要被刪除的文字~~
```

解析為：

~~我要被刪除的文字~~

# &lt;table>｜表格
以`-`切分`<thead>`與`<tbody>`，以用`|`切分`<td>`（非最左最右者，左右必須各有一個空格）。

為了美觀，整體表格最左邊最右邊可以用`|`畫線，中間表格處可以用空格填滿撐開。

將切分`<thead>`與`<tbody>`的`-`改為以下三種不同寫法，可有不同對齊方式：  
- `:---` 置左（預設）
- `:--:` 置中
- `---:` 置右

輸入：
```ruby
名稱 | 年齡
---- | ---
DowYuu | 18
Leon |  24

|    名稱    | 年齡 |
| ---------- | --- |
| DowYuu     |  18 |
| Leon       |  24 |

| 置左 | 置中 | 置右 |
| :--- | :----: | ----: |
| aaaa | bbbbbb | ccccc |
| a    | b      | c     |
```

解析為：

名稱 | 年齡
---- | ---
DowYuu | 18
Leon |  24

|    名稱    | 年齡 |
| ---------- | --- |
| DowYuu     |  18 |
| Leon       |  24 |

| 置左 | 置中 | 置右 |
| :--- | :----: | ----: |
| aaaa | bbbbbb | ccccc |
| a    | b      | c     |

# 跳脫字元
當你的文字裡面有用到`Markdown`中的保留字，但你單純只希望他顯示出來是個符號，就會用到跳脫字元`\`。

支援的符號字元有：
```ruby
\   反斜線
`   反引號
*   星號
_   底線
{}  大括號
[]  方括號
()  括號
#   井字號
+   加號
-   減號
.   英文句點
!   驚嘆號
```

輸入：
```ruby
**左右兩個星號會是粗體**

\*\*左右兩個星號會是粗體，但是我跳脫啦！\*\*
```

解析為：

**左右兩個星號會是粗體**

\*\*左右兩個星號會是粗體，但是我跳脫啦！\*\*
