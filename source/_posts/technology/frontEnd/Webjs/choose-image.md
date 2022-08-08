---
title: 图片选择组件
date: 2022-08-08 14:38:33
tags:
  - Vue3
  - ElementPlus
categories:
  - web前端
  - Element组件
author: 糖醋灬里脊
img: "/medias/background/js-background.png"
---

{% title h2, 介绍%}

该组件在原有的 el-upload 组件基础上增加了限制比例上传功能，并且会根据传入比例自动适配上传按钮样式，以及回显已经上传的图片。

{% title h2, 组件内代码%}

{% folding green, html 代码 %}

```html
<template>
  <el-dialog
    v-model="centerDialogVisible"
    :title="title"
    width="30%"
    destroy-on-close
    :close-on-click-modal="false"
    center
    @close="closeLessionChange()"
    top="200px"
  >
    <div class="dialogForm">
      <p class="content-upload-p2">
        图片上传要求:
        <br />1.大小:5MB <br />2.格式: JPG、PNG、JPEG <br />3.尺寸比例为:{{
        props.width }}:{{ props.height }}
      </p>
      <div id="d1" class="card">
        <el-upload
          action=""
          list-type="picture-card"
          accept=".png,.jpg,.jpeg"
          :class="{ disUoloadSty: noneBtnImg1 }"
          :on-change="(file, fileList) => handleChange(file, fileList)"
          :on-preview="(file) => handlePictureCardPreview(file)"
          :before-remove="(file, fileList) => beforeRemove(file, fileList)"
          :multiple="false"
          :limit="1"
          :auto-upload="false"
        >
          <i class="el-icon-plus" />
        </el-upload>
        <el-dialog center v-model="frontDialogVisible">
          <div class="imgclass">
            <img :src="checkUrl" alt="" />
          </div>
        </el-dialog>
        <div>
          <div v-if="fileListOld.length !== 0" class="fileListOldStyle">
            <img class="fileImgStyle" :src="fileListOld" />

            <i
              class="el-icon-zoom-in"
              @click="handlePictureCardPreview(fileListOld)"
            ></i>
            <i
              class="el-icon-delete"
              @click="handleClickFileListOldDelete()"
            ></i>
          </div>
        </div>
      </div>
    </div>
    <template #footer>
      <el-button
        type="success"
        @click="finishCheck"
        :loading="loading"
        :disabled="disabled ? true : false"
        >保存
      </el-button>
    </template>
  </el-dialog>
</template>
```

{% endfolding %}

{% folding green, js 代码 %}

```javascript
<script setup>
/**
 * Vue3 element-plus 图片选择组件
 *
 * 1.针对不同的尺寸对DOM设置了不同的样式，在changeDome中自动计算
 *
 * 2.限制图片类型 "JPG", "PNG", "JPEG"  限制大小 5M
 *
 * 3.需要按尺寸上传图片，尺寸为长宽比
 *
 */
import { ref, onMounted, nextTick } from "vue";
import { useRouter } from "vue-router";
import { useStore } from "vuex";
import { ElMessage } from "element-plus";
import { uploadImageApi } from "@/http/api"; //图片上传接口
import _ from "lodash";

const store = useStore();
const router = useRouter();

const emits = defineEmits(["finishChoose", "closeImage", "clearImages"]);
const props = defineProps({
  oldImageRow: {
    //旧图片的url
    type: String,
    default: () => {
      return "";
    },
  },
  height: {
    //限制高度
    type: Number,
    default: () => {
      return 300;
    },
  },
  width: {
    //限制宽度
    type: Number,
    default: () => {
      return 400;
    },
  },
  title: {
    //标题
    type: String,
    default: () => {
      return "选择封面";
    },
  },
});

let centerDialogVisible = ref(true); //弹窗显示
let loading = ref(false); //按钮
let disabled = ref(false); //按钮
let checkUrl = ref(""); //点击图片ID
let fileListOld = ref([]); //回显图片ID
let noneBtnImg1 = ref(false);
let frontDialogVisible = ref(false);
let oldUrl = ref(""); //旧的图片ID
let newsPicture = ref("");

//改变样式
const changeDome = async () => {
  await nextTick();
  // var card = document.getElementById("d1");
  let height = 0;
  let width = 0;
  let percentage = 0;

  if (props.height > props.width) {
    percentage = Math.round((props.width / props.height) * 100) / 100;
    height = 350;
    width = Math.round(height * percentage);
  } else {
    percentage = Math.round((props.height / props.width) * 100) / 100;
    width = 350;
    height = Math.round(width * percentage);
  }
  setDomStyle(width, height);
};

//完成选择
const finishCheck = async () => {
  buttonLoading();
  let imgUrl = "";
  let param = new FormData();

  if (newsPicture.value.raw) {
    param.append(
      "file",
      new File([newsPicture.value.raw], newsPicture.value.name, {
        type: newsPicture.value.raw.type,
      })
    );
    param.append("oldUrl", "");
    let { data } = await uploadImageApi(param);
    imgUrl = data[0];
    emits("finishChoose", imgUrl);
    buttonLoading();
    closeLessionChange();
  } else {
    imgUrl = "";
    buttonLoading();
    ElMessage.error("未选择新的图片");
  }
};

//↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓上传照片↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓//
//上传.校验.赋值
const handleChange = async (file, fileList) => {
  if (fileList.length > 0) {
    noneBtnImg1.value = true;
  }
  const isLt2M = file.size / 1024 / 1024 < 5;
  var testmsg = file.name.substring(file.name.lastIndexOf(".") + 1);
  let type = ["JPG", "PNG", "JPEG", "jpg", "png", "jpeg"];
  let isPdf = type.includes(testmsg);

  if (!isPdf) {
    ElMessage.error("仅支持*.JPG、.PNG、.JPEG类型文件!");
    fileList.pop();
    noneBtnImg1.value = false;
    return false;
  }
  if (!isLt2M) {
    ElMessage.error("单个文件大小不超过5MB!");
    fileList.pop();
    noneBtnImg1.value = false;
    return false;
  }

  const isHW = new Promise(function (resolve, reject) {
    let minwidth = props.width;
    let minheight = props.height;
    let img = new Image();

    img.src = file.url;
    img.onload = function () {
      let value = img.width / img.height == minwidth / minheight;
      value ? resolve() : reject();
      console.log("你的图片尺寸", img.width, img.height);
      console.log("你的图片尺寸比例", img.height / img.width);
    };
  }).then(
    () => {
      newsPicture.value = file;
      changeDome();
    },
    () => {
      ElMessage.error(
        "上传图片尺寸比例应为:" + props.width + " / " + props.height + "！"
      );
      fileList.pop();
      noneBtnImg1.value = false;
      return false; //必须加上return false; 才能阻止
    }
  );
};
//删除
const beforeRemove = (file, fileList) => {
  noneBtnImg1.value = false;
  newsPicture.value = "";
};
const handlePictureCardPreview = (file) => {
  if (file.url) {
    checkUrl.value = file.url;
  } else {
    checkUrl.value = file;
  }
  frontDialogVisible.value = true;
};
const handleClickFileListOldDelete = (item, index) => {
  fileListOld.value = [];
  noneBtnImg1.value = false;
};
//↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑上传照片↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑//

//拉取数据
const getQueryList = async () => {
  if (props.oldImageRow) {
    oldUrl.value = _.cloneDeep(props.oldImageRow);
    newsPicture.value = _.cloneDeep(props.oldImageRow);
    fileListOld.value.push(oldUrl.value);
    if (fileListOld.value.length > 0) {
      noneBtnImg1.value = true;
    }
  }
};

//按钮loading
const buttonLoading = () => {
  loading.value = !loading.value;
  disabled.value = !disabled.value;
};

//关闭
const closeLessionChange = () => {
  fileListOld.value = [];
  loading = false; //按钮
  disabled = false; //按钮
  checkUrl = ""; //点击图片ID
  noneBtnImg1 = false;
  frontDialogVisible = false;
  oldUrl = ""; //旧的图片ID
  newsPicture = "";
  emits("closeImage");
  emits("clearImages");
};

const setDomStyle = (width, height) => {
  if (document.getElementsByClassName("el-icon-plus").length > 0) {
    document.getElementsByClassName(
      "el-icon-plus"
    )[0].style.lineHeight = `${height}px`;
  }
  if (
    document.getElementsByClassName("el-upload-list__item-actions").length > 0
  ) {
    document.getElementsByClassName(
      "el-upload-list__item-actions"
    )[0].style.width = `${width}px`;
    document.getElementsByClassName(
      "el-upload-list__item-actions"
    )[0].style.height = `${height}px`;
  }
  if (document.getElementsByClassName("el-upload-list__item").length > 0) {
    document.getElementsByClassName(
      "el-upload-list__item"
    )[0].style.width = `${width}px`;
    document.getElementsByClassName(
      "el-upload-list__item"
    )[0].style.height = `${height}px`;
  }
  if (document.getElementsByClassName("el-upload--picture-card").length > 0) {
    document.getElementsByClassName(
      "el-upload--picture-card"
    )[0].style.width = `${width}px`;
    document.getElementsByClassName(
      "el-upload--picture-card"
    )[0].style.height = `${height}px`;
  }
  if (document.getElementsByClassName("fileImgStyle").length > 0) {
    document.getElementsByClassName(
      "fileImgStyle"
    )[0].style.width = `${width}px`;
    document.getElementsByClassName(
      "fileImgStyle"
    )[0].style.height = `${height}px`;
    document.getElementsByClassName(
      "fileImgStyle"
    )[0].style.hover = `${width}px`;
    document.getElementsByClassName(
      "fileImgStyle"
    )[0].style.hover = `${height}px`;
  }
  if (document.getElementsByClassName("fileListOldStyle").length > 0) {
    document.getElementsByClassName(
      "fileListOldStyle"
    )[0].style.width = `${width}px`;
  }
  if (document.getElementsByClassName("el-icon-delete").length > 0) {
    document.getElementsByClassName("el-icon-delete")[0].style.left = `${
      width / 2 + 25
    }px`;
    document.getElementsByClassName(
      "el-icon-delete"
    )[0].style.lineHeight = `${height}px`;
  }
  if (document.getElementsByClassName("el-icon-zoom-in").length > 0) {
    document.getElementsByClassName("el-icon-zoom-in")[0].style.left = `${
      width / 2 - 25
    }px`;
    document.getElementsByClassName(
      "el-icon-zoom-in"
    )[0].style.lineHeight = `${height}px`;
  }
};

onMounted(() => {
  getQueryList();
  changeDome();
});
</script>
```

{% endfolding %}

{% folding green, css 代码 %}

```css
<style lang="scss" scoped>
.dialogForm {
  padding: 0 20px 0 20px;
  max-height: 700px;
  min-height: 230px;
  overflow: auto;

  .content-upload-p2 {
    margin-left: 30%;
    text-align: left;
    margin-bottom: 20px;
  }

  .card {
    display: flex;
    align-items: center;
    flex-direction: column;
    ::v-deep(.el-upload--picture-card) {
      opacity: 0.6;
      background-size: cover;
    }

    .imgclass {
      height: 650px;
      display: flex;
      align-items: center;
      justify-content: center;
      overflow: auto;
      text-align: center;
    }

    .fileImgStyle {
      border-radius: 6px;
    }

    .fileImgStyle:hover {
      border-radius: 6px;
      -webkit-filter: brightness(50%);
      filter: brightness(50%);
    }

    .fileListOldStyle {
      -webkit-transition: all 0.5s cubic-bezier(0.55, 0, 0.1, 1);
      transition: all 0.5s cubic-bezier(0.55, 0, 0.1, 1);
      font-size: $font-size-xxlg;
      color: #fff;
      line-height: 1.8;
      margin-top: 5px;
      position: relative;
      border-radius: 6px;
    }

    .fileListOldStyle:hover .fileImgStyle {
      border-radius: 6px;
      -webkit-filter: brightness(50%);
      filter: brightness(50%);
    }

    .fileListOldStyle .el-icon-delete {
      display: none;
      position: absolute;
      cursor: pointer;
      color: #fff;
    }

    .fileListOldStyle .el-icon-zoom-in {
      display: none;
      position: absolute;
      cursor: pointer;
      color: #fff;
    }
  }

  .el-select-dropdown__item {
    padding: 0 32px 0 20px !important;
  }

  .icons {
    display: flex;
  }

  ::v-deep(.disUoloadSty .el-upload--picture-card) {
    display: none;
    /* 上传按钮隐藏 */
  }

  .fileListOldStyle:hover .el-icon-zoom-in {
    display: inline-block;
  }

  .fileListOldStyle:hover .el-icon-delete {
    display: inline-block;
  }

  .fileListOldStyle:hover .content-upload-p {
    display: inline-block;
  }

  .fileListOldStyle .content-upload-p {
    display: none;
    font-size: $font-size-sm;
    color: #666666;
    margin: 0;
    padding: 0;
    border: 0;
    line-height: 2;
  }
}
</style>
```

{% endfolding %}

{% title h2, 引入组件%}

### html

```html
<CheckImage
  title="编辑图标"
  :oldImageRow="oldImageRow"
  @closeImage="closeImage"
  @clearImages="clearImages"
  @finishChoose="finishChoose"
  :height="height"
  :width="width"
  v-if="checkImageState"
/>
```

### js

```javascript
let checkImageState = ref(false); //图片弹窗打开
let oldImageRow = ref(""); //被编辑图片数据
let height = ref(9);
let width = ref(16);

//打开组件
const changeImages = (val) => {
  //val是原有的图片地址，如果没有就为空
  oldImageRow.value = val;
  closeImage();
};
//完成选择
const finishChoose = (val) => {
  //val是组件返回的图片地址
  oldImageRow.value = val;
};
//变更打开/关闭状态
const closeImage = () => {
  checkImageState.value = !checkImageState.value;
};
//清空图片
const clearImages = () => {
  oldImageRow.value = "";
};
```
