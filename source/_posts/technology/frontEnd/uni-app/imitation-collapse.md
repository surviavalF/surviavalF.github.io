---
date: 2022-08-10 17:38:54
title: 模仿 u-collapse 折叠面板
tags:
  - uni-app
categories:
  - web前端
author: 糖醋灬里脊
img: "/medias/background/uni-app-background.png"
---

{% title h2, 前言%}

为什不用 u-view 自带的 u-Collapse? 有几个条件：1.页面需要快速的响应接口不能放在 onReady() 里请求数据。2.页面有下拉刷新可以刷新数据。
这就导致了我不能在调用完接口后直接使用 u-Collapse 的 this.$refs.collapseView.init(); 只能写在 setTimeout 里来延时刷新。而页面又有下拉刷新，就会导致不停下拉刷会产生无数个 setTimeout 页面卡死，只能自己写一个类似的组件。

{% title h2, 使用%}

### HTML 代码

```html
<template>
  <view class="page qr-wrap">
    <u-navbar is-back title="模仿 Collapse 折叠面板" />
    <view class="grid-container">
      <view
        v-for="(collapseItem, collapseIndex) in kitList"
        :key="'kits_' + collapseItem.appId"
        class="onceCollapse"
        :id="'kits_' + collapseIndex"
        :style="{ 'max-height': openMyAppHeight(collapseItem, collapseIndex) }"
      >
        <view
          class="collapseTitle"
          @click="collapseItemChange(collapseIndex, collapseIndex)"
        >
          <view class="collapseTitle_left">
            <image class="image" :src="collapseItem.appIcon"></image>
          </view>
          <view class="collapseTitle_mid">
            <view class="collapseTitle_mid_title u-line-2"
              >{{ collapseItem.appName }}
            </view>
          </view>
          <view class="collapseTitle_right">
            <view class="showTag">
              <view v-if="!!collapseItem.show">
                收起<u-icon name="arrow-up" size="20" color="#ffffff"></u-icon>
              </view>
              <view v-else>
                展开<u-icon name="arrow-down" size="20" color="#ffffff"></u-icon
              ></view>
            </view>
          </view>
        </view>
        <view v-if="!!collapseItem.show" class="bordertop"></view>
        <u-empty
          text="暂无套件菜单"
          mode="list"
          v-if="
            !collapseItem.menuTypeList || collapseItem.menuTypeList.length == 0
          "
        ></u-empty>
        <view
          class="grid-container_allCard"
          v-for="(allItem, allIndex) in collapseItem.menuTypeList"
          :key="'kits_' + collapseItem.appId + '_' + allIndex"
        >
          <view v-if="allItem.menuList.length > 0" class="card-box">
            <view class="card-box_title"> {{ allItem.appMenuTypeName }} </view>
            <view class="card-box_allData">
              <view
                class="onceData"
                v-for="(subItem, subIndex) in allItem.menuList"
                :key="
                  'kits_' + collapseItem.appId + '_' + allIndex + '_' + subIndex
                "
              >
                <view class="onceData_top">
                  <image class="image" :src="subItem.applicationIcon"></image>
                </view>
                <view class="onceData_down u-line-1">
                  {{ subItem.applicationName }}
                </view>
              </view>
            </view>
          </view>
        </view>
      </view>
    </view>
  </view>
</template>
```

### JS 代码

```javascript
<script>
import dataList from "./data.js";
export default {
  data() {
    return {
      kitList: [],
      kitLoading: true
    };
  },
  components: {},
  onLoad(option) {
    this.getQueryCheckKitList();
  },

  methods: {
    //动态计算高度
    openMyAppHeight(item, index) {
      let title = 140;
      let minTitleNumber = item.menuTypeList.length;
      let menuNumber = 0;
      item.menuTypeList.forEach((items) => {
        menuNumber = Math.ceil(items.menuList.length / 4) + menuNumber;
      });
      let defaultLen = item.show
        ? menuNumber * 214 + minTitleNumber * 80 + title
        : title;
      return defaultLen + "rpx";
    },
    //拉取已经配置的套件应用
    getQueryCheckKitList() {
      this.kitList = dataList;
      this.kitLoading = false;
    },
    collapseItemChange(index) {
      const self = this;
      if (self.kitList[index].show) {
        self.kitList[index].show = !self.kitList[index].show;
      } else {
        self.kitList[index].show = true;
      }
      self.$forceUpdate();
    }
  }
};
</script>
```

### CSS 代码

```css
<style lang="scss" scoped>
.qr-wrap {
  width: 100%;
  background: #f6f6f6;
  .grid-container {
    background-color: #f6f6f6;
    padding: 30rpx;
    .onceCollapse {
      overflow: hidden;
      transition: max-height 0.2s linear;
      margin-bottom: 30rpx;
      background: #ffffff;
      border-radius: 24rpx;
      box-shadow: 0rpx 20rpx 32rpx 0rpx rgba(0, 0, 0, 0.04);

      .collapseTitle {
        padding: 20rpx 30rpx;
        display: flex;
        align-items: center;
        width: 100%;
        &_left {
          .image {
            width: 100rpx;
            height: 100rpx;
            text-align: center;
            border-radius: 28rpx;
            aspect-ratio: auto 138 / 180;
            display: block;
          }
        }
        &_mid {
          flex: 1;
          width: 0;
          margin: 0 30rpx;
          padding: 6rpx 0;
          &_title {
            line-height: 36rpx;
            font-size: 32rpx;
            font-weight: bold;
            letter-spacing: 1rpx;
            color: #444;
          }
        }
        &_right {
          background: #ff6a00;
          color: #fff;
          border-radius: 50rpx;
          font-size: 24rpx;
          .showTag {
            padding: 6rpx 10rpx;
            /deep/.u-icon__icon {
              margin-left: 6rpx;
            }
          }
        }
      }
      .bordertop {
        margin: 0 30rpx;
        border-top: 1rpx solid #ddd;
      }
      .grid-container_allCard {
        border-radius: 24rpx;
        .card-box {
          border-radius: 24rpx;
          background: #ffffff;
          &_title {
            color: #222222;
            font-size: 32rpx;
            font-weight: bold;
            padding: 35rpx 35rpx 0 35rpx;
          }
          &_allData {
            border-radius: 24rpx;
            display: grid;
            grid-template-columns: repeat(4, 1fr);
            grid-row-gap: 10rpx;
            grid-column-gap: 10rpx;
            .onceData {
              width: 100%;
              margin: 35rpx 0;
              @include flex-box(column, space-between);
              border-radius: 24rpx;
              background: #fff;
              &_top {
                .image {
                  width: 100rpx;
                  height: 100rpx;
                  text-align: center;
                  border-radius: 28rpx;
                  aspect-ratio: auto 138 / 180;
                }
              }
              &_down {
                text-align: center;
                width: 100%;
                font-size: 24rpx;
                color: #808080;
              }
            }
          }
        }
        & ~ .allCard {
          margin-top: 20rpx;
        }
      }
    }
    .u-empty {
      margin: 20rpx 0 !important;
    }
  }
}
</style>
```

### dataList 数据

```javascript
var dataList = [
  {
    appIcon: "/static/image/1-1Img.jpg",
    appName: "标题1",
    expirationTime: "2022-08-05 23:59:59",
    appDesc: null,
    appId: "1",
    menuTypeList: [
      {
        appMenuTypeName: "副标题1-1",
        menuList: [
          {
            applicationIcon: "/static/image/1-1Img.jpg",
            applicationId: "111",
            applicationName: "1-1-1"
          }
        ]
      },
      {
        appMenuTypeName: "副标题1-2",
        menuList: [
          {
            applicationIcon: "/static/image/1-1Img.jpg",
            applicationId: "121",
            applicationName: "1-2-1"
          },
          {
            applicationIcon: "/static/image/1-1Img.jpg",
            applicationId: "122",
            applicationName: "1-2-2"
          }
        ]
      }
    ]
  },
  {
    appIcon: "/static/image/1-1Img.jpg",
    appName: "标题2",
    expirationTime: "2022-08-05 23:59:59",
    appDesc: null,
    appId: "2",
    menuTypeList: [
      {
        appMenuTypeName: "副标题2-1",
        menuList: [
          {
            applicationIcon: "/static/image/1-1Img.jpg",
            applicationId: "211",
            applicationName: "2-1-1"
          }
        ]
      },
      {
        appMenuTypeName: "副标题2",
        menuList: [
          {
            applicationIcon: "/static/image/1-1Img.jpg",
            applicationId: "221",
            applicationName: "2-2-1"
          },
          {
            applicationIcon: "/static/image/1-1Img.jpg",
            applicationId: "222",
            applicationName: "2-2-2"
          }
        ]
      }
    ]
  }
];

export default dataList;
```
