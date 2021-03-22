---
layout: post
title:  "Leaflet－輕量且易懂易用的互動地圖"
date:   2021-03-22 10:00:00 +0800
categories:
- Program
tags: Leaflet JavaScript jQuery
description: 關於Leaflet的小小筆記。
---

###### 最後修改日期｜Mar 22, 2021
###### 撰文時版本　｜1.7.1

# DowYuu言

最近滿常用到[Leaflet](https://leafletjs.com/)，順手來記錄一下。

![Leaflet]({{site.url}}/img/2021-03-22-Leaflet-note/leaflet.png "Leaflet")

# 地圖初始化

[官方文件 Map](https://leafletjs.com/reference-1.7.1.html#map)

```js
const openstreet = L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
    attribution: '&copy; <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a> contributors'
});
let map = L.map('map', {
  layers: [openstreet],
  center: [23.604799, 120.7976256], // 台灣中心點
  zoom: 8
});
```

## 移除地圖

移除後即可重新進行初始化動作。

```js
map.remove();
```

# 經緯位置資訊 LatLng

地圖中所有的經緯度位置都可以任意切換成如下的格式：

  - [lat, lon]
  - { lat: lat, lon: lon }
  - { lon: lon, lat: lat }
  - L.latLng(lat, lon)

PS. `Latitude` 是緯度，`Longitude` 是經度。

# 設置視圖

同時設置經緯位置與縮放等級。

```js
map.setView([23.604799, 120.7976256], 8);
```

## 只設置經緯位置資訊

只要設置經緯位置，延伸自`setView`。

```js
map.panTo([23.604799, 120.7976256]);
```

## 只設置縮放等級（Zoom）

只設置縮放等級，延伸自`setView`。

```js
map.setZoom(8);
```

# 取得地圖資訊狀態

[官方文件 Methods for Getting Map State](https://leafletjs.com/reference-1.7.1.html#map-methods-for-getting-map-state)

## 取得地圖中心點座標

```js
map.getCenter(); // 回傳中心點座標
```

## 取得地圖縮放級別

```js
map.getZoom(); // 回傳數字
```

## 取得地圖邊界點

會回傳東北點及西南點的座標。

```js
map.getBounds(); // 回傳東北點及西南點的座標
```

# 地圖事件

## 點擊地圖取得該點資訊

```js
map.on('click', function(e){
  var coord = e.latlng;
  var lat = coord.lat;
  var lng = coord.lng;
  console.log('latitude（緯度）', lat);
  console.log('longitude（經度）', lng);
});
```

## 開始地圖拖動

```js
map.on('movestart', function (){
  console.log('你開始地圖拖動了');
});
```

## 結束地圖拖動

```js
map.on('moveend', function (){
  console.log('你結束地圖拖動了');
});
```

# 圖層群組

[官方文件 LayerGroup](https://leafletjs.com/reference-1.7.1.html#layergroup)

## 將圖層群組加至地圖中

```js
let markersLayer = new L.layerGroup();
markersLayer.addTo(map);
```

## 移除圖層群組

```js
map.removeLayer(markersLayer);
```

## 清除圖層群組

會將此圖層內所有圖層（例如你加到此群組中的`Marker`）清除。

```js
markersLayer.clearLayers();
```

# 圖層控制器

[官方文件 Control.layers](https://leafletjs.com/reference-1.7.1.html#control-layers)

```js
let baseLayers = {
  '開放街圖': baseOpenStreet,
  '臺灣通用電子地圖': baseEMAP
};
let overlays = {
  '地標': markersLayer
};
let controler = L.control.layers(baseLayers, overlays).addTo(map);
```

注意原本`map`有設定`layers`還是不能拿掉，`layers`設定中的圖層會成為預設載入圖層。

```js
let map = L.map('map', {
  layers: [openstreet], // 不能拿掉，這個會是預設載入圖層
  center: [23.604799, 120.7976256],
  zoom: 8
});
```

底圖圖層`baseLayers`會被設置為`radio`供使用者點選，疊加圖層`overlays`會被設置為`checkbox`供使用者點選。

該底圖圖層在不支援的縮放等級時，會被設置`disabled`。

像下方範例，目前在縮放等級20，中間三個底圖圖層是支援到20的，其餘上下兩個底圖圖層不支援，`radio`就會被設置`disabled`。

![該底圖圖層在不支援的縮放等級時，會被設置disabled]({{site.url}}/img/2021-03-22-Leaflet-note/demo_Control.layers.png "該底圖圖層在不支援的縮放等級時，會被設置disabled")

## 顯示圖層名稱

顯示的圖層名稱可以包含`HTML`。

```js
let baseLayers = {
  '<i class="bi bi-map-fill"></i><span>開放街圖</span>': baseOpenStreet
};
let overlays = {
  '<i class="bi bi-show"></i><span>餐廳</span>': markersLayer
};
```

## 新增底圖圖層baseLayer

要注意參數是先`layer`再圖層名稱。

```js
controler.addBaseLayer(basePHOTO2, '正射影像圖');
```

## 新增疊加圖層overlay

要注意參數是先`layer`再圖層名稱。

```js
controler.addOverlay(busStopLayer, '公車站點');
```

# 地圖比例尺

[官方文件 Control.Scale](https://leafletjs.com/reference-1.7.1.html#control-scale)

```js
L.control.scale().addTo(map);
```

預設公制單位(m/km)和英制單位(mi/ft)都會顯示，可在`option`中設置。

```js
L.control.scale({
  metric: true,   // 控制公制單位(m/km)顯示
  imperial: true  // 控制英制單位(mi/ft)顯示
}).addTo(map);
```

# 瀏覽器支援、版本判斷

[官方文件 Browser](https://leafletjs.com/reference-1.7.1.html#browser)

`Leaflet`也有提供瀏覽器支援、版本判斷方法，有是否支援某元素（如是否支援`svg`），或是瀏覽器版本判斷（是否為`ie`），相當方便。

使用上就是用`L.Browser.<Properties>`，回傳皆為`boolean`值，`<Properties>`列表與回傳值代表含意請詳見[官方文件 Browser.Properties](https://leafletjs.com/reference-1.7.1.html#browser-property)。

```js
if(L.Browser.ie){
  alert('兄Day，換個先進點的瀏覽器好不？');
}
```

# Marker標誌

[官方文件 Marker](https://leafletjs.com/reference-1.7.1.html#marker)

```js
let m = new L.Marker([23.604799, 120.7976256], {markerName: '我是臺灣的中心啦'}).on('click', function(e){
  console.log(e.target.options); // 可以看到options設置的東西
  console.log(e.target); // e.target是此L.Marker
});
markersLayer.addLayer(m);
```
這邊範例是把`Marker`加到圖層群組`markersLayer`中，當然你要直接加到地圖裡也可以。

```js
map.addLayer(m);
```

# Icon

[官方文件 Icon](https://leafletjs.com/reference-1.7.1.html#icon)

預設的`Marker`會是一個藍色的icon，這邊也可以用圖片自訂。

```js
var greenIcon = L.icon({
    iconUrl: 'leaf-green.png',
    shadowUrl: 'leaf-shadow.png',

    iconSize:     [38, 95], // icon 寬, 長
    shadowSize:   [50, 64], // 陰影 寬, 長
    iconAnchor:   [22, 94], // icon 中心偏移
    shadowAnchor: [4, 62],  // 陰影 中心偏移
    popupAnchor:  [-3, -76] // 綁定popup 中心偏移
});
let m = L.marker([23.604799, 120.7976256], {icon: greenIcon});
```

要用類似預設標點但是帶icon圖示的話，可以參考[Leaflet.awesome-markers](#leafletawesome-markers讓marker附帶icon且支援多色)。

# Popup彈出窗

[官方文件 Popup](https://leafletjs.com/reference-1.7.1.html#popup)

簡易設定：

```js
m.bindPopup('Hello');
```

進階設定：

```js
let p = new L.Popup({ className: 'station-popup'}) // 一些設定
                    .setContent('<div>HELLO</div>') // popup內容HTML
                    .setLatLng([23.604799, 120.7976256]);
m.bindPopup(p).openPopup(); // 將popup綁定至marker並開啟popup
```

## 綁定至Marker、開啟Popup、關閉Popup

`Popup`的開關是取決於綁定的`Marker`上。

```js
m.bindPopup(p); // 將popup綁定至marker

m.openPopup(); // marker開啟綁定的popup

m.closePopup(); // marker關閉綁定的popup
```

## 讓Popup的開啟時不會關掉其他Popup

預設點了一個`Popup`就會關掉原本開啟的`Popup`。

所以這邊可以設置`autoClose: false`和`closeOnClick: false`，這樣`Popup`的開關就不會影響到其他的`Popup`。

```js
let p = new L.Popup({ className: 'station-popup', autoClose: false, closeOnClick: false})
                    .setContent('<div>HELLO</div>')
                    .setLatLng([23.604799, 120.7976256]);
m.bindPopup(p).openPopup();
```

## 讓Popup維持顯示無法關閉

可以用CSS把`Popup`上的關閉按鈕隱藏，並且用`javascript`設置點擊`Marker`時會把`Popup`關閉，這樣關掉後觸發反向就會又自動開起來。

```css
.leaflet-popup-close-button{
  display: none;
}
```

```js
let m = new L.Marker([22, 120], {markerName: 'OO公車站'}).on('click', function(e){
  e.target.options.closePopup(); // 多加這行
});
```

# 免費地圖圖資

**※ 這只是個人整理，使用上勞煩自行查證細讀各圖資使用之版權、授權條款，有糾紛不負責RRR**

這邊放上[OpenStreetMap開放街圖](https://www.openstreetmap.org/)、[國土測繪中心 國土測繪圖資服務雲](https://maps.nlsc.gov.tw/)、以及[地理資訊科學研究專題中心](http://gis.rchss.sinica.edu.tw/)所提供的部分圖資，下方也有簡介與介接說明。

## OpenStreetMap開放街圖

![OpenStreetMap開放街圖 圖資]({{site.url}}/img/2021-03-22-Leaflet-note/Map_openstreetmap.png "OpenStreetMap開放街圖 圖資")

是自由而且開源的全球地圖。

要注意使用上要參照[OpenStreetMap版權與授權條款](https://www.openstreetmap.org/copyright)，必須標註圖資作者`© OpenStreetMap contributors`。

```js
// OpenStreetMap開放街圖
const baseOpenStreet = L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
    attribution: '&copy; <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a> contributors'
});
```

## 國土測繪中心圖資

國土測繪中心圖資介接說明是寫縮放層級到19，但實測都有到20。**要注意`Leaflet`預設最高縮放層級只到18**，所以使用上記得設定最大限制`maxNativeZoom: 20`和`maxZoom: 20`。

網站上寫了很多次，所以特別附註上來：**請注意不可以將圖資大量的用 cache 的方式儲存於伺服器上再提供服務，這將構成「著作權法」第3條的「重製」及「發行」，屬嚴重侵權的違法行為。**

這邊只放幾個自己常用的。

### 臺灣通用電子地圖

![臺灣通用電子地圖 圖資]({{site.url}}/img/2021-03-22-Leaflet-note/Map_EMAP.png "臺灣通用電子地圖 圖資")

優點是有臺灣的門牌號碼（於縮放層級19與20）。

```js
// 國土測繪中心 臺灣通用電子地圖
const baseEMAP = L.tileLayer('https://wmts.nlsc.gov.tw/wmts/EMAP/default/GoogleMapsCompatible/{z}/{y}/{x}', {
  maxNativeZoom: 20,
  maxZoom: 20
});
```

### 臺灣通用電子地圖（高DPI文字）

![臺灣通用電子地圖（高DPI文字） 圖資]({{site.url}}/img/2021-03-22-Leaflet-note/Map_EMAP96.png "臺灣通用電子地圖（高DPI文字） 圖資")

背景同通用電子地圖，沒有門牌且只有圖資臺灣地區，但是圖資上的文字變得比較清晰銳利也變大了，拿來**看街道名稱更易讀一點**。

![相同位置與Zoom情況下，臺灣通用電子地圖 與 臺灣通用電子地圖（高DPI文字） 比較]({{site.url}}/img/2021-03-22-Leaflet-note/Map_Compare_EMAP_EMAP96.png "相同位置與Zoom情況下，臺灣通用電子地圖 與 臺灣通用電子地圖（高DPI文字） 比較")

縮放等級拉到6以下地圖就會消失，所以使用上記得設定最小限制`minZoom: 6`。

```js
// 國土測繪中心 臺灣通用電子地圖（高DPI文字）
const baseEMAPHighDPI = L.tileLayer('https://wmts.nlsc.gov.tw/wmts/EMAP96/default/GoogleMapsCompatible/{z}/{y}/{x}', {
  maxNativeZoom: 20,
  maxZoom: 20,
  minZoom: 6
});
```

### 正射影像圖

![正射影像圖 圖資]({{site.url}}/img/2021-03-22-Leaflet-note/Map_PHOTO2.png "正射影像圖 圖資")

實拍照，縮放等級拉到7以下臺灣就會消失，所以使用上記得設定最小限制`minZoom: 7`。

```js
// 國土測繪中心 正射影像圖
const basePHOTO2 = L.tileLayer('https://wmts.nlsc.gov.tw/wmts/PHOTO2/default/GoogleMapsCompatible/{z}/{y}/{x}', {
  maxNativeZoom: 20,
  maxZoom: 20,
  minZoom: 7
});
```

### 圖資介接

[圖資介接說明](https://maps.nlsc.gov.tw/S09SOA/)，進網頁後點選「WMTS」並點選「列表」，進到「WMTS列表」畫面後點擊介接網址處的類別網址，會下載下`xml`說明檔案。

![進網頁後點選「WMTS」並點選「列表」]({{site.url}}/img/2021-03-22-Leaflet-note/API_nlsc_1.png "進網頁後點選「WMTS」並點選「列表」")

![進到「WMTS列表」畫面後點擊介接網址處的類別網址，會下載下xml說明檔案]({{site.url}}/img/2021-03-22-Leaflet-note/API_nlsc_2.png "進到「WMTS列表」畫面後點擊介接網址處的類別網址，會下載下xml說明檔案")

打開說明檔案，找到該圖層`ResourceURL`中的`template`網址，`{Style}`對應到該圖層的`Style`底下的`ows:Identifier`設定值，`{TileMatrixSet}`對應到該圖層的`TileMatrixSet`設定值，把`{TileMatrix}`換成`{z}`、`{TileRow}`換成`{y}`、`{TileCol}`換成`{x}`即可。

![國土測繪中心 圖資介接說明]({{site.url}}/img/2021-03-22-Leaflet-note/API_nlsc_3.png "國土測繪中心 圖資介接說明")

## 地理資訊科學研究專題中心圖資

說明文件中的`TileMatrixSet`部分也有提到，本單位的縮放等級最大到17，到17以上地圖就會消失，所以使用上記得設定最大限制`maxZoom: 17`。

這邊只放「福衛二號衛星影像」。

### 福衛二號衛星影像

![福衛二號衛星影像 圖資]({{site.url}}/img/2021-03-22-Leaflet-note/Map_F2.png "福衛二號衛星影像 圖資")

實拍照，影像範圍只有臺灣周圍，不過每個縮放層級的周圍範圍不太一樣，有點奇妙XD

```js
// 福衛二號衛星影像
const baseF2 = L.tileLayer('http://gis.sinica.edu.tw/tgos/file-exists.php?img=F2IMAGE_W-png-{z}-{x}-{y}', {
  maxZoom: 17
});
```

### 圖資介接

[圖資介接說明](https://gis.sinica.edu.tw/tgos/wmts)，找到該圖層`ResourceURL`中的`template`網址，把`{TileMatrix}`換成`{z}`、`{TileCol}`換成`{x}`、`{TileRow}`換成`{y}`即可。

![地理資訊科學研究專題中心圖資 圖資介接說明]({{site.url}}/img/2021-03-22-Leaflet-note/API_sinica.png "地理資訊科學研究專題中心圖資 圖資介接說明")

# 常用套件

## Leaflet.awesome-markers－讓Marker附帶icon且支援多色

###### 撰文時版本｜2.0.2

![Leaflet.awesome-markers 展示]({{site.url}}/img/2021-03-22-Leaflet-note/demo_Leaflet.awesome-markers.png "Leaflet.awesome-markers 展示")

[Leaflet.awesome-markers](https://github.com/lvoogdt/Leaflet.awesome-markers)，可在原`Marker`上加上icon，並且支持多種顏色。

```js
let redMarker = L.AwesomeMarkers.icon({
  icon: 'arrow-alt-circle-right',
  prefix: 'fa',
  markerColor: 'red'
});

let m = L.marker([23.604799, 120.7976256], {icon: redMarker});
```

目前似乎無法使用目前常見的svg icon，只能用Font Icon型式的icon。

`icon`參數就是各套件中，icon的個別名稱。  
要注意Ionicons，因為本身有分樣式類型，所以在icon頁面看到的名稱記得要加上樣式類型字首（ios風格就加上'ios-'，Material風格就加上'md-'）。

`prefix`參數是icon的字首，以下為官方說明有提到的icon套件與其CDN連結頁面，可自己挑看選看：

<table>
  <thead>
    <tr>
      <th>套件</th>
      <th>prefix</th>
      <th>CDN 頁面</th>
      <th>備註</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><a href="https://icons.getbootstrap.com/" target="_blank">Bootstrap Icons</a></td>
      <td>bi</td>
      <td><a href="https://icons.getbootstrap.com/#cdn" target="_blank">CDN 頁面</a></td>
      <td></td>
    </tr>
    <tr>
      <td><a href="https://getbootstrap.com/docs/3.3/" target="_blank">Bootstrap3 Glyphicons</a></td>
      <td>glyphicon</td>
      <td><a href="https://getbootstrap.com/docs/3.3/getting-started/#download-cdn" target="_blank">CDN 頁面</a></td>
      <td>預設值</td>
    </tr>
    <tr>
      <td><a href="https://fontawesome.com/" target="_blank">Font Awesome</a></td>
      <td>fa</td>
      <td><a href="https://fontawesome.com/how-to-use/customizing-wordpress/snippets/setup-cdn-webfont" target="_blank">CDN 頁面</a></td>
      <td></td>
    </tr>
    <tr>
      <td><a href="https://ionicons.com/" target="_blank">Ionicons</a></td>
      <td>ion</td>
      <td><a href="https://ionicons.com/usage" target="_blank">CDN 頁面</a></td>
      <td>
        Font Icon只到版本4.5。<br>
        因為Ionicons本身有分樣式類型，所以在icon頁面看到的名稱記得要加上樣式類型字首（ios風格就加上'ios-'，Material風格就加上'md-'）。
      </td>
    </tr>
  </tbody>
</table>

`markerColor`參數目前版本（2.0.1）有red、darkred、lightred、orange、beige、green、darkgreen、lightgreen、blue、darkblue、lightblue、purple、darkpurple、pink、cadetblue、white、gray、lightgray、black。

其他參數還有：

`iconColor` 就icon的顏色，預設是白色。

`spin` 當使用`Font Awesome`，設為true即可使該icon旋轉。

`extraClasses` 可加上自訂的class

## Leaflet.markercluster－群聚Markers展開縮起

###### 撰文時版本｜1.5.0

![Leaflet.markercluster 展示]({{site.url}}/img/2021-03-22-Leaflet-note/demo_Leaflet.markercluster.png "Leaflet.markercluster 展示")

[Leaflet.markercluster](https://github.com/Leaflet/Leaflet.markercluster)，當部分`Marker`太過密集，就會自動縮起並顯示縮起的`Marker`數目，不同數目級距也會顯示不同顏色。

```js
let markersLayer = new L.MarkerClusterGroup();
```

裡面可以設置參數但是我都沒用過，基本上不設定就很好用了（？）

使用上就是把`Marker`加到`MarkerClusterGroup`群組中，就會自動展開縮起了。

可以和前面提到的[Leaflet.awesome-markers](#leafletawesome-markers讓marker附帶icon且支援多色)併用。

## leaflet-velocity－繪製風場

###### 撰文時版本｜1.9.0

![leaflet-velocity 展示]({{site.url}}/img/2021-03-22-Leaflet-note/demo_leaflet-velocity.png "leaflet-velocity 展示")

[leaflet-velocity](https://github.com/danwild/leaflet-velocity)，將`json`格式（格式請參考[grib2json](https://github.com/cambecc/grib2json)）的風場資料視覺化。

```js
let windLayer = L.velocityLayer({
  displayValues: true, // 若為true，會顯示目前滑鼠游標處的風場資料
  displayOptions: { // displayValues: true時顯示資料的設定
    velocityType: "台灣海域風",
    displayPosition: "bottomleft",
    displayEmptyString: "無資料"
  },
  data: windData // 資料不一定要在此時放入，也可用下方setData方法
});
```

若要初始化後再設置設定值或風場資料，可以用`setOptions`或`setData`。

```js
windLayer.setData(windData);
```

## Leaflet-Ruler－地圖量測

![Leaflet-Ruler 展示]({{site.url}}/img/2021-03-22-Leaflet-note/demo_Leaflet-Ruler.png "Leaflet-Ruler 展示")

[Leaflet-Ruler](https://github.com/gokertanrisever/leaflet-ruler)，簡易的地圖量測工具。

這邊有修改樣式以及語言顯示，如果不需要修改就不用加上`rulerOption`這段了，樣式部分也同樣不必要，但只是換成自己比較喜歡的模樣。

```js
let rulerOption = {
                    position: 'topright',
                    circleMarker: {
                      color: '#ac1515',
                      radius: 2
                    },
                    lineStyle: {
                      color: 'rgba(0, 0, 0, 0.4)',
                      dashArray: '1,6'
                    },
                    lengthUnit: {
                      display: 'km',
                      decimal: 2,
                      factor: null,
                      label: '總距離:'
                    },
                    angleUnit: {
                      display: '&deg;',
                      decimal: 2,
                      factor: null,
                      label: '方位角:'
                    }
                  };
L.control.ruler(rulerOption).addTo(map);
```

```css
/* 移動時即時資料顯示區塊 */
.moving-tooltip{
  border: 0;
  background-color: rgba(0, 0, 0, 0.8);
  color: #FFF;
  letter-spacing: 0.5px;
  line-height: 1.2;
  opacity: 1;
}
.moving-tooltip.leaflet-tooltip-left::before{
  border-left-color: rgba(0, 0, 0, 0.8);
}
.moving-tooltip.leaflet-tooltip-right::before{
  border-right-color: rgba(0, 0, 0, 0.8);
}

/* 計算結果顯示區塊 */
.result-tooltip{
  border: 0;
  background-color: rgba(0, 0, 0, 0.6);
  color: #FFF;
  letter-spacing: 0.5px;
  line-height: 1.2;
}
.result-tooltip.leaflet-tooltip-left::before{
  border-left-color: rgba(0, 0, 0, 0.6);
}
.result-tooltip.leaflet-tooltip-right::before{
  border-right-color: rgba(0, 0, 0, 0.6);
}

/* 量測狀態時的開關按鈕 */
.leaflet-ruler-clicked{
  border-color: #d81111 !important;
}
```
