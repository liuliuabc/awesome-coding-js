---
{
"title": "HTML5",
}
---
## HTML5
HTML5是HTML的第5个版本。
- HTML5 拥有更强的语义
- 图形以及多媒体元素。canvas/video/embed/audio/svg/CSS3-2D/3D转换
  - video/audio的常用属性：autoplay/controls/loop
  - SVG (Scalable Vector Graphics)SVG 指可伸缩矢量图形，图像在放大或改变尺寸的情况下其图形质量不会有损失， 是一种使用 XML 描述 2D 图形的语言。 
  ```html
  //圆形
  <svg xmlns="http://www.w3.org/2000/svg" version="1.1">
     <circle cx="100" cy="50" r="40" stroke="black" stroke-width="2" fill="red" />
  </svg>
  ```
  - Canvas 通过 JavaScript 来绘制 2D 图形。最适合图像密集型的游戏，其中的许多对象会被频繁重绘。
  ```html
  <canvas id="myCanvas" width="200" height="100"></canvas>
  ```
  ```js
  var c=document.getElementById("myCanvas");
  var ctx=c.getContext("2d");
  ctx.fillStyle="#FF0000";
  ctx.fillRect(0,0,150,75);
  ```
- 跨平台能力更强。
- 对本地离线存储的更好的支持。
  - 本地 SQL 数据
  - 访问本地文件
  - 本地数据存储
- 新的语义元素，比如 article、footer、header、nav、section
- 新的表单控件，比如 calendar、date、time、email、url、search

### HTML5支持和兼容性
现代的浏览器都支持 HTML5，只有Internet Explorer 9以下版本不支持 。 IE9 以下版本浏览器兼容HTML5的方法。添加html5shiv.js
```js
<!--[if lt IE 9]>
    <script src="http://cdn.static.runoob.com/libs/html5shiv/3.7/html5shiv.min.js"></script>
<![endif]-->
```
### HTML5存储功能
#### localStorage 和 sessionStorage
localStorage - 用于长久保存整个网站的数据，保存的数据没有过期时间，直到手动去除。

sessionStorage - 用于临时保存同一窗口(或标签页)的数据，在关闭窗口或标签页之后将会删除这些数据。

在使用 web 存储前,应检查浏览器是否支持 localStorage 和 sessionStorage:
```js
if(typeof(Storage)!=="undefined")
{
// 是的! 支持 localStorage  sessionStorage 对象!
// 一些代码.....
} else {
// 抱歉! 不支持 web 存储。
}
```
不管是 localStorage，还是 sessionStorage，可使用的API都相同，常用的有如下几个（以localStorage为例）：

- 保存数据：localStorage.setItem(key,value);
  - 也可以 localStorage.something="something"
- 读取数据：localStorage.getItem(key);
  - 也可以 localStorage.something
- 删除单个数据：localStorage.removeItem(key);
- 删除所有数据：localStorage.clear();
- 得到某个索引的key：localStorage.key(index);

#### Web SQL
引入了一组使用 SQL 操作客户端数据库的 APIs，以下是规范中定义的三个核心方法：

openDatabase：这个方法使用现有的数据库或者新建的数据库创建一个数据库对象。
transaction：这个方法让我们能够控制一个事务，以及基于这种情况执行提交或者回滚。
executeSql：这个方法用于执行实际的 SQL 查询。
```js
var db = openDatabase('mydb', '1.0', 'Test DB', 2 * 1024 * 1024);
db.transaction(function (tx) {
   tx.executeSql('CREATE TABLE IF NOT EXISTS LOGS (id unique, log)');
   tx.executeSql('INSERT INTO LOGS (id, log) VALUES (1, "菜鸟教程")');
   tx.executeSql('INSERT INTO LOGS (id, log) VALUES (2, "www.runoob.com")');
});

```
#### 应用程序缓存Application Cache(已废除)
使用 HTML5，通过创建 cache manifest 文件，可以轻松地创建 web 应用的离线版本（类似于手机APP）。注意：manifest 的技术已被 web 标准废弃，不再推荐使用此功能。

应用程序缓存为应用带来三个优势：
- 离线浏览 - 用户可在应用离线时使用它们
- 速度 - 已缓存资源加载得更快
- 减少服务器负载 - 浏览器将只从服务器下载更新过或更改过的资源。

manifest 文件是简单的文本文件，它告知浏览器被缓存的内容（以及不缓存的内容）。

manifest 文件可分为三个部分：
- CACHE MANIFEST - 在此标题下列出的文件将在首次下载后进行缓存
- NETWORK - 在此标题下列出的文件需要与服务器的连接，且不会被缓存
- FALLBACK - 在此标题下列出的文件规定当页面无法访问时的回退页面（比如 404 页面）
```txt
CACHE MANIFEST
# 2012-02-21 v1.0.0
/theme.css
/logo.gif
/main.js

NETWORK:
login.php

FALLBACK:
/html/ /offline.html
```

manifest文件使用
```html
<!DOCTYPE HTML>
<html manifest="demo.appcache">
<body>
文档内容......
</body>
</html>
```
一旦应用被缓存，它就会保持缓存直到发生下列情况：
- 用户清空浏览器缓存
- manifest 文件被修改（参阅下面的提示）
- 由程序来更新应用缓存

### Web Workers

web worker 是运行在后台的 JavaScript，独立于其他脚本，不会影响页面的性能。您可以继续做任何愿意做的事情：点击、选取内容等等，而此时 web worker 在后台运行。

在创建 web worker 之前，请检测用户的浏览器是否支持它：
```js
if(typeof(Worker)!=="undefined")
{
// 是的! Web worker 支持!
// 一些代码.....
}
else
{
//抱歉! Web Worker 不支持
}
```
创建 worker.js
```js
var i=0;

function timedCount()
{
    i=i+1;
    postMessage(i);
    setTimeout("timedCount()",500);
}
timedCount();

```
使用 worker.js
```html
<!DOCTYPE html>
<html>
<head> 
<meta charset="utf-8"> 
<title>菜鸟教程(runoob.com)</title> 
</head>
<body>
<p>计数： <output id="result"></output></p>
<button onclick="startWorker()">开始工作</button> 
<button onclick="stopWorker()">停止工作</button>
<p><strong>注意：</strong> Internet Explorer 9 及更早 IE 版本浏览器不支持 Web Workers.</p>
<script>
var w;
function startWorker() {
    if(typeof(Worker) !== "undefined") {
        if(typeof(w) == "undefined") {
            w = new Worker("demo_workers.js");
        }
        w.onmessage = function(event) {
            document.getElementById("result").innerHTML = event.data;
        };
    } else {
        document.getElementById("result").innerHTML = "抱歉，你的浏览器不支持 Web Workers...";
    }
}
function stopWorker() 
{ 
    w.terminate();
    w = undefined;
}
</script>
</body>
</html>

```
### WebSocket
WebSocket 是 HTML5 开始提供的一种在单个 TCP 连接上进行全双工通讯的协议。

```js
      <script type="text/javascript">
         function WebSocketTest()
         {
            if ("WebSocket" in window)
            {
               alert("您的浏览器支持 WebSocket!");
               // 打开一个 web socket
               var ws = new WebSocket("ws://localhost:9998/echo");
               ws.onopen = function(){
                  // Web Socket 已连接上，使用 send() 方法发送数据
                  ws.send("发送数据");
                  alert("数据发送中...");
               };
               ws.onmessage = function (evt){ 
                  var received_msg = evt.data;
                  alert("数据已接收...");
               };
               ws.onclose = function(){ 
                  // 关闭 websocket
                  alert("连接已关闭..."); 
               };
            }
         }
      </script>
```