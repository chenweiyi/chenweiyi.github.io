---
title: '上一篇的姊妹篇-谈谈对象的offsetX,clientX,pageX,screenX,layerX,x的区别'
date: 2018-11-14 22:03:22
categories:
- 技术贴
tags:
- javascript
- html
---

今天来讲一下offsetX,clientX,pageX,screenX,layerX,x的区别，以及在chrome,firefox,edge浏览器中的相同和不同点

下面先上一段代码：

```javascript
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>offsetX,clientX,pageX,screenX,layerX,x</title>
  <style>
    body {
      height: 1600px;
      margin: 20px;
      padding: 20px;
      border: 20px solid #f38;
      background: #ccc;
      overflow: auto;
    }
    .main {
      width: 400px;
      height: 330px;
      position: relative;
      margin: 250px auto 0;
      background: #eee;
      overflow: auto;
      border: 10px solid #f38;
    }
    .box {
      width: 220px;
      height: 420px;
      position: absolute;
      left: 50px;
      top: 50px;
      background: orange;
      margin: 20px;
      padding: 20px;
      border: 10px solid #ccc;
    }
    .show {
      position: fixed;
      right: 10px;
      top: 10px;
    }
    .show > label,
    .show > span {
      display: table-cell;
    }
    .show > label {
      width: 150px;
      text-align: right;
      padding-right: 10px;
    }
    .show > span {
      width: 100px;
      text-align: left;
    }

    .show2 {
      top: 270px;
    }

  </style>
</head>
<body>
  <div class="main">
    <div class="box" id="box"></div>
  </div>
  <div class="show">
    <label>offsetX:</label><span id="offsetx"></span><br>
    <label>clientX:</label><span id="clientx"></span><br>
    <label>pageX:</label><span id="pagex"></span><br>
    <label>screenX:</label><span id="screenx"></span><br>
    <label>layerX:</label><span id="layerx"></span><br>
    <label>x:</label><span id="x"></span>
  </div>

  <div class="show show2">
    <label>offsetY:</label><span id="offsety"></span><br>
    <label>clientY:</label><span id="clienty"></span><br>
    <label>pageY:</label><span id="pagey"></span><br>
    <label>screenY:</label><span id="screeny"></span><br>
    <label>layerY:</label><span id="layery"></span><br>
    <label>y:</label><span id="y"></span>
  </div>

  <script type="text/javascript">
    var $id = function (id) {
      return document.getElementById(id)
    }
    window.onload = function () {
      $id('box').onmousedown = function (event) {
        var e = event || window.event
        $id('offsetx').innerText = e.offsetX
        $id('clientx').innerText = e.clientX
        $id('pagex').innerText = e.pageX
        $id('screenx').innerText = e.screenX
        $id('layerx').innerText = e.layerX
        $id('x').innerText = e.x

        $id('offsety').innerText = e.offsetY
        $id('clienty').innerText = e.clientY
        $id('pagey').innerText = e.pageY
        $id('screeny').innerText = e.screenY
        $id('layery').innerText = e.layerY
        $id('y').innerText = e.y
      }
    }
  </script>
</body>
</html>
```

### 分析结果

Chrome

offsetX --- 点击位置相对padding左上角的距离，因此如果存在border，点击在border上为负值，点击在padding内为正值
clientX --- 相对于浏览器左侧可视区域的距离
pageX --- 相对于浏览器左侧页面的滚动距离 = clientX + 滚动条滚动距离， 因此在没有滚动条的情况下等于clientX，有滚动条的时候也可能大于screenX
screenX --- 相对于显示屏左侧的距离
layerX --- 点击位置相对于border左上角的距离，因此如果存在border，其值等于offsetX + border-left
x --- 等于clientX 

Firefox

offsetX --- 与chrome一致
clientX --- 与chrome一致
pageX --- 与chrome一致
screenX --- 与chrome一致
layerX --- 与chrome一致
x --- 与chrome一致 

Edge

offsetX --- 与chrome一致
clientX --- 与chrome一致
pageX --- 与chrome一致
screenX --- 与chrome一致
layerX --- 这个值与chrome和firefox行为不一致，其值相对于父容器的border左上角的距离
x --- 与chrome一致

### 注意

没有scrollX，scrollY属性