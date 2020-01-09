--- 
layout:     post
title:      javaScript学习笔记
subtitle:   错误处理
date:       2017-09-05
author:     Ruiyoung
header-img: img/post-bg-js-version.jpg
catalog: true
tags:
    - javaScript
---

### 前言  

> 执行过程中，程序可能遇到无法预测的异常情况而报错，例如，网络连接中断，读取不存在的文件，没有操作权限等。对于这种错误，我们需要处理它，并可能需要给用户反馈。  

#### try ... catch ... finally  

```{.javaScript}
var r1, r2, s = null;
try {
    r1 = s.length; // 此处应产生错误
    r2 = 100; // 该语句不会执行
} catch (e) {
    console.log('出错了：' + e);
} finally {
    console.log('finally');
}
console.log('r1 = ' + r1); // r1应为undefined
console.log('r2 = ' + r2); // r2应为undefined
```

运行后可以发现，输出提示类似“出错了：TypeError: Cannot read property 'length' of null”。  

###### try ... catch ... finally的执行流程  

当代码块被try { ... }包裹的时候，就表示这部分代码执行过程中可能会发生错误，一旦发生错误，就不再继续执行后续代码，转而跳到catch块。catch (e) { ... }包裹的代码就是错误处理代码，变量e表示捕获到的错误。最后，无论有没有错误，finally一定会被执行

catch和finally可以不必都出现，也就是说，try语句一共有三种形式：  

try ... catch ... finally ...
try ... catch ...  
try ... finally ...  

#### 错误类型  

JavaScript有一个标准的Error对象表示错误，还有从Error派生的TypeError、ReferenceError等错误对象。我们在处理错误时，可以通过catch(e)捕获的变量e访问错误对象：  

```{.javaScript}
try {
    ...
} catch (e) { // 变量e是一个习惯用法，也可以以其他变量名命名
    if (e instanceof TypeError) {
        alert('Type error!');
    } else if (e instanceof Error) {
        alert(e.message);
    } else {
        alert('Error: ' + e);
    }
}
```

#### 抛出错误  

程序也可以主动抛出一个错误，让执行流程直接跳转到catch块。抛出错误使用throw语句。  

```{.javaScript}
var r, n, s;
try {
    s = prompt('请输入一个数字');
    n = parseInt(s);
    if (isNaN(n)) {
        throw new Error('输入错误');
    }
    // 计算平方:
    r = n * n;
    console.log(n + ' * ' + n + ' = ' + r);
} catch (e) {
    console.log('出错了：' + e);
}
```

#### 错误传播  

如果在一个函数内部发生了错误，它自身没有捕获，错误就会被抛到外层调用函数，如果外层函数也没有捕获，该错误会一直沿着函数调用链向上抛出，直到被JavaScript引擎捕获，代码终止执行。  

所以，我们不必在每一个函数内部捕获错误，只需要在合适的地方来个统一捕获，一网打尽：  

```{.javaScript}
function main(s) {
    console.log('BEGIN main()');
    try {
        foo(s);
    } catch (e) {
        console.log('出错了：' + e);
    }
    console.log('END main()');
}

function foo(s) {
    console.log('BEGIN foo()');
    bar(s);
    console.log('END foo()');
}

function bar(s) {
    console.log('BEGIN bar()');
    console.log('length = ' + s.length);
    console.log('END bar()');
}

main(null);
// 当bar()函数传入参数null时，代码会报错，错误会向上抛给调用方foo()函数，foo()函数没有try ... catch语句，所以错误继续向上抛给调用方main()函数，main()函数有try ... catch语句，所以错误最终在main()函数被处理了
```

#### 异步错误处理  

```{.javaScript}
function printTime() {
    throw new Error();
}

try {
    setTimeout(printTime, 1000); // 捕获时，回调函数printTime还未执行
    console.log('done');
} catch (e) {
    console.log('error');
}
// 没有捕获到抛出的错误
```

**涉及到异步代码，无法在调用时捕获，原因就是在捕获的当时，回调函数并未执行。**

类似的，当我们处理一个事件时，在绑定事件的代码处，无法捕获事件处理函数的错误。  

```{.javaScript}
try {
    $(.btn).click(()=> {
      var x = parseFloat($('x').val())
      if(isNaN(X)){
        throw new Error('输入有误')
      }
    })
} catch (e) {
    console.log('error');
}
```

必须这样写  

```{.javaScript}
$(.btn).click(()=> {
  var x = parseFloat($('x').val())
  try {
    if(isNaN(X)){
      throw new Error('输入有误')
    }
  } catch (e) {
    console.log('error');
  }
})
```
