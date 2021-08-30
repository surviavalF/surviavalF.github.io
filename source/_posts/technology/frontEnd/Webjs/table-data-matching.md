---
title: table列表匹配
date: 2021-08-24 10:44:12
tags:
  - Vue
categories:
  - web前端
  - Element组件
author: zhangyifeng
---

## table 列表数据匹配

```javascript
//Html
<el-table-column
    prop="rcCreatePerson"
    align="center"
    label="创建人"
    :formatter="rcCreatePersonFormatter"
    show-overflow-tooltip
></el-table-column>

//js
// 创建人
rcCreatePersonFormatter(row, column, cellValue) {
    try {
        return this.userList.find((item) => item.userId === cellValue).   userName;
    } catch (e) {
        return cellValue;
    }
},
```
