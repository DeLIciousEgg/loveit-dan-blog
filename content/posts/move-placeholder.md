---
title: "讓 placeholder 動起來吧"
subtitle: ""
date: 2020-05-06T21:21:01+08:00
lastmod: 2020-05-06T21:21:01+08:00
draft: false
description: ""

tags: ['HTML', 'CSS']
categories: ['前端']

featuredImage: ""
featuredImagePreview: ""

toc:
  enable: true
  auto: true
---

![e.g.](https://imgur.com/wVGpA5g.gif) 

## 前言
網頁中常常會出現表單，註冊、登入、問卷等等，透過一些技巧讓`input`的`placeholder`「看起來」像是動起來吧
</br>

## 最常看到的就是一個`label`配上`input`
```html
<label for="Username">Username:</label>
<input type="text" id="Username">
```
</br>

## 有些輸入框會使用 `placeholder` 來顯示是什麼欄位
但這種方式在有輸入的情況下，會看不到 `placeholder` 的文字
```html
<input type="text" id="username" placeholder="Username">
```
</br>

## `placeholder` 動起來
```html
<div class="container">
  <input type="text" placeholder="Username">
  <label>Username</label>
</div>
```
```scss
.conatiner {
  position: relative;
}

.container > input {
  padding: 10px;
  border: 2px solid #ccc;
  border-radius: 3px;
  transition: .3s;
  outline: none;
  &::placeholder {
    color: transparent;
  }
  &:focus + label, &:not(:placeholder-shown) + label {
    background-color: #fff;
    transform: scale(0.75) translate(-15px, -20px);
  }
  &:focus {
    border-color: blue;
  } 
  &:focus + label{
    color: blue;
  }
}

.container > label {
  position: absolute;
  left: 20px; 
  top: 15px;
  color: #ccc;
  pointer-events: none;
  transition: .3s;
}
```

### 1. 首先需要把真的 `placeholder` 隱藏
```css
input::placeholder {
    color: transparent;
  }
```
### 2. 將 `label` (假的 `placeholder`) 使用絕對定位移到 `input` 裡
  - 使用 `pointer-events: none;` 讓點擊的時候不會點到 `label`
```css
label {
  position: absolute;
  left: 20px; 
  top: 15px;
  color: #ccc;
  pointer-events: none;
  transition: .3s;
}
```
### 3. `重點`：在`focus`以及輸入框有輸入文字的時候，移動`label`至定位
  - css 僞類`:placeholder-shown`爲佔位符顯示時，換句話說就是沒有輸入文字的情況。那要在有輸入文字的情況下就要加個`:not()`，變成`:not(:placeholder-shown)`
> IE 需要加前綴 `:-ms-input-placeholder`
```css
  input:focus + label, 
  input:not(:placeholder-shown) + label {
    background-color: #fff;
    transform: scale(0.75) translate(-15px, -20px);
  }
```