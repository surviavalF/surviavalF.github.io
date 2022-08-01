---
date: 2021-08-24 10:49:19
title: css修改
tags:
  - Vue
  - CSS
categories:
  - web前端
  - CSS
author: 糖醋灬里脊
img: "/medias/background/3.jpg"
top: true
---

{% title h2, 箭头%}

↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓  
↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑

{% title h2, el-input 属性 type="number"时去掉右侧箭头%}

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

{% title h2, 选中改变样式%}

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

{% title h2, Element 滚动条样式%}

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

{% title h2, 鼠标悬停样式%}

用 css 添加手状样式,鼠标移上去变小手,变小手

cursor:pointer;

cursor 其他取值  
auto          ：标准光标  
default       ：标准箭头  
pointer, hand         ：手形光标  
wait ：等待光标  
text  ：I 形光标  
vertical-text          ：水平 I 形光标  
no-drop      ：不可拖动光标  
not-allowed ：无效光标  
help ：帮助光标  
all-scroll         ：三角方向标  
move ：移动标  
crosshair ：十字标  
e-resize  
n-resize  
nw-resize  
w-resize  
s-resize  
se-resize  
sw-resize

用 JS 使鼠标变小手 onmouseover(鼠标越过的时候)
onmouseover="this.style.cursor='hand'"

{% title h2, 纯英文，数字不换行问题处理%}

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

{% title h2, Css :nth-child() 选择器%}

### 1、first-child

first-child 表示选择列表中的第一个标签。例如：

```css
li:first-child {
  background: #fff;
}
```

### 2、last-child

last-child 表示选择列表中的最后一个标签，例如：

```css
li:last-child {
  background: #fff;
}
```

### 3、nth-child(3)

表示选择列表中的第 3 个标签，例如：

```css
li:nth-child(3) {
  background: #fff;
}
```

代码中的 3 也可以改成其它数字，如 4、5 等。想选择第几个标签，就填写几。

### 4、nth-child(2n)

这个表示选择列表中的偶数标签，即选择 第 2、第 4、第 6…… 标签，例如：

```css
li:nth-child(2n) {
  background: #fff;
}
```

### 5、nth-child(2n-1)

这个表示选择列表中的奇数标签，即选择 第 1、第 3、第 5、第 7……标签，例如：

```css
li:nth-child(2n-1) {
  background: #fff;
}
```

### 6、nth-child(n+3)

这个表示选择列表中的标签从第 3 个开始到最后。

### 7、nth-child(-n+3)

这个表示选择列表中的标签从 0 到 3，即小于 3 的标签。

### 8、nth-last-child(3)

这个表示选择列表中的倒数第 3 个标签。

{% title h2, 头像堆叠样式%}

html

```html
<div class="people">
  <div><span>人</span></div>
  <div><span>人</span></div>
</div>
```

css

```css
.people {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  div {
    margin-bottom: 10px;
    margin-right: 1px;
    width: 20px;
    height: 25px;
    text-align: center;
    line-height: 25px;
    color: #fff;
    span {
      display: inline-block;
      width: 25px;
      height: 25px;
      background: $bg-color-orange;
      border-radius: 50%;
      border: 2px solid #fff;
    }
  }
}
```
