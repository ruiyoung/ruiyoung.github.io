--- 
layout:     post
title:      javaScript学习笔记
subtitle:   匿名函数和闭包
date:       2017-07-05
author:     Ruiyoung
header-img: img/post-bg-js-version.jpg
catalog: true
tags:
    - javaScript
---
###  匿名函数
> 没有函数名字的函数  
- 单独的匿名函数是无法运行和调用的  
- 可以把匿名函数赋值给变量  
- 通过表达式自我执行，语法：(匿名函数)()  
- 匿名函数传递参数，语法：(匿名函数)(参数)  
```
// 情况1.把匿名函数赋值给变量 
var fn=function (){
    alert('我是匿名函数')
}
alert(fn)   //会将函数表达式输出
fn() 

//情况2.匿名函数通过表达式自我执行
(function (){
    alert('我是匿名函数')
}
)()

//匿名函数传递参数
function myfn(m,n){
    alert(m+n)
}
myfn(100,100);

(function(m,n){
    alert(m+n)
})(1000,1000)
```
### 闭包
> 闭包的英文单词是closure，是指有权访问另一个函数作用域中变量的函数。  
> 在本质上，闭包就是将函数内部和函数外部连接起来的一座桥梁。内层的函数可以使用外层函数的所有变量，即使外层函数已经执行完毕。  
```
function myfn(){
    return function (){
        return('**********')
    }
}
//alert(myfn)  //输出整个函数表达式
//alert(myfn())  //输出匿名函数表达式

//调用方式1
alert(myfn()())
//调用方式2
var bb=myfn()
alert(bb())
```
##### 闭包的相关知识点 
- 常见的方式是在函数内部创建另一个函数  
- 闭包的第一个用途：通过闭包可以访问局部变量 
```
function myfn(){
    var bb='局部变量bb'
    return function (){
        return(bb) // 通过匿名函数返回myfn()的局部变量bb;
    }
}
//调用方式1
//alert(myfn()())

//调用方式2
var csbb=myfn()
alert(csbb())
``` 
- 闭包的第二个用途：可以让局部变量的值始终保持在内存中  
&emsp;&emsp;优点:可以把局部变量驻留在内存中,可以避免使用全局变量;[全局变量在复杂程序中会造成许多麻烦（比如命名冲突，垃圾回收等），所以推荐使用私有的,封装的局部变量。而闭包可以实现这一点。]  
&emsp;&emsp;缺点：由于闭包里作用域返回的局部变量资源不会被立刻销毁回收,所以可能会占用更多的内存;所以过度使用闭包会导致性能下降  


##### 闭包中的this问题  
- this是在运行时基于函数的执行环境来绑定的  
- 全局函数中的this是window，而当函数作为某个对象的方法调用时，this就是指的那个对象  
- 匿名函数的执行环境具有全局性，this通常是指向window的  
