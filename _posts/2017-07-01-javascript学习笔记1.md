--- 
layout:     post
title:      javaScript学习笔记
subtitle:   函数
date:       2017-07-01
author:     Ruiyoung
header-img: img/post-bg-js-version.jpg
catalog: true
tags:
    - javaScript
---
### 基本类型和引用类型
**1. 基本类型值有：undefined，NUll，Boolean，Number和String**  
这些类型分别在内存中占有固定的大小空间，例如：数值型在内存中占有八个字节，布尔值只占有一个字节...... 
他们的值保存在栈空间，我们通过按值来访问的。

**2.引用类型：对象、数组、函数**  
引用类型内存中占有的空间不固定，但是内存地址大小是固定的，因此存储的实际上是数据的内存地址。

**3.在变量复制时候，基本类型复制的是值本身，而引用类型复制的是地址**  

**4.函数的参数都是按值传递的**
```
//函数的参数都是按值传递的
var num = 100; 
function box(num){ 
    num+=100;      
    return num;        
} 
var result = box(num); 
alert(result);
alert(num);//这里输出100而不是200
```
### 函数创建方式  
- 普通声明方式 
```
function fun (m,n) {
    alert(m + n)
}
fun(3,2)
```  
- 使用变量声明  
```
var fun = function(m, n) {
    alert(m + n)
}
fun(3, 2)
```
- 使用构造函数 (了解) 
```
var fun = new Function('m', 'n', 'alert(m+n)')
fun(3, 2)
```  

#### 函数的内部属性     
- arguments  

```
// arguments.length检测函数的参数个数
function sum() {
    //alert(arguments.length);
    var result=0;
    for(var i=0;i<arguments.length;i++){
        result+=arguments[i]
    }
    return result
}
alert(sum(12,3,5,10,5,3))
```

- this  

```
//在函数外部使用this,this就指的是window对象
//alert(this)

//全局变量可以看做window对象的属性
var x=1;
alert(window.x)
alert(this.x)

//函数内部调用
function test(){
    var x=0;
    alert(x) //这里的x为0
    alert(this.x); //这里的x为1
    alert(this)
}
//test()
//用new来调用，那么绑定的将是新创建的对象
function test2(){ 
　　this.x = 100; 
} 
var obj = new test2();
//alert(x); //这里的x为1
//alert(obj.x);//这里的x为100

//作为某个对象的方法调用
function test3(){ 
　　alert(this.x); 
}
var objo={}; 
objo.x = 1000; 
objo.m = test3; 
//alert(x);
//objo.m(); //1000

//事件监听函数中的this
var div1 = document.getElementById('div1');
div1.onclick = function(){
    alert( this.innerHTML); //this指向的是div元素
};
```  

### 函数的属性和方法  

```
//length:当前函数希望接受的命名参数的个数
function test(num1,num2,num3) {
        alert(test.length);
    alert(arguments.length) //注意arguments是实际传人的参数
}
//test(10,20,10,35)


function sum(num1, num2) {
    return num1 + num2;
}
function applySum1(num1, num2) {
    return sum.apply(this, [num1, num2]);
}
function applySum2(num1, num2) {
    return sum.apply(this, arguments);//传入arguments对象 
}
// alert(sum(10,20))
// alert(applySum1(10,20))
// alert(applySum2(10,20))

//对于apply、call二者而言，作用完全一样，只是接受参数的方式不太一样。　
function callSum(num1, num2) {
    return sum.call(this, num1, num2); //传入的是具体的参数
}
//alert(callSum(30, 20));

//扩充函数作用域
var color = "red";
var o = {color: "blue"};
function sayColor() {
    alert(this.color);
}
// sayColor();
// sayColor.call(this);
// sayColor.call(window);
sayColor.call(o)
```

### 执行环境及作用域  
- 执行环境定义了变量或函数有权访问其他数据。  
- 全局执行环境是最外围的执行环境，在web浏览器中，全局执行环境是window对象，因此，所有的全局变量的函数都是作为window的属性和方法创建的。  

```
var name = "张三";      //定义全局变量
alert(name)     
alert(window.name);    //全局变量，最外围，属于window属性

function setName(){      
    return "李四";   
} 
alert(setName());
alert(window.setName()); //全局函数，最外围，属于window方法 
```  
- 变量没有在函数内声明或者声明的时候没有带var就是全局变量，拥有全局作用域,window对象的所有属性拥有全局作用域；在代码任何地方都可以访问，函数内部声明并且以var修饰的变量就是局部变量，只能在函数体内使用，函数的参数虽然没有使用var但仍然是局部变量。  
```
var name = "张三";      //定义全局变量
function setName(){  
    //var name= "李四";    //定义局部变量
    name="李四";    //去掉var变成了全局变量 
    //alert(name);
}
setName()
alert(name);


function setName(){ 
    var name="张三"  
    function setYear(){ //setYear()方法的作用域在setName()内 
    var age=21;
    var str=name+age+'岁了' 
    return str;
    }  
    //alert(setYear())  
    //alert(age)
    return setYear()
}
setName()
// alert(setYear()) 
alert(setName());
```  

- 内部环境可以访问所有的外部环境，但是外部环境不能访问内部环境中的任何变量和函数。  
- 在变量的查询中，访问局部变量要比全局变量快。  

### 内存管理
> JS中内存的分配和回收都是自动完成的，内存在不使用的时候会被垃圾回收器自动回收。 

- 内存的生命周期,JS环境中分配的内存一般有如下生命周期:  
&emsp;&emsp;1.内存分配：当我们申明变量、函数、对象的时候，系统会自动为他们分配内存  
&emsp;&emsp;2.内存使用：即读写内存，也就是使用变量、函数等  
&emsp;&emsp;3.内存回收：使用完毕，由垃圾回收自动回收不再使用的内存  
- 垃圾回收算法：对垃圾回收算法来说，核心思想就是如何判断内存已经不再使用了。
- JavaScript 的内存管理注意事项:  
&emsp;&emsp;1.避免不必要的定义全局变量(当一个变量被定义在全局作用域中，默认情况下JavaScript 引擎就不会将其回收销毁。如此该变量就会一直存在于老生代堆内存中，直到页面被关闭。)  
&emsp;&emsp;2.及时解除不再使用的变量引用,即将其赋值为 null;(在内存回收周期中，收回内存不是立即收回，浏览器每隔一段时间检查一次)  
&emsp;&emsp;3.合理的使用函数，函数中的局部变量函数执行结束后就会自动释放内存。  

### 全局函数
> 全局函数和属性可用于所有内建的 JavaScript 对象。全局函数又叫顶层函数或系统函数。  

- parseInt() 函数可解析一个字符串，并返回一个整数。  
- parseFloat() 函数可解析一个字符串，并返回一个浮点数。  
- isNaN() 函数用于检查其参数是否是非数字值。  
- String() 函数把对象的值转换为字符串。  
- Number() 把对象的值转换为数字。  
- eval() 函数可计算某个字符串，并执行其中的的 JavaScript 代码。(该方法只接受字符串作为参数，要计算的字符串中必须含有要计算的 JavaScript 表达式或要执行的语句。)  
- escape() 对字符串进行编码。  
&emsp;&emsp;返回值:已编码的 string 的副本。其中某些字符被替换成了十六进制的转义序列。  
&emsp;&emsp;该方法不会对 ASCII 字母和数字进行编码，也不会对下面这些 ASCII 标点符号进行编码： * @ - _ + . / 。其他所有的字符都会被转义序列替换。  
- unescape() 对由 escape() 编码的字符串进行解码。  
- encodeURI() 把字符串编码为 URI。 
- decodeURI() 解码某个编码的 URI。  
- decodeURIComponent() 解码一个编码的 URI 组件。  
- encodeURIComponent() 把字符串编码为 URI 组件。  
- 三种编码方式的区别:  
&emsp;&emsp;1.escape不编码字符有69个：*，+，-，.，/，@，_，0-9，a-z，A-Z(主要是为了防止特殊字符造成计算错误时候应用)  
&emsp;&emsp;2.encodeURI不编码字符有82个：!，#，$，&，'，(，)，*，+，,，-，.，/，:，;，=，?，@，_，~，0-9，a-z，A-Z(防止特殊字符串造成URI的传递错误，一般用于页面跳转的时候)  
&emsp;&emsp;3.encodeURIComponent不编码字符有71个：!， '，(，)，*，-，.，_，~，0-9，a-z，A-Z(防止URI参数中特殊字符串造成参数读取错误，一般用来传递参数。)  
- isFinite() 检查某个值是否为无穷大的数。  
如果 number 是有限数字（或可转换为有限数字），那么返回 true。否则，如果 number 是 NaN（非数字），或者是正、负无穷大的数，则返回 false。  
&emsp;&emsp;Infinity无穷大（系统定义常量）  
&emsp;&emsp;-Infinity无穷小（系统定义常量）

