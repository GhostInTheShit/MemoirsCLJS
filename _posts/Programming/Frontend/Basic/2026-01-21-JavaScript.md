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

`...arg` 剩余参数的数组，如果在解析对象的时候则包装为为对象.
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

`call`,`apply`,`bind`
```js
//将某个object设置为该方法的this.
functionName.call(your object,arg1,arg2,...)
//和call的区别是将后面的参数放到数组
functionName.apply(your object,[arg1,arg2,...])
//bind不会立即执行,而是创建一个新的函数
var newFunction=functionName.bind(your object);
newFunction(arg1,arg2,...);
```


### 对象

用大括号表示,和JSon有点像.但是JSon两边都要用`""`,所有的属性名都是字符串.
```js
{a:10,b:function(){}}
```
--------------

`this` 指当前对象,在外面默认为`{}`.

>[!warning]
>`this`很差劲,避免使用.`new`也最好不要用(和`this`配套用的).甚至这个`this`也不特指自己,如果自己没有的属性,`__proto__`有,那么这个`this`指`__proto__`.

```js
console.log(this);

//out
//{}
```

在常规函数里面为全局对象.这是常规函数的默认属性.如果用`setTimeout`(这是一个异步方法),`this`会被传入一个超时对象.常规函数的`this`容易被修改.
```js
let a=function (){
    console.log(this);
}

a();

/*输出
<ref *1> Object [global] {
  global: [Circular *1],
  clearImmediate: [Function: clearImmediate],
  setImmediate: [Function: setImmediate] {
    Symbol(nodejs.util.promisify.custom): [Getter]
  },
  clearInterval: [Function: clearInterval],
  clearTimeout: [Function: clearTimeout],
  setInterval: [Function: setInterval],
  setTimeout: [Function: setTimeout] {
    Symbol(nodejs.util.promisify.custom): [Getter]
  },
  queueMicrotask: [Function: queueMicrotask],
  structuredClone: [Function: structuredClone],
  atob: [Function: atob],
  btoa: [Function: btoa],
  performance: [Getter/Setter],
  fetch: [Function: fetch],
  crypto: [Getter],
  navigator: [Getter]
}
*/
```
箭头函数为父对象,如果没有则为`{}`,不会被异步或其他函数修改.
```js
let b=()=>{
    console.log(this);
}

b();

//out
//{}
```



-------------------------------------

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

赋值,如果没有则创建,有则覆盖值.
```js
var employee={
    firstName:"Bob",
    getName:function(){
        return this.firstName;
    }
};
employee.x=1;

//out
//{ firstName: 'Bob', getName: [Function: getName], x: 1 }
```

属性访问,用`.`或者`[""]`
```js
var employee={
    firstName:"Bob",
    getName:function(){
        return this.firstName;
    }
};

console.log(employee.firstName);
//如果不加双引号会报错.
console.log(employee["firstName"]);
```

`for-in` 可以直接遍历对象的属性名.
```js
let person={
    Name:"bob",
    age:19,
    gender:"man",
}

for(let prop in person){
    if(person[prop]=="man"){
        person[prop]="woman"
    };
    console.log(person[prop]);
}
```

解析符`{}`
```js
let person={
    Name:"bob",
    age:19,
    gender:"man",
}

//解析里面的person里面的Name为personName
const {Name:personName} =person;
console.log(personName);

//不写就是同名属性
const {Name}=person;
console.log(Name);

//使用...
const {...arg}=person;
console.log(arg);
//console.log(...arg); 会报错,因为它会解析为对象.在function里面...会被解析为数组
```

方法解析
```js
let person={
    Name:"bob",
    age:19,
    gender:"man",
}

//将传入的person解析同名属性.
let fun1 =function({Name,age}){
    console.log(Name+"-"+age);
}

fun1(person);
```

-------------------------------

`__proto__` 继承,继承的是原型,隐式.严格来说不是类.它没有原型的方法,而是委托它使用,但是又可能需要自己的属性,那么就要用到`this`.有点坑.

>[!warning]
>`js`中的继承只是形成了原型链,js中并没有所谓的类的概念,ES6中的类概念也是原型链的语法糖,本质还是原型链.

```js
let person={
    Name:"bob",
    age:19,
}

let man={gender:"man"}

//设置其父类
man.__proto__=person;

console.log(man);
console.log(man.__proto__);
console.log(man.Name);


//out
//{ gender: 'man' }
//{ Name: 'bob', age: 19 }
```
------------------------------
`prototype` 原型类型,也就是具体的构造函数创造对象时候的模型.在会在构造函数执行之前设置.

```js
let person=function(){
    this.getMe=function(){
        return this.Name+"--"+this.age;
    }
};

//指person这个构造函数创造时候会有Name和age的属性
person.prototype.Name="bob";
person.prototype.age=10;

//new 是用函数创建一个对象,其实就是执行函数并且将其包含到一个对象里面.
let realyPerson=new person();

realyPerson.eat="apple";
```

输出
```js
//person { getMe: [Function (anonymous)], eat: 'apple' }
//只有初始的方法和被直接赋值的eat
console.log(realyPerson);

//{ Name: 'bob', age: 10 }
//它来源于原型类型,所以它的__proto__就为原型类型
console.log(realyPerson.__proto__);

//bob--10
//不包含原型类型
console.log(realyPerson.getMe());

//{ Name: 'bob', age: 10 }
//构造函数的原型类型
console.log(person.prototype);
```
原型类型是创造它时候的规则,会被记录在对象的`__proto__`中,不在本体中.

------------------------

`抽象构造函数`,其实就是在父类直接抛出异常.然后子类可以创建而不报错(因为`js`的代码用到才会报错,不用不报).

```js
let a=function(){
    throw new error("aaa");
};

//js是解释型语言,用到才报错,不用不报错.
//a();
```

### 闭包

简单的说**被暴露出来的的函数能够保留其作用域的私有变量的引用且使用**


















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
