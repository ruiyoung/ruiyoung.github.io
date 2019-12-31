--- 
layout:     post
title:      javaScript学习笔记
subtitle:   正则表达式RegExp
date:       2017-07-15
author:     Ruiyoung
header-img: img/post-bg-js-version.jpg
catalog: true
tags:
    - javaScript
---
### 前言  

> RegExp 对象表示正则表达式，它是对字符串执行模式匹配的强大工具。  
> 正则表达式简洁且功能强大，通常用来匹配字符串，比如在表单验证中检验用户输入是否合法。它并不仅仅在JavaScript中可以使用，众多的高级编程语言都支持正则表达式。  

#### 字符串常用操作方法  

- search() 方法用于检索字符串中指定的子字符串，或检索与正则表达式相匹配的子字符串。(该参数可以是需要在 stringObject 中检索的子串，也可以是需要检索的 RegExp 对象。)  
- match() 方法可在字符串内检索指定的值，或找到一个或多个正则表达式的匹配。(该方法类似 indexOf() 和 lastIndexOf()，但是它返回指定的值，而不是字符串的位置。)  
- replace() 方法用于在字符串中用一些字符替换另一些字符，或替换一个与正则表达式匹配的子串。 
- split() 方法用于把一个字符串分割成字符串数组。  

#### 创建正则表达式的两种方法  

- new RegExp(pattern, attributes);  
- /pattern/attributes  
- 参数介绍：  
&emsp;&emsp;参数 pattern 是一个字符串，指定了正则表达式的模式或其他正则表达式。  
&emsp;&emsp;参数 attributes 是一个可选的字符串，包含属性 "g"、"i" 分别用于指定全局匹配、区分大小写的匹配。  
- 返回值：一个新的 RegExp 对象，具有指定的模式和标志。  

#### 修饰符  

| 修饰符 | 描述 |  
| :------: | :------|
| i | 执行对大小写不敏感的匹配。|
| g | 执行全局匹配（查找所有匹配而非在找到第一个匹配后停止）。|
| m | 执行多行匹配。|

#### 方括号  

用于查找某个范围内的字符  

| 表达式 | 描述 |
| :------: | :------|
| [abc] | 查找方括号之间的任何字符。 |
| [^abc] | 查找任何不在方括号之间的字符。 |
| [0-9] | 查找任何从 0 至 9 的数字。 |  
| [a-z] | 查找任何从小写 a 到小写 z 的字符。 |
| [A-Z] | 查找任何从大写 A 到大写 Z 的字符。 |
| [A-z] | 查找任何从大写 A 到小写 z 的字符。 |  
| [adgk] | 查找给定集合内的任何字符。 |  
| [^adgk] | 查找给定集合外的任何字符。 |  
| (red \| blue \| green) | 查找任何指定的选项。|

#### 元字符  

拥有特殊含义的字符  

| 元字符 | 描述 |
| :------: | :------ |
| . | 查找单个字符，除了换行和行结束符。|  
| \w | 查找单词字符(包括英文字母，数字，下划线)。|
| \W | 查找非单词字符(包括非英文字母，数字，下划线之外的字符)。 |
| \d | 查找数字。|
| \D | 查找非数字字符。|
| \s | 查找空白字符。 |
| \S | 查找非空白字符。|
| \b | 匹配单词边界。|
| \0 | 查找 NUL 字符。|
| \n |　查找换行符。　|
| \f  | 查找换页符。 |
| \r | 查找回车符。 |
| \t | 查找制表符。 |
| \v | 查找垂直制表符。|
| \xxx | 查找以八进制数 xxx 规定的字符。|
| \xdd | 查找以十六进制数 dd 规定的字符。|
| \uxxxx | 查找以十六进制数 xxxx 规定的 Unicode 字符。|

#### 量词  

| 量词 | 描述 |
| :------: | :------ |
| n+ | 匹配任何包含至少一个 n 的字符串。|
| n* | 匹配任何包含零个或多个 n 的字符串。 |
| n? | 匹配任何包含零个或一个 n 的字符串。 |
| n{X} | 匹配包含 X 个 n 的序列的字符串。 |
| n{X,Y} | 匹配包含 X 或 Y 个 n 的序列的字符串。|
| n{X,} | 匹配包含至少 X 个 n 的序列的字符串。 |
| n$ | 匹配任何结尾为 n 的字符串。 |
| ^n  | 匹配任何开头为 n 的字符串。 |
| ?=n | 匹配任何其后紧接指定字符串 n 的字符串。|
| ?!n | 匹配任何其后没有紧接指定字符串 n 的字符串。 |

#### RegExp 对象属性  

- **global RegExp 对象是否具有标志 g。**  
&emsp;&emsp;语法：RegExpObject.global  
&emsp;&emsp;如果 g 标志被设置，则该属性为 true，否则为 false。  
- **ignoreCase RegExp 对象是否具有标志 i。**  
&emsp;&emsp;语法：RegExpObject.ignoreCase  
&emsp;&emsp;如果设置了 "i" 标志，则返回 true，否则返回 false。  
- **multiline RegExp 对象是否具有标志 m。**  
&emsp;&emsp;语法：RegExpObject.multiline  
&emsp;&emsp;如果 m 标志被设置，则该属性为 true，否则为 false。  
- **source 正则表达式的源文本。**  
&emsp;&emsp;语法：RegExpObject.source  
&emsp;&emsp;source 属性用于返回模式匹配所用的文本。该文本不包括正则表达式直接量使用的定界符，也不包括标志 g、i、m。  
- **lastIndex 一个整数，标示开始下一次匹配的字符位置。**  
&emsp;&emsp;语法：RegExpObject.lastIndex  
&emsp;&emsp;该属性存放一个整数，它声明的是上一次匹配文本之后的第一个字符的位置。多用于在一个字符串中进行多次匹配  
&emsp;&emsp;上次匹配的结果是由方法 RegExp.exec() 和 RegExp.test() 找到的，它们都以 lastIndex 属性所指的位置作为下次检索的起始点。这样，就可以通过反复调用这两个方法来遍历一个字符串中的所有匹配文本。  
&emsp;&emsp;不具有标志 g 和不表示全局模式的 RegExp 对象不能使用 lastIndex 属性。  

#### RegExp 对象方法  

- compile 编译正则表达式。  
&emsp;&emsp;compile 方法将正则表达式转换为内部的格式，从而执行得更快。例如，这允许在循环中更有效地使用正则表达式。当重复使用相同的表达式时，编译过的正则表达式使执行加速。

- test 检索字符串中指定的值。返回 true 或 false。
&emsp;&emsp;语法：RegExpObject.test(string)  
&emsp;&emsp;如果字符串 string 中含有与 RegExpObject 匹配的文本，则返回 true，否则返回 false。

- exec 检索字符串中指定的值。返回找到的值，并确定其位置。
&emsp;&emsp;如果 exec 方法没有找到匹配，将返回 null。如果找到匹配项，则 exec 方法返回一个数组。
&emsp;&emsp;数组元素 0 包含了完整的匹配项，而元素 1 到 n 包含的是匹配项中出现的任意一个子匹配项。
&emsp;&emsp;除了数组元素和 length 属性之外，exec() 方法还返回两个属性。index 属性声明的是匹配文本的第一个字符的位置。input 属性则存放的是被检索的字符串 string。在调用非全局的 RegExp 对象的 exec() 方法时，返回的数组与调用方法 String.match() 返回的数组是相同的。
&emsp;&emsp;当 RegExpObject 是一个全局正则表达式时，exec() 会在 RegExpObject 的 lastIndex 属性指定的字符处开始检索字符串 string。当 exec() 找到了与表达式相匹配的文本时，在匹配后，它将把 RegExpObject 的 lastIndex 属性设置为匹配文本的最后一个字符的下一个位置。
