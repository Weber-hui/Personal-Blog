---

title: iframe父子组件跨域实现通信
toc: 1
date: 2021-1-11 23:08:08
tags:
- iframe
categories:
- iframe
---

![image](https://ss2.bdstatic.com/70cFvnSh_Q1YnxGkpoWK1HF6hhy/it/u=2050318681,1081448419&fm=26&gp=0.jpg)

 今天项目中有涉及到一个iframe父子组件跨域通信的问题，于是我就去小小的探索了一番，主要因为父子组件域名不同所以只有这种方法，只能通过window.postMessage()方法来向其他页面发送信息，其他页面要通过window.addEventListener()监听事件来接收信息。

<!--more-->

### 1.window.postMessage() 跨域发送消息

```js
otherWindow.postMessage(message, targetOrigin, [transfer]);
```
 otherWindow

    其他窗口的一个引用，写的是你要通信的window对象。
    
    例如：在iframe中向父窗口传递数据时，可以写成window.parent.postMessage()，window.parent表示父窗口。
 message

    需要传递的数据，字符串或者对象都可以。
 targetOrigin

    表示目标窗口的源，协议+域名+端口号，如果设置为“*”，则表示可以传递给任意窗口。在发送消息的时候，如果目标窗口的协议、域名或端口这三者的任意一项不匹配targetOrigin提供的值，那么消息就不会被发送；只有三者完全匹配，消息才会被发送。例如：
    window.parent.postMessage('hello world','http://xxx.com:8080')

    只有父窗口是http://xxx.com:8080时才会接受到传递的消息。

 [transfer]

    可选参数。是一串和message 同时传递的 Transferable 对象，这些对象的所有权将被转移给消息的接收方，而发送一方将不再保有所有权。我们一般很少用到。

### 2.跨域接收消息

需要监听的事件名为"message"

```
window.addEventListener('message', function (e) {
  console.log(e.data) //e.data为传递过来的数据
  console.log(e.origin) //e.origin为调用 postMessage 时消息发送方窗口的 origin（域名、协议和端口）
  console.log(e.source) //e.source为对发送消息的窗口对象的引用，可以使用此来在具有不同origin的两个窗口之间建立双向通信
})
```

### 3.示例Demo

 父页面代码：
```html
<body>
  <button onClick="sendInfo()">向子窗口发送消息</button>
  <iframe id="sonIframe" src="http://192.168.1.15/son.html"></iframe>
  <script type="text/javascript">
 
    var info = {
      message: "Hello Son!"
    };
    //发送跨域信息
    function sendInfo(){
      var sonIframe= document.getElementById("sonIframe");
      sonIframe.contentWindow.postMessage(info, '*');
    }
    //接收跨域信息
    window.addEventListener('message', function(e){
        alert(e.data.message);
    }, false);
  </script>
</body>
```
 子页面代码


```html
<body>
  <button onClick="sendInfo()">向父窗口发送消息</button>
  <script type="text/javascript">
 
    var info = {
      message: "Hello Parent!"
    };
    //发送跨域信息
    function sendInfo(){
      window.parent.postMessage(info, '*'); // *代表所有域名都可以接收到
       // window.parent.postMessage(info, 'http://192.168.1.15'); // 代表只有http://192.168.1.15可以收到消息
    }
    //接收跨域信息
    window.addEventListener('message', function(e){
        alert(e.data.message);
    }, false);
  </script>
</body>
```












