--- 
layout:     post
title:      webpack 性能调优与 Gzip 原理
subtitle:   如何从webpack着手性能优化
date:       2018-12-20
author:     Ruiyoung
header-img: img/post-bg-debug.png
catalog: true
tags:
    - 前端性能优化
    - webpack
---
##### 对于 DNS 解析和 TCP 连接两个步骤，我们前端可以做的努力非常有限。相比之下，HTTP 连接这一层面的优化才是我们网络优化的核心。
###### HTTP 优化有两个大的方向：
  - 减少请求次数
  - 减少单次请求所花费的时间
###### webpack 的优化瓶颈，主要是两个方面：
  - webpack 的构建过程太花时间
  - webpack 打包的结果体积太大



##### webpack 优化方案
###### 不要让 loader 做太多事情
 最常见的优化方式是，用 include 或 exclude 来帮我们避免不必要的转译
```
module: {
  rules: [
    {
      test: /\.js$/,
      exclude: /(node_modules|bower_components)/,
      use: {
        loader: 'babel-loader',
        options: {
          presets: ['@babel/preset-env']
        }
      }
    }
  ]
}
```
通过限定文件范围带来的性能提升是有限的。除此之外，如果我们选择开启缓存将转译结果缓存至文件系统，则至少可以将 babel-loader 的工作效率提升两倍
```
loader: 'babel-loader?cacheDirectory=true'
```
不要放过第三方库(第三方库以 node_modules 为代表，它们庞大得可怕，却又不可或缺。)

###### DllPlugin 是基于 Windows 动态链接库（dll）的思想被创作出来的。这个插件会把第三方库单独打包到一个文件中，这个文件就是一个单纯的依赖库。这个依赖库不会跟着你的业务代码一起被重新打包，只有当依赖自身发生版本变化时才会重新打包。
用 DllPlugin 处理文件，要分两步走：
1.基于 dll 专属的配置文件，打包 dll 库
2.基于 webpack.config.js 文件，打包业务代码

###### Happypack——将 loader 由单进程转为多进程
大家知道，webpack 是单线程的，就算此刻存在多个任务，你也只能排队一个接一个地等待处理。这是 webpack 的缺点，好在我们的 CPU 是多核的，Happypack 会充分释放 CPU 在多核并发方面的优势，帮我们把任务分解给多个子进程去并发执行，大大提升打包效率。

###### 删除冗余代码
一个比较典型的应用，就是 Tree-Shaking。
基于 import/export 语法，Tree-Shaking 可以在编译的过程中获悉哪些模块并没有真正被使用，这些没用的代码，在最后打包的时候会被去除。

###### 按需加载
一次不加载完所有的文件内容，只加载此刻需要用到的那部分（会提前做拆分）
当需要更多内容时，再对用到的内容进行即时加载

###### Gzip 压缩
HTTP 压缩是一种内置到网页服务器和网页客户端中以改进传输速度和带宽利用率的方式。在使用 HTTP 压缩的情况下，HTTP 数据在从服务器发送前就已压缩：兼容的浏览器将在下载所需的格式前宣告支持何种方法给服务器；不支持压缩方法的浏览器将下载未经压缩的数据。最常见的压缩方案包括 Gzip 和 Deflate。
