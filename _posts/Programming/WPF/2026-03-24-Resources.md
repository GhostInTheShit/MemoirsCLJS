# Resources 资源

StaticResource 静态资源,加载后不会被改变

DynamicResource 动态资源,使用的时候获取,可以使用设计时尚未存在的资源

## 区域

资源是继承的,子类的能调用父类的.

写在`Window`中,该窗口中的都能使用

写在`App`中,整个应用都可以使用

## 后置代码

`FindResource("")` 查找资源.

##