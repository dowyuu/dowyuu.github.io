---
layout: post
title:  "To checked or not to checked－表單元件的不明確選取狀態「indeterminate」與偽類「:indeterminate」"
date:   2021-01-07 18:00:00 +0800
categories:
- Program
tags: HTML jQuery indeterminate  CSS pseudo-classes :indeterminate
description: 當有全選功能的checkbox時，若只有部分選項被選取了，既不是全選也不是全不選，那就給它個不明確選取狀態「indeterminate」吧，內文中也不只checkbox「indeterminate」狀態，還包括「CSS偽類 :indeterminate」、「radio的indeterminate狀態與偽類 :indeterminate應用」、「checkbox全選功能搭配indeterminate狀態」喔！
---

<style>
  input[type="checkbox"] + label, input[type="radio"] + label{ margin-left: 5px; }
  .example-show ul{ margin: 0; padding: 0; }
  .example-show li{ list-style: none !important; }
  .example-show li > ul{ padding-left: 20px; }
  .breakfasts{ display: flex; }
  .breakfasts li + li{ margin-left: 10px; }
  .breakfasts input:indeterminate + label{ color: red; }
  .breakfasts li:last-child input:indeterminate + label::after{ content: '＊ Oops！你還沒有選擇早餐要吃什麼喔！'; margin-left: 15px; font-weight: bold; }
  .btn-reset{ margin-top: 10px; }
  #checkbox-test label, #radio-test label{ display: inline-block; min-width: 22em; }
  .indeterminate-value{ margin-left: 15px; }
  #pseudo-ck + label, .pseudo-pg + label{ margin-left: 20px; }
  #pseudo-ck:checked + label:before{ content: '您已同意所有上述同意書項目'; color: #666; }
  #pseudo-ck:not(:checked) + label:before{ content: '請勾選同意書項目'; color: red; }
  #pseudo-ck:not(:checked):indeterminate + label:before{ content: '必須要「勾選所有」上述同意書項目，才可進行到下一步！'; color: red; }
  .pseudo-pg:indeterminate + label::before{ content: '讀取執行進度中，請稍後...'; color: red; }
  .pseudo-pg + label::before{ content: '執行中'; color: #666; }
</style>

# DowYuu言

當一個群組中有很多checkbox時，有時會搭配一個有全選功能的`checkbox`讓使用者可以不用點到手痠，增加使用者體驗。

如果群組中的checkbox全部都被選取了，那全選就會自動勾起，沒問題。  
如果群組中的checkbox全部都沒有被選取，那全選就會自動取消勾起，沒問題。  
如果群組中的checkbox只有部分被選取，那全選就...嗯？To checked or not to checked？

# 瀏覽器支援 Browser compatibility

支援表[在此](https://caniuse.com/indeterminate-checkbox)，IE6+，可以舒服使用。

# indeterminate狀態（不明確狀態）基本使用

先提一下`checkbox`的`indeterminate`（不明確）狀態沒辦法用`HTML`屬性設置（`HTML`中沒有這個屬性），僅能靠`JavaScript`去設置。

HTML：
```html
<input type="checkbox" id="ck0"><label for="ck0">沒勾選</label>
<input type="checkbox" id="ck1" checked><label for="ck1">勾選</label>
<input type="checkbox" id="ck2"><label for="ck2">我是不明確狀態，要用js設置</label>
<input type="checkbox" id="ck3" checked><label for="ck3">我是不明確狀態並且有勾選，看不出來齁</label>
<input type="checkbox" id="ck4" indeterminate><label for="ck4">錯誤示範，不明確狀態用HTML設置是無效的</label>
```

JavaScript：
```js
// JavaScript
document.getElementById('ck2').indeterminate = true; // 設indeterminate值為true
let ck2Ind = document.getElementById('ck2').indeterminate; // 取indeterminate值，ck2Ind = true

// jQuery
$('#ck3').prop('indeterminate', true); // 設indeterminate值為true
let ck3Ind = $('#ck3').prop('indeterminate'); // 取indeterminate值，ck3Ind = true
```

只要點擊`indeterminate`狀態的`checkbox`後，會自動取消`indeterminate`狀態（取得`indeterminate`值會取得`false`），恢復到一般的`checkbox`勾選或不勾選的狀態，使用者怎麼點都無法回到`indeterminate`狀態。

<div class="example-show" id="checkbox-test">
  <div>點checkbox看看indeterminate值會不會變動：</div>
  <div><input type="checkbox" id="ck0"><label for="ck0">沒勾選</label><span class="indeterminate-value"></span></div>
  <div><input type="checkbox" id="ck1" checked><label for="ck1">勾選</label><span class="indeterminate-value"></span></div>
  <div><input type="checkbox" id="ck2"><label for="ck2">我被js設置了不明確狀態</label><span class="indeterminate-value"></span></div>
  <div><input type="checkbox" id="ck3" checked><label for="ck3">我被js設置了不明確狀態並且有勾選</label><span class="indeterminate-value"></span></div>
  <div><input type="checkbox" id="ck4" indeterminate><label for="ck4">錯誤示範，不明確狀態用HTML設置是無效的</label><span class="indeterminate-value"></span></div>
  <button type="button" id="checkbox-test-reset" class="btn-reset">回復初始狀態</button>
</div>

要知道，`indeterminate`狀態和`checked`狀態是**獨立的兩種狀態**，你可以在`indeterminate`狀態下同時是勾選的，或是在`indeterminate`狀態下不是勾選的，長的還都一個樣，就是一個不明確狀態的樣式蓋在`checkbox`上的感覺。

這意味`indeterminate`狀態**僅僅是視覺上的不同而已，目的在於優化使用者在前端的交互**，且表單送出時也只會依據`checked`狀態送出，完全不管`indeterminate`狀態與否喔。

# CSS偽類 :indeterminate

`:indeterminate`代表不確定的表單元件，**不同表單元件會在不同情況下觸發該偽類**，並不是所有表單元件用`js`設置`indeterminate`狀態為`true`就會套用到`:indeterminate`（只有`checkbox`是如此），使用時要注意IE的[兼容性](https://caniuse.com/css-indeterminate-pseudo)。

<table>
  <thead>
    <tr>
      <th>元件</th>
      <th>長相</th>
      <th>情況</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>checkbox</td>
      <td><input type="checkbox" id="indeterminate-ck"></td>
      <td><code class="language-plaintext highlighter-rouge">indeterminate</code>狀態被<code class="language-plaintext highlighter-rouge">JavaScript</code>設置為<code class="language-plaintext highlighter-rouge">true</code></td>
    </tr>
    <tr>
      <td>radio</td>
      <td><input type="radio" id="indeterminate-rd"></td>
      <td>在相同<code class="language-plaintext highlighter-rouge">name</code>中的所有<code class="language-plaintext highlighter-rouge">radio</code>都沒有選取</td>
    </tr>
    <tr>
      <td>progress</td>
      <td><progress id="indeterminate-pg"></progress></td>
      <td>處於不確定狀態（未設置<code class="language-plaintext highlighter-rouge">value）</code></td>
    </tr>
  </tbody>
</table>

表單元件本身沒辦法使用太多CSS屬性，但可以還是運用CSS選擇器做出不同變化，`radio`的應用可見[下方](#radio推薦應用偽類indeterminate)。

HTML：
```html
<input type="checkbox" id="pseudo-ck"><label for="pseudo-ck"></label>
<progress class="pseudo-pg" id="pseudo-pg-0"></progress><label for="pseudo-pg-0"></label>
<progress class="pseudo-pg" id="pseudo-pg-1" value="40" max="100"></progress><label for="pseudo-pg-1"></label>
```

CSS：
```css
#pseudo-ck + label,
.pseudo-pg + label{
  margin-left: 20px;
}
#pseudo-ck:checked + label:before{
  content: '您已同意所有上述同意書項目';
  color: #666;
}
#pseudo-ck:not(:checked) + label:before{
  content: '請勾選同意書項目';
  color: red;
}
#pseudo-ck:not(:checked):indeterminate + label:before{
  content: '必須要「勾選所有」上述同意書項目，才可進行到下一步！';
  color: red;
}
.pseudo-pg:indeterminate + label::before{
  content: '讀取執行進度中，請稍後...';
  color: red;
}
.pseudo-pg + label::before{
  content: '執行中';
  color: #666;
}
```

<div class="example-show" id="pseudo-test">
  <div><input type="checkbox" id="pseudo-ck"><label for="pseudo-ck"></label></div>
  <div><progress class="pseudo-pg" id="pseudo-pg-0"></progress><label for="pseudo-pg-0"></label></div>
  <div><progress class="pseudo-pg" id="pseudo-pg-1" value="40" max="100"></progress><label for="pseudo-pg-1"></label></div>
  <button type="button" id="pseudo-test-reset" class="btn-reset">回復初始狀態</button>
</div>

# radio元件應用

## radio設置indeterminate狀態沒什麼用

`radio`元件在用`JavaScript`設置了`indeterminate`狀態後，在外觀上並沒有區別，但與`checkbox`不同，有選取後`indeterminate`值不會自動變為`false`，有設就一直為`true`，沒設就一直為`false`，`indeterminate`狀態對`radio`來說似乎沒什麼使用必要，**`radio`比較有用的運用是配合偽類` :indeterminate`**（詳見[下方](#radio推薦應用偽類indeterminate)）。

<div class="example-show" id="radio-test">
  <div>點radio看看indeterminate值會不會變動：</div>
  <div><input type="radio" id="rd0" name="rd0"><label for="rd0">我沒有被選取</label><span class="indeterminate-value"></span></div>
  <div><input type="radio" id="rd1" name="rd1" checked><label for="rd1">我被選取了</label><span class="indeterminate-value"></span></div>
  <div><input type="radio" id="rd2" name="rd2"><label for="rd2">我是不明確狀態，沒有被選取</label><span class="indeterminate-value"></span></div>
  <div><input type="radio" id="rd3" name="rd3" checked><label for="rd3">我是不明確狀態，有被選取</label><span class="indeterminate-value"></span></div>
  <button type="button" id="radio-test-reset" class="btn-reset">回復初始狀態</button>
</div>

## radio推薦應用－偽類:indeterminate

上面有提到的CSS偽類`:indeterminate`會在**相同`name`中的所有`radio`都沒有選取**的狀況下被套用，自己覺得`radio`比較能實際應用在這部分，但請注意**IE不支援**。

HTML：
```html
<div>早餐想吃什麼呢？</div>
<ul class="breakfasts">
  <li><input type="radio" id="bf0" name="breakfast"><label for="bf0">鍋燒冬粉</label></li>
  <li><input type="radio" id="bf1" name="breakfast"><label for="bf1">蛋餅</label></li>
  <li><input type="radio" id="bf2" name="breakfast"><label for="bf2">鐵板麵</label></li>
</ul>
```

CSS：
```css
  .breakfasts{
    display: flex;
  }
  .breakfasts li + li{
    margin-left: 10px;
  }
  .breakfasts input:indeterminate + label{
    color: red;
  }
  .breakfasts li:last-child input:indeterminate + label::after{
    content: '＊ Oops！你還沒有選擇早餐要吃什麼喔！';
    margin-left: 15px;
    font-weight: bold;
  }
```

<div class="example-show">
  <div>早餐想吃什麼呢？</div>
  <ul class="breakfasts">
    <li><input type="radio" id="bf0" name="breakfast"><label for="bf0">鍋燒冬粉</label></li>
    <li><input type="radio" id="bf1" name="breakfast"><label for="bf1">蛋餅</label></li>
    <li><input type="radio" id="bf2" name="breakfast"><label for="bf2">鐵板麵</label></li>
  </ul>
  <button type="button" id="breakfasts-reset" class="btn-reset">取消選取早餐選項</button>
</div>

# checkbox全選功能應用

前面有說到`indeterminate`狀態**僅僅是視覺上的不同而已，目的在於優化使用者在前端的交互**，也有提到像是全選功能中，若你只有勾選其中幾個選項，那顆全選`checkbox`在一般情況下無論勾選與不勾選意義上都不是那麼到位，這裡就可以用`indeterminate`狀態來解決這個情況。

HTML：
```html
<ul>
  <li><input type="checkbox" id="cks_all"><label for="cks_all">全選</label></li>
  <li>
    <ul class="options">
      <li><input type="checkbox" id="cks0" name="cks"><label for="cks0">馬來貘</label></li>
      <li><input type="checkbox" id="cks1" name="cks"><label for="cks1">山貘</label></li>
      <li><input type="checkbox" id="cks2" name="cks"><label for="cks2">中美貘</label></li>
      <li><input type="checkbox" id="cks3" name="cks"><label for="cks3">南美貘</label></li>
      <li><input type="checkbox" id="cks4" name="cks"><label for="cks4">卡波馬尼貘</label></li>
    </ul>
  </li>
</ul>
```

jQuery：
```js
$(function(){

  let $options = $('.options'); // 選項列表

  // 變動選項列表中的checkbox
  $options.on('change', 'input[type="checkbox"]', function(){
    let cksName = this.name, // 取得選項列表中的checkbox的name
      $allCks = $('#'+cksName+'_all'); // 全選checkbox
    if($options.find('input[name="'+cksName+'"]').length === $options.find('input[name="'+cksName+'"]:checked').length){
      // checkbox數量 = 勾選的checkbox數量，即勾選全選，非不明確狀態
      $allCks.prop('indeterminate', false).prop('checked', true);
    }else if($options.find('input[name="'+cksName+'"]:checked').length === 0){
      // 勾選的checkbox數量 = 0，即不勾選全選，非不明確狀態
      $allCks.prop('indeterminate', false).prop('checked', false);
    }else{
      // checkbox數量 != 勾選的checkbox數量，且勾選的checkbox數量 != 0
      // 為不明確狀態，且不勾選全選
      $allCks.prop('indeterminate', true).prop('checked', false);
    }
  })

  // 點擊全選checkbox
  $('#cks_all').on('click', function(){
    let $cks = $options.find('input[name="'+this.id.split('_')[0]+'"]'); // 選項列表中的checkbox
     // 將選項列表中的checkbox設為全選checkbox本身的勾選狀態
    $cks.prop('checked', $(this).prop('checked'));
  })

});
```

<div class="example-show">
  <ul>
    <li><input type="checkbox" id="cks_all"><label for="cks_all">全選</label></li>
    <li>
      <ul class="options">
        <li><input type="checkbox" id="cks0" name="cks"><label for="cks0">馬來貘</label></li>
        <li><input type="checkbox" id="cks1" name="cks"><label for="cks1">山貘</label></li>
        <li><input type="checkbox" id="cks2" name="cks"><label for="cks2">中美貘</label></li>
        <li><input type="checkbox" id="cks3" name="cks"><label for="cks3">南美貘</label></li>
        <li><input type="checkbox" id="cks4" name="cks"><label for="cks4">卡波馬尼貘</label></li>
      </ul>
    </li>
  </ul>
</div>

完畢灑花！

<script
  src="https://code.jquery.com/jquery-2.2.4.min.js"
  integrity="sha256-BbhdlvQf/xTY9gja0Dq3HiwQF8LaCRTXxZKRutelT44="
  crossorigin="anonymous"></script>
<script>
  $(function(){
    document.getElementById('ck2').indeterminate = true;
    $('#ck3, #rd2, #rd3, #indeterminate-ck, #pseudo-ck').prop('indeterminate', true);

    <!-- 全選 -->
    let $options = $('.options');
    $options.on('change', 'input[type="checkbox"]', function(){
      let cksName = this.name, $allCks = $('#'+cksName+'_all');
      if($options.find('input[name="'+cksName+'"]').length === $options.find('input[name="'+cksName+'"]:checked').length){
        $allCks.prop('indeterminate', false).prop('checked', true);
      }else if($options.find('input[name="'+cksName+'"]:checked').length === 0){
        $allCks.prop('indeterminate', false).prop('checked', false);
      }else{
        $allCks.prop('indeterminate', true).prop('checked', false);
      }
    })
    $('#cks_all').on('click', function(){
      let $cks = $options.find('input[name="'+this.id.split('_')[0]+'"]');
      $cks.prop('checked', $(this).prop('checked'));
    })

    <!-- checkbox與radio測試區塊 取得並設置indeterminate值 -->
    $('#checkbox-test, #radio-test').find('input').each(setIndeterminateVal);
    $('#checkbox-test, #radio-test').on('click', 'input', setIndeterminateVal);
    function setIndeterminateVal(){
      let indeterminateValue = $(this).prop('indeterminate');
      $(this).nextAll('.indeterminate-value').text('indeterminate值為：'+indeterminateValue);
    }

    <!-- 重設按鈕 -->
    $('#pseudo-test-reset').on('click', function(){
      $('#pseudo-ck').prop('checked', false).prop('indeterminate', true);
    })
    $('#checkbox-test-reset').on('click', function(){
      $('#checkbox-test .indeterminate-value').text('');
      $('#ck0, #ck4').prop('checked', false);
      $('#ck1').prop('checked', true);
      $('#ck2').prop('checked', false).prop('indeterminate', true);
      $('#ck3').prop('checked', true).prop('indeterminate', true);
      $('#checkbox-test').find('input').each(setIndeterminateVal);
    })
    $('#radio-test-reset').on('click', function(){
      $('#radio-test .indeterminate-value').text('');
      $('#rd0').prop('checked', false);
      $('#rd1').prop('checked', true);
      $('#rd2').prop('checked', false).prop('indeterminate', true);
      $('#rd3').prop('checked', true).prop('indeterminate', true);
      $('#radio-test').find('input').each(setIndeterminateVal);
    })
    $('#breakfasts-reset').on('click', function(){
      $('.breakfasts').find('input').prop('checked', false);
    });
  });
</script>
