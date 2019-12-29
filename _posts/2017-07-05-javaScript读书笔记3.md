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

```
function add(){
    var num= 100; // 这里改为局部变量;
    return function(){
        num++;
        alert(num);
    }
};
// add()();add()();add()();//这种调用方式会出错,因为每次调用 num都会初始化一次;  
var fn=add()//只在这里初始化一次，后边调用的时候执行的是里边的匿名函数
fn();fn();fn();
fn=null //应及时解除引用，否则会占用更多内存
```  

```
function fun(){
    var arr=[];
    for(var i=0; i<5; i++){
        arr[i]=function(n){ 
            return function(){ 
                return '元素'+n;
            }
        }(i) 
    }
    return arr
}
var Bb=fun()
//alert(Bb.length)
// alert(Bb[0]())
for(var i=0; i<5; i++){
    //alert(Bb[i]) 
    alert(Bb[i]()) 
}
//这次成功的输出了 ‘元素0 元素1 元素2 元素3 元素4 ’，而不再都是[元素5]
/*
    1.这里的匿名函数有一个参数 n，也就是最终将返回的结果数值；
    2.在调用每个匿名函数时传入变量i
    3.变量i的当前值会赋值给n，
    4.匿名函数内部创建并返回了一个访问n的闭包
    5.如此数组arr中的每个函数中都有了自己的n变量的一个副本(闭包可以将局部变量贮存在内存中)

*/
```  

##### 闭包中的this问题  
- this是在运行时基于函数的执行环境来绑定的  
- 全局函数中的this是window，而当函数作为某个对象的方法调用时，this就是指的那个对象  
- 匿名函数的执行环境具有全局性，this通常是指向window的  

```
var name='The Window';
var obj={
    name:'my obj',
    get:function(){
        return function(){
            return this.name;
        }
    }
}

alert(obj.get()()) //这次返回的是全局变量 'The Window'
alert(obj.get().call(obj))//这次又返回的是'my obj'，因为call()强制改变了this的指向


var name='The Window';
var obj={
    name:'my obj',
    get:function(){
        //这里的this指的是对象，这里为obj
        var self=this
        return function(){
            //闭包里的this指的是window
            return self.name;
        }
    }
}
alert(obj.get()()) 
```  

##### 模仿块级作用域    

```
function myfun() {
    for(var i=0;i<5;i++){

    }  //i不会因为离开了for块就失效;
    var i; //重新声明后i还是5,
    alert(i)  //此时的i=5
}

//模仿块级作用域
function myfun() {
    (function(){
      for(var i=0;i<5;i++){
          alert(i)  
      } 
    })()  // 这里定义并立即调用了一个匿名函数; 
    alert(i) 
    //此时的i已结不存在 会报错:'i is not defined'
}
myfun()
```