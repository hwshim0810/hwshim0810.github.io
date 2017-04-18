---
layout: post
title: Call-back hell이란?
description: "Call-back hell"
tags: [Javascript, Async, Node.js]
---
# Call-back hell의 개념
- Javascript를 이용한 비동기 프로그래밍시 발생하는 문제
- 함수들을 순차적으로 실행하고 할 때 Call-back함수들의 중첩이 생겨 코드가 복잡해지는 문제가 발생할 수 있다

```javascript
asyncfunction(params,function(){
    asyncfunction(params,function(){
        asyncfunction(params,function(){
            asyncfunction(params,function(){
                asyncfunction(params,function(){
                    asyncfunction(params,function(){
                    });
                });
            });
        });
    });
});         
```

## 실제 코드를 통한 예제

```javascript
var fs = require('fs');
var src = '/tmp/myfile.txt';
var des = '/tmp/myfile_async.txt';

fs.readFile(src,'utf-8',function(err,data) {
    console.log(data);
    if (err) {
        console.log("Read file error");
    } else {
        console.log("Read file is done");
        fs.writeFile(des,data,function(err) {
            if (err) {
                console.log("Write file error");
                return;
            }
            console.log("Write file is done");
        });
    }
});
```