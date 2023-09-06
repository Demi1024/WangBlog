---
title: 【每日一题】字符串的处理
date: 2020-08-06 22:20:55
tags: 
  - JavaScript
categories:
  - 面试题
---

## 题目一

完成字符串相加,使得111与222相加的结果为2333

```js
"111" + "2222" = "2333"
```

## 题目二

对字符串进行处理，实现repeat方法，输出结果'abcabcbac'

'abc'.repeat(3) 结果 'abcabcbac'

```js
// 方法一：
function repeat(val,num){
  let newVal = val
  for(let i = 0; i<num-1;i++){
    newVal = newVal + val
  }
  return newVal
}
console.log(repeat('abc',3))

// 方法二
new Array(3).fill('abc').join('')
```

## 题目三

给定字符串'abbaccd'，采用相邻字母相同删除的原则，使得最后输出=> 'd'

```js
let str = 'abbaccd'
let Len = str.length
let arr = []
for(let i =0; i<Len;i++){
  if(arr.length > 0){
    let last = arr.pop()
    if(last !== str[i]){
      arr.push(last)
      arr.push(str[i])
    }
  } else {
    arr.push(str[i])
  }
}
console.log(arr.join(''))
```

采用栈处理，先进后出的思想，对比相邻数据是否一致
