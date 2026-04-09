---
categories:React
---

# React 基础

## 为什么使用React

使用原生JavaScript操作Dom繁琐,效率低,每次要重新绘制和排列.

没有组件化,复用率低

## React特点

声明式编码(另一种是命令式)

采用组件化

React Native 可以进行移动端开发

使用**虚拟DOM+Diffing算法**.尽量减少真实DOM的交互

## React 包

react.development.js React核心包

react-dom.development.js 用于支持react操作DOM

babel.min.js 将js转化为jsx

## 虚拟DOM

创建虚拟DOM  
`const VDOM=<h1>Hello React</h1>`

渲染虚拟DOM到页面  
`ReactDom.render(VDOM,document.getElementById('test'))`

虚拟DOM其实就是个Object对象,非常轻便  
最后会被React转化为真实DOM

## JSX

全称JavaScript XML,是JS的扩展语法

### 语法
- 定义虚拟DOM时,不要写引号
- 标签混入JS表达式时要写{}
- 样式的类名指定不要用class,而是className
- 内联样式,要用style={{key:value}}的形式.
- 虚拟DOM只能有一个根标签
- 标签首字母
  - 若小写字母开头,则转为html同名元素，若没有则报错
  - 若大写字母开头,则react就去渲染对应组件,若没有，就报错
- 数组里面每个子元素要有唯一的key

## 组件

可以安装网页扩展 `React Developer Tools`帮助开发

>[!Important]
>该文章只以函数式讲解

- **函数式组件**

    ```jsx
    //首字母大写
    function FuncComponent(){
        return <h1>函数式组件</h1>
    }

    //左括号一定不能换行.
    function FuncComponent(){
        return (
            <div>
                <h1>函数式组件</h1>
            </div>
        )
    }
    ```

- 类式组件

    ```jsx
    //首字母大写,必须继承React.Component
    class MyComponent extends React.Component{
        //构造器可以不写,传入参数用props,因为React帮你new的,默认使用props.
        constructor(props){
            //一定要调用父类构造
            super(props)
        }

        //必须有render()，页面渲染
        render(){
            return <h1>类式组件</h1>
        }
    }
    ```

### 组件核心属性

~~只有类式组件才有~~现在hooks的存在使得function也能有此功能.

1.  State 状态

1.  Props 属性
    
    只读属性
    ```jsx
    //可以以这种方式赋值  
    <person name="tom">

    //装入数据
    const p={name:"tom",age:18}
    <person {...p}>
    ```

1.  refs 引用
    不要过于使用`ref`

## 数据

属性不可以直接更改,不会渲染


要使用`setState`,并且需要`import`.修改并且渲染,会触发render()

```jsx
//声明一个使用了useState的变量,setSome是改变它的方法
const [some,setSome]=useState(1);
//将some修改为2
setSome(2);
```

## 快速入门

使用 next.js

`npx create-next-app@latest`

