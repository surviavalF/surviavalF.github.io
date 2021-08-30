---
date: 2021-08-24 10:49:19
title: css修改
tags:
  - Vue
  - CSS
categories:
  - web前端
  - CSS
author: zhangyifeng
top: true
---

## 1.箭头

↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓  
↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑

## 2.el-input 属性 type="number"时去掉右侧箭头

{% tabs tab-id %}

<!-- tab 本人使用过的 -->

```css
input::-webkit-outer-spin-button,
input::-webkit-inner-spin-button  {
    -webkit-appearance: none !important;
    -moz-appearance: none !important;
    -o-appearance: none !important;
    -ms-appearance: none !important;
    appearance: none !important;
    margin: 0;
}
input[type="number"]  {
    -webkit-appearance: textfield;
    -moz-appearance: textfield;
    -o-appearance: textfield;
    -ms-appearance: textfield;
    appearance: textfield;
}
```

<!-- endtab -->

<!-- tab 未确定好不好用的 -->

```css
/deep/.[className] input::-webkit-outer-spin-button,
/deep/.[className] input::-webkit-inner-spin-button  {
    -webkit-appearance: none !important;
}
/deep/.[className] input[type="number"]  {
    -moz-appearance: textfield;
}
```

<!-- endtab -->

{% endtabs %}

## 3.选中改变样式

data 里
isActive:-1,

method 里

```js
checkedItem(index){
	this.isActive=index;
},
```

页面里

```html
<div v-for="(item,index) in nameoptions" class="class名" :class="{新加的class样式:index==isActive}" @click="checkItem(index)>{{item.name}}</div>
```

## 4.Element 滚动条样式

```css
 .el-table__body-wrapper::-webkit-scrollbar {
     width: 0px !important;
     height: 0px !important;
 }
 .el-table__body-wrapper::-webkit-scrollbar-thumb {
    border-radius: 0px !important;
    background: transparent !important;

}
```

## 5.鼠标悬停样式

用 css 添加手状样式,鼠标移上去变小手,变小手

cursor:pointer;

cursor 其他取值  
auto                    ：标准光标  
default                 ：标准箭头  
pointer, hand                   ：手形光标  
wait                     ：等待光标  
text                      ：I 形光标  
vertical-text          ：水平 I 形光标  
no-drop                ：不可拖动光标  
not-allowed           ：无效光标  
help                     ：帮助光标  
all-scroll         ：三角方向标  
move                     ：移动标  
crosshair           ：十字标  
e-resize  
n-resize  
nw-resize  
w-resize  
s-resize  
se-resize  
sw-resize

用 JS 使鼠标变小手 onmouseover(鼠标越过的时候)
onmouseover="this.style.cursor='hand'"

## 6.纯英文，数字不换行问题处理

1.问题描述：

用户输入一串英文字母，设置了换行，但是只有中文换行，英文不换行

2.处理方式：

英文一串字母，被认为是一个单词，所以不换行

```css
 {
  overflow: hidden;
  white-space: normal;
  word-wrap: break-word;
  word-break: break-all;
  text-overflow: ellipsis;
}
```
