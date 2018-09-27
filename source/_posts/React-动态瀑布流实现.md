---
title: React 动态瀑布流布局实现
date: 2018-09-20 13:44:30
categories:
  - 前端
tags:
  - React
---

# 前言

> 瀑布流布局是一种常见的前端布局方式，这种布局适合于小数据块，每个数据块内容相近且没有侧重。通常，随着页面滚动条向下滚动，这种布局还会不断加载数据块并附加至当前尾部。本文将使用[react-stack-grid](https://tsuyoshiwada.github.io/react-stack-grid/#/)实现自适应的动态瀑布流布局，并实现无限下拉加载。


{% qnimg pinterest-masonry.png alt:pinterest瀑布流示意%}


<!-- more -->

# 瀑布流实现方式

虽然瀑布流可以用CSS方式实现，但是最优的实现方式是通过绝对定位方式，这种方式通过js计算列数/数据块的宽和高度。可以方便的增加数据块；同时在窗口变化时，也会自适应的进行调整的。

可以实现瀑布流的react控件主要有：

1. [react-grid-layout](https://github.com/STRML/react-grid-layout) 

- 通过自行计算每个数据块的位置。

2. [react-stack-grid](https://tsuyoshiwada.github.io/react-stack-grid/#/)

- 自动计算数据块的位置。
- 支持在数据块里图片的动态加载，完成图片加载之后，数据块的高度会发生改变，控件自动重新排列

> 这两个控件都是通过绝对定位的方式对数据块的位置进行排列的。

由于 react-grid-layout 计算起来较为繁琐，如无特殊定制的需要，不推荐使用。本文将介绍如何使用 [react-stack-grid](https://tsuyoshiwada.github.io/react-stack-grid/#/) 实现可动态加载图片的数据流布局。还将说明如何使用 [react-infinite-scroller](https://github.com/CassetteRocks/react-infinite-scroller) 实现无限加载。


# 示例实现