---
title: hexo博客引入动态图表chart
tags:
  - Hexo
categories: Hexo
author: zhangyifeng
date: 2021-08-30 10:55:43
---

## Hexo 中的 Chartjs

[Chartjs 的文档首页](https://chartjs.bootcss.com/)，相比于 ECharts，感觉 Chart 的某些图表或动画的帧率不高，但是画风清新、干净，令人很舒适，前提是需要引用其他博主写的插件（十分感谢 shen-yu，[博主地址](https://shen-yu.gitee.io/2020/chartjs/)）,插件用 npm 安装

```js
npm install hexo-tag-chart --save
```

之后在文章内使用 chart 的 tag 就可以了

```js
{% chart 90% 300 %}
\\TODO option goes here
{% endchart %}
```

## 示例
(可能因本人使用主题原因，第一次加载会失败，需要刷新)
{% chart 90% 300 %}
{
type: 'line',
data: {
    labels: ['January', 'February', 'March', 'April', 'May', 'June', 'July'],
    datasets: [{
        label: 'My First dataset',
        backgroundColor: 'rgb(112, 66, 185,0.5)',
        borderColor: 'rgb(112, 66, 185,0.5)',
        data: [0, 10, 5, 2, 20, 30, 45]
    }]
},
options: {
    responsive: true,
    title: {
        display: true,
        text: 'Chart.js Line Chart'
    }
}
}
{% endchart %}
代码

```js
{% chart 90% 300 %}
    {
    type: 'line',
    data: {
    labels: ['January', 'February', 'March', 'April', 'May', 'June', 'July'],
    datasets: [{
        label: 'My First dataset',
        backgroundColor: 'rgb(112, 66, 185,0.5)',
        borderColor: 'rgb(112, 66, 185,0.5)',
        data: [0, 10, 5, 2, 20, 30, 45]
        }]
    },
    options: {
        responsive: true,
        title: {
        display: true,
        text: 'Chart.js Line Chart'
        }
    }
}
{% endchart %}
```
