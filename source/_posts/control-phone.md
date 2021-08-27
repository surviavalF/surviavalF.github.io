---
title: 电脑控制手机的小软件
author: zhangyifeng
date: 2021-08-27 14:13:50
---

## QtScrcpy 软件

![](/medias/else/qtscrcpy.png)

实时显示 Android 设备屏幕,实时键鼠控制 Android 设备。支持 GNU/Linux，Windows 和 MacOS

## 使用方法

在你的电脑上接入 Android 设备，然后运行程序，按顺序点击如下按钮即可连接到 Android 设备

无线连接步骤（保证手机和电脑在同一个局域网）：

安卓手机端在开发者选项中打开 usb 调试

通过 usb 连接安卓手机到电脑

点击刷新设备，会看到有设备号更新出来

点击获取设备 IP(可能需要开启wifi)

点击启动 adbd

无线连接

再次点击刷新设备，发现多出了一个 IP 地址开头的设备，选择这个设备

启动服务

备注：启动 adbd 以后不用再连着 usb 线了，以后连接断开都不再需要，除非安卓 adbd 停了需要重新启动
