---
title: Vue.js-vue.js中使用setInterval()、clearInterval()、clearLnterval()、setTimeout()
date: 2017-10-16 11:18:52
tags: [vue.js, timer, setInterval, clearInterval, clearLnterval, setTimeout]
---
前些日子在做FreeCodeCamp的題目「Build a Pomodoro Clock」需要做計數功能，但是大部分使用setTimeout的範例都是JQuery版本，因此在此做個紀錄。

| 方法 | 描述 |
| -------- | -------- |
| setInterval | 定期去調用function或者執行一段程式。 |
| clearInterval | 取消掉setInterval所重複執行的動作。 |
| setTimeout | 在指定的延遲時間之後調用一個函數或者執行一段程式。 |
| clearTimeout | 可取消由 setTimeout() 設置的 timeout。 |

## Example
### clearInterval()
```js
var myVar = setInterval(() => {
  myTimer()
}, 1000)

function myTimer() {
  var d = new Date()
  var t = d.toLocaleTimeString()
  console.log(t)
}

function myStopFunction() {
  clearInterval(myVar)
}
```

### clearTimeout()
```js
var myVar

function myFunction() {
  myVar = setTimeout(() => {
    console.log('Hello')
  }, 3000)
}

function myStopFunction() {
  clearTimeout(myVar)
}
```

## 用vue實做
```js
let vm = new Vue({
  el: '#app',
  data: {
    time: null,
  },
  methods: {
    timer() {
      let my = this
      this.time = setInterval(() => {
        // do something...
      }, 1000)
    },
    setTime() {
      this.timer()
    },
    stopTime() {
      if (this.time) {
        clearInterval(this.time)
      }
    }
  }
})
```

## Demo
![Build a Pomodoro Clock](https://imgur.com/y7Xyuuo.png)
[Codepen: Build a Pomodoro Clock](https://codepen.io/jd615645/full/VMNzaM/)

## 參考資料
* [w3c school: clearInterval() Method](https://www.w3schools.com/jsref/met_win_clearinterval.asp)
* [w3c school: setInterval() Method](https://www.w3schools.com/jsref/met_win_setinterval.asp)
* [w3c school: setTimeout() Method](https://www.w3schools.com/jsref/met_win_settimeout.asp)
* [w3c school: clearTimeout() Method](https://www.w3schools.com/jsref/met_win_cleartimeout.asp)