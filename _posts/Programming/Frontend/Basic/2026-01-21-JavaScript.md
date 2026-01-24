---
categories: js
---

# JavaScript

使网页有交互性.

`ECMAScript` 脚本语言的标准规范,简称`ES`,为了防止浏览器大战.`JS`遵从`ES`


内部javascript 无法在html中共享
```html
<script>
    //your code
</script>
```

外部javascript ,`filename.js`
```html
<script src=""filename.js>
</script>
```

## node.js

大致为js的虚拟机,内部为V8引擎

使用node.js ,可以在浏览器之外执行js,命令行

`https://nodejs.org/en` 官网下载,不需要`Chocolatey`

## JS语法

弱类型,不能固定类型.默认值为`undefined`,不会报错.

`Hoisting` 变量提升

**会将声明提到第一行,但赋值则不改变顺序.  
但是const和let会被锁定不能被访问,而var不会.  
这是ES6的保护机制,提醒你不要在声明前使用变量.  
所以优先使用const和let.**

**var是函数作用域,它在括号外面也能使用.const和let是块级作用域**

>[!warning]
>var在for循环的使用中会导致闭包问题,

```js
//这里x为undefined.
console.log("Statement 1",x);
//y会报错
console.log("Statement 1",y);
//a也会导致报错
console.log("Statement 1",a);

var x=1;
//优先使用let和const
let y=1;
const a=1;
```

`arguments` 内置关键字,是个伪数组,有些方法没有,所有参数.
>[!important]
>js默认能接收多个参数,没有被赋值的为undefined,不会报错.

```js
let method =function(a,b){
    console.log(a);
    console.log(b);
    console.log(arguments[2]);
}

method(1);
method(1,2,3);

/* 
输出
1
undefined
undefined
1
2
3
*/
```

`...arg` 剩余参数的数组
```js
//...b表示剩余的参数
let method =function(a,...b){
    console.log(a);
    //输出数组
    console.log(b);
    //会将数组展开
    console.log(...b);
}
method(1,2,3);

/* 输出
1
[ 2, 3 ]
2 3
*/
```

`setTimeout(callBackFunction,milliseconds)` 在多少秒之后执行回调函数.

`setInterval(callBackFunction,milliseconds)` 每多少秒执行回调函数.

`clearInterval(setIntervalFuction)`  用于停止由 setInterval() 启动的重复定时器。
```js
const intervalId = setInterval(() => {
  console.log("每秒执行一次");
}, 1000);

// 例如：5 秒后停止
setTimeout(() => {
  clearInterval(intervalId);
  console.log("定时器已停止");
}, 5000);
```


### 对象

用大括号表示,和JSon有点像.但是JSon两边都要用`""`
```js
{a:10,b:function(){}}
```

`this` 当前对象,在`function`里面为`function`对象,在外面默认为`{}`

这里的对象不是类,有点像字典.
```JS
var employee={
    firstName:"Bob",
    getName:function(){
        //这里必须写this要不然报错,它得不到外面的属性.
        return this.firstName;
    }
};

console.log(employee.getName());
```

























<details>
<summary>过时版本</summary>

# JavaScript

- `constructor()` 构造器方法,是个关键字

-   ~~javascript好像没有实例方法,所有方法放在类中.~~ 直接声明是static静态方法,如果赋值了就在实例  
    ```jsx
    //类中
    someWay(){};
    
    //实例中
    someWay=function(){};
    
    someWay=()=>{};
    ```
- 属性写了`static`是静态,方法不写static就是静态
- 在Javascript中的方法可以不需要实例调用.  
onclick调用的方法为直接调用.this为Window(严格模式为undefined)

- `bind` 生成新的函数,并且改变触发者. 

-   return 不能换行
    ```js
    return <></>
    //return不能换行,下面是错的
    return 
    <></>
    ```
-   `console.log(...arg)` arg为数组.该代码会复制一个新的数组,输出的时候会foreach
-   `export`可以在其他文件中访问
-   `default`表示此为主要函数
-   `ref` 关键字,这个实例会被收集到refs中,拿到的是真实节点，不是虚拟DOM
    - `ref={c=>this.input1=c}` 回调模式,传入的参数其实是自己
    - `ref="string"` 字符串形式,**不推荐使用,效率低**

-   表单提交,哪怕不提供地址也仍然会刷新页面.

## 样式

.css

`import "./some.css"` 当前目录下的某个css

>[!WARNING]
>空字符默认顶端与父表格顶端对齐，加入字体后则会对准文字基线(baseline),所以以下直接设置`top`,全部顶端对齐,就不会位置变化  
>`vertical-align:middle` 文字的下行线

----
javacript 感觉不太规范，有点乱  
可能ES6之后的好一点
</details>
