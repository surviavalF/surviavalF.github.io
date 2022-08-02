---
title: hexo博客底部显示本站已运行xx年xx月xx日
tags:
  - Hexo
categories: Hexo
author: 糖醋灬里脊
date: 2021-09-02 17:17:43
img: "/medias/background/hexo-background.png"
---

{% title h2, Hexo 中的 .ejs%}

需要找到主题文件夹中的 footer.ejs (可能在{% span red, Hexo\themes\< theme~name >\layout\ _partial\footer %}，主题不同位置不同)

在合适的位置写

```js
//不同主题配置的路径不同
<%- partial('_partial/footer/runTime') %>
```

然后在刚才的文件夹里新建 runTime.ejs 文件，文件中写如下代码，全部复制即可

```js
<div>
  <span id="htmer_time"></span>
</div>

<script type="text/javascript">
  function secondToDate(second) {
    if (!second) {
      return 0;
    }
    second = Math.ceil(second / 1000)
    var time = new Array(0, 0, 0, 0, 0);
    if (second >= 365 * 24 * 3600) {
      time[0] = parseInt(second / (365 * 24 * 3600));
      second %= 365 * 24 * 3600;
    }
    if (second >= 24 * 3600) {
      time[1] = parseInt(second / (24 * 3600));
      second %= 24 * 3600;
    }
    if (second >= 3600) {
      time[2] = parseInt(second / 3600);
      second %= 3600;
    }
    if (second >= 60) {
      time[3] = parseInt(second / 60);
      second %= 60;
    }
    if (second > 0) {
      time[4] = second;
    }
    return time;
  }

  function setTime() {
      //写自己博客创建的时间
    var create_time = new Date('2021-8-16 14:11:30').getTime();
    var timestamp = Date.now();
    currentTime = secondToDate(timestamp - create_time);
    currentTimeHtml =
      "本站已勉强运行：" +
      currentTime[0] +
      "年" +
      currentTime[1] +
      "天" +
      currentTime[2] +
      "时" +
      currentTime[3] +
      "分" +
      currentTime[4] +
      "秒";
    document.getElementById("htmer_time").innerHTML = currentTimeHtml;
  }
  setInterval(setTime, 1000);
</script>

```
