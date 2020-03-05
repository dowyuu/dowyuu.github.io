---
layout: post
title:  "不要一次進來那麼大的－DataTables之ServerSide屬性筆記"
date:   2020-03-04 18:00:00 +0800
categories:
- Program
tags: DataTables ServerSide
description: DataTables之ServerSide屬性筆記。
---

# DowYuu言

[DataTables][https://datatables.net/]在載入資料時有很多不同方法，像是直接以HTML的`<table>`啟動、以`ajax`載入...等，而這些方法皆是一次撈回所有的資料，對於巨量資料而言不是個好做法，因此才有了`ServerSide`屬性。

# ServerSide用法

用法很簡單，設置`ServerSide`為`true`（預設為`false`），並設置`ajax`屬性即可，因會去及時撈資料，通常會搭配`"processing": true`（顯示等待中）增加使用者體驗。

以下為預設示範：

```js
$(document).ready(function() {
  $('#example').DataTable( {
    "processing": true,
    "serverSide": true,
    "ajax": "scripts/server_processing.php"
  } );
} );
```

設置後會使得各種操作像是「切換頁」、「選擇每頁資料數」、「排序」、「查詢」...等等，都會再一次的去後端撈資料回來顯示，對巨量資料而言是較佳的做法。

更多屬性設置可以看[官方ServerSide範例][https://datatables.net/examples/server_side/]。

完畢灑花<3
