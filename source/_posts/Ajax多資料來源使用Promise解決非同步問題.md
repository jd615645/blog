---
title: Ajax多資料來源使用Promise解決非同步問題
date: 2017-10-16 11:18:52
tags: 
- JavaScript
- Promise
- Ajax
categories:
- JavaScript
---
偶爾會處理到需要Ajax不同來源的資料，這時就會需要用promise來解決非同步問題，同時也可以擺脫callback波動拳。

```js
var ary = []
var url = ['example1.com', 'example2.com', 'example3.com']

$.each(url, (key, val) => {
  ary.push($.getJSON(url))
});

$.when
  .apply($, ary)
  .then((...inputData) => {
    $.each(inputData, (key, val) => {
      // val[0] => url[0] data
      // val[1] => url[1] data
    })
  })
```

