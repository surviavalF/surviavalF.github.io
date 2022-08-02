---
date: 2022-08-02 09:42:54
title: 简易封面生成器
tags:
  - CSS
categories:
  - web前端
author: 糖醋灬里脊
img: "/medias/background/css-background.png"
---

{% title h2, 参数输入%}

  <table width="100%" border="0" cellspacing="0" cellpadding="0" class="content">
    <tr>
      <td valign="top" class="area">
        <table
          style="border-collapse: collapse"
          bordercolor="#d9d9d9"
          cellspacing="0"
          cellpadding="8"
          width="100%"
          bgcolor="#fcfcfc"
          border="1"
        >
          <tbody>
            <tr>
              <td>
                <div style="display:flex;justify-content: space-between;">
                  <div>基础参数传入 </div>
                  <div>
                  <button onclick="showImg()" type="button">刷新</button>
                  <button onclick="downLoadImage()" type="button">保存</button>
                  </div>
                </div>
                <table border="0" cellpadding="0" cellspacing="4">
                  <tbody>
                    <tr>
                      <td width="240">
                        图片宽：<input
                          name="imgWidth"
                          value="1280"
                          onKeyDown="getkey(event,'imgWidth');"
                        />
                        图片高：<input
                          name="imgHeight"
                          value="720"
                          onKeyDown="getkey(event,'imgHeight');"
                        />
                        图片背景色：<input
                          name="imgBackground"
                          value="#64b5ff"
                          onKeyDown="getkey(event,'imgBackground');"
                        />
                      </td>
                    </tr>
                    <tr>
                    <td width="180">
                        文字内容：<input
                          name="textData"
                          value="JS"
                          onKeyDown="getkey(event,'textData');"
                        />
                        文字颜色：<input
                          name="textColor"
                          value="#3897f9"
                          onKeyDown="getkey(event,'textColor');"
                        />
                        文字大小：<input
                          name="textFont"
                          value="300"
                          onKeyDown="getkey(event,'textFont');"
                        />
                        文字距左侧距离：<input
                          name="textLeft"
                          value="490"
                          onKeyDown="getkey(event,'textLeft');"
                        />
                        文字距顶部距离：<input
                          name="textTop"
                          value="350"
                          onKeyDown="getkey(event,'textTop');"
                        />
                      </td>
                    </tr>
                    <tr>
                      <td width="180">
                        阴影颜色：<input
                          name="shadowColor"
                          value="#383dff"
                          onKeyDown="getkey(event,'shadowColor');"
                        />
                        阴影偏移量：<input
                          name="shadowLeft"
                          value="15"
                          onKeyDown="getkey(event,'shadowLeft');"
                        />
                      </td>
                    </tr>
                    <tr>
                      <td width="180">
                        导出文件名：<input
                          name="imgName"
                          value=""
                          onKeyDown="getkey(event,'imgName');"
                        />
                      </td>
                    </tr>
                  </tbody>
                </table>
              </td>
            </tr>
          </tbody>
        </table>
      </td>
    </tr>
  </table>
  <script type="text/javascript" src="/js/html2canvas.js"></script>
  <script>
  function showImg() {
    changeImage("all");
  }
  function changeImage(val) {
    var timer = setTimeout(() => {
      // 图片宽：
      if (val == "imgWidth" || val == "all") {
        document.getElementsByClassName("canvasImg")[0].style.width = `${
          document.getElementsByName("imgWidth")[0].value
        }px`;
        document.getElementsByClassName("centerVerticalLine")[0].style.left = `${
          Number(document.getElementsByName("imgWidth")[0].value / 2)
        }px`;
        document.getElementsByClassName("centerAcrossLine")[0].style.width = `${
         document.getElementsByName("imgWidth")[0].value
        }px`;
      }
      // 图片高：
      if (val == "imgHeight" || val == "all") {
        document.getElementsByClassName("canvasImg")[0].style.height = `${
          document.getElementsByName("imgHeight")[0].value
        }px`;
        document.getElementsByClassName("centerVerticalLine")[0].style.height = `${
           Number(document.getElementsByName("imgHeight")[0].value)
        }px`;
        document.getElementsByClassName("centerAcrossLine")[0].style.top = `${
          Number(document.getElementsByName("imgHeight")[0].value / 2)
        }px`;
      }
      // 图片背景色：
      if (val == "imgBackground" || val == "all") {
        document.getElementsByClassName("canvasImg")[0].style.background = `${
          document.getElementsByName("imgBackground")[0].value
        }`;
      }
      // 文字内容：
      if (val == "textData" || val == "all") {
        inputText1.innerHTML = `${
          document.getElementsByName("textData")[0].value
        }`;
        inputText2.innerHTML = `${
          document.getElementsByName("textData")[0].value
        }`;
      }
      // 文字颜色：
      if (val == "textColor" || val == "all") {
        document.getElementsByClassName("inputTextClass1")[0].style.color = `${
          document.getElementsByName("textColor")[0].value
        }`;
      }
      // 文字大小：
      if (val == "textFont" || val == "all") {
        document.getElementsByClassName("inputTextClass1")[0].style.fontSize = `${
          document.getElementsByName("textFont")[0].value
        }px`;
        document.getElementsByClassName("inputTextClass2")[0].style.fontSize = `${
          document.getElementsByName("textFont")[0].value
        }px`;
      }
      // 文字距左侧距离：
      if (val == "textLeft" || val == "all") {
        document.getElementsByClassName("inputTextClass1")[0].style.left = `${
          document.getElementsByName("textLeft")[0].value
        }px`;
        document.getElementsByClassName("inputTextClass2")[0].style.left = `${
          Number(document.getElementsByName("textLeft")[0].value) +
          Number(document.getElementsByName("shadowLeft")[0].value)
        }px`;
      }
      // 文字距顶部距离：
      if (val == "textTop" || val == "all") {
        document.getElementsByClassName("inputTextClass1")[0].style.top = `${
          document.getElementsByName("textTop")[0].value
        }px`;
        document.getElementsByClassName("inputTextClass2")[0].style.top = `${
          document.getElementsByName("textTop")[0].value
        }px`;
      }
      // 阴影颜色：
      if (val == "shadowColor" || val == "all") {
        document.getElementsByClassName("inputTextClass2")[0].style.color = `${
          document.getElementsByName("shadowColor")[0].value
        }`;
      }
      // 阴影偏移量：
      if (val == "shadowLeft" || val == "all") {
        document.getElementsByClassName("inputTextClass2")[0].style.left = `${
          Number(document.getElementsByName("textLeft")[0].value) +
          Number(document.getElementsByName("shadowLeft")[0].value)
        }px`;
      }
      clearTimeout(timer);
    }, 100);
  }
  function getkey(e, n) {
    changeImage(n);
  }
  function downLoadImage(){
    document.getElementsByClassName("centerVerticalLine")[0].style.display = 'none'
    document.getElementsByClassName("centerAcrossLine")[0].style.display = 'none'
    var timer2 = setTimeout(() => {
      html2canvas(document.querySelector("#myImg")).then(canvas => {
          var imgName = new Date().toISOString() + '.png'
          if(document.getElementsByName("imgName")[0].value){
            imgName = document.getElementsByName("imgName")[0].value+'.png'
          }
          var a = document.createElement("a");
          a.href = canvas.toDataURL("image/png");    //canvas转base64图片
          a.download = imgName;
          a.click();
          document.getElementsByClassName("centerVerticalLine")[0].style.display = 'unset'
          document.getElementsByClassName("centerAcrossLine")[0].style.display = 'unset'
      });
      clearTimeout(timer2);
    }, 100);
  }
  $(document).ready(function() { 
	   changeImage("all");
  })
</script>

{% title h2, 图片%}

  <div style="overflow: auto;">
    <div
      id='myImg'
      class="canvasImg"
    >
      <div class='centerVerticalLine'></div>
      <div class='centerAcrossLine'></div>
      <div
        id="inputText1" class='inputTextClass1'
      >
      </div>
      <div
        id="inputText2" class='inputTextClass2'
      >
      </div>
    </div>
  </div>

  <style>
    .canvasImg{
      position: relative;
      font-family: SimHei;
    }
    .centerVerticalLine{
      position: absolute;
      z-index: 10;
      border-right: 1px dashed #eeeeee;
    }
    .centerAcrossLine{
      position: absolute;
      z-index: 10;
      border-top: 1px dashed #eeeeee;
    }
    .inputTextClass1{
      position: absolute;
      z-index: 9;
    }
    .inputTextClass2{
      position: absolute;
      z-index: 8;
    }
    input{
      width:100px;
    }
    button{
      width:100px;
      cursor:pointer;
    }
  </style>
