---
title: hexo博客的搭建
tags:
  - Hexo
categories: Hexo
author: zhangyifeng
date: 2021-09-07 11:05:43
top: true
---

{% title h2, 前言 %}
搭建自己的博客是我设想了很久的事，最近才落实下来，至于为什么搭建这么多天后才出这个搭建过程一是因为需要熟悉 hexo 博客的一些操作，二是因为懒，本文仅是描述本博客的搭建过程，并不是非常全面，不能称之为教程，只是一个记录，每个电脑的情况不同不要完全相信我接下来要说的步骤，请灵活变通。文章底部有我搭建过程中参考的几个博客，有需自取。

{% title h2, 安装Git Bash %}

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

{% title h2, 安装nvm %}

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

{% title h2, 安装hexo %}

先创建一个文件夹（用来存放所有 blog 的东西），然后在该文件夹下打开 gitbush，或者你可以 cd 过来。

{% timeline %}

{% timenode %}
安装 hexo 命令：

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

{% timenode %}
我使用的是 VScode 编译器 [我使用的版本](/package/VSCodeUserSetup-x64-1.42.0.exe)
在终端里按顺序输入下面代码，就可以访问 http://localhost:4000，这就是你的博客啦

```js
hexo clean
hexo generate
hexo server
```

{% endtimenode %}

{% endtimeline %}

{% title h2, 部署到github %}
完成上面的步骤你的博客只能在你的本地运行，别人是看不到的，所以你如果想让别人看到需要搭桥到 github 上
{% timeline %}

{% timenode %}
没有账号去创建一个账号，名字取好一点，本人 github 的名字当时是瞎起的，单词还拼错了，也懒的改，建议第一次就选个好名字。

{% endtimenode %}

{% timenode %}
创建好账号后去创建一个 repo，名称为 yourname.github.io, 其中 yourname 是你的 github 名称，按照这个规则创建才有用，如下：
![](/medias/createHexo/github1.png)
![](/medias/createHexo/github2.png)

{% endtimenode %}

{% timenode %}
回到 gitbash 中，配置 github 账户信息（YourName 和 YourEail 都替换成你自己的）：
![](/medias/createHexo/github3.png)
![](/medias/createHexo/github4.png)

{% endtimenode %}
{% timenode %}
创建 SSH
在 gitbash 中输入：

```js
ssh-keygen -t rsa -C "youremail@example.com"
```

生成 ssh。然后按下图的方式找到 id_rsa.pub 文件的内容。
![](/medias/createHexo/github5.png)

将上面获取的 ssh 放到 github 中：
![](/medias/createHexo/github6.png)
![](/medias/createHexo/github7.png)
添加一个 New SSH key ，title 随便取，key 就填刚刚那一段。

在 gitbash 中验证是否添加成功：

```js
ssh -T git@github.com
```

{% endtimenode %}

{% timenode %}
用编辑器打开你的 blog 项目，修改\_config.yml 文件的一些配置(冒号之后都是有一个半角空格的)

```text
deploy: 
  type: git;
  repo: https://github.com/YourgithubName/YourgithubName.github.io.git
  branch: master;

```

![](/medias/createHexo/vue1.png)

{% endtimenode %}

{% timenode %}
在你的 blog 文件夹下打开 gitbush，先安装一波：

```js
npm install hexo-deployer-git --save
```

（这样才能将你写好的文章部署到 github 服务器上并让别人浏览到）
执行命令(建议每次都按照如下步骤部署)：
hexo clean
hexo generate
hexo deploy
注意 deploy 的过程中要输入你的 username 及 passward。

{% note warning,
你应该会需要输入3次密码（没记错是三次，一次是gitbush里，一次是github的弹出框，一次是密码的单独弹出框），当出现一个单独的密码输入框时，这时候的passward的不再是登录密码，而是一个token，需要去设置，跟下面这个博客学
[github开发人员在七夕搞事情](https://blog.csdn.net/weixin_41010198/article/details/119698015)%}

在浏览器中输入http://yourgithubname.github.io 就可以看到你的自己的博客了。
{% endtimenode %}

{% endtimeline %}

{% title h2, （可选）安装TortoiseGit %}
TortoiseGit，我认为小乌龟（俗称）还是挺好用的，至少某些情况下提交代码不需要打命令了

{% timeline %}

{% timenode %}
[我使用的版本](/package/TortoiseGit-2.10.0.2-64bit.msi)
[我使用的汉化](/package/TortoiseGit-LanguagePack-2.10.0.0-64bit-zh_CN.msi)
安装步骤：[Git 的安装与 TortoiseGit 的安装和汉化](https://www.cnblogs.com/nicaicai/p/12383735.html)
{% endtimenode %}

{% timenode %}
第一次拉取代码时进入到 github 仓库，找到如图 url
![](/medias/createHexo/wugui1.png)
复制后在你可以随便选一个文件夹,右键选 Git 克隆,他会自动填充
![](/medias/createHexo/wugui2.png)
![](/medias/createHexo/wugui3.png)
{% endtimenode %}

{% timenode %}
以后拉取代码时进入克隆的文件夹内，右键如图
![](/medias/createHexo/wugui4.png)
{% endtimenode %}

{% timenode %}
推送代码时需要两步操作如图
点击 Git 提交
![](/medias/createHexo/wugui5.png)
点击提交
![](/medias/createHexo/wugui6.png)
点击推送
![](/medias/createHexo/wugui7.png)
{% endtimenode %}

{% endtimeline %}

{% title h2, 结语 %}
有很东西本文是没有涉及到的，我认为在看这篇文章的你需要一“内内”（neinei）的探索精神，自己摸索一下写出来的东西才会带给你成就感，所以加油吧。
下面有几个比较好的博主写的可以去看，本人使用的主题是[Bamboo](https://yuang01.github.io/),喜欢的可以点链接进去看。
[hexo 从零开始到搭建完整](https://www.cnblogs.com/visugar/p/6821777.html)
[hexo 史上最全搭建教程](https://blog.csdn.net/sinat_37781304/article/details/82729029)(此文章的“3. git 分支进行多终端工作”是本人正在使用但是此篇文章没有介绍的，建议去读一下)
[教你半小时搭建 Hexo-hueman 主题博客](https://blog.csdn.net/weixin_44105459/article/details/103500535)
[hexo,史上最全搭建个人博客](https://blog.csdn.net/Wanguyunxiaodaniu/article/details/104716776?utm_medium=distribute.pc_relevant.none-task-blog-2~default~baidujs_baidulandingword~default-0.control&spm=1001.2101.3001.4242)

{% note info,
虽然本文不会有商业用途，但是本文有多处的引用，如有侵权请及时联系本人，本人会立马改正删除。%}
