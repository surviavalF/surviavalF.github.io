---
date: 2022-08-01 13:35:19
title: hexo写文章创作规范
tags:
  - Hexo
categories:
  - Hexo
author: 糖醋灬里脊
top: true
---

{% title h2, 前言%}

本文仅是作者本人在文章时给自己设定的规范，是为了统一文章样式以及预防长时间鸽更新导致忘记如何写文章而设定的规范，此规范仅适用与作者个人。

{% title h2, 标题规范%}
示例写法：

```js
----{% title h1, 一级分区标题%}（仅大型文章时使用）

    ----{% title h2, 二级分区标题%}

        ----### 1.xxxxx （不需要分区时使用，使用时标明序号）
        ----{% title h3, 三级分区标题%}
```

{% title h2, 内容规范%}

### 1.特殊内容需使用 noteblock 标注

示例：
{% noteblock base, 标题（可选） %}
asdsd
{% endnoteblock %}

{% noteblock quote, 标题（可选） %}
asdsd
{% endnoteblock %}

{% noteblock warning, 标题（可选） %}
asdsd
{% endnoteblock %}

{% noteblock success, 标题（可选） %}
asdsd
{% endnoteblock %}

{% noteblock danger, 标题（可选） %}
asdsd
{% endnoteblock %}

{% noteblock info, 标题（可选） %}
asdsd
{% endnoteblock %}

上述示例代码：

```js
{% noteblock base, 标题（可选） %}
    asdsd
{% endnoteblock %}

{% noteblock quote, 标题（可选） %}
    asdsd
{% endnoteblock %}

{% noteblock warning, 标题（可选） %}
    asdsd
{% endnoteblock %}

{% noteblock success, 标题（可选） %}
    asdsd
{% endnoteblock %}

{% noteblock danger, 标题（可选） %}
    asdsd
{% endnoteblock %}

{% noteblock info, 标题（可选） %}
    asdsd
{% endnoteblock %}
```

### 2.涉及操作步骤的文章需使用 timeline 模仿操作步骤

示例：

{% timeline %}
{% timenode 第一段标题 %}
第一段内容
{% endtimenode %}
{% timenode 第二段标题 %}
第二段内容
{% endtimenode %}
{% endtimeline %}

上述示例代码：

```js
{% timeline %}
    {% timenode 第一段标题 %}
        第一段内容
    {% endtimenode %}
    {% timenode 第二段标题 %}
        第二段内容
    {% endtimenode %}
{% endtimeline %}
```

{% title h2, 提交规范%}
提交到 github 需要使用以下提交规范
{% noteblock base , 描述%} 1. 缺陷修复 2. 配置变动 3. 新增文章 4. 更新文章 6. 修改文章 6. 格式修复 7. 新增静态文件
{% endnoteblock %}
