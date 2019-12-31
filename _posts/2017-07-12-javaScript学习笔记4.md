--- 
layout:     post
title:      javaScript学习笔记
subtitle:   cookie
date:       2017-07-05
author:     Ruiyoung
header-img: img/post-bg-js-version.jpg
catalog: true
tags:
    - javaScript
---
### 前言  

> cookie 是存储于访问者的计算机中的变量。每当同一台计算机通过浏览器请求某个页面时，就会发送这个 cookie。当用户下一次访问同一个页面时，服务器会先查看有没有上传留下的cookie资料，如果有就更根据cookie里的资料判断访问者，发送特定的页面内容.  
> 常见应用场景：自动登录，记住用户名......  

### 创建cookie  

- 格式：将document下的cookie属性设置为为如下格式的字符串：name=value  
- 为了避免特殊字符造成的错误，有时需要对数据进行编码解码  
&emsp;&emsp;使用encodeURIComponent() 进行编码  
&emsp;&emsp;读取时 使用decodeURIComponent()解码  
&emsp;&emsp;cookie值不能含有分号，逗号和空白符  

```{.javaScript}
document.cookie='name=张三';
document.cookie='age=35'
//不会像变量赋值那样覆盖前一条信息，而是会添加一条新内容
alert(document.cookie)
```

### cookie可选参数  

- expires=时间：过期时间
&emsp;&emsp;默认值为浏览器关闭后过期(即会话结束后)
&emsp;&emsp;将expires设置为过去的时间可以删除cookie  

```{.javaScript}
var day=new Date();
day.setDate(day.getDate()-1)
document.cookie='name=张三;expires='+day; 
```  

- path:他指定了与cookie关联在一起的网页。默认值是在和当前网页同一目录的网页中有效。如果把path设置为'/',那么它对该网站的所有网页可见  
- domain:设定cookie的有效域名，一般使用默认值，即绑定当前域名，本地测试无效  
- secure：指定了网络上如何传输cookie.默认为普通http协议传输；若设置为安全的，将只能通过https安全协议才可以传输。  

### 封装cookie创建/读取/删除的函数  

- 创建cookie数据的函数封装  

```{.javaScript}
function setCookie(key,value,expires){
    document.cookie=encodeURIComponent(key)+'='+encodeURIComponent(value)+';expires='+ddate(expires);
}
function ddate(expires){
    var ddate=new Date()
    ddate.setDate(ddate.getDate()+expires)
    return ddate
}
setCookie('name','吉延鹏')
setCookie('qq','365966179')
setCookie('email','365966179@qq.com')
```

- 读取cookie数据的函数封装(split() 方法用于把一个字符串分割成字符串数组。)  

```{.javaScript}
function getCookie(name){
    var arrStr=document.cookie.split(';');
    //alert(arrStr)
    for(var i=0;i<arrStr.length;i++){
        var arr=arrStr[i].split('=')
        //alert(arr[0]+'\n'+arr[1])
        if(arr[0]==name){return arr[1] }
    }
    return ''
}
```

- 删除cookie的函数封装  

```{.javaScript}
function removeCookie(name){
    setCookie(name,'随意值',-1)
}
removeCookie('name')
```

### cookie的限制  

- 数量(20-50,不同浏览器有差异)，大小有限(4k)  
- 有些数据不适合使用cookie保存，比如银行卡号等重要的信息  
