---
layout: post
title: CORS (跨域资源共享)
date: 2017-08-27 17:56:56
tags: 
	跨域
categories:
	网络
comments: true
---
### jsonp的缺陷
1)只能进行GET请求
2)缺乏有效的错误捕获机制。因此在XMLHttpRequest v2标准下，提出了CORS(Cross Origin Resourse-Sharing)的模型，试图保证跨域请求的安全性。
<!-- more --> 
### CORS(跨域资源共享) 
CORS（Cross-Origin Resource Sharing）是W3C在05年提出的跨域资源请求机制，它要求当前域（常规为存放资源的服务器）在响应报头添加Access-Control-Allow-Origin标签，从而允许指定域的站点访问当前域上的资源。客户端的实现跟常规的请求没啥出入，不过CORS默认只支持GET/POST这两种http请求类型，如果要开启PUT/DELETE之类的方式，需要在服务端在添加一个"Access-Control-Allow-Methods"报头标签(ie6,7不行！)
#### CORS兼容性
<img src="/imgs/cors.png" width="700" height="300"> 
IE使用XDomainRequest，实现了cors的部分规范，请求只支持GET和POST，协议只支持http和https，XDomainRequest()只接受两个参数method和url，所有请求都是异步执行的的，不能创建同步请求，触发load事件，访问的数据保存在responseText中。
```javascript
var xdr = new XDomainRequest();
		xdr.onload = function () {
			console.log(xdr.responseText);
		}
		xdr.open('GET', 'www.baidu.com');
		xdr.send(null);
```
其他浏览器使用XMLHttpRequest level2
#### CORS实现跨域
```javascript
		function creatCORS(method, url, flag) {
			var cors = new XMLHttpRequest();
			if ('withCredentials' in cors) {
              //XMLHttpRequest level2 独有的属性				
              //标准的CORS请求，默认情况下是不会发送或者设置cookie值的。为了再请求时，附带cookies，我们需要设置XMLHttpRequest的withCredentials属性为true
				cors.open(method, url, flag);
			}else if (typeof XDomainRequest != 'undefined') {
            //兼容ie8+
				cors = new XDomainRequest();
				cors.open(method, url);
			}else {
				cors = null;
			}
			return cors;
		}
        //创建cors请求
		function makeCorsRequest(method, url, flag, callBack) {
			var cors = creatCORS(method, url, flag);
			if (!cors) {
				alert('CORS is not supported');
			}
			cors.onload = function () {
				callBack(cors.responseText);
			}
			cors.onerror = function () {
				console.log('CORS error!');
			}
			cors.send();
		}
```

