---
title: Promise
date: 2017-11-20 12:47:21
category: 库学习
tags: Promise
---
思想
---
promise代表异步操作的结果，有三种状态：
* pending - 初始化状态
* fulfilled - 代表一个成功操作
* rejected - 代表一个失败操作

promise 变成fulfilled/rejected状态，状态就不会再改变
```nodejs
var Promise = require('promise');
var fs = require('fs');

function readFile(filename) {
    return new Promise(function (fulfill, reject) {
        fs.readFile(filename, function (err, res) {
            if(err) reject(err);
            fulfill(res);
        })
    });
};
```
promise.done 等待异步处理是成功还是失败
```nodejs
readFile('node.txt').done(function (res) {
    console.log(res.toString());
}, function (err) {
    console.log(err);
});
```

Promise转换和链式
---
promise.then可以对结果进行处理，返回新的promise
```nodejs
function readJSON(filename){
    return readFile(filename).then(function (res){
        return JSON.parse(res)
    })
}

readJSON('node.txt').then(function (json) {
    console.log(json);
});
```