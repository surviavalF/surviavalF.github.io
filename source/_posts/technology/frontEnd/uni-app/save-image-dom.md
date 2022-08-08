---
date: 2022-08-08 09:42:54
title: uni-app使用html2canvas-renderjs根据页面dom生成图片
tags:
  - uni-app
categories:
  - web前端
author: 糖醋灬里脊
img: "/medias/background/uni-app-background.png"
---

{% title h2, 原作者%}

插件市场 ：https://ext.dcloud.net.cn/plugin?id=2466
GitHub : https://github.com/zjcboy/html2canvas-renderjs

{% title h2, 使用%}

### 1.npm 引入 html2canvas 以及相关依赖

```javascript
  "html2canvas": "1.0.0-rc.5",
  "css-line-break": "1.1.1",
  "base64-arraybuffer": "0.2.0"
```

### 2.引入 image-tools.js 图片处理 js 文件

{% folding green, image-tools.js 代码 %}

```javascript
function getLocalFilePath(path) {
  if (
    path.indexOf("_www") === 0 ||
    path.indexOf("_doc") === 0 ||
    path.indexOf("_documents") === 0 ||
    path.indexOf("_downloads") === 0
  ) {
    return path;
  }
  if (path.indexOf("file://") === 0) {
    return path;
  }
  if (path.indexOf("/storage/emulated/0/") === 0) {
    return path;
  }
  if (path.indexOf("/") === 0) {
    var localFilePath = plus.io.convertAbsoluteFileSystem(path);
    if (localFilePath !== path) {
      return localFilePath;
    } else {
      path = path.substr(1);
    }
  }
  return "_www/" + path;
}

export function pathToBase64(path) {
  return new Promise(function (resolve, reject) {
    if (typeof window === "object" && "document" in window) {
      if (typeof FileReader === "function") {
        var xhr = new XMLHttpRequest();
        xhr.open("GET", path, true);
        xhr.responseType = "blob";
        xhr.onload = function () {
          if (this.status === 200) {
            let fileReader = new FileReader();
            fileReader.onload = function (e) {
              resolve(e.target.result);
            };
            fileReader.onerror = reject;
            fileReader.readAsDataURL(this.response);
          }
        };
        xhr.onerror = reject;
        xhr.send();
        return;
      }
      var canvas = document.createElement("canvas");
      var c2x = canvas.getContext("2d");
      var img = new Image();
      img.onload = function () {
        canvas.width = img.width;
        canvas.height = img.height;
        c2x.drawImage(img, 0, 0);
        resolve(canvas.toDataURL());
        canvas.height = canvas.width = 0;
      };
      img.onerror = reject;
      img.src = path;
      return;
    }
    if (typeof plus === "object") {
      plus.io.resolveLocalFileSystemURL(
        getLocalFilePath(path),
        function (entry) {
          entry.file(
            function (file) {
              var fileReader = new plus.io.FileReader();
              fileReader.onload = function (data) {
                resolve(data.target.result);
              };
              fileReader.onerror = function (error) {
                reject(error);
              };
              fileReader.readAsDataURL(file);
            },
            function (error) {
              reject(error);
            }
          );
        },
        function (error) {
          reject(error);
        }
      );
      return;
    }
    if (typeof wx === "object" && wx.canIUse("getFileSystemManager")) {
      wx.getFileSystemManager().readFile({
        filePath: path,
        encoding: "base64",
        success: function (res) {
          resolve("data:image/png;base64," + res.data);
        },
        fail: function (error) {
          reject(error);
        }
      });
      return;
    }
    reject(new Error("not support"));
  });
}

export function base64ToPath(base64, extName) {
  return new Promise(function (resolve, reject) {
    if (typeof window === "object" && "document" in window) {
      base64 = base64.split(",");
      var type = base64[0].match(/:(.*?);/)[1];
      var str = atob(base64[1]);
      var n = str.length;
      var array = new Uint8Array(n);
      while (n--) {
        array[n] = str.charCodeAt(n);
      }
      return resolve(
        (window.URL || window.webkitURL).createObjectURL(
          new Blob([array], { type: type })
        )
      );
    }
    var fileName;
    if (!extName) {
      extName = base64.match(/data\:\S+\/(\S+);/);
      if (extName) {
        extName = extName[1];
      } else {
        reject(new Error("base64 error"));
      }
      fileName = Date.now() + "." + extName;
    } else {
      fileName = Date.now() + extName;
    }
    if (typeof plus === "object") {
      var bitmap = new plus.nativeObj.Bitmap("bitmap" + Date.now());
      bitmap.loadBase64Data(
        base64,
        function () {
          var filePath = "_doc/uniapp_temp/" + fileName;
          bitmap.save(
            filePath,
            {},
            function () {
              bitmap.clear();
              resolve(filePath);
            },
            function (error) {
              bitmap.clear();
              reject(error);
            }
          );
        },
        function (error) {
          bitmap.clear();
          reject(error);
        }
      );
      return;
    }
    if (typeof wx === "object" && wx.canIUse("getFileSystemManager")) {
      var filePath = wx.env.USER_DATA_PATH + "/" + fileName;
      wx.getFileSystemManager().writeFile({
        filePath: filePath,
        data: base64.replace(/^data:\S+\/\S+;base64,/, ""),
        encoding: "base64",
        success: function () {
          resolve(filePath);
        },
        fail: function (error) {
          reject(error);
        }
      });
      return;
    }
    reject(new Error("not support"));
  });
}
```

{% endfolding %}

### 3.新建一个 html2canvas.vue 的组件

文件内有两个 script，如果不明白以及学习一下两个 script 的互相通信。并且我自行增加了 domId 为空的判断，如果为空就不生成图片。

```javascript
<template>
  <view>
    <view class="html2canvas" :prop="domId" :change:prop="html2canvas.create">
      <slot></slot>
    </view>
  </view>
</template>

<script>
import { base64ToPath } from '@/utils/image-tools.js'
export default {
  name: 'html2canvas',
  props: {
    domId: {
      type: String,
      required: true
    }
  },
  methods: {
    async renderFinish(base64) {
      try {
        const imgPath = await base64ToPath(base64, '.jpeg')
        this.$emit('renderFinish', imgPath)
      } catch (e) {
        //TODO handle the exception
        console.log('html2canvas error', e)
      }
    },
    showLoading() {
      uni.showToast({
        title: '正在生成海报',
        icon: 'none',
        mask: true,
        duration: 100000
      })
    },
    hideLoading() {
      uni.hideToast()
    }
  }
}
</script>

<script module="html2canvas" lang="renderjs">
import html2canvas from 'html2canvas';
export default {
	methods: {
		async create(domId) {
			if (domId !== '') {
				try {
					this.$ownerInstance.callMethod('showLoading', true);
					const timeout = setTimeout(async ()=> {
						const shareContent = document.querySelector(domId);
						const canvas = await html2canvas(shareContent,{
							width: shareContent.offsetWidth,//设置canvas尺寸与所截图尺寸相同，防止白边
							height: shareContent.offsetHeight,//防止白边
							logging: true,
							useCORS: true
						});
						const base64 = canvas.toDataURL('image/jpeg', 1);
						this.$ownerInstance.callMethod('renderFinish', base64);
						this.$ownerInstance.callMethod('hideLoading', true);
						clearTimeout(timeout);
					}, 500);
				} catch(error){
					console.log(error)
				}
			}
		}
	}
}
</script>


<style lang="scss">
</style>


```

### 4.在所需要的文件内使用

因为该组件是由 domId 的值变化而触发的所以想要多次触发保存同一个 dom 内的东西需要每次生成成功后清空 domId，也需要在上面的逻辑里判断 domId 是否为空

{% noteblock warning, 保存dom节点内有二维码注意事项 %}
如果你的 dom 节点内有二维码需要注意，有一些二维码插件也是用 canvas 生成的会跟本文介绍的插件产生冲突，解决方案是将二维码生成后保存成一个图片放在 dom 节点内。有些二维码生成后会有一个临时 base64 图片的本地地址，可以用上文中的 image-tools.js 中的 pathToBase64() 方法进行转换
{% endnoteblock %}

```javascript

<template>
  <view class="content">
    <!-- html -->
    <html2canvas ref="html2canvas" :domId="domId" @renderFinish="renderFinish">
      <view id="detailMiddle"> 这dom里放你想要的东西 </view>
    </html2canvas>
    <view @click="createImg()">生成图片</view>
  </view>
</template>

<script>
export default {
  data() {
    return {
      domId: "",
    };
  },
  methods: {
    //点击保存按钮
    createImg() {
      this.domId = "#detailMiddle";
    },
    /**
     * 渲染完毕接收图片
     * @param {String} filePath
     */
    renderFinish(filePath) {
      this.domId = ""; //因为该组件是由domId的值变化而触发的，所以清空domId可以多次生成图片
      this.filePath = filePath;
      console.log("filePath", filePath);
      uni.saveImageToPhotosAlbum({
        filePath: filePath,
        success: function () {
          uni.showToast({ icon: "success", title: "保存成功", duration: 1500 });
        },
      });
    },
  },
};
</script>

<style>
.content {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  text-align: center;
}
</style>

```
