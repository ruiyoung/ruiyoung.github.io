--- 
layout:     post
title:      javaScript学习笔记
subtitle:   Ajax
date:       2017-07-15
author:     Ruiyoung
header-img: img/post-bg-js-version.jpg
catalog: true
tags:
    - javaScript
---
### 前言  
> AJAX = Asynchronous JavaScript and XML（异步的 JavaScript 和 XML）  
> AJAX 可以通过在后台与服务器进行少量数据交换，使网页实现异步更新。这意味着可以在不重新加载整个网页的情况下，对网页的某部分进行更新。(传统的网页（不使用 AJAX）如果需要更新内容，必需重载整个网页面。)  

### 基本用法  
#### 1. 创建 XMLHttpRequest 对象  
- 语法：var myAjax=new XMLHttpRequest();  
- 老版本的 IE（IE5 和 IE6）使用 ActiveX 对象 ： var myAjax=new ActiveXObject("Microsoft.XMLHTTP");  
#### 2. 向服务器发送请求:使用open() 和 send() 方法：
- open(method,url,async):规定请求的类型、URL 以及是否异步处理请求。  
&emsp;&emsp;method：请求的类型；GET 或 POST     
&emsp;&emsp;url：文件在服务器上的位置   
&emsp;&emsp;async：true（异步）或 false（同步）  
- send(string):string：仅用于 POST 请求  

#### 3. 服务器响应  

