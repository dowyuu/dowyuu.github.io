---
layout: post
title:  "Autosize－輕量套件讓Textarea自己的長大吧！"
date:   2020-03-23 18:00:00 +0800
categories:
- Program
tags: Textarea Autosize 套件
description: 讓textarea根據設定與輸入內容自動調整高度！
---

# DowYuu言

[Autosize](http://www.jacklmoore.com/autosize/)是個相當輕量小巧的套件，用於讓textarea根據設定與輸入內容自動調整高度，使用簡單上手迅速呦。

此套件作者為[Jack Moore](https://github.com/jackmoore)，在此由衷感謝Jack Moore大大的分享奉獻。

# 下載

點擊頁面中最受人矚目的那顆星－藍色的「Download」按鈕即可。

![點擊藍色的「Download」按鈕下載]({{site.url}}/img/2020-03-23-Textarea-Autosize/Download-Autosize.png "點擊藍色的「Download」按鈕下載")

或使用`npm`下載。
```
npm install autosize
```

# 瀏覽器支援 Browser compatibility

| Chrome | Firefox | IE | Safari | iOS Safari | Android | Opera Mini |
| yes           | yes        | 9+ | yes      | yes              |  4              | ?         |

# 使用 Usage

可以用`Javascript`的節點、節點集合或以`jQuery`物件宣告，簡簡單單。

```js
// from a NodeList
autosize(document.querySelectorAll('textarea'));

// from a single Node
autosize(document.querySelector('textarea'));

// from a jQuery collection
autosize($('textarea'));
```

# 方法 Method

## 狀態更新 update

在一般輸入狀況下`Autosize`可以偵測出`textarea`內容值的變化進而去改變高度。

但當你是使用`script`去更動內容值或隱藏/顯示該物件，就會失效。這時候要用`autosize.update(elements)`去更新該物件的狀態。

## 解除綁定 destroy

讓`textarea`膝蓋中一箭－`autosize.destroy(elements)`。

更多用法資訊可以去看看[Autosize官網](http://www.jacklmoore.com/autosize/)喔。
