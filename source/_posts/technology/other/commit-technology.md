---
date: 2022-08-01 10:49:19
title: commit规范
tags:
  - Git
categories:
  - Git
author: 糖醋灬里脊
img: "/medias/background/10.jpg"
top: false
---

{% title h2, commit 基本规范%}

commit 信息包括三个字段: type (必需)， scope(可选) 和 subject(必需)。

### 1.type

type 是用于说明该 commit 的类型的，一般我们会规定 type 的类型如下：

```js
• feat: 新功能(feature)
• fix: 修复 bug
• docs: 文档(documents)
• style: 代码格式(不影响代码运行的格式变动，注意不是指 CSS 的修改)
• refactor: 重构(既不是新增功能，也不是修改 bug 的代码变动)
• test: 提交测试代码(单元测试，集成测试等)
• chore: 构建或辅助工具的变动
• misc: 一些未归类或不知道将它归类到什么方面的提交
```

### 2.scope

scope 说明 commit 影响的范围，比如数据层，控制层，视图层等等，这个需要视具体场景与项目的不

### 3.subject

subject 是对于该 commit 目的的简短描述
○ 使用第一人称现在时的动词开头，比如 modify 而不是 modified 或 modifies
○ 首字母小写，并且结尾不加句号

### 4.ISSUEE_ID

这个与公司的需求管理与项目管理有关，假设你的项目放在 github 上，你的需求或者 bug 修复可能会有对应的 issues 记录，你可以加到你的 commit 信息中如 issue-37938634。

{% title h2, 中文 type%}

考虑到开发人员母语描述更加精准，有些公司会使用中文进行 commit
{% noteblock base , 描述%} 1. 缺陷修复 2. 功能迭代 3. 功能重构 4. 配置变动 5. 格式修复 6. 单元测试
{% endnoteblock %}
