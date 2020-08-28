---
layout: post
title:  "網頁取得安裝程式－getInstalledRelatedApps"
date:   2020-08-28 18:00:00 +0800
categories:
- Program
tags: JavaScript PWA ProgressiveWebApp getInstalledRelatedApps
description: 安裝程式一把抓，讓你的網頁取到與網頁設定相關聯的Android App、Progressive Web App(PWA)、Windows App(UWP)吧！
---

# DowYuu言

在PWA中，可以透過監聽`beforeinstallprompt`，將安裝至主畫面的功能事件存放到指定按鈕中，等待使用者自行點擊後觸發。

這時我們會希望成功將PWA安裝至桌面後，那顆「加入主畫面」按鈕就會消失，這可以透過監聽`appinstalled`來取得安裝PWA過程完成，若完成則隱藏「加入主畫面」按鈕。

這步驟看似正常，但當使用者重新以 **瀏覽器** 進入該頁面時，會發現雖然你已經安裝PWA了，但是那顆「加入主畫面」按鈕還是會出現。

雖然點擊後可跳出訊息提示使用者已安裝，但使用者多半還是會疑惑...為什麼安裝完了還會顯示這顆按鈕呢？

思考一下，啊嗯？網頁能抓的到已經安裝的應用程式嗎？

前面有提到一個監聽事件叫`appinstalled`，但它只會監聽到安裝PWA「過程完成」而非該 PWA已經安裝...幫被名稱誤導的我QQ

後來一查，啊嗯？getInstalledRelatedApps？雖然是個新鮮貨但還真的做得到啊？！

# getInstalledRelatedApps

顧名思義就是可以取得 **相關聯** 的已安裝程式，而支援種類列表如下：

| 種類 | 支援瀏覽器與版本 |
| ------ | ----------------------- |
| Android App | Android Chrome 80（含）以上 |
| Progressive Web App(PWA) | Android  Chrome 84（含）以上 |
| Windows App(UWP) | Windows Chrome 85（含）以上；Edge 85（含）以上 |

對Android用戶而言應該支援度算不錯，Windows用戶目前可以用金絲雀Chrome玩看看，蘋果用戶目前88888

官方說明上特別強調只能取到 **你自己** 的程式，也就是**程式與網頁雙方面都有設置關聯設定**，才能成功取得安裝判斷。你是沒辦法取到所有該使用者所有安裝的程式或第三方程式的，不然隱私性上來說也太可怕了XD

# 用法

```js
const relatedApps = await navigator.getInstalledRelatedApps();
relatedApps.forEach((app) => {
  console.log(app.id, app.platform, app.url);
});
```

注意！`getInstalledRelatedApps()`只作用在`HTTPS`上！

不要傻傻如我在那邊用127.0.0.1去試，會試不出東西Q_Q

# 各版本取得是否安裝方式

官方文件中寫的滿清楚的，這邊只是列出來。不過下方有我自己用取得PWA安裝遇到的小問題。

## Android App

支援：Android Chrome 80（含）以上

[官方說明](https://web.dev/get-installed-related-apps/#check-android)在此，[DEMO](https://get-installed-apps.glitch.me/)在此。

## Progressive Web App(PWA)，不論scope是否相同

支援：Android  Chrome 84（含）以上

[官方說明](https://web.dev/get-installed-related-apps/#check-pwa-in-scope)在此，[DEMO](https://gira-same-domain.glitch.me/pwa/)在此。

不要說電腦版Chrome裝了PWA怎麼讀不到，人家只支援「**Android**  」Chrome 84（含）以上。

## Windows App(UWP)

支援：Windows Chrome 85（含）以上、Edge 85（含）以上

[官方說明](https://web.dev/get-installed-related-apps/#check-windows)在此，無DEMO。

# 取得PWA是否安裝遇到的小問題

前面也有說到這次用`getInstalledRelatedApps`是為了PWA，原本是用了我自認為是（？）`in scope`的用法，在`manifest.json`中加入`related_applications`設定，但就是一直抓不到已安裝。

後來成功是在根目錄中放了一個只寫了`related_applications`設定的`manifest.json`，並把我原本PWA用的`manifest.json`放到別的目錄下（同樣也有加`related_applications`設定），然後就抓的到了，超級不解XD

請願有能為小女子我解惑的大大出現，感恩感恩。

---

本筆記內容參考：

* [Is your app installed? getInstalledRelatedApps() will tell you!](https://web.dev/get-installed-related-apps/)
