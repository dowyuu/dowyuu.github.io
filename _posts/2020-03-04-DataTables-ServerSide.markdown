---
layout: post
title:  "不要一次進來那麼大的－DataTables之ServerSide（伺服器處理）屬性筆記"
date:   2020-03-04 18:00:00 +0800
categories:
- Program
tags: DataTables ServerSide
description: DataTables之ServerSide屬性筆記。
---

###### 最後修改日期｜Jul 07, 2020

# DowYuu言

[DataTables](https://datatables.net/)在加載`ajax`數據時，會是一次撈回所有的資料，對於巨量資料而言效能上並不優，因此有了`ServerSide`屬性。

# ServerSide用法

用法很簡單，設置`ServerSide`為`true`（預設為`false`），並設置`ajax`屬性即可，因會去及時撈資料，通常會搭配`"processing": true`（顯示等待中）增加使用者體驗。

```js
$(function(){
  $('#example').DataTable({
    "processing": true,
    "serverSide": true,
    "ajax": "scripts/getData.php"
  });
});
```

設置後會使得各種操作像是「切換頁」、「選擇每頁資料數」、「排序」、「查詢」...等等，都會再一次以`ajax`去後端撈資料回來顯示，對巨量資料而言是較佳的做法。

更多屬性設置可以看[官方ServerSide說明](https://datatables.net/examples/server_side/)。

# 前端延遲渲染

另外提供另一種延遲選擇－`deferRender`，此種方式是讓`ajax`一次撈回所有資料，並且在要顯示時才將`HTML`元素建立出來，同樣可以提升巨量資料使用`Datatables`上的效能。

詳細可以參考[DataTables之deferRender（延遲渲染）屬性筆記](https://dowyuu.github.io/program/2020/07/07/DataTables-deferRender/)。

完畢灑花<3
