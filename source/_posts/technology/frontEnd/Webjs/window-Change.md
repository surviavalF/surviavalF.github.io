---
title: js监听html页面大小变化
tags:
categories:
  - web前端
  - js
author: 糖醋灬里脊
date: 2021-09-06 16:07:50
img: "/medias/background/js-background.png"
---

{% title h2, js 常用的浏览器窗体可视区域大小位置的方法%}

{% noteblock info, 获取窗口的宽度和高度，不包括滚动条 %}

<div>
    Width:<input type="text" id="txt1" disabled="disabled" value="1"/>
    Height:<input type="text" id="txt2" disabled="disabled" value="1"/>
</div>
{% endnoteblock %}
{% noteblock info, 获取窗口的宽度和高度，包括边线和滚动条的宽 %}
<div>
    Width:<input type="text" id="txt3" disabled="disabled" value="1"/>
    Height:<input type="text" id="txt4" disabled="disabled" value="1"/>
</div>
{% endnoteblock %}
{% noteblock info, 获取网页正文全文宽高 %}
<div>
    Width:<input type="text" id="txt5" disabled="disabled" value="1"/>
    Height:<input type="text" id="txt6" disabled="disabled" value="1"/>
</div>
{% endnoteblock %}
{% noteblock info, 获取网页上方或者左边被卷起来的部分 %}
<div>
    Top:<input type="text" id="txt7" disabled="disabled" value="1"/>
    Left:<input type="text" id="txt8" disabled="disabled" value="1"/>
</div>
{% endnoteblock %}
{% noteblock info, 获取你整个显示器屏幕大小的 %}
<div>
    Width:<input type="text" id="txt9" disabled="disabled" value="1"/>
    Height:<input type="text" id="txt10" disabled="disabled" value="1"/>
</div>
{% endnoteblock %}
{% noteblock info, 获取浏览器窗口顶部/左端距离屏幕大小的(需要刷新，如果你是多屏此数值是根据一号屏定位) %}
<div>
    Top:<input type="text" id="txt11" disabled="disabled" value="1"/>
    Left:<input type="text" id="txt12" disabled="disabled" value="1"/>
</div>
{% endnoteblock %}

<script>
    // 定义事件侦听器函数
      function watchWindowSize() {
        // 获取窗口的宽度和高度，不包括滚动条
        var txt1=document.getElementById("txt1")
        var txt2=document.getElementById("txt2")
        txt1.value = document.documentElement.clientWidth;
        txt2.value = document.documentElement.clientHeight;
        //获取窗口的宽度和高度，包括边线和滚动条的宽
        var txt3=document.getElementById("txt3")
        var txt4=document.getElementById("txt4")
        txt3.value = document.documentElement.offsetWidth;
        txt4.value = document.documentElement.offsetHeight;
        //获取网页正文全文宽高
        var txt5=document.getElementById("txt5")
        var txt6=document.getElementById("txt6")
        txt5.value = document.documentElement.scrollWidth;
        txt6.value = document.documentElement.scrollHeight;
        //获取网页上方或者左边被卷起来的部分
        var txt7=document.getElementById("txt7")
        var txt8=document.getElementById("txt8")
        txt7.value = document.documentElement.scrollTop;
        txt8.value = document.documentElement.scrollLeft;
        //获取你整个显示器屏幕大小的
        var txt9=document.getElementById("txt9")
        var txt10=document.getElementById("txt10")
        txt9.value = window.screen.width;
        txt10.value = window.screen.height;
        //浏览器窗口顶部/左端距离屏幕大小的(需要刷新，如果你是多屏此数值是根据一号屏定位)
        var txt11=document.getElementById("txt11")
        var txt12=document.getElementById("txt12")
        txt11.value = window.screenTop;
        txt12.value = window.screenLeft;

      }
      // 将事件侦听器函数附加到窗口的resize事件
      window.addEventListener("resize", watchWindowSize);
      // 第一次调用该函数
      watchWindowSize();
</script>

```js
// 获取窗口的宽度和高度，不包括滚动条
var txt1 = document.getElementById("txt1");
var txt2 = document.getElementById("txt2");
txt1.value = document.documentElement.clientWidth;
txt2.value = document.documentElement.clientHeight;
//获取窗口的宽度和高度，包括边线和滚动条的宽
var txt3 = document.getElementById("txt3");
var txt4 = document.getElementById("txt4");
txt3.value = document.documentElement.offsetWidth;
txt4.value = document.documentElement.offsetHeight;
//获取网页正文全文宽高
var txt5 = document.getElementById("txt5");
var txt6 = document.getElementById("txt6");
txt5.value = document.documentElement.scrollWidth;
txt6.value = document.documentElement.scrollHeight;
//获取网页上方或者左边被卷起来的部分
var txt7 = document.getElementById("txt7");
var txt8 = document.getElementById("txt8");
txt7.value = document.documentElement.scrollTop;
txt8.value = document.documentElement.scrollLeft;
//获取你整个显示器屏幕大小的
var txt9 = document.getElementById("txt9");
var txt10 = document.getElementById("txt10");
txt9.value = window.screen.width;
txt10.value = window.screen.height;
//浏览器窗口顶部/左端距离屏幕大小的(需要刷新，如果你是多屏此数值是根据一号屏定位)
var txt11 = document.getElementById("txt11");
var txt12 = document.getElementById("txt12");
txt11.value = window.screenTop;
txt12.value = window.screenLeft;
```
