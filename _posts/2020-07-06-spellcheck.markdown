---
layout: post
title:  "驅除紅色彎曲下底線！關閉拼字檢查「spellcheck」"
date:   2020-05-27 18:00:00 +0800
categories:
- Program
tags: HTML5 spellcheck
description: HTML5屬性，設定輸入格的拼字檢查！
---

###### 註｜對於不同瀏覽器，拼字檢查的預設值是會有所不同的，本篇的測試截圖皆由`Chrome`（版本83.0.4103.116）實作擷取。

<style>
  input{ display: block; margin-bottom: 10px; }
  textarea{ display: block; margin-bottom: 10px; }
  .block{
    padding: 8px 16px;
    background-color: #FFF;
  }
  .block + .block{
    margin-top: 10px;
  }
</style>

# DowYuu言

某個案子中，要讓使用者輸入設定一連串代號，但那些代號因為都是眾多單字簡寫拼湊而成，所以就像把代碼打在WORD上一樣，會出現拼字檢查錯誤而產生的紅色彎曲下底線...

HTML：
```html
<div>
  <input type="text" value="aa123">
  <textarea rows="3">aa123</textarea>
  <p contenteditable="true">aa123</p>
</div>
```

<div class="exampleShow">
  <div>
    <input type="text" value="aa123">
    <textarea rows="3">aa123</textarea>
    <p contenteditable="true">aa123</p>
  </div>
</div>

`Chrome`使用者可以看的到紅色彎曲下底線，其他預設未開啟拼字檢查的瀏覽器使用者可看以下截圖。

![預設開啟拼字檢查的瀏覽器所呈現的拼字檢查錯誤]({{site.url}}/img/2020-07-06-spellcheck/spellcheck_error.png "預設開啟拼字檢查的瀏覽器所呈現的拼字檢查錯誤")

在可編輯元素中進行編輯時就會看到這紅色彎曲下底線，實在有夠押ㄍㄚˊ。

後來查了一下，發現原來有這個拼字檢查屬性`spellcheck`可以設定RRR

# 瀏覽器支援 Browser compatibility

為HTML5的新標籤，支援表[在此](https://caniuse.com/#search=spellcheck){:target="_blank"}，IE10+，在寫文章的今天大多的瀏覽器都已經有支援咯。

# 使用

## 基本使用

```html
<element spellcheck="true|false">
```

用起來簡簡單單，就是對元素設置`spellcheck`屬性並給予其值為`true`或`false`。

本文一開始有提到，對於不同瀏覽器，拼字檢查的預設值是會有所不同的喔！

## 僅對可編輯元素生效

設置`spellcheck="true"`只會對**可編輯元素**進行拼字檢查（`input[type="password"]`除外）。

意思是即便瀏覽器有支援`spellcheck`屬性，元素也有設置`spellcheck="true"`，但只要該元素不是可編輯的元素，一樣不會有拼字檢查的效果。

```html
<div spellcheck="true">非編輯元素沒有拼字檢查：aaa123</div>
<div contenteditable="true" spellcheck="true">可編輯元素有拼字檢查：aaa123</div>
```

<div class="exampleShow">
  <div class="block" spellcheck="true">非編輯元素沒有拼字檢查：aaa123</div>
  <div class="block"  contenteditable="true" spellcheck="true">可編輯元素有拼字檢查：aaa123</div>
</div>

`Chrome`使用者可以看的到差別，其他預設未開啟拼字檢查的瀏覽器使用者可看以下截圖。

![可編輯元素與非編輯元素若開啟拼字檢查設置之表現差異]({{site.url}}/img/2020-07-06-spellcheck/contenteditable.png "可編輯元素與非編輯元素若開啟拼字檢查設置之表現差異")

## 大量設置？

你想說，如果要設置頁面中100個輸入格，阿不就要針對100個輸入格去設置這個屬性嗎？

答案當然是不用，有個更方便的用法。

`spellcheck`屬性可以對任何元素使用（雖然對於可編輯元素才會有效執行拼字檢查），其中有個特點：當祖先元素有設置`spellcheck`屬性，則所有的子孫元素預設都會繼承祖先元素設置之屬性。

HTML：
```html
<div spellcheck="false">
  <input type="text" value="aa123">
  <textarea rows="3">aa123</textarea>
  <div contenteditable="true">aa123</div>
</div>
```

<div class="exampleShow">
  <div spellcheck="false">
    <input type="text" value="aa123">
    <textarea rows="3">aa123</textarea>
    <div contenteditable="true">aa123</div>
  </div>
</div>

`Chrome`使用者可以看出來沒有紅色下底線了，其他預設未開啟拼字檢查的瀏覽器使用者可看以下截圖。

![祖先元素設置關閉拼字檢查後，子孫元素也會預設繼承其設置而關閉拼字檢查]({{site.url}}/img/2020-07-06-spellcheck/spellcheck_false.png "祖先元素設置關閉拼字檢查後，子孫元素也會預設繼承其設置而關閉拼字檢查")

簡單說，你要頁面裡所有輸入元素都關閉拼字檢查，那在`<body>`設置`spellcheck="true"`就行了，一勞永逸。

HTML：
```html
<body spellcheck="false">
  <!-- 裡面所有可編輯的元素都被關閉拼字檢查了，灑花。 -->
</body>
```

驅逐成功，紅線退散！！

---

本筆記內容參考：

* [MDN web docs－spellcheck](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Global_attributes/spellcheck){:target="_blank"}
* [Ｗ3school－HTML spellcheck Attribute](https://www.w3schools.com/tags/att_global_spellcheck.asp){:target="_blank"}
