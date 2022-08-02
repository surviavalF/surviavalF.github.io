---
title: 动态表头
date: 2021-08-24 10:35:48
tags:
  - Vue
categories:
  - web前端
  - Element组件
author: 糖醋灬里脊
img: "/medias/background/js-background.png"
---

{% title h2, vue 动态表头%}

### 1.Html

表内数据的字段名需要对应表头中的 prop

```javascript
<el-table
    :data="peopleDataList"
    height="92%"
    border
    style="width: 100%"
    v-loading="tableLoading"
>
    <el-table-column
        prop="tfUpdateDate"
        align="center"
        label="更新时间"
    ></el-table-column>
    <el-table-column
        align="center"
        v-for="(item, index, key) in equTable_columns"
        :item="item"
        :key="key"
        :index="index"
        :label="item.label"
    >
    <template v-slot:header>
            <el-popover width="400" trigger="hover" placement="top">
              <span  style="color: white;">{{ item.label }}</span>
              <div slot="reference" class="header-ellipsis">
                <span>{{ item.label }}</span>
              </div>
            </el-popover>
          </template>
        <template slot-scope="scope">
            {{ scope.row[item.prop] }}
        </template>
    </el-table-column>
</el-table>
```

### 2.Data

```javascript
//表头信息
equTable_columns: [
{
    prop: "name1",
    label: "事件A",
    width: "",
},
{
    prop: "name2",
    label: "事件B",
    width: "",
},
{
    prop: "name3",
    label: "事件C",
    width: "",
},
{
    prop: "name4",
    label: "事件D",
    width: "",
},
],
//表内信息
peopleDataList:[
    {
        tfUpdateDate: "2021-10-9",
        name1:"事件1"
    },
    {
        tfUpdateDate: "2021-10-9",
    }
]
```

{% title h2, 表头过长隐藏%}

```html
<template v-slot:header>
  <el-popover width="400" trigger="hover" placement="top">
    <span style="color: white;">{{ item.label }}</span>
    <div slot="reference" class="header-ellipsis">
      <span>{{ item.label }}</span>
    </div>
  </el-popover>
</template>
```

```css
/deep/.el-popover {
    max-width: 200px;
    width: none;
    text-align: center !important;
}
.header-ellipsis  {
    width: 120px;
    padding: 0px;
    overflow: hidden;
    text-overflow: ellipsis;
    white-space: nowrap;
}
```
