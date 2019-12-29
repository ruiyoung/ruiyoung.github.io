--- 
layout:     post
title:      javaScript学习笔记
subtitle:   JSON
date:       2017-07-05
author:     Ruiyoung
header-img: img/post-bg-js-version.jpg
catalog: true
tags:
    - javaScript
---

> JSON 是存储和交换文本信息的语法。类似 XML。(JSON 比 XML 更小、更快，更易解析。)  
> JSON 是轻量级的文本数据交换格式  
> JSON 独立于语言(JSON 使用 JavaScript 语法来描述数据对象，但是 JSON 仍然独立于语言和平台。)  
> JSON 具有自我描述性，更易理解  
> JSON是在AJAX中代替XML交换数据的更佳方案。  

### JSON 语法
- 数据在名称/值对中  
- 数据由逗号分隔  
- 花括号保存对象  
- 方括号保存数组  
- JSON 值可以是：  
&emsp;&emsp;数字（整数或浮点数）  
&emsp;&emsp;字符串（在双引号中）   
&emsp;&emsp;逻辑值（true 或 false）   
&emsp;&emsp;数组（在方括号中）   
&emsp;&emsp;对象（在花括号中）   
&emsp;&emsp;null  
```
//js中的对象表示
var user={
    name:'张三',
    age:'30'
}
//josn对象表示  
{
    "name":"张三",
    "age":"30"
}
//普通数组 
var arr=["aaa",100,true]
//json数组
["aaa",100,true] //少了变量赋值  
//数组对象组合嵌套使用
[{
    "name":"aaa",
    "age":30
},{
    "name":"bbb",
    "age":25
},{
    "name":"ccc",
    "age":18
}]
```
### JSON 的解析和序列化
> 一般情况下，我们的json数据都是从服务端获取到的，获取的json数据是以字符串的形式返回的。这个字符串虽然是json格式的，但是不能被直接使用，我们必须将该字符串转化为一个对象才能正常解析它  
- JavaScript 函数 eval() 可用于将 JSON 文本转换为 JavaScript 对象。
<font color=red>eval()函数可编译并执行任何 JavaScript 代码。这隐藏了一个潜在的安全问题。(如果JSON中包含恶意代码也会被直接执行)</font>

- 使用 JSON 解析器将 JSON 转换为 JavaScript 对象是更安全的做法。JSON解析器只会识别JSON文本,而不会执行。  
&emsp;&emsp;JSON 的解析:json数据转换成js对象 JSON.parse()   
&emsp;&emsp;JSON的序列化:js对象转换成json数据(字符串)  JSON.stringify() 
```
//实际使用的时候json数据需要从服务器加载，这里假定下面的数据是从服务器加载过来的，来演示后续的操作。
var jsonstr='[{"name":"aaa","age":30},{"name":"bbb","age":25},{"name":"ccc","age":18}]';
var jsonobj=JSON.parse(jsonstr);
jsonstr=JSON.stringify(jsonobj)
```
### JSON 法创建对象  
>  JSON非常易于人阅读与编写，同时利于机器解析与生成.我们可以使用JSON语法创建JavaScript对象  
- 优点：语法简单  
- 缺点：不适用多个对象的创建