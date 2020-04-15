---
layout: post
title:  "jQuery.html()包含當前節點"
date:   2020-04-15 18:00:00 +0800
categories:
- Program
tags: jQuery
description: 讓jQuery.html()也包含自己本身節點吧！
---

# DowYuu言

其實自己一直沒注意到這個狀況，原來`jQuery.html()`是不會包含本身節點的嗎XD

# jQuery.html()

這邊是一個普通的元素：

```html
<div id="currentDiv">
  <div class="subDiv">我是子元素</div>
</div>
```

用`jQuery.html()`取得其中元素：

```js
  $('#currentDiv').html();
  //得到：
  // "<div class="subDiv">我是子元素</div>"
```

# 包含當前節點

請安心服用`jQuery.prop("outerHTML")`。

```js
  $('#currentDiv').prop("outerHTML");
  //得到：
  // <div id="currentDiv">
  // <div class="subDiv">我是子元素</div>
  // </div>
```

唯一要小注意的是`.prop()`是jQuery1.6以上才能使用，不過都2020年了這應該不是什麼大問題(?)
