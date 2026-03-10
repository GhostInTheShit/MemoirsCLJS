---
categories: Nodejs
---
# Nodejs

不需要html,直接执行JavaScript.需要在命令行里面 `node yourjs.js` 运行.

## 模块

各模块之间默认不共享

`exports` 将某个变量暴露出去.是一个默认变量.

`required("")` 获取某个模块的共享,返回值接收.`./为当前文件夹`,不写`./`则为`nodejs`默认模块.

```js
//module1.js
var number=10;

exports.number=number;
```

```js
//module2.js
//用一个变量接收
var ref=require("./first.js");

console.log(ref.number);
```

### FS模块,内置模块

开箱即用,推荐使用异步方法.

`readFile()` 

```js
var fs= require("fs");

//异步方法不会等待,所以text没有意义
//如果不写第二个参数,data为二进制,可以Tostring()
var text=fs.readFile("sample.txt","utf-8",(error,data)=>
    {
        //null
        console.log(error); 
        //hello world
        console.log(data);
    });

//undefined-------------text
console.log(text+"-------------text");

//输出顺序
//undefined-------------text
//null
//hello world
```

`readFileSync()` 同步的`readFile`

`writeFile()`

`writeFileSync()`

`rename()`

`unlink()`

`stat()`

`readdir()`

## 包管理

npm

npx 局部安装

## Next.js

专门用于React