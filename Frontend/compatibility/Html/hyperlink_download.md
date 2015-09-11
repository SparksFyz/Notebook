## The Attribute  Download Of HyperLink Tag

> KeyWord : <a href="xxx.jpg" download="fileName.jpg"></a>

data:image/png;base64, iVBORw0KGgoAAAANSUhEUgAAAAEAAAAkCAYAAAB...

### Summary

Chrome and FF can use `href` and `download` attributes to generate a download frame for user can
download the file.

> However, this method does not support IE,Safari and Opera.

### 栗子

> This compatibility problem can be resolve by ` window.navigator.msSaveBlob()` .

#### Html

```html
<!DOCTYPE html>
<html>
<head>
    <title>title</title>
</head>
<body>
    <img id="img1" src="data:image/png;base64,iVBORw0KGgoAAAANSUh..." />
    <canvas id="canvas1"></canvas>
</body>
</html>
```
#### Javascipt

```javascript
  var image = document.getElementById('img1');
        var canvas = document.getElementById('canvas1');
        var ctx = canvas.getContext('2d');
        ctx.drawImage(image, 0, 0, image.width, image.height);
        window.navigator.msSaveBlob(canvas.msToBlob(), 'drawingFileName.png');
```

> If the <img> tag is replaced by <a> tag ,we can just get the base64 and send it to a <canvs> tag.

### In FF and Chrome

#### 栗子

> 当我们需要点击一个元素(例如button或者a)  去触发下载一个canvas或者img标签中的图片文件.

1. 获取到canvas或者img标签中图片的base64编码
2. 通过Dom操作生成一个a标签
3. 通过自定义事件去触发a标签中的download属性

```javascript
_download = function(imageData, filename) {
        var event, saveLink;
        saveLink = document.createElementNS('http://www.w3.org/1999/xhtml', 'a');
        saveLink.href = imageData;
        saveLink.download = _filenameEndode(filename);
        event = document.createEvent('MouseEvents');
        event.initMouseEvent('click', true, false, window, 0, 0, 0, 0, 0, false, false, false, false, 0, null);
        saveLink.dispatchEvent(event);
      };
```
