---
layout: post
title:  "瀏覽器排版引擎模式ｘIE文件模式ｘDOCTYPE－三者交織而成的小樂章"
date:   2020-05-20 18:00:00 +0800
categories:
- Program
tags: HTML compatMode X-UA-Compatible DOCTYPE
description: 本篇為對「瀏覽器排版引擎模式」、「IE文件模式（ X-UA-Compatible）」、「DOCTYPE」的簡單小介紹與筆記！
---

# DowYuu言

很久沒出沒了，最近都在龍之谷和阿福快打，超級怠惰XD

本篇的緣由是因為某天遇上了關於「IE文件模式」的問題，後來延伸出「瀏覽器排版引擎模式」和「DOCTYPE」的 **微微關聯**，既然要筆記就一起寫一寫，就這麼簡單XD

# 瀏覽器的排版引擎模式

在W3C創立網路標準前，網頁通常有兩種版本：網景（Netscape）的 Navigator 以及微軟（Microsoft）的 Internet Explorer，因為直接啟用新標準會導致舊有網頁跑版，瀏覽器因而導入了能分辨符合新規範、或屬於舊有網站的兩種模式。

除非你在維護遠古網頁不然完全沒必要使用怪異模式，但是若在IE下設定調整文件模式為5（關於文件模式可參考[下方](#ie文件模式internet-explorer-document-mode)）或網頁的DOCTYPE格式錯誤可能會導致進入怪異模式／接近標準模式（關於DOCTYPE可參考[下方](#doctype)），要多注意！

## 怪異模式（Quirks mode）

就是前面所指的舊有網頁，在怪異模式下，排版會模擬Navigator 4與Internet Explorer 5的「非標準」行為。  
若網頁的DOCTYPE格式錯誤也會進入怪異模式／接近標準模式。

怪異模式行為請參考[此列表](https://developer.mozilla.org/en-US/docs/Mozilla/Mozilla_quirks_mode_behavior){:target="_blank"}。

## 標準模式（Standards mode）

HTML與CSS表現與W3C規範相同。

## 接近標準模式（Almost standards mode）

顧名思義就是接近標準模式了，但還是有少部分行為同怪異模式。
接近標準模式行為參考[此列表](https://developer.mozilla.org/zh-TW/docs/Mozilla/Gecko_Almost_Standards_Mode){:target="_blank"}。

## 以JS檢測瀏覽器排版引擎模式（compatMode）

```js
document.compatMode
```

回傳會有`BackCompat`或`CSS1Compat`。  
`BackCompat`表示該頁面為怪異模式。  
`CSS1Compat`表示該頁面不是怪異模式，因此可能為標準模式或接近標準模式。
 
# IE文件模式（Internet Explorer Document Mode）

從IE8以上就有開發者人員工具了，而IE可以透過開發者人員工具中點選不同文件模式去做轉換，或是也可以透過頁面中<meta>設置X-UA-Compatible去設定。

## 開發者人員工具調整

這應該不用多說什麼，就是在IE中打開開發者人員工具（F12），通常在右上角會有可以調整的選項。

![IE11中的開發者人員工具，調整文件模式部分在右上角]({{site.url}}/img/2020-05-20-compatMode-X-UA-Compatible-DOCTYPE/IE11_F12.png "IE11中的開發者人員工具，調整文件模式部分在右上角")

預設值就是目前所運行的文件模式版本，要設置預設值可以用下方[X-UA-Compatible設置IE文件模式](#x-ua-compatible設置ie文件模式)的方法去設置。

## X-UA-Compatible設置IE文件模式

```html
<meta http-equiv="X-UA-Compatible" content="ie=edge,chrome=1">
```

在上方的範例中，在content中設置不同屬性，就可以以不同IE文件模式去運行此頁面。
以下為一些使用情境：

### 以可行之最高版本執行（推薦）：

```html
<meta http-equiv="X-UA-Compatible" content="ie=edge">
// 或
<meta http-equiv="X-UA-Compatible" content="ie=edge,chrome=1">
```

先說「ie=edge」，這是指以目前可行之最高版本的意思，你使用IE9運行就會用IE9的模式下去運作，使用IE11運行就會用IE11的模式下去運作，不用出了新版本IE就一直加一直換。

而下方一行為什麼有「chrome=1」這東西呢？
因為在IE6-IE9上為了使用者有更快、更安全的使用體驗且令其支援HTML5，發展出了 **外掛程式** [Google Chrome Frame](https://zh.wikipedia.org/wiki/Google_Chrome_Frame){:target="_blank"}（Google Chrome內嵌框架，簡稱GCF），讓可以用著IE的皮，Chrome的Webkit、V8引擎內在。  
若使用者有安裝GCF，且使用的IE8（含）以下，該代碼會自動引導瀏覽器啟用外掛程式進行排版及運算。

不過畢竟經過時代的進行，IE10也開始支援HTML5，該項目就在2014年停止維護了。

### 指定某一版本（此處範例為指定IE7）：

```html
<meta http-equiv="X-UA-Compatible" content="IE=7">
```

### 模擬為某一版本（此處範例為模擬為IE8）：

```html
<meta http-equiv="X-UA-Compatible" content="IE=EmulateIE8">
```

IE9（含）以前的瀏覽器怪異模式為IE5.5支持的功能，IE10、IE11的怪異模式則是符合HTML5中規範的怪異模式差異（IE5模式）， 會有部分差異。  
使用模擬版本時，該模擬版本的怪異模式會以IE5模式進行。

`content`屬性中若設置了指定IE版本，還是可以透過開發者人員工具去更動文件模式，但是每次重新載入該頁面時，預設顯示的文件模式會是你content中所指定的那個版本。

## JS檢測IE文件模式

```js
document.documentMode
```

以何種文件模式執行則會回傳該版本（文件模式IE8則會回傳8以此類推）。  
**僅IE且版本在8（含）以上支援**，非IE或IE7（含）以下該屬性值為undefined。
 
# DOCTYPE

文件類型，**必須放在網頁頁面的最頂端（第一行）**，若DOCTYPE前有任何語句（包含註解），在IE9（含）以下的IE都會觸發[怪異模式](#怪異模式quirks-mode)。

而DOCTYPE若書寫格式錯誤也會使網頁進入怪異模式或接近標準模式，各種DOCTYPE寫法觸發之瀏覽器排版引擎模式對照表請看[此表](https://zh.wikipedia.org/wiki/%E6%80%AA%E5%BC%82%E6%A8%A1%E5%BC%8F#%E6%96%87%E6%A1%A3%E7%B1%BB%E5%9E%8B%E7%9A%84%E6%AF%94%E8%BE%83){:target="_blank"}。

## HTML5

```html
<!DOCTYPE html>
```

在HTML5下僅此一種書寫格式，HTML5因不基於SGML因此也不需要在後面引用DTD，舒舒服服。  
即便是不支援HTML5的IE6，用此種文件類型也會讓文件進入標準模式。

## HTML4

基於SGML，因此DOCTYPE後必須引用DTD，引用不同的DTD會代表支援顯示的元件不同。

### HTML 4.01 Strict

```html
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01//EN" "http://www.w3.org/TR/html4/strict.dtd">
```

不包含展示性、棄用的元素，不允許框架（Framesets）。

### HTML 4.01 Transitional

```html
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
```

包含展示性、棄用的元素，不允許框架（Framesets）。

### HTML 4.01 Frameset

```html
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Frameset//EN" "http://www.w3.org/TR/html4/frameset.dtd">
```

包含展示性、棄用的元素，允許框架（Framesets）。

---

本筆記內容參考：

* [MDN web docs－怪異模式與標準模式](https://developer.mozilla.org/zh-TW/docs/Web/HTML/Quirks_Mode_and_Standards_Mode){:target="_blank"}
* [Ｗ3school－HTML <!DOCTYPE>標籤](https://www.w3school.com.cn/tags/tag_doctype.asp){:target="_blank"}
* [wiki－Google Chrome內嵌框架](https://zh.wikipedia.org/wiki/Google_Chrome_Frame){:target="_blank"}
* [stackoverflow 問題](https://stackoverflow.com/questions/6771258/what-does-meta-http-equiv-x-ua-compatible-content-ie-edge-do){:target="_blank"}
* [wiki－怪異模式](https://zh.wikipedia.org/wiki/%E6%80%AA%E5%BC%82%E6%A8%A1%E5%BC%8F){:target="_blank"}
