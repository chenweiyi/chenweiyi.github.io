---
title: '谈谈clientXXX, offsetXXX, scrollXXX的用法'
date: 2018-11-06 20:44:28
categories:
- 技术贴
tags:
- javascript
- html
---

今天我想来探索一下几个经常搞混淆的概念，比如：

1. `clientWidth`, `offsetWidth`, `scrollWidth`有什么区别？

2. `clientHeight`, `offsetHeight`, `scrollHeight`有什么区别？

3. `clientTop`, `offsetTop`, `scrollTop`有什么区别？

4. `clientLeft`, `offsetLeft`, `scrollLeft`有什么区别？ 

5. 上面这些属性和`style.width`, `style.height`, `style.left`, `style.top`有什么区别？

6. `div.clientXXX` 和 `document.body.clientXXX` 有没有区别？

7. 不同浏览器之间有没有什么不同？（主要是chrome，firefox，Edge）

要解答以上问题，最直接的方式是写一个demo测试一下，下面放出测试代码。

```javascript
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>clientTop, offsetTop, srcollTop各属性介绍</title>
  <style type="text/css">
    html, body {
      /*width: 100%;
      height: 100%;*/
    }
    html {
      margin: 0px;
      padding: 0px;
    }
    body {
      border: 10px solid #cc9999;
      padding: 10px;
      margin: 10px;
      background-color: #ffffcc;
    }
    .wrap {
      position: relative;
      margin: 10px;
      padding: 10px;
      border: 10px solid #333399;

    }
    .div {
      position: relative;
      width: 300px;
      height: 200px;
      overflow: auto;
      top: 20px;
      left: 20px;
      border: 10px solid #ff6666;
      margin: 10px;
      padding: 10px;
      background-color: #ccffff;
      /*box-sizing: border-box;*/
    }
    .div > pre,
    .div > p {
      margin: 0px;
    }
    .divide {
      margin-top: 50px;
      margin-bottom: 20px;
    }
    .divide > div {
      width: 90%;
      height: 1px;
      border: 0px;
      margin: 0 auto;
      border-bottom: 1px dashed #555;
    }
    .result {
      display: inline-block;
      width: 400px;
      vertical-align: top;
    }
  </style>
  <script type="text/javascript">
    var $$ = function (id) {
      return document.querySelector('#' + id)
    }

    var calc = function () {
      var body = document.body
      // ----------------------  body  --------------------------
      $$('bodyClientWidth').innerText = body.clientWidth
      $$('bodyOffsetWidth').innerText = body.offsetWidth
      $$('bodyScrollWidth').innerText = body.scrollWidth

      $$('bodyClientHeight').innerText = body.clientHeight
      $$('bodyOffsetHeight').innerText = body.offsetHeight
      $$('bodyScrollHeight').innerText = body.scrollHeight

      $$('bodyStyleTop').innerText = body.style.top      // 必须定义在行内样式中，否则取不到这个值
      $$('bodyStyleLeft').innerText = body.style.left    // 必须定义在行内样式中，否则取不到这个值

      $$('bodyClientLeft').innerText = body.clientLeft
      $$('bodyClientTop').innerText = body.clientTop

      $$('bodyOffsetLeft').innerText = body.offsetLeft
      $$('bodyOffsetTop').innerText = body.offsetTop

      $$('bodyScrollLeft').innerText = body.scrollLeft
      $$('bodyScrollTop').innerText = body.scrollTop

      $$('bodyStylePadding').innerText = body.style.padding      // 必须定义在行内样式中，否则取不到这个值
      $$('bodyStyleMargin').innerText = body.style.margin        // 必须定义在行内样式中，否则取不到这个值
      $$('bodyStyleWidth').innerText = body.style.width          // 必须定义在行内样式中，否则取不到这个值
      $$('bodyStyleHeight').innerText = body.style.height        // 必须定义在行内样式中，否则取不到这个值



      // -------------------    div    --------------------------
      $$('clientWidth').innerText = $$('div').clientWidth       // Chrome: 内容可视宽度，值不等于style.width，包括内容区域和padding,不包括border; Firefox: 和chrome一致
      $$('offsetWidth').innerText = $$('div').offsetWidth       // Chrome: 在clientWidth的基础上 + border水平宽度；firefox和chrome一致
      $$('scrollWidth').innerText = $$('div').scrollWidth       // Chrome: 整个滚动内容区宽度 = 内容可视宽度 + scrollLeft(滚动条滑到最右边时); Firefox: 和chrome一致
      $ && $('#jqWidth').text($('#div').width())                // 值是数值，等于style.width的值，    
      $ && $('#jqOuterWidth').text($('#div').outerWidth())      // 值是数值，等于div.offsetWidth的值

      $$('clientHeight').innerText = $$('div').clientHeight     // Chrome: 内容可视高度，值不等于style.height，包括内容区域和padding,不包括border; Firefox: 和chrome一致
      $$('offsetHeight').innerText = $$('div').offsetHeight     // Chrome: 在clientHeight的基础上 + border水平高度；firefox和chrome一致
      $$('scrollHeight').innerText = $$('div').scrollHeight     // Chrome: 整个滚动内容区宽度 = 内容可视高度 + scrollTop(滚动条滑到最底部时); Firefox: 和chrome一致
      $ && $('#jqHeight').text($('#div').height())              // 值是数值，等于style.height的值， 
      $ && $('#jqOuterHeight').text($('#div').outerHeight())    // 值是数值，等于div.offsetHeight的值

      $$('styleTop').innerText = $$('div').style.top            // 必须定义在行内样式中，否则取不到这个值
      $$('styleLeft').innerText = $$('div').style.left          // 必须定义在行内样式中，否则取不到这个值

      $$('clientLeft').innerText = $$('div').clientLeft         // Chrome: 左侧border宽度； Firefox: 和chrome一致
      $$('clientTop').innerText = $$('div').clientTop           // Chrome: 上侧border宽度； Firefox: 和chrome一致
      // 与当前节点的offsetParent有关
      // Chrome: 当offsetParent 为 Body 时，此时值为 :
      // Body的margin + border + padding + 当前节点的margin + left(当前节点如果定位了包括relative, absolute，否则，不需要这个值)
      // 当offsetParent 为其它元素时，以这个元素为基准，此时值为：
      // 当前offsetParent元素的padding + 当前节点的margin + left(当前节点如果定位了包括relative, absolute，否则，不需要这个值)
      // Firefox: 当offsetPrent 为 Body 时，去掉Body的border，其它和Chrome一样
      // 当offsetParent为其它元素时，和chrome行为一致
      $$('offsetLeft').innerText = $$('div').offsetLeft
      // 与offsetLeft算法一致，只不过需要把对应的left换成top         
      $$('offsetTop').innerText = $$('div').offsetTop

      // 当元素作为容器时，随着滚动条移动值不断变化，chrome和firefox行为一致
      // scrollLeft 表示滚动条右移时，表示右移动值，单位像素
      // scrollLeft 和 scrollTop 是可写的
      $$('scrollLeft').innerText = $$('div').scrollLeft
      // scrollTop 表示滚动条下移时，表示下移动值，单位像素
      $$('scrollTop').innerText = $$('div').scrollTop


      $$('stylePadding').innerText = $$('div').style.padding      // 必须定义在行内样式中，否则取不到这个值
      $$('styleMargin').innerText = $$('div').style.margin        // 必须定义在行内样式中，否则取不到这个值
      $$('styleWidth').innerText = $$('div').style.width          // 必须定义在行内样式中，否则取不到这个值
      $$('styleHeight').innerText = $$('div').style.height        // 必须定义在行内样式中，否则取不到这个值
    }

    var getOffsetParent = function (id) {
      var offsetParent = $$(id).offsetParent
      console.log(`offsetParent.tagName: $${offsetParent.tagName}`)
    }
  </script>
</head>
<body onscroll="calc()">
  <div class="wrap">
    <div class="div" id="div" onscroll="calc()">
      <p>sdfsdfsadfsadfa<br>sdfasdfasdfasdf<br>sdfsdfsdfasdfsdfsdffffffffffffdsfasdfsadfsadfsdfsdfsadfsadfsadfsdfsd<br>ffffff士大夫撒旦法师fff<br>fffffff<br>ffffffffff<br>ffffsadf<br>sadfasdfsd<br>fsadsfa<br>sdfasdfasdf<br>asdfas<br>dfas<br>dfasdfsadfsd<br>afasdf<br>sadfsafsadf</p>
      
    </div>
    <div class="divide">
      <div></div>
    </div>
    <div class="result">
      <h2>body</h2>
      <!-- xxxWidth -->
      <code>body.clientWidth</code>: <span id="bodyClientWidth"></span><br>
      <code>body.offsetWidth</code>: <span id="bodyOffsetWidth"></span><br>
      <code>body.scrollWidth</code>: <span id="bodyScrollWidth"></span><br>
      <br>
      <!-- xxxHeight -->
      <code>body.clientHeight</code>: <span id="bodyClientHeight"></span><br>
      <code>body.offsetHeight</code>: <span id="bodyOffsetHeight"></span><br>
      <code>body.scrollHeight</code>: <span id="bodyScrollHeight"></span><br>
      <br>  
      <!-- left, top -->
      <code>body.style.top</code>: <span id="bodyStyleTop"></span><br> 
      <code>body.style.left</code>: <span id="bodyStyleLeft"></span><br>
      <br>
      <!-- padding, margin, border, width, height -->
      <code>body.style.padding</code>: <span id="bodyStylePadding"></span><br> 
      <code>body.style.margin</code>: <span id="bodyStyleMargin"></span><br> 
      <code>body.style.width</code>: <span id="bodyStyleWidth"></span><br> 
      <code>body.style.height</code>: <span id="bodyStyleHeight"></span><br> 
      <br>
      <!-- xxxLeft, xxxTop -->
      <code>body.clientLeft</code>: <span id="bodyClientLeft"></span><br> 
      <code>body.clientTop</code>: <span id="bodyClientTop"></span><br> 
      <code>body.offsetLeft</code>: <span id="bodyOffsetLeft"></span><br> 
      <code>body.offsetTop</code>: <span id="bodyOffsetTop"></span><br>
      <code>body.scrollLeft</code>: <span id="bodyScrollLeft"></span><br> 
      <code>body.scrollTop</code>: <span id="bodyScrollTop"></span><br> 
      <br>
    </div>
    <div class="result">
      <h2>div</h2>
      <!-- xxxWidth -->
      <code>div.clientWidth</code>: <span id="clientWidth"></span><br>
      <code>div.offsetWidth</code>: <span id="offsetWidth"></span><br>
      <code>div.scrollWidth</code>: <span id="scrollWidth"></span><br>
      <code>jQuery width</code>: <span id="jqWidth"></span><br>
      <code>jQuery outerWidth</code>: <span id="jqOuterWidth"></span><br>
      <br>
      <!-- xxxHeight -->
      <code>div.clientHeight</code>: <span id="clientHeight"></span><br>
      <code>div.offsetHeight</code>: <span id="offsetHeight"></span><br>
      <code>div.scrollHeight</code>: <span id="scrollHeight"></span><br>
      <code>jQuery height</code>: <span id="jqHeight"></span><br>
      <code>jQuery outerHeight</code>: <span id="jqOuterHeight"></span><br>
      <br>  
      <!-- left, top -->
      <code>div.style.top</code>: <span id="styleTop"></span><br> 
      <code>div.style.left</code>: <span id="styleLeft"></span><br>
      <br>
      <!-- padding, margin, border, width, height -->
      <code>div.style.padding</code>: <span id="stylePadding"></span><br> 
      <code>div.style.margin</code>: <span id="styleMargin"></span><br> 
      <code>div.style.width</code>: <span id="styleWidth"></span><br> 
      <code>div.style.height</code>: <span id="styleHeight"></span><br> 
      <br>
      <!-- xxxLeft, xxxTop -->
      <code>div.clientLeft</code>: <span id="clientLeft"></span><br> 
      <code>div.clientTop</code>: <span id="clientTop"></span><br> 
      <code>div.offsetLeft</code>: <span id="offsetLeft"></span><br> 
      <code>div.offsetTop</code>: <span id="offsetTop"></span><br>
      <code>div.scrollLeft</code>: <span id="scrollLeft"></span><br> 
      <code>div.scrollTop</code>: <span id="scrollTop"></span><br> 
      <br>
    </div>
  </div>
  <script src="http://code.jquery.com/jquery-2.1.1.min.js"></script>
  <script type="text/javascript">
    window.onload = function () {
      calc()
      getOffsetParent('div')

    }
  </script>
</body>
</html>
```


### 额外说明
1. 在所在容器的css属性是`box-sizing: content-box(默认)` 时，`style.width`实际上是：实际内容宽度 + 浏览器默认滚动条宽度（在chrome和firefox上测试了都等于17px），而`clientWidth`这时等于= 实际内容（style.width - 浏览器滚动条默认宽度） + padding(即paddingLeft + paddingRight)，不包括滚动条宽度，因此此时`clientWidth != style.width`;在所在容器的css属性是`box-sizing: border-box`时，这时`style.width`等于`offsetWidth`，即实际内容宽度 + padding + border，而这时`clientWidth`等于 = 实际内容 + padding，但是这时实际内容时多少呢？实际内容等于 = （style.width - border - padding - 浏览器滚动条默认宽度）

2. 在css属性是`box-sizing:content-box(默认)`时，`jquery.width() = style.width , jquery.outerWidth() = dom.offsetWidth` ; 在css属性是`box-sizing:border-box`时，`jquery.width() = style.width - border - padding , jquery.outerWidth() = dom.offsetWidth`
