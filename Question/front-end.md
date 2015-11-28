## Front End Questions

> 1. NodeType

存在12种不同的节点类型,常见的有:

- 1 => Element
- 2 => Attr
- 8 => Comment

> 2. css中 background-color填充的content范围

background-color为元素设置一种纯色, 会填充元素的内容,内边距和边框(不包括外边距).

> 3. HTTP,哪些属性控制HTTP缓存

HTTP协议中关于缓存的信息头关键字包括,Cache Control(1.1),Pragma(1.0),last-Modified,Expires等.

HTTP1.1中启用Cache-Control 来控制页面的缓存与否,常用的几个属性

- no-cache
- public
- no-store
- must-revalidate
- Last-Modified
- Expires

> 4. Html Onload 事件

onload属性在对象已加载时触发,常用在 `<body>`中,一旦完全加载所有内容(包括图像,脚本文件,css等),就执行一段脚本.

> 5. js中的substring和substr

- substring
  - 用于提取字符串中介于两个指定下标之间的字符
  - substring(start,end)

- substr
  - 方法用于返回一个从指定位置开始的指定长度的子字符串
  - stringObject.substr(start [, length ])

> 6. html5 中哪些标签可以用来播放声音

- `<embed height="100" width="100" src="song.mp3" />`
- `<object height="100" width="100" data="song.mp3"></object>`
- `<audio controls="controls"><source src="song.mp3" type="audio/mp3" /></audio>`

> 7. Javascript 同源策略

 同源策略是由Netscape提出的一个著名的安全策略,防止其他网页对本网页的非法篡改,同源就是指域名、协议、端口相同


> 8.常见的http status code

- 2xx(成功)
  - 200 => success
- 3xx(重定向)
  - 304 => 未修改(自从上次请求后，请求的网页未修改过。服务器返回此响应时，不会返回网页内容。)
- 4xx(请求错误)
  - 400 => bad request
  - 401 => 未授权
  - 403 => forbidden
  - 404 => not found
  - 408 => timeout
- 5xx(服务器错误)
  - 500 => server error
  - 503 => server timeout

> 9. 原生javascript中, 如何停止事件冒泡和阻止浏览器默认行为事件

- 阻止冒泡事件

```js
var el = window.document.getElementById("a");
    el.onclick = function (e) {
        //如果提供了事件对象，则这是一个非IE浏览器
        if (e && e.stopPropagation) {
            //因此它支持W3C的stopPropagation()方法
            e.stopPropagation();
        }
        else {
            //否则，我们需要使用IE的方式来取消事件冒泡
            window.event.cancelBubble = true;
            return false;
        }
    }
```

- 阻止默认事件

```js
 var el = window.document.getElementById("a");
    el.onclick = function (e) {
        //如果提供了事件对象，则这是一个非IE浏览器
        if (e && e.preventDefault) {
            //阻止默认浏览器动作(W3C)
            e.preventDefault();
        }
        else {
            //IE中阻止函数器默认动作的方式
            window.event.returnValue = false;
            return false;
        }
    }
```

> 10. userAgent获取

> 11. css生效的顺序

  - id选择器指定的样式 > 类选择器指定的样式 > 元素类型选择器指定的样式
  - 对于相同类型选择器制定的样式，在样式表文件中，越靠后的优先级越高
  - !important 使优先级变高

> 12. web安全

- sql注入
- CSRF
- XSS

> 13. http method

- GET / POST / PUT / DELETE / HEAD
- TRACE
- OPTIONS => 获取指定url中能接收的请求方法
- CONNECT => 连接指定频段(客户端需要通过代理服务器连接HTTPS服务器是用到)
