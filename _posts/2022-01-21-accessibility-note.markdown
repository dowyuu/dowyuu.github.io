---
layout: post
title:  "無障礙網頁課程筆記－頁面操作一家親"
date:   2022-01-21 18:00:00 +0800
categories:
- Program
tags: HTML HTML5 semantic-elements Accessibility freego
description: 去年上了無障礙網頁課程的小小記錄。
---

# DowYuu言

使用滑鼠移動至按鈕上進行點擊動作，對一般使用者來說是十分平常的事情。但對於身心障礙者而言，他們或許無法控制手指操控滑鼠並精準的在按鈕上進行點擊，取而代之的是使用各類電腦輔具，像是螢幕報讀軟體...等。

那要怎麼使我們的頁面能夠被螢幕報讀軟體精準解讀頁面各區塊、各元件，就得靠開發者進行無障礙網頁優化，進而改善此類使用者的操作體驗。

# 用鍵盤操作也可以操作網頁？

平常用慣了用滑鼠點點滾滾滑網頁，有沒有試過用鍵盤進行相同操作呢？

在鍵盤操作中：

* `tab` 可將焦點切換至下一個互動元件
* `shift` + `tab` 可將焦點切換至上一個互動元件
* `enter` 可觸發超連結
* `enter`或`space`可觸發像是「送出表單」此種控制項
* `page up` 可向上滾動滾輪位置
* `page down` 可向下滾動滾輪位置
* `home` 可跳至滾輪位置最頂端
* `end` 可跳至滾輪位置最底端

先在你的頁面中進行這些操作，就可以初步知道你的頁面對於身心障礙者的不友善程度(?)了。

# 「互動元件」和「焦點」

前面有說到「互動元件」，如其名就是可以與使用者互動的元件，像是可以點擊連至超連結的`<a>`、可以輸入的`<input>`、可以點選選項的`<select>`，當你在與之互動時，就是將焦點放在該互動元件上。

根據[WHATWG（網頁超文本應用技術工作小組）](https://whatwg.org/)定義，互動元件有：
* `<a>`鏈結
* `<button>`按鈕
* `<select>`下拉選單、`<option>`選項
* `<input>`輸入
* `<details>`詳細內容、`<summary>`概要

在預設情況下，取得焦點的互動元件，會顯示焦點環（Focus Ring）。

## 焦點環（Focus Ring）

互動元件在取得焦點時，預設下會顯示焦點環（Focus Ring），就如同螢幕上的滑鼠，能表明目前的鍵盤聚焦於何者互動元件上，而各瀏覽器表現的焦點環樣式不同。

| Chrome Focus表現 | 火狐Focus表現 |
| :--: | :--: |
| ![Chrome Focus表現]({{site.url}}/img/2022-01-21-accessibility-note/2-focus_on_chrome.jpg "Chrome Focus表現") | ![火狐Focus表現]({{site.url}}/img/2022-01-21-accessibility-note/2-focus_on_FF.jpg "火狐Focus表現") |

若覺得預設的焦點環樣式不好看，個人是推薦用`box-shadow`代替，但**請絕對不要消滅焦點環的存在**。

```css
    input:focus{
        outline: none;
        box-shadow: 0 0 0 4px rgba(39, 13, 252, 0.3);
    }
```

![用box-shadow代替預設焦點環樣式]({{site.url}}/img/2022-01-21-accessibility-note/3-focus_ring_style.jpg "用box-shadow代替預設焦點環樣式")

## 為什麼用tab沒辦法到達那個有綁定點擊事件的元件？

在用tab走訪頁面時，會發現在`<div>`上綁定的點擊事件，滑鼠點了也準確執行，但當使用鍵盤就會發現沒辦法走訪到該元件上，就導致沒辦法觸發點擊，原因是因為該標籤不是預設的「互動元件」，預設沒辦法被鍵盤走訪。

當然可以透過給予`tabindex`數值來讓該標籤可以被鍵盤走訪，但是很有可能因此破壞的tab走訪順序等等，因此還是建議有互動事件的元件，就乖乖的用對應的標籤就好...好按鈕不用嗎？

```html
<input type="text" id="account" name="account" value="" placeholder="帳號">
<input type="password" id="password"　name="password" value="" placeholder="密碼">
<button type="button" name="login" id="login">登入</button>
<div id="alert" onclick="alert('目前看診到16號')">點我顯示目前看診號碼</div>
<button type="button" name="singup" id="singup">註冊</button>
```

![tab無法走訪<div>]({{site.url}}/img/2022-01-21-accessibility-note/4-tab-div.jpg "tab無法走訪&#60;div&#62;")

# 用「語意化標籤」來增強Accessibility Tree

## 語意ｘDOM TreeｘAccessibility Tree

`DOM Tree`全名為「Document Object Model（文件物件模型）」，是瀏覽器看得懂的樹狀結構語言，而瀏覽器會根據你的`DOM Tree`資訊去建構出`Accessibility Tree`，供搜尋引擎、輔助閱讀器...等其他軟體解析。

而當`DOM Tree`中的語意越強，越能建構出結構更優良更精確的`Accessibility Tree`，可以讓搜尋引擎或是其他軟體工具，更清楚的了解網頁中各個區塊的設計目的，**增強網頁的SEO**。

| 無語意的程式範例 | 有語意的程式範例 |
| :--: | :--: |
| ![DOM Tree]({{site.url}}/img/2022-01-21-accessibility-note/0-none_semantic_code.jpg "無語意的程式範例") | ![Accessibility Tree]({{site.url}}/img/2022-01-21-accessibility-note/1-semantic_code.jpg "有語意的程式範例") |

## 語意化標籤

前面示範了有無語意差別的程式碼，相較之下語意優良的程式明顯地更容易得知該區塊作用為何（無論是機器或是開發者XD）。

在以前HTML4的年代，頁面區塊化分多用`<div>`，會變成無限`<div>`包`<div>`的情況，而在HTML5中改善了這點，讓開發者可以根據需求將該區塊設置為不同標籤。

用不用語意化標籤並不影響排版，樣式也都可透過CSS去調整，但如前面所說，對於搜尋引擎或是其他軟體工具而言可以更清楚的了解網頁中各個區塊的設計目的，進而提供給使用者更好的查詢結果、語音報讀等，因此非常建議開發時優先使用先對應的語意化標籤的。

### 網頁版面劃分

<div class="htmlDiv">
  <div class="headerDiv">
    <div class="tagName">&#60;header></div>
  </div>
  <div class="row">
    <div class="asideDiv">
      <div class="tagName">&#60;aside></div>
    </div>
    <div class="mainDiv">
      <div class="tagName">&#60;main></div>
      <div class="articleDiv">
        <div class="tagName">&#60;article></div>
        <div class="sectionDiv">
          <div class="tagName">&#60;section></div>
        </div>
        <div class="sectionDiv">
          <div class="tagName">&#60;section></div>
        </div>
      </div>
      <div class="sectionDiv">
        <div class="tagName">&#60;section></div>
        <div class="articleDiv">
          <div class="tagName">&#60;article></div>
        </div>
        <div class="articleDiv">
          <div class="tagName">&#60;article></div>
        </div>
      </div>
    </div>
  </div>
  <div class="footerDiv">
    <div class="tagName">&#60;footer></div>
  </div>
</div>

<style>
  .htmlDiv{
    border: 1px #DDD solid;
    padding: 30px;
  }
  .htmlDiv div + div{
    margin-top: 15px;
  }
  .row{
    display: flex;
  }
  .row > div + div{
    margin-top: 0;
    margin-left: 15px;
  }
  .headerDiv,
  .asideDiv,
  .mainDiv,
  .footerDiv,
  .articleDiv,
  .sectionDiv{
    border: 1px #DDD solid;
    padding: 15px;
  }
  .mainDiv{
    flex-grow: 1;
    flex-shrink: 1;
  }
  .asideDiv{
    flex-basis: 140px;
    flex-grow: 0;
    flex-shrink: 0;
  }
</style>

| 標籤名稱 | 名稱 |  |
| `<header>` | 頁首區塊 | [說明](#header標籤)  |
| `<nav>` | 導覽區塊 | [說明](#nav標籤) |
| `<aside>` | 側邊欄位區塊 | [說明](#aside標籤) |
| `<main>` | 主要內容區塊 | [說明](#main標籤) |
| `<article>` | 文章區塊 | [說明](#article標籤) |
| `<section>` | 段落區塊 | [說明](#section標籤) |
| `<h1>`～`<h6>` | 標題 | [說明](#h1h6標籤) |
| `<hgroup>` | 標題群組 | [說明](#hgroup標籤) |
| `<footer>` | 頁尾區塊 | [說明](#footer標籤) |

### header標籤

頁首區塊，通常會放置Logo、`<nav>`（導覽列）...等。

`<article>`、` <section>`、`<aside>`、`<nav>`都可以有自己的`<header>`

```html
<header>
  <div id="logo">
    <img src="logo.png" alt="LOGO圖片">
    <span>貘貘資訊網</span>
  </div>
  <nav>
    <ul>
      <li><a href="page0.html">馬來貘</a></li>
      <li><a href="page1.html">山貘</a></li>
      <li><a href="page2.html">中美貘</a></li>
      <li><a href="page3.html">南美貘</a></li>
      <li><a href="page4.html">卡波馬尼貘</a></li>
    </ul>
  </nav>
</header>
```

### nav標籤

導覽區塊，網站內所要用於導覽的區塊都可以使用，內部通常搭配`<ul>`（無序列表）使用。

```html
<nav>
  <ul>
    <li><a href="page0.html">馬來貘</a></li>
    <li><a href="page1.html">山貘</a></li>
    <li><a href="page2.html">中美貘</a></li>
    <li><a href="page3.html">南美貘</a></li>
    <li><a href="page4.html">卡波馬尼貘</a></li>
  </ul>
</nav>
```

### aside標籤

側邊欄位區塊，不過雖然叫側邊但是不一定要放在側邊。

通常會放置與主要內容「相關」的資訊，像是廣告、相關文章等。

可以被嵌套在其他標籤中，但不要`<aside>`裡面再嵌一個`<aside>`，相關了又再相關，沒意義。

```html
<aside>
  <p>友站連結</p>
  <ul>
    <li><a href="web0.html">食蟻獸資訊網</a></li>
    <li><a href="web1.html">穿山甲資訊網</a></li>
  </ul>
</aside>
```

### main標籤

主要內容區塊，頁面中僅能有一個`<main>`，且獨立於其他標籤（不能放在其他標籤中）。

### article標籤

文章區塊，獨立文章用，把該篇文章以外的所有HTML元素拔掉依舊是一篇可讀的文章，通常需包含標題（`<h1>`～`<h6>`）。

可和`<section>`相互嵌套，你可以一篇文章中有好多段落，也可以一個段落中有很多篇文章。當你的**內容完整**時，就建議使用`<article>`而非`<section>`。

內部可以嵌入`<headers>`、`<asides>`、`<footer>`等標籤。

```html
<main>
  <article>
    <headers>
      <h1>豈有此理！新島民不請自來，使島主困擾不已！</h1>
    </headers>
    <p>日前DowYuu因不知道一天只能邀請一名新島民，而一天建了兩個空地。邀請到一名新島民後，就再也遇不到其他島民了！</p>
    <p>第二天竟發現該空地已被標註成交，再隔日該島民就自行入住到島上了，令DowYuu不禁怨嘆，天理何在！</p>
    <footer>
      <p>記者阿中在無人島獨家報導。</p>
    </footer>
  </article>
</main>
```

### section標籤

段落區塊，有相同主題的區塊內容，通常需包含標題（`<h1>`～`<h6>`）。

如果僅僅是為了排版或樣式用要分段，請用`<div>`！

可和`<article>`相互嵌套，你可以一篇文章中有好多段落，也可以一個段落中有很多篇文章。當你的**內容完整**時，就建議使用`<article>`而非`<section>`。

內部可以嵌入`<headers>`、`<asides>`、`<footer>`等標籤。

### h1～h6標籤

標題，依據分級`<h1>`為最高級別，`<h6>`為最低級別。通常`<article>`和`<section>`會需要內含一個標題標籤。

### hgroup標籤

標題群組，可將主標題與副標題進行組合。

```html
<hgroup>
  <h1>初級日文I</h1>
  <h2>基本招呼語</h2>
</hgroup>
```

### footer標籤

頁尾區塊，通常會放置簡要資訊、版權法律聲明、`<address>`...等。

`<article>`、` <section>`、`<aside>`、`<nav>`都可以有自己的`<footer>`。

### 其他語意化標籤

這邊另外介紹部分語意化標籤。

#### time標籤

日期時間標籤，如其名就是放日期或時間用的，若標籤內文字無法正確地表示合法日期，就可以`datetime `屬性設置。

日期或時間請以[合法格式](https://html.spec.whatwg.org/multipage/text-level-semantics.html#the-time-element)書寫，不要寫出不明確的時間，日期請用西元年。

```html
<p>我今天準時<time>18:00</time>下班。</p>
<p>我的新島民－小雅在<time datetime="2021-01-06">今天<time>入住我的無人島了！</p>
```

#### mark標籤

`<mark>`為標記元件，由於標記的段落在封閉上下文中的相關性或重要性，HTML元素表示為參考或註釋目的而標記或突出顯示的文本。

```html
<p>查詢「馬來貘」之結果：</p>
<hr>
<p><mark>馬來貘</mark>（學名：Tapirus indicus）又名亞洲貘、印度貘，是貘屬下的一個種，屬於奇蹄目貘科。<mark>馬來貘</mark>為現存5種貘中體型最大的物種，也是唯一生活於亞洲的現存物種。</p>
<p><mark>馬來貘</mark>黑白相間，全身中後段為白色體毛，其餘為黑色；耳朵豎立，耳緣末端為白色。剛出生的<mark>馬來貘</mark>寶寶深褐色的身驅，布滿白色條紋及花斑，有益於融入雨林地表層的光影裡 ...</p>
```
<div class="example-show just-show">
<p>查詢「馬來貘」之結果：</p>
<hr>
<p><mark>馬來貘</mark>（學名：Tapirus indicus）又名亞洲貘、印度貘，是貘屬下的一個種，屬於奇蹄目貘科。<mark>馬來貘</mark>為現存5種貘中體型最大的物種，也是唯一生活於亞洲的現存物種。</p>
<p><mark>馬來貘</mark>黑白相間，全身中後段為白色體毛，其餘為黑色；耳朵豎立，耳緣末端為白色。剛出生的<mark>馬來貘</mark>寶寶深褐色的身驅，布滿白色條紋及花斑，有益於融入雨林地表層的光影裡 ...</p>
</div>

#### figure標籤和figcaption標籤

`<figure>`為可附標題之內容獨立元件，`<figcaption>`就是該元件之標題元件。

```html
<figure>
    <img src="Map_EMAP.png" alt="臺灣通用電子地圖">
    <figcaption>臺灣通用電子地圖</figcaption>
</figure>
```

<div class="example-show just-show">
<figure>
<img src="{{site.url}}/img/2021-03-22-Leaflet-note/Map_EMAP.png" alt="臺灣通用電子地圖">
<figcaption>臺灣通用電子地圖</figcaption>
</figure>
</div>

#### details標籤和summary標籤

`<details>`為可顯示／隱藏關於文件的詳細資訊的區塊，預設為關閉狀態，需要點擊標題文字才可開啟顯示詳細資訊。可設置`open`屬性使其預設為開啟狀態。

`<summary>`為設置`<details>`之顯示標題，應該要是`<details>`中的第一個子元素，沒有設置`<summary>`的話`<details>`的標題文字會是預設文字。

```html
<details>
  <summary>點我顯示關於馬來貘的詳細訊息</summary>
  <p>馬來貘（學名：Tapirus indicus）又名亞洲貘、印度貘，是貘屬下的一個種，屬於奇蹄目貘科。馬來貘為現存 5 種貘中體型最大的物種，也是唯一生活於亞洲的現存物種。－段落取自維基百科馬來貘頁面</p>
</details>
<hr>
<details open>
  <p>我是沒有設置標題文字（summary），且預設就開啟看得到的詳細資訊！！</p>
</details>
```

<div class="example-show">
  <details>
    <summary>點我顯示關於馬來貘的詳細訊息</summary>
    <p>馬來貘（學名：Tapirus indicus）又名亞洲貘、印度貘，是貘屬下的一個種，屬於奇蹄目貘科。馬來貘為現存 5 種貘中體型最大的物種，也是唯一生活於亞洲的現存物種。－段落取自維基百科馬來貘頁面</p>
  </details>
  <hr>
  <details open>
    <p>我是沒有設置標題文字（summary），且預設就開啟看得到的詳細資訊！！</p>
  </details>
</div>

### 常見標籤之語意列表

此處列出並非所有皆為HTML5的語義化標籤，反正無論是否為被定義為語意化標籤，**若是符合該標籤特性，就用上那個標籤**，告別無限`<div>`輪迴！！

| 標籤名稱 | 語意 | 說明或備註 | 語意化標籤 |
| :--- | :--- | :--- | :--: |
| `<header>` | 頁首區塊 | [說明](#header標籤)  | ✓ |
| `<nav>` | 導覽區塊 | [說明](#nav標籤) | ✓ |
| `<aside>` | 側邊欄位區塊 | [說明](#aside標籤) | ✓ |
| `<main>` | 主要內容區塊 | [說明](#main標籤) | ✓ |
| `<article>` | 文章區塊 | [說明](#article標籤) | ✓ |
| `<section>` | 段落區塊 | [說明](#section標籤) | ✓ |
| `<h1>`～`<h6>` | 標題 | [說明](#h1h6標籤) | ✓ |
| `<hgroup>` | 標題群組 | [說明](#hgroup標籤) | ✓ |
| `<footer>` | 頁尾區塊 | [說明](#footer標籤) | ✓ |
| `<time>` | 日期時間 | [說明](#time標籤) | ✓ |
| `<mark>` | 標記 | [說明](#mark標籤) | ✓ |
| `<figure>` | 可附標題之內容獨立元件 | [說明](#figure標籤和figcaption標籤) | ✓ |
| `<figcaption>` | 可附標題之內容獨立元件之標題 | [說明](#figure標籤和figcaption標籤) | ✓ |
| `<details>` | 詳細資訊 | [說明](#details標籤和summary標籤) | ✓ |
| `<summary>` | 詳細資訊之標題 | [說明](#details標籤和summary標籤) | ✓ |
| `<address>` | 聯絡資訊 | 除了地址還可以放Email、電話等所有「能聯絡用的資訊」，通常會放在`<footer>`中 |
| `<p>` | 段落 |  |
| `<hr>` | 水平線 |  |
| `<pre>` | 保留格式的文字區塊 |  |
| `<blockquote>` | 引用 |  |
| `<a>` | 鏈結 |  |
| `<small>` | 註釋和附属細則 | 原本只是讓文字字體變小，在HTML5中被重新定義為表示註釋和附属細則，包括版權和法律文本...等 |
| `<del>` | 被刪除文字內容 | 表示一些被從文檔中刪除的文字內容。比如可以在需要顯示修改記錄或者源代碼差異的情況使用這個標籤。 |
| `<ins>` | 被插入文字內容 | 已經被插入文檔中的文本。 |
| `<cite>` | 引用參考內容 |  |
| `<q>` | 短引用內容 | 內容太常請改用`<blockquote>` |
| `<blockquote>` | 塊級引用內容 |  |
| `<dfn>` | 定義術語 |  |
| `<abbr>` | 縮寫或首字母縮略詞 | 可設置`title`屬性為縮寫提供擴展或描述內容 |
| `<ruby>` | 旁註（發音）標記 | 旁註標記用於標示東亞文字的發音 |
| `<code>` | 程式碼 |  |
| `<var>` | 變數 |  |
| `<samp>` | 程式輸入文字 |  |
| `<kbd>` | 鍵盤文字 |  |
| `<sub>` | 下標文字 |  |
| `<sup>` | 上標文字 |  |
| `<strong>` | 重要內容 | |
| `<em>` | 強調 | |
| `<ul>` | 無序列表 |  |
| `<ol>` | 有序列表 |  |
| `<li>` | 列表項目 |  |
| `<dl>` | 術語定義描述列表 |  |
| `<dt>` | 術語定義描述項目 |  |
| `<dd>` | 術語定義描述內容 |  |
| `<table>` | 表格 |  |
| `<thead>` | 表格頁首 |  |
| `<tbody>` | 表格主體 |  |
| `<tfoot>` | 表格頁尾 |  |
| `<caption>` | 標題 |  |
| `<tr>` | 行 |  |
| `<th>` | 表頭 |  |
| `<td>` | 單元格 |  |
| `<form>` | 表單 |  |
| `<input>` | 輸入格 |  |
| `<textarea>` | 多行輸入框 |  |
| `<label>` | 元件說明 |  |
| `<fieldset>` | 表單元件群組 |  |
| `<legend>` | 表單元件群組標題 |  |
| `<select>` | 下拉選單 |  |
| `<option>` | 下拉選單選項 |  |
| `<button>` | 按鈕 |  |
| `<map>` | 影像地圖 |  |
| `<area>` | `<map>`中的標註區塊，可以有超連結 |  |
| `<audio>` | 音檔 |  |
| `<img>` | 圖片 |  |
| `<track>` | 字幕 |  |
| `<video>` | 影片 |  |
| `<embed>` | 嵌入外部內容 |  |
| `<iframe>` | 內聯框架 |  |
| `<picture>` | 多來源圖片 |  |
| `<source>` | 媒體資源 |  |
| `<canvas>` | 繪布 |  |
| `<progress>` | 進度條 |  |

### 常見的可改善標籤寫法

以下是之前看過不優的標籤寫法，在此提出。

#### 表格之一路到底的`<td>`

```html
<table>
    <tr>
        <td class="title">員工</td>
        <td class="title">飲料</td>
        <td class="title">甜度</td>
        <td class="title">冰度</td>
    </tr>
    <tr>
        <td class="title">郭阿妍</td>
        <td>珍珠鮮奶茶</td>
        <td>無糖</td>
        <td>去冰</td>
    </tr>
    <tr>
        <td class="title">王俊俊</td>
        <td>新鮮水果茶</td>
        <td>3分糖</td>
        <td>微冰</td>
    </tr>
</table>
```

優化寫法：
```html
<table>
    <thead>
        <tr>
            <th>員工</th>
            <th>飲料</th>
            <th>甜度</th>
            <th>冰度</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <th>郭阿妍</th>
            <td>珍珠鮮奶茶</td>
            <td>無糖</td>
            <td>去冰</td>
        </tr>
        <tr>
            <th>王俊俊</th>
            <td>新鮮水果茶</td>
            <td>3分糖</td>
            <td>微冰</td>
        </tr>
    </tbody>
</table>
```

#### 用符號代替的勾選框

```html
<div>
    <i class="glyphicon is-check">☑</i>
    <span>我要參加</span>
</div>
```

優化寫法：
```html
<div>
    <input type="checkbox" id="agree">
    <label for="agree">我要參加</label>
</div>
```

# 華麗變身－WAI-ARIA

無障礙網絡倡議的無障礙豐富互聯網應用規範（WAI-ARIA，簡稱 ARIA）適用於跨越某些領域的障礙，這些領域存在的無障礙問題無法通過原生 HTML 進行管理。  
它通過允許您指定某些屬性來發揮作用，這些屬性可以修改元素轉換成無障礙樹的方式。

簡單說，你有難言之隱(?)實在沒辦法用符合語意的標籤建構時，就可以透過`WAI-ARIA`設定將該元件變成其他元件，像是我可以把`<li>`變成`<input type="checkbox">`，當然這是在沒有辦法的情況下才這樣做，不然好好的原生元件你幹嘛不用啦？？？？？？

`WAI-ARIA`可以：
* 將原本並無語義的元件（如：`<div>`、`<span>`加上語意）
* 修改或是強化現有語義，例如一個實現switch功能的`<button>`，為其加上role設定
* 額外的標籤語敘述
* 表達元素與元素間的關係

`WAI-ARIA`無法：
* 沒有使元件取得焦點的能力（Focusability）
* 沒辦法監聽鍵盤事件處理器（Event Handler）

若要該元件可取得焦點，必須賦予該元件`tabindex=0`，就會將該元件照文件結構排入取得焦點的順序。

`WAI-ARIA`重點三要素：
* role（角色）
* states（狀態）
* properties（屬性）

## 簡單示範

```html
<!-- 原生的進度條 -- >
<progress max="100" value="75"></progress>

<!-- 客製化的進度條 -- >
<div role="progressbar" aria-valuenow="75" aria-valuemin="0" aria-valuemax="100" />
```

在客製化的進度條中，可以看到透過`role`設定，這個`<div>`會判讀為「進度條」，並且透過各個`aria-*`屬性得知目前進度條的狀態。

各設定請自行查找。

# Freego檢測軟體

下載：[單機版檢測工具 Freego（適用「網站無障礙規範(110.07)」](https://accessibility.ncc.gov.tw/Download/Detail/1749?Category=70)

Freego是以JAVA 64位元所開發的網路爬蟲應用程式運行於64位元之JRE環境，用於掃描整個網站。舊版的則是運行於32位元JRE（已不支援舊版），不要下載錯。

![Freego檢測軟體介面（截自無障礙課程之講義）]({{site.url}}/img/2022-01-21-accessibility-note/5-freego.jpg "Freego檢測軟體介面（截自無障礙課程之講義）")

## 簡易操作方法

1. 於網址框（圖中３號區塊）中輸入該網站網址
1. 若該網站需要運行Javascript才可正常運作，請先點選操作選單（圖中２號區塊）中的「設定」＞「Javascript」＞「啟用」
1. 若該網站需要帳號密碼登入，請先點選帳號密碼登錄資訊（圖中６號區塊），跳出頁面後進行登入動作並關閉
1. 點選要檢測的等級（圖中５號區塊）
1. 按下工具選單中的開始按鈕（圖中７號區塊的左上角按鈕）![Freego工具選單中的開始按鈕（截自無障礙課程之講義）]({{site.url}}/img/2022-01-21-accessibility-note/6-freego_go.jpg "Freego工具選單中的開始按鈕（截自無障礙課程之講義）")
1. 檢測結果即可顯示於下方

## 檢測結果判斷

檢測結果會條列出整個網站中所有內容頁面之檢測結果為YES或NO，當該頁面檢測結果為NO時，雙擊該列即可跳出「修正工具視窗」。

![修正工具視窗（截自無障礙課程之講義）]({{site.url}}/img/2022-01-21-accessibility-note/7-freego_result.jpg "修正工具視窗（截自無障礙課程之講義）")

在修正工具視窗中，會很明確的在檢測碼區塊（圖中３號區塊）中看到各檢測等級下未通過的檢測碼，點擊後會在右方說明區塊（圖中４號區塊）顯示檢測規則說明文字，不合格之檢測碼與其原因會顯示於網頁原始碼區塊（圖中５號區塊）。

# 網頁定位點－看似意義不明的三個冒號:::

![網頁定位點（導盲磚）]({{site.url}}/img/2022-01-21-accessibility-note/8-anchor.jpg "網頁定位點（導盲磚）")

在瀏覽許多公家機關網站時，常常會看到在各區塊起始處會有三個冒號`:::`的出現，用滑鼠點擊看起來也沒什麼反應...其實這是讓身心障礙者很方便定位至特定區塊位置的「定位點」喔。

定位點，原名為導盲磚，其顯示方式是利用三個冒號`:::`來代表，依據不同瀏覽器所定之按鍵搭配上設定的快捷鍵，就可以將焦點快速定位至該元件上，建議一個頁面不要超過4個。

通常在被設置`accesskey`屬性的元件上，會設置`title`屬性，並註明該元件為定位至哪個區塊，並且簡述該區塊作用或內容。

```html
<a href="#accesskeyU" id="accesskeyU" accesskey="U" title="上方選單連結區，此區塊列有本網站的主要連結">:::</a>
<a href="#accesskeyC" id="accesskeyC" accesskey="C" title="中間主要內容區，此區塊呈現網頁的網頁內容">:::</a>
<a href="#accesskeyZ" id="accesskeyZ" accesskey="Z" title="下方版權宣告區，此區塊列有本處之服務時間、交通資訊，及[網站安全政策]、[隱私權政策]等連結">:::</a>
```

像是上方第一項設置了`accesskey="U"`，若是使用`Chrome`進行網頁瀏覽，只要按下鍵盤上的`Alt`+`U`，焦點就會移至該元素上。

![搭配accesskey屬性之不同瀏覽器所定按鍵（截自MDN web Docs）]({{site.url}}/img/2022-01-21-accessibility-note/9-accesskey.jpg "搭配accesskey屬性之不同瀏覽器所定按鍵（截自MDN web Docs）")

不過`accesskey`屬性其實是個不太被推薦使用的屬性，因為會有與瀏覽器其他按鍵操作相衝的問題、除非有先說明否則使用者不知道按什麼按鍵會定位...等等的問題，但臺灣目前還是將此列入無障礙設計中，好與不好就見仁見智了。
