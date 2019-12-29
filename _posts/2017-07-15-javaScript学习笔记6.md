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
> 如需获得来自服务器的响应，请使用 XMLHttpRequest 对象的 responseText 或 responseXML 属性。 
- responseText 属性:responseText 属性返回字符串形式的响应  
- responseXML 属性:如果来自服务器的响应是 XML，而且需要作为 XML 对象进行解析，请使用 responseXML 属性  

#### 4. onreadystatechange 事件  
> 当请求被发送到服务器时，我们需要执行一些基于响应的任务。  
> 每当 readyState 改变时，就会触发 onreadystatechange 事件。  

- onreadystatechange:存储函数（或函数名），每当 readyState 属性改变时，就会调用该函数。  
- readyState:存有 XMLHttpRequest 的状态。从 0 到 4 发生变化。 
&emsp;&emsp;0: 请求未初始化  
&emsp;&emsp;1: 服务器连接已建立  
&emsp;&emsp;2: 请求已接收  
&emsp;&emsp;3: 请求处理中  
&emsp;&emsp;4: 请求已完成，且响应已就绪   

```
// 1.创建Ajax对象
var myajax=new XMLHttpRequest()
//alert(myajax) //IE6及其以下版本不支持
//2.连接服务器
// open(方法，文件路径，异步传输)
myajax.open('GET','test.txt',true);
//3.发送请求
myajax.send(null);
//4.接受返回的数据
myajax.onreadystatechange=function(){
    if(myajax.readyState==4){
        if (myajax.status==200) {
        //alert('成功'+myajax.responseText)
        var str=myajax.responseText;
        div1.innerHTML=str
        }else{
            alert('失败'+'\n'+myajax.statusText)
        }
    }
}
``` 
#### 封装思路  
```
function ajax(url,funSucc,fnFaild){

	var xhr=new XMLHttpRequest();

	xhr.open('GET',url,true);

	xhr.send(null);

	xhr.onreadystatechange=function(){

		if (xhr.readyState==4) {

			if (xhr.status==200) {

				funSucc(xhr.responseText)

			}else{
				if (fnFaild){
                    
				 	fnFaild(xhr.statusText)
				}
			}
		}
	}

}
```
<table>
    <caption>
        XMLHttpRequest 对象 status 和statusText 属性对照表
    </caption>
    <tbody>
        <tr>
            <th width="40">
                status</th>
            <th>
                statusText</th>
            <th width="501">
                说明</th>
        </tr>
        <tr>
            <th>
                0**</th>
            <th>
                -</th>
            <th>
                未被始化</th>
        </tr>
        <tr>
            <th>
                1**</th>
            <th>
                -</th>
            <th>
                请求收到，继续处理</th>
        </tr>
        <tr>
            <td>
                100</td>
            <td>
                Continue</td>
            <td>
                客户必须继续发出请求</td>
        </tr>
        <tr>
            <td>
                101</td>
            <td>
                Switching protocols</td>
            <td>
                客户要求服务器根据请求转换HTTP协议版本</td>
        </tr>
        <tr>
            <th>
                2**</th>
            <th>
                -</th>
            <th>
                操作成功收到，分析、接受</th>
        </tr>
        <tr>
            <td>
                200</td>
            <td>
                OK</td>
            <td>
                交易成功</td>
        </tr>
        <tr>
            <td>
                201</td>
            <td>
                Created</td>
            <td>
                提示知道新文件的URL</td>
        </tr>
        <tr>
            <td>
                202</td>
            <td>
                Accepted</td>
            <td>
                接受和处理、但处理未完成</td>
        </tr>
        <tr>
            <td>
                203</td>
            <td>
                Non-Authoritative Information</td>
            <td>
                返回信息不确定或不完整</td>
        </tr>
        <tr>
            <td>
                204</td>
            <td>
                No Content</td>
            <td>
                请求收到，但返回信息为空</td>
        </tr>
        <tr>
            <td>
                205</td>
            <td>
                Reset Content</td>
            <td>
                服务器完成了请求，用户代理必须复位当前已经浏览过的文件</td>
        </tr>
        <tr>
            <td>
                206</td>
            <td>
                Partial Content</td>
            <td>
                服务器已经完成了部分用户的GET请求</td>
        </tr>
        <tr>
            <th>
                3**</th>
            <th>
                -</th>
            <th>
                完成此请求必须进一步处理</th>
        </tr>
        <tr>
            <td>
                300</td>
            <td>
                Multiple Choices</td>
            <td>
                请求的资源可在多处得到</td>
        </tr>
        <tr>
            <td>
                301</td>
            <td>
                Moved Permanently</td>
            <td>
                删除请求数据</td>
        </tr>
        <tr>
            <td>
                302</td>
            <td>
                Found</td>
            <td>
                在其他地址发现了请求数据</td>
        </tr>
        <tr>
            <td>
                303</td>
            <td>
                See Other</td>
            <td>
                建议客户访问其他URL或访问方式</td>
        </tr>
        <tr>
            <td>
                304</td>
            <td>
                Not Modified</td>
            <td>
                客户端已经执行了GET，但文件未变化</td>
        </tr>
        <tr>
            <td>
                305</td>
            <td>
                Use Proxy</td>
            <td>
                请求的资源必须从服务器指定的地址得到</td>
        </tr>
        <tr>
            <td>
                306</td>
            <td>
                &nbsp;</td>
            <td>
                前一版本HTTP中使用的代码，现行版本中不再使用</td>
        </tr>
        <tr>
            <td>
                307</td>
            <td>
                Temporary Redirect</td>
            <td>
                申明请求的资源临时性删除</td>
        </tr>
        <tr>
            <th>
                4**</th>
            <th>
                -</th>
            <th>
                请求包含一个错误语法或不能完成</th>
        </tr>
        <tr>
            <td>
                400</td>
            <td>
                Bad Request</td>
            <td>
                错误请求，如语法错误</td>
        </tr>
        <tr>
            <td>
                401</td>
            <td>
                Unauthorized</td>
            <td>
                请求授权失败</td>
        </tr>
        <tr>
            <td>
                402</td>
            <td>
                Payment Required</td>
            <td>
                保留有效ChargeTo头响应</td>
        </tr>
        <tr>
            <td>
                403</td>
            <td>
                Forbidden</td>
            <td>
                请求不允许(由于服务器上文件或目录的权限设置导致资源不可用)</td>
        </tr>
        <tr>
            <td>
                404</td>
            <td>
                Not Found</td>
            <td>
                没有发现文件、查询或URl(没有找到指定的资源)</td>
        </tr>
        <tr>
            <td>
                405</td>
            <td>
                Method Not Allowed</td>
            <td>
                用户在Request-Line字段定义的方法不允许</td>
        </tr>
        <tr>
            <td>
                406</td>
            <td>
                Not Acceptable</td>
            <td>
                根据用户发送的Accept拖，请求资源不可访问</td>
        </tr>
        <tr>
            <td>
                407</td>
            <td>
                Proxy Authentication Required</td>
            <td>
                类似401，用户必须首先在代理服务器上得到授权</td>
        </tr>
        <tr>
            <td>
                408</td>
            <td>
                Request Timeout</td>
            <td>
                客户端没有在用户指定的饿时间内完成请求</td>
        </tr>
        <tr>
            <td>
                409</td>
            <td>
                Conflict</td>
            <td>
                对当前资源状态，请求不能完成</td>
        </tr>
        <tr>
            <td>
                410</td>
            <td>
                Gone</td>
            <td>
                服务器上不再有此资源且无进一步的参考地址</td>
        </tr>
        <tr>
            <td>
                411</td>
            <td>
                Length Required</td>
            <td>
                服务器拒绝用户定义的Content-Length属性请求</td>
        </tr>
        <tr>
            <td>
                412</td>
            <td>
                Precondition Failed</td>
            <td>
                一个或多个请求头字段在当前请求中错误</td>
        </tr>
        <tr>
            <td>
                413</td>
            <td>
                Request Entity Too Large</td>
            <td>
                请求的资源大于服务器允许的大小</td>
        </tr>
        <tr>
            <td>
                414</td>
            <td>
                Request-URI Too Long</td>
            <td>
                请求的资源URL长于服务器允许的长度</td>
        </tr>
        <tr>
            <td>
                415</td>
            <td>
                Unsupported Media Type</td>
            <td>
                请求资源不支持请求项目格式</td>
        </tr>
        <tr>
            <td>
                416</td>
            <td>
                Requested Range Not Suitable</td>
            <td>
                请求中包含Range请求头字段，在当前请求资源范围内没有range指示值，请求也不包含If-Range请求头字段</td>
        </tr>
        <tr>
            <td>
                417</td>
            <td>
                Expectation Failed</td>
            <td>
                服务器不满足请求Expect头字段指定的期望值，如果是代理服务器，可能是下一级服务器不能满足请求</td>
        </tr>
        <tr>
            <th>
                5**</th>
            <th>
                -</th>
            <th>
                服务器执行一个完全有效请求失败</th>
        </tr>
        <tr>
            <td>
                500</td>
            <td>
                Internal Server Error</td>
            <td>
                服务器产生内部错误</td>
        </tr>
        <tr>
            <td>
                501</td>
            <td>
                Not Implemented</td>
            <td>
                服务器不支持请求的函数</td>
        </tr>
        <tr>
            <td>
                502</td>
            <td>
                Bad Gateway</td>
            <td>
                服务器暂时不可用，有时是为了防止发生系统过载</td>
        </tr>
        <tr>
            <td>
                503</td>
            <td>
                Service Unavailable</td>
            <td>
                服务器过载或暂停维修</td>
        </tr>
        <tr>
            <td>
                504</td>
            <td>
                Gateway Timeout</td>
            <td>
                关口过载，服务器使用另一个关口或服务来响应用户，等待时间设定值较长</td>
        </tr>
        <tr>
            <td>
                505</td>
            <td>
                HTTP Version Not Supported</td>
            <td>
                服务器不支持或拒绝支请求头中指定的HTTP版本</td>
        </tr>
        <tr>
            <td>
                12029</td>
            <td>
                an unknown error occurred while processing the request on the server. the status code returned from the server was : 12029</td>
            <td>
                原因:网络不通. 刷新一下就知道了</td>
        </tr>
    </tbody>
</table>

### 了解GET和POST请求的区别  

### 了解同步和异步的区别  
- 同步：提交请求->等待服务器处理->处理完毕返回这个期间客户端浏览器不能干任何事  
&emsp;&emsp;当前只能做一件事，其他事情必须等当前的事情完成，才能继续后面的事情。  
&emsp;&emsp;不推荐使用,但某些情况下需要：比如当前请求的结果是下一步请求的前提，知道当前请求的结果才可以处理后续请求   
- 异步:请求通过事件触发->服务器处理（这是浏览器仍然可以作其他事情）->处理完毕  
&emsp;&emsp;同时可以做多件事情

