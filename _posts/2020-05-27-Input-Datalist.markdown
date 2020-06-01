---
layout: post
title:  "input+datalist的select偽裝術－強化附帶文字輸入篩選選項功能的選單輸入格"
date:   2020-05-27 18:00:00 +0800
categories:
- Program
tags: input datalist HTML5 jQuery
description: HTML5新標籤，讓選單可用文字輸入方式篩選出其中選項，並且讓它偽裝成select的行為！
---

<style>
  #msg1, #msg3{ margin-left: 10px; }
  #btnSend1, #btnSend3{ margin-left: 5px; }
</style>

# DowYuu言

我原先是在[Angular Material](https://material.angular.io/components/autocomplete/overview){:target="_blank"}中看到這個功能的，對於選項非常多的選單而言用起來相當舒服，用一般的下拉選單到底是要拉到民國幾年啦XD

到了非`Angular`的案子中，想說來找找有沒有類似的套件可以用，意外的發現了HTML5的新標籤`<datalist>`，欣喜若狂呀。

# 瀏覽器支援

為HTML5的新標籤，支援表[在此](https://caniuse.com/#search=datalist){:target="_blank"}，IE10+，在寫文章的今天大多的瀏覽器都已經有支援咯。

若是要支援更低版本的IE，可以試試[jQuery UI中的Autocomplete](https://jqueryui.com/autocomplete/){:target="_blank"}。

# 使用

## 一般使用

一般使用上是用一個`<input>`加上一個`<datalist>`，在`<input>`中要設置`list`屬性並對應到`<datalist>`的`id`屬性。

這邊必須要說一點，`<datalist>`和`<select>`最大不一樣的點是，`<datalist>`預設是 **允許輸入非選單內選項** 的，當然這點你要另外做處理也是可以啦XD

HTML：
```html
<span>選擇你最喜歡的動物：</span>
<input type="text" id="input0" list="options0">
<datalist id="options0">
  <option>馬來貘</option>
  <option>山貘</option>
  <option>中美貘</option>
  <option>南美貘</option>
  <option>卡波馬尼貘</option>
</datalist>
```

<div class="exampleShow">
  <span>選擇你最喜歡的動物：</span>
  <input type="text" id="input0" list="options0">
  <datalist id="options0">
    <option>馬來貘</option>
    <option>山貘</option>
    <option>中美貘</option>
    <option>南美貘</option>
    <option>卡波馬尼貘</option>
  </datalist>
</div>

執行結果可以看到，只要有輸入關鍵字，下方就會跳出對應的選項。只要點了選項，上方輸入格的值就會被設定進去。  
而不點選項的話也可以隨意輸入，不像`<select>`一定只能選擇裡面的選項。

## 偽裝select第一步：限制只能輸入選項內容

這時候你會想，能不能像`<select>`一樣，限制一定只能輸入裡面的選項呢？

實際開發上或許會遇到，我要能用關鍵字找選項，也要只輸入裡面的選項，這時候可以用JS進行改良。

這邊的JS參考了[Stephan Muller
](https://stackoverflow.com/users/124238/stephan-muller){:target="_blank"}在[stackoverflow問題中的回答](https://stackoverflow.com/questions/29882361/show-datalist-labels-but-submit-the-actual-value){:target="_blank"}，感謝大大的分享，如果要純Javascript寫法可以參考連結中的寫法，下面的範例中我改成jQuery的寫法了。

HTML：
```html
<span>選擇你最喜歡的動物：</span>
<input type="text" id="input1" list="options1">
<datalist id="options1">
  <option>馬來貘</option>
  <option>山貘</option>
  <option>中美貘</option>
  <option>南美貘</option>
  <option>卡波馬尼貘</option>
</datalist>
<button type="button" id="btnSend1">送出</button>
<span id="msg1"></span>
```

jQuery：
```js
// 點擊送出按鈕
$('#btnSend1').click(function(){
  if(verifiDatalist('#input1')){
    // 驗證成功
    $('#msg1').css('color', '#328c32').text('送出成功 (●´ω｀●)ゞ');
  }else{
    // 驗證錯誤，並非輸入選項內文字
    $('#msg1').css('color', 'red').text('請輸入內建選項文字！ヽ(ﾟQ Д Q)ﾉﾟ');
  }
});

// 驗證方法
// 傳入參數：
// inputId：欲驗證之input的id
// 回傳：
// 若輸入選項內文字則回傳true
// 若輸入非選項內文字則回傳false
function verifiDatalist(inputId){
  const $input = $('#'+inputId),
        $options = $('#' + $input.attr('list') + ' option'),
        inputVal = $input.val();
  let verification = false;
  for(let i = 0; i < $options.length; i++) {
    const $option = $options.eq(i),
          showWord = $option.text();
    if(showWord == inputVal){
      verification = true;
    }
  }
  return verification;
}
```

HTML：
<div class="exampleShow">
  <span>選擇你最喜歡的動物：</span>
  <input type="text" id="input1" list="options1">
  <datalist id="options1">
    <option>馬來貘</option>
    <option>山貘</option>
    <option>中美貘</option>
    <option>南美貘</option>
    <option>卡波馬尼貘</option>
  </datalist>
  <button type="button" id="btnSend1">送出</button>
  <span id="msg1"></span>
</div>

## 偽裝select第二步：讓選項顯示文字與實際選取值不同

又思考了一下，在`<select>`中`<option>`顯示的文字和其`value`屬性可以是不同值並且取值時是取得`value`屬性中的值，那`<datalist>`呢？

HTML：
```html
<span>選擇你最喜歡的動物：</span>
<input type="text" id="input2" list="options2">
<datalist id="options2">
  <option value="0">馬來貘</option>
  <option value="1">山貘</option>
  <option value="2">中美貘</option>
  <option value="3">南美貘</option>
  <option value="4">卡波馬尼貘</option>
</datalist>
```

<div class="exampleShow">
  <span>選擇你最喜歡的動物：</span>
  <input type="text" id="input2" list="options2">
  <datalist id="options2">
    <option value="0">馬來貘</option>
    <option value="1">山貘</option>
    <option value="2">中美貘</option>
    <option value="3">南美貘</option>
    <option value="4">卡波馬尼貘</option>
  </datalist>
</div>

在Chrome（版本 81.0.4044.138，64位元）中，在選項中可以同時看到設置的`value`屬性值和其設置文字，並被給上了美美的樣式，並且點了選項後依舊可以同時看到設置的`value`屬性值「0」和其設置文字「馬來貘」。

![Chrome中表現，可同時看到設置的value屬性值和其設置文字]({{site.url}}/img/2020-05-27-Input-Datalist/datalist_option_value_Chrome.png)  

而在IE11（版本11.836.18362.0）中，在選項中僅能看到設置文字，但點下選項後，可以在輸入格中看到該值原本的設置文字「馬來貘」。

![IE11中表現，可以在輸入格中看到該值原本的設置文字]({{site.url}}/img/2020-05-27-Input-Datalist/datalist_option_value_IE.png)  

再緊接著Firefox（版本76.0.1，32 位元），在選項中一樣僅能看到設置文字，但點下選項後...就沒有然後了，留下一個讓使用者不知所云的「0」，無法回頭看那個「0」是來自哪個選項...當然也不是說它錯，但就不方便啊XD

![Firefox中表現，只剩value屬性，無法回頭看是來自哪個設置文字]({{site.url}}/img/2020-05-27-Input-Datalist/datalist_option_value_Firefox.png)  

測試了三個瀏覽器，雖然大家最後都取得了「馬來貘」中的`value`屬性值「0」的結果，但是在**顯示上非常不直覺**－畢竟對使用者而言，他所選的是「馬來貘」選項而不是「0」。

解決的方法百百種，這邊提供一個方法參考：多一個儲存實際值的`<input>`。

這邊多一個儲存實際值的`<input id="input3">`，並且將其設置`hidden`隱藏起來，顯示選項部分就交給`<input id="input3_show">`。為了不要讓Chrome使用者看到不需要看到的`value`屬性值，所以將`value`屬性值改為`data-value`屬性值存放。

接著修改[上一部分](#偽裝<select>第一步：限制只能輸入選項內容)的程式，經過驗證後將其`data-value`屬性值放回`<input id="input3">`中即可。

HTML：
```html
<span>選擇你最喜歡的動物：</span>
<input type="hidden" id="input3">
<input type="text" id="input3_show" list="options3">
<datalist id="options3">
  <option data-value="0">馬來貘</option>
  <option data-value="1">山貘</option>
  <option data-value="2">中美貘</option>
  <option data-value="3">南美貘</option>
  <option data-value="4">卡波馬尼貘</option>
</datalist>
<button type="button" id="btnSend3">送出</button>
<span id="msg3"></span>
```

jQuery：
```js
// 點擊送出按鈕
$('#btnSend3').click(function(){
  const val = verifiDatalist('input3_show');
  if(val!==false){
    $('#input3').val(val);
    $('#msg3').css('color', '#328c32').text('送出成功 (●´ω｀●)ゞ，送出值為：'+ $('#input3').val());
  }else{
    $('#input3').val('');
    $('#msg3').css('color', 'red').text('請輸入內建選項文字！ヽ(ﾟQ Д Q)ﾉﾟ');
  }
});

// 驗證方法
// 傳入參數：
// inputId：欲驗證之input的id
// 回傳：
// 若輸入選項內文字則回傳其data-value值
// 若輸入非選項內文字則回傳false
function verifiDatalist(inputId){
  const $input = $('#'+inputId),
        $options = $('#' + $input.attr('list') + ' option'),
        inputVal = $input.val();
  let verification = false;
  for(let i = 0; i < $options.length; i++) {
    const $option = $options.eq(i),
          dataVal = $option.data('value'),
          showWord = $option.text(),
          val =  $option.val();
    if(showWord == inputVal){
      verification = dataVal;
    }
  }
  return verification;
}
```

<div class="exampleShow">
  <span>選擇你最喜歡的動物：</span>
  <input type="hidden" id="input3">
  <input type="text" id="input3_show" list="options3">
  <datalist id="options3">
    <option data-value="0">馬來貘</option>
    <option data-value="1">山貘</option>
    <option data-value="2">中美貘</option>
    <option data-value="3">南美貘</option>
    <option data-value="4">卡波馬尼貘</option>
  </datalist>
  <button type="button" id="btnSend3">送出</button>
  <span id="msg3"></span>
</div>

偽裝`<select>`成功，下台一鞠躬★

<script
  src="https://code.jquery.com/jquery-2.2.4.min.js"
  integrity="sha256-BbhdlvQf/xTY9gja0Dq3HiwQF8LaCRTXxZKRutelT44="
  crossorigin="anonymous"></script>
<script>
  $(function(){
    $('#btnSend1').click(function(){
      if(verifiDatalist1('input1')){
        $('#msg1').css('color', '#328c32').text('送出成功 (●´ω｀●)ゞ');
      }else{
        $('#msg1').css('color', 'red').text('請輸入內建選項文字！ヽ(ﾟQ Д Q)ﾉﾟ');
      }
    });
    function verifiDatalist1(inputId){
      const $input = $('#'+inputId), $options = $('#' + $input.attr('list') + ' option'), inputVal = $input.val();
      let verification = false;
      for(let i = 0; i < $options.length; i++) {
        const $option = $options.eq(i), showWord = $option.text();
        if(showWord == inputVal){ verification = true; }
      }
      return verification;
    }
    $('#btnSend3').click(function(){
      const val = verifiDatalist3('input3_show');
      if(val!==false){
        $('#input3').val(val);
        $('#msg3').css('color', '#328c32').text('送出成功 (●´ω｀●)ゞ，送出值為：'+ $('#input3').val());
      }else{
        $('#input3').val('');
        $('#msg3').css('color', 'red').text('請輸入內建選項文字！ヽ(ﾟQ Д Q)ﾉﾟ');
      }
    });
    function verifiDatalist3(inputId){
      const $input = $('#'+inputId), $options = $('#' + $input.attr('list') + ' option'), inputVal = $input.val();
      let verification = false;
      for(let i = 0; i < $options.length; i++) {
        const $option = $options.eq(i),
            dataVal = $option.data('value'),
            showWord = $option.text(),
            val =  $option.val();
        if(showWord == inputVal){
          verification = dataVal;
        }
      }
      return verification;
    }
  });
</script>
