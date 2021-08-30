---
title: El-tree设置
date: 2021-08-24 11:08:33
tags:
  - Vue
categories:
  - web前端
  - Element组件
author: zhangyifeng
---

## El-tree 设置

### El-tree 横向显示

```html
<el-tree
  class="el-tree"
  ref="refElTree"
  :data="routeList"
  show-checkbox
  node-key="value"
  :props="defaultProps"
  @node-expand="handleExpand"
  :render-content="renderContent"
></el-tree>
```

data

```js
let routeList = ref([]); //树节点let defaultProps = ref({
  children: "children",
  label: "label",
}); //树节点
```

js

```js
//树节点的内容区的渲染 Function
const renderContent = (h, { node, data, store }) => {
  let classname = ""; // 由于项目中有三级菜单也有四级级菜单，就要在此做出判断
  if (node.level === 4) {
    classname = "foo";
  }
  if (node.level === 3 && node.childNodes.length === 0) {
    classname = "foo";
  }
  if (node.level === 2 && node.childNodes.length === 0) {
    classname = "foo";
  }
  return h(
    "p",
    {
      class: classname
    },
    node.label
  );
};
//节点被展开时触发的事件
const handleExpand = () => {
  //因为该函数执行在renderContent函数之前，所以得加this.$nextTick()
  proxy.$nextTick(() => {
    changeCss();
  });
};
//更改css
const changeCss = () => {
  let levelName = document.getElementsByClassName("foo"); // levelname是上面的最底层节点的名字
  for (let i = 0; i < levelName.length; i++) {
    // cssFloat 兼容 ie6-8  styleFloat 兼容ie9及标准浏览器
    levelName[i].parentNode.style.cssFloat = "left"; // 最底层的节点，包括多选框和名字都让他左浮动
    levelName[i].parentNode.style.styleFloat = "left";
    levelName[i].parentNode.onmouseover = function () {
      this.style.backgroundColor = "#fff";
    };
  }
};
```

### El-tree 选择的节点获取

```js
let Nodes = [
  ...refElTree.value.getHalfCheckedKeys(),
  ...refElTree.value.getCheckedKeys()
];
```

当父节点中的子节点未完全选择，父组件是半选择状态时，想要获取全部节点（包括半选择的父节点）使用此方法

#### 以下为引用

↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓
el-tree 组件在获取选择的节点时，默认的逻辑是，选中父节点时所有的子节点会被选中（checked），但是当该节点下不是选中所有子节点的时候，主节点不会被选中，而是处于一种半选中状态，提交时通过 getCheckedKeys() 方法也不会提交父节点，因为半选中状态下 checked 属性是 false 的。

解决办法
通常如果只是为了提交数据，我们可以使用 getHalfCheckedKeys()+getCheckedKeys()来获取需要的数据
const data=[
...this.$refs.tree.getHalfCheckedKeys(),
...this.$refs.tree.getCheckedKeys()
]

但是如果需要保存数据且需要编辑，那在编辑初始设置选中项的时候将遇到问题，所以我们只能使用 el-tree 现有的 api 来自己实现需要的功能，el-tree 中有一个属性 check-strictly（在显示复选框的情况下，是否严格的遵循父子不互相关联的做法，默认为 false）意思就是勾选父节点和勾选子节点，没有任何的关系。这是为了解决默认选中节点加载时，选中父节点会同时选中所以子节点，显然这不是我们想要的。
<el-tree
show-checkbox
node-key="id"
check-strictly
@check-change="checkChange"
ref="tree">
</el-tree>
然后使用 check-change 事件（节点选中状态发生变化时的回调）
共三个参数，依次为：传递给 data 属性的数组中该节点所对应的对象、节点本身是否被选中、节点的子树中是否有被选中的节点

```js
checkChange(data, check) {
    // 父节点操作
    if (data.parentId !== null) {
        if (check === true) {
            // 如果选中，设置父节点为选中
            this.$refs.tree.setChecked(data.parentId, true);
        }
        else {
            // 如果取消选中，检查父节点是否该取消选中（可能仍有子节点为选中状态）
            var parentNode = this.$refs.tree.getNode(data.parentId);
            var parentHasCheckedChild = false;
            for (
            var i = 0, parentChildLen = parentNode.childNodes.length;
            i < parentChildLen;
            i++
            ) {
                if (parentNode.childNodes[i].checked === true) {
                parentHasCheckedChild = true;
                break;
                }
            }
            if (!parentHasCheckedChild)
            this.$refs.tree.setChecked(data.parentId, false);
        }
    }
    // 子节点操作，如果取消选中，取消子节点选中
    if (data.children != null && check === false) {
        for (var j = 0, len = data.children.length; j < len; j++) {
        var childNode = this.$refs.tree.getNode(data.children[j].id);
            if (childNode.checked === true) {
            this.$refs.tree.setChecked(childNode.data.id, false);
            }
        }
    }
}
```

作者：段邵华
链接：https://www.jianshu.com/p/a482b25ac212
来源：简书
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑
