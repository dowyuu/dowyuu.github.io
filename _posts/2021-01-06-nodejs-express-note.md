---
layout: post
title:  "Node.js Express－又小又靈活的web應用框架"
date:   2021-01-06 18:00:00 +0800
categories:
- Program
tags: Node.js Express Express.js JavaScript
description: 關於Node.js Express的小小筆記。
---

# DowYuu言

我來掃一下這裡積了半年的灰塵了，咳咳。

最近空閒時在玩[Express](https://expressjs.com/)，寫個筆記紀錄紀錄。往後有遇到新狀況也會不定時來更新一下...吧（？）

# 安裝Express

```
npm install express -g
```

# 查看Express版本

```
npm express -v
```

# 安裝Express生成工具

```
npm install express-generator -g
```

# 初始專案

`Express`預設使用`Jade` 視圖引擎與純CSS，`Jade` 視圖引擎語法請參考[連結](https://naltatis.github.io/jade-syntax-docs/#attributes)。

```
express <project-name>

<!-- e.g. -->
express my-project-la

<!-- 後面可加參數（生成器參數見下方網址或express --help） -->
<!-- 像是個人css引擎想用less，就可用參數：-c less -->
express <project-name> -c less
```

如上面範例中所示，會在目前目錄下建立並產製一個叫`my-project-la`的專案資料夾，後面可帶參數，參數請見`express --help`或是查看[生成器參數](https://developer.mozilla.org/zh-TW/docs/Learn/Server-side/Express_Nodejs/skeleton_website)

初始專案後記得進入該專案資料夾中使用`npm install`語法安裝該專案所有使用的套件。

```
cd my-project-la
npm install
```

# 啟動

```
npm start
```

# 開發階段好幫手：nodemon－讓伺服器在檔案更改時自動重新啟動

在開發階段，前端檔案在檔案更動後還可以立刻看到效果，但後端檔案更動必須要重啟整個伺服器才會刷新。

每當一個小小的修正就要不斷的重啟是很煩人的，裝了`nodemon`之後它會自動偵測每當有檔案更改時會自動重啟伺服器，舒舒服服！！

```
npm install --save nodemon
```

安裝完`nodemon`後，開啟`package.json`並在`scripts`區塊加上語法：

```json
{
  ...
  "scripts": {
    "start": "node ./bin/www",
    "devstart": "nodemon ./bin/www"
  },
  ...
}
```

這邊示範取名`devstart`，於是啟動伺服器的語法改為`npm run devstart`：

```
<!-- 一般啟動語法（更動檔案必須手動重啟伺服器） -->
npm start

<!-- 使用nodemon啟動語法（更動檔案自動重啟伺服器） -->
npm run devstart
```

用了nodemon啟動語法會發現，每當有檔案更動，就會有自動重啟的訊息跑出來呦！舒舒服服！！

```
...（更動檔案前）
[nodemon] restarting due to changes...
[nodemon] starting `node ./bin/www`
...（更動檔案後自動重啟伺服器了，灑花！）
```

# 引入Router

初始化後的`Express`專案其實都有基本的範例，像是預設產出的`Express`專案就有引入`index`和`users`這兩個`Router`，這可以在`app.js`中看到：

```javascript
// in app.js

var indexRouter = require('./routes/index');
var usersRouter = require('./routes/users');
...
app.use('/', indexRouter);
app.use('/users', usersRouter);
```

這表示當URL連結到`/`（根目錄）時，會使用`indexRouter`，當URL連結到`/users`（e.g. 127.0.0.1/users）時，會使用`usersRouter`。

對應的Router檔案預設是放在專案資料夾中的`routes`資料夾，若要自己新增Router，就比照原先範例，在`app.js`中將該routes引入（`require`可省略副檔名`.js`）：

```javascript
// in app.js

var newRouter = require('./routes/newJs');
...
// 當url為「/new」時，就會進入「newRouter」（即檔案routes/newJs.js）。
app.use('/new', newRouter);
```

```javascript
// in routes/newJs.js

var express = require('express');
var router = express.Router();

// 「router.」可以在後方一直串下去
router.get('/', function(req, res, next) {
  // 當url為「網址/new/」時（e.g. http://127.0.0.1:3000//new/），做某件事
  ...
}).get('/test', function(req, res, next) {
  // 當url為「網址/new/test」時（e.g. http://127.0.0.1:3000//new/test），做某件事
  ...
});

module.exports = router;
```

# render（頁面渲染）

前面[初始專案](#初始專案)時有提到，`Express`預設使用`Jade` 視圖引擎，你可以在`views`資料夾中看到初始的範例`.jade`檔案，且在`app.js`中看到視圖引擎設置，預設還貼心的有註解說明：

```javascript
// in app.js

// view engine setup
app.set('views', path.join(__dirname, 'views'));
app.set('view engine', 'jade');
```

因為如此，在`Router`中使用`res.render(<filename>, <params>)`，就會去`views`資料夾中找到對應`<filename>`檔案（不需要加附檔名.jade）解析成`HTML`，顯示給你看：

```javascript
// in routes/index.js

router.get('/', function(req, res, next) {
  // 渲染「views/index.jade」並帶入參數「{ title: 'Express' }」
  res.render('index', { title: 'Express' });
});
```

至於為什麼會連同`layout`一同渲染，是因為預設`views/index.jade`中有寫到`extends layout`，後面也有提到[頁面渲染不使用layout](#render頁面渲染不使用layout)要怎麼做。

# render（頁面渲染）不使用layout

於該`.jade`頁面中將`extends layout`和`block content`拿掉即可。

要記得預設的`HTML`宣告是寫在`layout.jade`中的，如果不要用記得要加上`HTML`宣告。

```
//- 原本的views/index.jade
extends layout

block content
  h1= title
  p Welcome to #{title}

//- 若要拿掉layout，記得放上HTML宣告
doctype html
html
  head
    title= title
    link(rel='stylesheet', href='/stylesheets/style.css')
  body
    h1= title
    p Welcome to #{title}
```

# 頁面引入非存放在public中的檔案

預設`Express`會將專案中的`public`資料夾路徑引用入根目錄。

```javascript
// in app.js

// 就是這行做了這件事
app.use(express.static(path.join(__dirname, 'public')));
```

所以在`views/layout.jade`中，的可見其`<link>`可直接引用根目錄底下的`/stylesheets`資料夾，就會自動連結至`public`資料夾中，不須依照相對路徑在那邊`../public`，也增加隱密性。

若要引用的檔案並非存放在`public`資料夾中，也可使用`app.use`引入設置路徑名稱與指定資料夾，像是要引用裝在`node_modules`中的程式。

這邊用`sweetalert2`（這是個美化`alert`的好用套件）當範例，頁面要引用其`js`和`css`時即可使用`/swal`，程式會自動對應到`node_modules/sweetalert2/dist`資料夾：

```javascript
// in app.js

app.use('/swal', express.static(path.join(__dirname, 'node_modules/sweetalert2/dist')))
```

```
// in .jade

// - 這裡的「/swal」其實是對應到「node_modules/sweetalert2/dist」嘿嘿
link(rel="stylesheet", href="/swal/sweetalert2.min.css")
script(src='/swal/sweetalert2.min.js')
```

# Router取得傳入參數

| 取得參數 | 使用語法 | 範例 | 取得 |
| ------------ | ----------- |  ----- |  ----- |
| `form`中的輸入格 | req.body.<該元件的name> | `<input type="text" name="account" >` | req.body.account |
| `網址`中帶的參數 | req.params.<參數> | app.get('/log/:date', ...) | req.params.date |
| `$.ajax`中帶的data | req.query.<data中的key> | $.ajax中的data:{ date: '2021.01.01' } | req.query.date |

```javascript
// in Router

var express = require('express');
var router = express.Router();

router.post('/login', function(req, res) {
  // 取得form中name=account與name=password的元件value
  let account = req.body.account;
  let password = req.body.password;
  ...
}).get('/file/:name', function (req, res) {
  // 取得網址中/file/？？？中的？？？值（ ？？？被命名為參數name）
  let fileName = req.params.name; // e.g. 網址為「http://127.0.0.1/file/test.docx」就會取得fileName = test.docx
  ...
}).get('/setLog', function(req, res){
  // 取得前端ajax傳來的data中的key參數

  // 前端$.ajax
  // $.ajax({
  //   url: nowUrl + '/setLog',
  //   type: 'GET',
  //   cache: false,
  //   dataType: 'json',
  //   data: { 'log': '使用者dowyuu登入成功' },
  //   success: function(data) {
  //     console.log(data); // { 'success': true }
  //   },
  //   error: function(){
  //     alert('傳送log至服務端失敗。');
  //   }
  // })
  let log = req.query.log; // log = 使用者dowyuu登入成功
  ...
})
```

[Router參考](http://expressjs.com/zh-tw/api.html#router)

# 在javascript中帶入jade參數

```
// in .jade

script.
    alert('這頁是：#{page}')
```

[參考](https://stackoverflow.com/questions/5858218/how-can-i-render-inline-javascript-with-jade-pug)
