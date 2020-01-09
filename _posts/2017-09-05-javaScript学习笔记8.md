--- 
layout:     post
title:      javaScript学习笔记
subtitle:   Date
date:       2017-09-05
author:     Ruiyoung
header-img: img/post-bg-js-version.jpg
catalog: true
tags:
    - javaScript
---

### 前言  

> Date对象用来表示日期和时间

#### 获取当前时间  

```{.javaScript}
var now = new Date();
now; // Wed Jun 24 2015 19:49:22 GMT+0800 (CST)
now.getFullYear(); // 2015, 年份
now.getMonth(); // 5, 月份，注意月份范围是0~11，5表示六月
now.getDate(); // 24, 表示24号
now.getDay(); // 3, 表示星期三
now.getHours(); // 19, 24小时制
now.getMinutes(); // 49, 分钟
now.getSeconds(); // 22, 秒
now.getMilliseconds(); // 875, 毫秒数
now.getTime(); // 1435146562875, 以number形式表示的时间戳  
```

**当前时间是浏览器从本机操作系统获取的时间，所以不一定准确，因为用户可以把当前时间设定为任何值。**  

#### 创建指定日期和时间的Date对象  

1. 日期信息值分别传入  

```{.javaScript}
var d = new Date(2015, 5, 19, 20, 15, 30, 123); // 年份、月份-1、天、时、分、秒、毫秒
d; // Fri Jun 19 2015 20:15:30 GMT+0800 (CST)
```

2. 解析一个符合ISO 8601格式的字符串  

```{.javaScript}
var d = Date.parse('2015-06-24T19:49:22.875+08:00');
d; // 1435146562875 时间戳
```

3. 使用日期字符串参数  

```{.javaScript}
var d = new Date('2017-08-25');
d; // Fri Aug 25 2017 08:00:00 GMT+0800 (CST)

var e = new Date('2017/08/25');
e; // Fri Aug 25 2017 08:00:00 GMT+0800 (CST)

var e = new Date('2017-08-25 12:30:15');
e; // Fri Aug 25 2017 12:30:15 GMT+0800 (CST)
```

4. 使用时间戳来创建日期  

```{.javaScript}
var d = new Date(1435146562875);
d; // Wed Jun 24 2015 19:49:22 GMT+0800 (CST)
d.getMonth(); // 5
```

#### 时区  

我们的世界有数百个时区。 在JavaScript中，我们只关心两个， 本地时间和协调世界时（UTC）。  

```{.javaScript}
var d = new Date(1435146562875);
d.toLocaleString(); // '2015/6/24 下午7:49:22'，本地时间（北京时区+8:00），显示的字符串与操作系统设定的格式有关
d.toUTCString(); // 'Wed, 24 Jun 2015 11:49:22 GMT'，UTC时间，与本地时间相差8小时
```
