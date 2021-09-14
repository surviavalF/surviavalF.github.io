---
title: hexo博客引入Echarts
tags:
  - Hexo
categories: Hexo
author: 糖醋灬里脊
date: 2021-09-01 09:55:43
---

## Hexo 中的 Echarts

[ECharts](https://echarts.apache.org/zh/index.html) 是一个使用 JavaScript 实现的开源可视化库，涵盖各行业图表，满足各种需求。功能非常强大，想要用好建议多翻看几遍官网的 API

## 步骤

### 1.下载

在 Hexo 博客中引入[ECharts](https://echarts.apache.org/zh/index.html)需要下载 Echarts 的 js 文件

[echarts.min.js 传送门](https://github.com/apache/echarts/tree/4.6.0/dist)

然后放在 {% span red, Hexo\themes\< theme~name >\source\js %} 下

### 2.写

直接在.md 文件中写就可以（本人使用的 Hexo 主题为[Bamboo](https://github.com/yuang01/theme)需要在主题的 {% span red, _config.yml %} 的 import: 下引入刚下载的 js 文件）

```js
    <div id="test2" style="width:100%;height:500px;"></div>
    <script type="text/javascript"src="/js/echarts.min.js"></script>
    <script type="text/javascript"src="/js/jquery.js"></script>
    <script type="text/javascript">
    var  myChart2 = echarts.init(document.getElementById("test2"));//div元素节点的对象
    option = {
        xAxis: {
            type: 'category',
            data: ['Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat', 'Sun']
        },
        yAxis: {
            type: 'value'
        },
        series: [{
            data: [150, 230, 224, 218, 135, 147, 260],
            type: 'line'
        }]
    };
    myChart2.setOption(option);
    window.οnresize=function(){
        myChart2.resize();
    }
    </script>
```
[MAKEAPIE](https://www.makeapie.com/explore.html)一个非常不错的Echarts示例网站
## 示例
<div style="display:flex;flex-wrap: wrap;">
    <div id="test1" style="width:580px;height:600px;"></div>
    <div id="test2" style="width:580px;height:600px;"></div>
    <div id="test3" style="width:580px;height:600px;"></div>
    <div id="test4" style="width:580px;height:600px;"></div>
</div>
<script type="text/javascript"src="/js/echarts.min.js"></script>
<script type="text/javascript"src="/js/jquery.js"></script>
<script type="text/javascript">
var  myChart1 = echarts.init(document.getElementById("test1"));
var  myChart2 = echarts.init(document.getElementById("test2"));
var  myChart3 = echarts.init(document.getElementById("test3"));
var  myChart4 = echarts.init(document.getElementById("test4"));//div元素节点的对象
option1 = {
    xAxis: {
        type: 'category',
        data: ['Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat', 'Sun']
    },
    yAxis: {
        type: 'value'
    },
    series: [{
        data: [150, 230, 224, 218, 135, 147, 260],
        type: 'line'
    }]
};
option2 = {
    xAxis: {
        type: 'category',
        data: ['Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat', 'Sun']
    },
    yAxis: {
        type: 'value'
    },
    series: [{
        data: [120, 200, 150, 80, 70, 110, 130],
        type: 'bar',
        showBackground: true,
        backgroundStyle: {
            color: 'rgba(180, 180, 180, 0.2)'
        }
    }]
};
option3 = {
    title: {
        text: '堆叠区域图'
    },
    tooltip: {
        trigger: 'axis',
        axisPointer: {
            type: 'cross',
            label: {
                backgroundColor: '#6a7985'
            }
        }
    },
    legend: {
        data: ['邮件营销', '联盟广告', '视频广告', '直接访问', '搜索引擎']
    },
    toolbox: {
        feature: {
            saveAsImage: {}
        }
    },
    grid: {
        left: '3%',
        right: '4%',
        bottom: '3%',
        containLabel: true
    },
    xAxis: [
        {
            type: 'category',
            boundaryGap: false,
            data: ['周一', '周二', '周三', '周四', '周五', '周六', '周日']
        }
    ],
    yAxis: [
        {
            type: 'value'
        }
    ],
    series: [
        {
            name: '邮件营销',
            type: 'line',
            stack: '总量',
            areaStyle: {},
            emphasis: {
                focus: 'series'
            },
            data: [120, 132, 101, 134, 90, 230, 210]
        },
        {
            name: '联盟广告',
            type: 'line',
            stack: '总量',
            areaStyle: {},
            emphasis: {
                focus: 'series'
            },
            data: [220, 182, 191, 234, 290, 330, 310]
        },
        {
            name: '视频广告',
            type: 'line',
            stack: '总量',
            areaStyle: {},
            emphasis: {
                focus: 'series'
            },
            data: [150, 232, 201, 154, 190, 330, 410]
        },
        {
            name: '直接访问',
            type: 'line',
            stack: '总量',
            areaStyle: {},
            emphasis: {
                focus: 'series'
            },
            data: [320, 332, 301, 334, 390, 330, 320]
        },
        {
            name: '搜索引擎',
            type: 'line',
            stack: '总量',
            label: {
                show: true,
                position: 'top'
            },
            areaStyle: {},
            emphasis: {
                focus: 'series'
            },
            data: [820, 932, 901, 934, 1290, 1330, 1320]
        }
    ]
};
option4 = {
    title: {
        text: '某站点用户访问来源',
        subtext: '纯属虚构',
        left: 'center'
    },
    tooltip: {
        trigger: 'item'
    },
    legend: {
        orient: 'vertical',
        left: 'left',
    },
    series: [
        {
            name: '访问来源',
            type: 'pie',
            radius: '50%',
            data: [
                {value: 1048, name: '搜索引擎'},
                {value: 735, name: '直接访问'},
                {value: 580, name: '邮件营销'},
                {value: 484, name: '联盟广告'},
                {value: 300, name: '视频广告'}
            ],
            emphasis: {
                itemStyle: {
                    shadowBlur: 10,
                    shadowOffsetX: 0,
                    shadowColor: 'rgba(0, 0, 0, 0.5)'
                }
            }
        }
    ]
};
myChart1.setOption(option1);
myChart2.setOption(option2);
myChart3.setOption(option3);
myChart4.setOption(option4);
window.οnresize=function(){
    myChart1.resize();
    myChart2.resize();
    myChart3.resize();
    myChart4.resize();
}
</script>


本文参考了CSDN博主「磊少1999」的原创文章 原文链接：https://blog.csdn.net/qq_41426117/article/details/105416911

