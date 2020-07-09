---
layout: post
title:  "需要的時候再叫上你－DataTables之deferRender（延遲渲染）屬性筆記"
date:   2020-07-09 18:00:00 +0800
categories:
- Program
tags: DataTables deferRender
description: DataTables之deferRender屬性筆記。
---

# DowYuu言

[DataTables](https://datatables.net/)在加載`ajax`或`javascript`數據時，預設情況下會一次取得所有的資料，並提前創建所需的所有`HTML`元素。

在加載巨量資料時，創建環節所累積的耗費時間就相當可觀了，因此有了`deferRender`屬性。

舉例來說就是你一次以`ajax`撈回來的資料有10000筆，在預設情況下撈回來時就建立了10000次所需的`HTML`元素，這渲染效能之差連`Chrome`都扛不住RRR

但若這選項開啟時，會在需要顯示資料時才會把它建立出來。像是若有設置分頁一頁顯示10筆，就會先建立這10筆需要顯示的資料列出來，換到下一頁時，才立即建立後10筆資料列。

但必須要注意啟用後，若有使用`nodes()`類API**可能會有無法使用的問題**。

# deferRender用法

用法很簡單，設置`deferRender`為`true`（預設為`false`），並設置`ajax`屬性即可。

```js
$(function(){
  $('#example').DataTable({
    "ajax": "scripts/getData.php"
    "deferRender": true,
  });
});
```

詳細可以看[官方deferRender說明](https://datatables.net/reference/option/deferRender)。

# 伺服器端延遲載入

[前面](#DowYuu言)有提到，預設情況下`ajax`會一次撈回所有的資料。

而之前寫過一篇[DataTables之ServerSide（伺服器處理）屬性筆記](https://dowyuu.github.io/program/2020/03/04/DataTables-ServerSide/)，是設置伺服器根據不同操作動作，再以`ajax`及時向伺服器撈回目前要顯示的資料，各位良民可以看自身需求去做選擇。

完畢灑花<3
