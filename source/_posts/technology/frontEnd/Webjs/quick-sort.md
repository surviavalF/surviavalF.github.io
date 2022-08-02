---
date: 2022-08-01 10:49:19
title: 快排
tags:
  - Vue
categories:
  - web前端
  - js
author: 糖醋灬里脊
img: "/medias/background/js-background.png"
top: false
---

{% title h2, 快速排序%}

```javascript
    sortArray(nums) {
      this.quickSort(0, nums.length - 1, nums)
      return nums
    },
    quickSort(start, end, arr) {
      let self = this
      if (start < end) {
        console.log("this.numbs",this.numbs);
        console.log("this.arrss",arr);
        const mid = self.sort(start, end, arr)
        console.log("mid",mid);
        self.quickSort(start, mid - 1, arr)
        self.quickSort(mid + 1, end, arr)
      }
    },
    sort(start, end, arr) {
      const base = arr[start]
      let left = start
      let right = end
      while (left !== right) {
        while (arr[right] >= base && right > left) {
          right--
        }
        arr[left] = arr[right]
        while (arr[left] <= base && right > left) {
          left++
        }
        arr[right] = arr[left]
      }
      arr[left] = base
      return left
    },

```
