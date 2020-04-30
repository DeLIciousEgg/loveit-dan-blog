---
title: "吃得到看不到的網頁捲軸"
subtitle: ""
date: 2020-04-30T15:11:30+08:00
lastmod: 2020-04-30T15:11:30+08:00
draft: false
description: ""

tags: ['HTML', 'CSS']
categories: ['前端']

featuredImage: ""
featuredImagePreview: ""
---

## 前言
當需要在網頁中某個區塊，使用無限滾動 InfiniteScroll 載入資料時，但又不希望在畫面上看到捲軸，以下記錄一個使用 CSS 的解決方法。

## 解決方法
```html
<div class="container">
  <ul class="list">
    <li>1</li>
    <li>2</li>
    <li>3</li>
    <!-- more <li> ... -->
  </ul>
</div>
```

```css {hl_lines=[5,15]}
.container {
  position: relative;
  width: 200px;
  height: 200px; /* 區塊所需高度 */
  overflow: hidden;
}

.list {
  overflow-x: hidden;
  overflow-y: scroll;
  position: absolute;
  top: 0;
  bottom: 0;
  left: 0;
  right: -17px;
}
```

透過絕對定位 `position: absolute;` 將 `list` 整個往右移 `17px`，捲軸就會超出 `container` 的範圍，再使用`overflow: hidden;` 將超出範圍的部分隱藏，就達到看不到捲軸，卻可以滾動的效果。