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
安装好后，打开 gitbash，输入命令

```js
git version
```

查看版本：
![](/medias/createHexo/git1.png)
{% endtimenode %}
{% endtimeline %}

{% title h2, 2.安装nvm %}

nvm 是为了管理不同版本的 node 与 npm 而产生的，安装 nvm 是为了下载 node，如果你不想用可以直接下载[node](https://nodejs.org/en/)

{% timeline %}
{% timenode %}
[下载地址](https://github.com/coreybutler/nvm-windows/releases)
[我使用的 nvm](/package/nvm-setup.exe)
{% note warning, 不要安装到C盘，C盘貌似会有一个虚拟空间，导致node安装失败
 %}
{% endtimenode %}
{% timenode %}
安装步骤：双击下载好的 exe 文件，一直 next
{% endtimenode %}
{% timenode %}
安装好后，右键打开 gitbash，输入命令

```js
nvm - v;
```

查看版本：
![](/medias/createHexo/nvm1.png)
{% endtimenode %}
{% timenode %}
然后你需要配置淘宝镜像，打开你刚刚 nvm 的安装文件夹，找到 settings.txt 文件
里面改成如下代码
eg://E:\nvm
//E:\nvm\nodejs

```js
root: 你安装的磁盘:\你安装的文件夹名 
path: 你安装的磁盘:\你安装的文件夹名\nodejs 
node_mirror: https://npm.taobao.org/mirrors/node/
npm_mirror: https://npm.taobao.org/mirrors/npm/
```

{% endtimenode %}
{% timenode %}
nvm 指令的[链接](https://www.runoob.com/w3cnote/nvm-manager-node-versions.html)
你也可以直接下载我用的 node 版本[下载链接](/package/v14.16.1.zip)
下载后放到刚才的文件夹（记得解压）
{% endtimenode %}

{% timenode %}
执行完上面的步骤后右键打开 gitbush 输入下面的命令就能看到你拥有的 node 版本

```js
nvm ls
```

![](/medias/createHexo/nvm2.png)

使用下面代码应用此版本 node

```js
nvm use 14.16.1
```

![](/medias/createHexo/nvm3.png)

使用下面代码查看 node 和 npm 是否安装成功

```js
node - v;
npm - v;
```

![](/medias/createHexo/nvm4.png)

{% endtimenode %}

{% endtimeline %}


{% title h2, 3.安装hexo %}

先创建一个文件夹（用来存放所有blog的东西），然后在该文件夹下打开gitbush，或者你可以cd过来。

{% timeline %}

{% timenode %}
安装hexo命令：
```js
npm i -g hexo
```
安装完后查看版本：
![](/medias/createHexo/hexo1.png)
{% endtimenode %}

{% timenode %}
安装完后初始化：
```js
hexo init
```
初始化完后会有如下文件：
![](/medias/createHexo/hexo2.png)
{% endtimenode %}

{% endtimeline %}

https://www.cnblogs.com/visugar/p/6821777.html

https://blog.csdn.net/sinat_37781304/article/details/82729029

https://blog.csdn.net/Wanguyunxiaodaniu/article/details/104716776?utm_medium=distribute.pc_relevant.none-task-blog-2~default~baidujs_baidulandingword~default-0.control&spm=1001.2101.3001.4242

https://blog.csdn.net/weixin_44105459/article/details/103500535
