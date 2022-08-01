---
title: 人员选择器
tags:
  - Vue3
  - ElementPlus
categories:
  - web前端
  - Element组件
author: 糖醋灬里脊
date: 2021-08-18 11:14:07
---
![](https://s2.loli.net/2022/08/01/H5bcjKsx74DpV3z.png)

使用 [Element-plus](https://element-plus.gitee.io/#/zh-CN) 中的 [Transfer 穿梭框](https://element-plus.gitee.io/#/zh-CN/component/transfer) 组件封装的一个人员选择器，使用的是 VUE3 中的 setup 语法糖写法

{% title h2, 选择器子组件的.vue 文件代码%}

userMailList  为全部人员

```javascript
<template>
  <el-dialog
    title="人员选择"
    v-model="show"
    width="640px"
    :close-on-click-modal="false"
    center
    @close="clearitems"
  >
    <div class="peopleCheck">
      <el-transfer
        v-model="yetPeople"
        filterable
        :filter-method="filterMethod"
        filter-placeholder="请输入人员拼音"
        :data="allPeople"
        @left-check-change="changeLeftPeople"
        :titles="['未选人员', '已选人员']"
      />
    </div>
    <template #footer>
      <span class="dialog-footer">
        <el-button @click="check">确认</el-button>
      </span>
    </template>
  </el-dialog>
</template>
<script setup>
import {
  ref,
  reactive,
  onMounted,
  defineEmits,
  defineProps,
  defineExpose,
  watch,
} from "vue";
import { userMailList } from "@/http/api/workbench";
const emits = defineEmits(["clearitems", "backData"]);
const props = defineProps({
  choosePeople: {
    type: Array,
    default: () => {
      return [];
    },
  },
  type: {
    type: String,
    default: () => {
      return "";
    },
  },
});
let show = ref(true);
let yetPeople = ref([]);
let allPeople = ref([]);
let leftPeople = ref("");
//拉取数据
const generateData = async () => {
  let res = await userMailList({});
  let peopleList = res.data;
  const datain = [];
  peopleList.forEach((item, index) => {
    datain.push({
      label: item.userName,
      key: item.accountId,
      spell: item.userName,
      disabled: item.isMain === true ? true : false,
      isAdmin: item.isMain,
    });
  });
  allPeople.value = datain;
  yetPeople.value = JSON.parse(JSON.stringify(props.choosePeople));
};
//确认
const check = () => {
  emits("backData", yetPeople.value);
  clearitems();
};
//左侧选择的人员监听
const changeLeftPeople = (query, item) => {
  leftPeople.value = query;
};
//退出
const clearitems = () => {
  emits("clearitems");
};
//自定义搜索方法
const filterMethod = (query, item) => {
  return item.spell.indexOf(query) > -1;
};
// 监听
//监听左侧人员选择状态
watch(leftPeople, (nVal) => {
  if (leftPeople.value.length > 0 && props.type == "limt1") {
    allPeople.value.forEach((item, index) => {
      if (item.key != leftPeople.value) {
        item.disabled = true;
      }
    });
  } else if (props.type == "limt1") {
    allPeople.value.forEach((item, index) => {
      if (item.isAdmin !== true) {
        item.disabled = false;
      }
    });
  }
});
//监听右侧是否有已选择的人员
watch(yetPeople, (nVal) => {
  if (yetPeople.value.length == 0 && props.type == "limt1") {
    allPeople.value.forEach((item, index) => {
      if (item.isAdmin !== true) {
        item.disabled = false;
      }
    });
  }
});
onMounted(() => {
  generateData();
});
</script>
<style lang="scss" scoped>
.peopleCheck {
  height: 500px;
  ::v-deep(.el-transfer-panel__body) {
    height: 350px;
  }
  ::v-deep(.el-transfer-panel__list.is-filterable) {
    height: 350px;
  }
}
</style>
```

{% title h2, 父组件的部分代码%}

### 1.template 内引入子组件

```javascript
<PeopleCheck
	ref="refPeopleCheck"
	v-if="checkPeopleCheck"
	:choosePeople="choosePeople" 已选的人员的ID数组
	type="limt1"                 打开只能从左侧选择一个人的模式
	@backData="backData"         回显
	@clearitems="clearitems"     关闭
    />

```

### 2.js

```javascript
//↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓成员选择组件↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓//
//打开成员选择
const peopleCheckShow = async (row) => {
  //已选择的人
  choosePeople.value = {{子组件中peopleList的key相对应 }}
  checkPeopleCheck.value = true;
};
//成员选择回显的数据
const backData = (data) => {};
//关闭
const clearitems = (row) => {
  checkPeopleCheck.value = false;
};
//↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑成员选择组件↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑//
```
