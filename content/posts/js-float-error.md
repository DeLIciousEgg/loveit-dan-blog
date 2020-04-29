---
title: "JavaScript 浮點數四則運算誤差"
subtitle: ""
date: 2020-04-29T23:23:39+08:00
lastmod: 2020-04-29T23:23:39+08:00
draft: false
description: ""

tags: ['JS']
categories: ['前端']

featuredImage: ""
featuredImagePreview: ""
---



## 0.1 + 0.2 = ?

打開 Google Chrome 開發人員工具，在 console 輸入 `0.1 + 0.2`，得到的答案不是 `0.3`，竟然是`0.30000000000000004`。

主要原因爲 JS 依循 IEEE 754 規範，運算時會將數值轉爲二進制進行計算，因此產生這樣的誤差。

## 解決方法

記錄一下簡易的解決方法，簡單說就是把浮點數轉爲整數再進行運算，最後再轉回浮點數。(以下方法不適用科學記號 `2.3e+1`，如需要處理這種情況可以參考 [number-precision](https://github.com/nefe/number-precision))

```js
/**
 * 取得小數位數
 */
function digitLength (number) {
  let digitNumber = number.toString().split('.')[1]
  return digitNumber ? digitNumber.length : 0
}

/**
 * 去除小數點
 */
function number2Int (number) {
  return +number.toString().replace('.', '')
}

/**
 * 乘法 (num1 * num2)
 */
function times (num1, num2) {
  const int1 = number2Int(num1)
  const int2 = number2Int(num2)
  return (int1 * int2) / Math.pow(10, digitLength(num1) + digitLength(num2))
}

/**
 * 加法 (num1 + num2)
 */
function plus (num1, num2) {
  const maxDigit = Math.pow(10, Math.max(digitLength(num1), digitLength(num2)))
  return (times(num1, maxDigit) + times(num2, maxDigit)) / maxDigit
}

/**
 * 減法 (num1 - num2)
 */
function minus (num1, num2) {
  const maxDigit = Math.pow(10, Math.max(digitLength(num1), digitLength(num2)))
  return (times(num1, maxDigit) - times(num2, maxDigit)) / maxDigit
}

/**
 * 除法 (num1 / num2)
 */
function divide (num1, num2) {
  const int1 = number2Int(num1)
  const int2 = number2Int(num2)
  return (int1 / int2) * Math.pow(10, digitLength(num2) - digitLength(num1))
}
```

## 同場加映 - 無條件捨去小數方法
當初因爲要將小數無條件捨去取第二位，因此踩到 JS 這個雷，配合上面方法順便記錄一下

```js
function roundDown (number, digit) {
  const MULTIPLE = Math.pow(10, digit)
  return divide(Math.floor(times(number, MULTIPLE)), MULTIPLE)
}
```

