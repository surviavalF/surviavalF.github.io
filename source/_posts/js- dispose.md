---
title: 部分数据方式
tags:
  - Vue
categories:
  - web前端
  - js
author: zhangyifeng
date: 2021-08-24 10:25:43
---

## 1.计算时间差代码

```javascript
 timeChange(data1, data2) {
      let dateBegin = new Date(data2.replace(/-/g, "/"));
      let dateEnd = new Date(data1.replace(/-/g, "/"));
      let dateDiff = dateEnd.getTime() - dateBegin.getTime(); //时间差的毫秒数
      let dayDiff = Math.floor(dateDiff / (24 * 3600 * 1000)); //计算出相差天数
      let leave1 = dateDiff % (24 * 3600 * 1000); //计算天数后剩余的毫秒数
      let hours = Math.floor(leave1 / (3600 * 1000)); //计算出小时数
      //计算相差分钟数
      let leave2 = leave1 % (3600 * 1000); //计算小时数后剩余的毫秒数
      let minutes = Math.floor(leave2 / (60 * 1000)); //计算相差分钟数
      //计算相差秒数
      let leave3 = leave2 % (60 * 1000); //计算分钟数后剩余的毫秒数
      let seconds = Math.round(leave3 / 1000);
      console.log(
        " 相差 " +
          dayDiff +
          "天 " +
          hours +
          "小时 " +
          minutes +
          " 分钟" +
          seconds +
          " 秒"
      );
    },

```
## 2.刷新数组

this.$forceUpdate()

## 3.小数取整


1，Math.ceil()方法向上取整，整数部分值+1：

eg：Math.ceil(3/2) 输出：2

2，Math.floor()方法向下取整，整数部分值不变：

eg：Math.floor(3/2) 输出：1

3，Math.round()方法四舍五入取整：

eg：Math.round(3/2) 输出：2

4，parseInt()方法 抛去小数部分，只取整数部分：

eg：parseInt(3/2) 输出：1

## 4.手机号/身份证

```js
//reg.js页面
//判断是否是手机号
export const isPhoneNumber = (mobile) => {
  let reg =
    /^1(3[0-9]|4[5,7]|5[0,1,2,3,5,6,7,8,9]|6[2,5,6,7]|7[0,1,7,8]|8[0-9]|9[1,8,9])\d{8}$/;
  return reg.test(mobile);
};
//判断是否是身份证
export const isCardNo = (card) => {
  let reg =
    /^[1-9]\d{5}[1-9]\d{3}((0\d)|(1[0-2]))(([0|1|2]\d)|3[0-1])\d{3}([0-9]|X)$/;
  return reg.test(card);
};
//手机号中间隐藏
export const mobileEncrypt = (mobile) => {
  mobile = mobile + "";
  return `+86${mobile.replace(/(\d{3})\d*(\d{4})/, `$1 **** $2`)}`;
};

//Vue
import { mobileEncrypt } from "@/utils/reg.js";
userMobilePhone = mobileEncrypt(userMobilePhone);

```

## 5.字符长度计算（中文算两个字符）
```js
export const getStringLength = (e) => {
  let [length, cn, en] = [0, 0, 0];
  Array.from(e).map((item) => {
    if (e.charAt(item).match(/[\u4e00-\u9fa5]/g)) {
      length += 2;
      cn++;
    } else {
      length += 1;
      en++;
    }
  });
  return length;
};
```