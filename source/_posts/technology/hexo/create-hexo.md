---
title: hexo博客的搭建
tags:
  - Hexo
categories: Hexo
author: zhangyifeng
date: 2021-09-07 11:05:43
---

{% title h2, 前言 %}
搭建自己的博客是我设想了很久的事，最近才落实下来，至于为什么搭建这么多天后才出这个搭建过程一是因为需要熟悉 hexo 博客的一些操作，二是因为懒，本文仅是描述本博客的搭建过程，并不是非常全面，不能称之为教程，只是一个记录。文章底部有我搭建过程中参考的几个博客，有需自取。

{% title h2, 1.安装Git Bash %}

[Git Bash](https://gitforwindows.org/)跟在 cmd 中操作命令行差不多，但是能方便一些

{% timeline %}
{% timenode %}
[官网下载地址](https://gitforwindows.org/)
[我使用的 Git Bash](/package/Git-2.17.0-64-bit.exe)
{% endtimenode %}
{% timenode %}
安装步骤：双击下载好的 exe 文件，一直 next
{% endtimenode %}
{% timenode %}
安装好后，打开 gitbash，输入命令 {% u git version %} 查看版本：
![](/medias/createHexo/git1.png)
{% endtimenode %}
{% endtimeline %}

{% title h2, 2.安装nvm %}
nvm 是为了管理不同版本的 node 与 npm 而产生的，安装nvm是为了下载node，如果你不想用可以直接下载[node](https://nodejs.org/en/)

{% timeline %}
{% timenode %}
[下载地址](https://github.com/coreybutler/nvm-windows/releases)
[我使用的 nvm](/package/nvm-setup.exe)
{% note warning,不要安装到C盘 %}
{% endtimenode %}
{% timenode %}
安装步骤：双击下载好的 exe 文件，一直 next
{% endtimenode %}
{% timenode %}
安装好后，打开 gitbash，输入命令 {% u git version %} 查看版本：
![](/medias/createHexo/git1.png)
{% endtimenode %}
{% endtimeline %}

https://www.cnblogs.com/visugar/p/6821777.html

https://blog.csdn.net/sinat_37781304/article/details/82729029

https://blog.csdn.net/Wanguyunxiaodaniu/article/details/104716776?utm_medium=distribute.pc_relevant.none-task-blog-2~default~baidujs_baidulandingword~default-0.control&spm=1001.2101.3001.4242

https://blog.csdn.net/weixin_44105459/article/details/103500535
