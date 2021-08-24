---
title: 递归
tags:
  - Vue
categories:
  - web前端
  - js
author: zhangyifeng
date: 2021-08-24 10:27:50
---

## vue 递归代码

```javascript
    caltaskScorePeopleData(item) {
        var that = this;
        let len = this.score.length - 1;
        let index = 0;
        setData();
        function setData() {
            let params = {
            taUuid: that.score[index].taUuid,
            evaluationScore: that.score[index].evaluationScore,
            userUuid: that.score[index].userUuid,
            eventId: that.score[index].eventId,
            };
            personnelAssessmentScore(params).then((res) => {
                index++;
                if (index > len) {

                    that.caltaskScorePeople2(item);

                } else {
                    setData();
                }
            });
        }
    },
```
