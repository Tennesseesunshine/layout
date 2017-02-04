# layout
关于页面各种布局包括水平居中，竖直居中，居中，定宽不定宽等<br>
### 一：居中
##### 1.水平居中（父子容器不定宽。）<br>
`HTML`这是两层具有父子关系的布局。结构如下：
```
<div class="parent">
		<div class="child">DEMO</div>
</div>
```
要做到水平居中的 `css` 为：<br>
```
方法一：
.parent{text-align: center;}
.child{display: inline-block;}
这是利用把子元素设置为行内元素，从而父元素利用text-align来让子元素居中。
优点：兼容性较好。缺点：子类会继承，子类需要再加入其它样式。
```
```
方法二：
.parent{display: flex;justify-content: center;}
直接设置父元素，利用的是CSS3弹性盒子，其中子元素的宽度为auto，由自己的内容决定。
优点：弹性布局。缺点：IE的兼容性可能不够好。
```
```
方法三：
.child{display: table;margin: 0 auto;}
这个方法是不需要外层的父容器，直接设置需要居中的元素即可。
```
```
方法四：
.parent{position:relative}
.child{position:absolute;left:50%;transform:translateX(-50%);}
这是利用定位方式实现的水平居中，其宽度由内容决定。
优点：因为元素脱离文档流，不会对其他元素产生影响。缺点：transform的兼容性不够好。
```
##### 2.垂直居中（父子容器不定高）<br>
`HTML`依旧是刚刚的布局。结构如下：
```
<div class="parent">
		<div class="child">DEMO</div>
</div>
```
要做到垂直居中，`css`：
```
方法一：
.parent{display: table-cell;vertical-align: middle;}
将父容器变成单元格，利用表格属性居中。
```
```
方法二：
.parent{display: flex;align-items: center;}
将stretch强制变为center
优点：弹性布局。缺点：IE的兼容性可能不够好。
```
```
方法三：
.parent{position:relative}
.child{position:absolute;top:50%;transform:translateY(-50%);}
依旧是定位方式实现的垂直居中。
优点：因为元素脱离文档流，不会对其他元素产生影响。缺点：transform的兼容性不够好。
```
##### 3.水平竖直居中（父容器宽高未知）<br>
`其实很简单`就是将`1.2`结合起来，父元素的`css`写到一起，子元素的`css`写到一起就可以实现居中。<br>
结构还是同`1.2`<br>
```
方法一：
.parent{text-align: center;display: table-cell;vertical-align: middle;}
.child{display: inline-block;}
```
```
方法二：
.parent{display: flex;justify-content: center;align-items: center;}
```
```
方法三：
.parent{position:relative}
.child{position:absolute;top:50%;left:50%;transform:translate(-50%,-50%);}
```
其中利用`display: flex；`设置的时候只需给父容器设置，不用管子元素。<br>
### 二：多列布局
##### 1.定宽+自适应<br>
`HTML`第一种结构如下，当然其中`class=left`的`div`后边可以任意加进去很多列，也可以没有`middle`，具体怎样是要看需求：
```
<div class="parent">
		<div class="left"><p>left</p></div>
		<!-- <div class="middle"><p>middle</p></div> -->
		<div class="right-fix"> 
			<div class="right"><p>right</p></div>
		</div>
</div>
```
```
.left{float: left;width: 100px;position: relative;background: pink;}
/*.middle{float: left;width: 100px;position: relative;background: blue;}*/
.right-fix{background: green;width: 100%;float: right;margin-left: -100px;}
.right{margin-left: 100px;};
left加上定位是提高层级避免被遮挡，其中right的margin是left的宽度。如果加上middle的话，就需要再加上middle的宽度。
```
第二种结构，同上需要不需要`middle`，都看需求，第二种结构`css`有两种方法实现`定宽+自适应`。
```
<div class="parent">
		<div class="left"><p>left</p></div>
		<!-- <div class="middle"><p>middle</p></div> -->
		<div class="right"><p>right</p></div>
</div>
```
```
方法一：
.left/*，.middle*/{float: left;width: 150px;background: #dfa545;margin-right: 10px;}
.right{overflow: hidden;background: #faa451;}
这种结构较上种能够稍微简单。但是IE6不支持overflow: hidden;
```
```
方法二：
.parent{display: table;width: 100%;table-layout: fixed;background: #0f0;}
.left{width: 200px;background: #ccc;}
.left,.right{display: table-cell;padding-left: 10px;}
给父级如此设置是为了加快渲染，布局优先，将其转化为table，宽度自适应，左边给200px,剩下的都是右边。
```
以上代码只是实现一列定宽+一列自适应，而多列定宽一列自适应的话只需在结构二中的left后边增加列数，至于样式就是在`方法一`中的`.left`后面直接加`,(类名orID)`
##### 2.不定宽+自适应<br>
1)一列不定宽+一列自适应<br>采用`结构二`中的`方法一和方法二`都合适，只需将固定的px值取消掉即可。
2)多列不定宽+自适应<br>>采用`结构二`中的`方法一`将增加的列的`类名`或者`ID`加在`.left`后面即可。
### 三：等分布局
##### 1.结构一<br>
```
<div class="parent">
		<div class="col">1</div>
		<div class="col">2</div>
		<div class="col">3</div>
		<div class="col">4</div>
</div>
 ```
 ```
 .col{float: left;background: #ccc;border-right: 1px solid #000;padding-left: 20px; width: 25%;box-sizing: border-box;}
 利用box-sizing: border-box;实现宽度包括padding,不至于一行内容放不下被挤倒另外一行。
 ```
 其实此结构中加不加parent效果都是一样，此方法IE8以上完全兼容。缺点是增加列数需要重新计算比例。
 ##### 2.结构二<br>
 ```
 <div class="parent">
    <div class="col">1</div>
    <div class="col">2</div>
    <div class="col">3</div>
    <div class="col">4</div>
<!--<div class="col">1</div>
    <div class="col">2</div>
    <div class="col">3</div>
    <div class="col">4</div> -->
</div>
```
```
.parent{display: table;table-layout: fixed;width: 100%;}
.col{display: table-cell;padding-left: 5px;background: #0f0;border-right: 1px solid red;}
其中将父元素转换成表格，利用table-layout: fixed;布局优先，加速渲染以及未给单元格设置宽度，则单元格平分的特性从而实现等分布局。
```
此方法优点是不论增加或者删除多少列，其都可以自己应付平分，真正意义上的"智能"。
### 四：全屏布局
##### 1.结构一<br>
```
<div class="parent">
  <div class="top">top</div>
  <div class="left">left</div>
  <div class="right">
    <div class="right-small">right</div>
  </div>
  <div class="bottom">bottom</div>
</div>
```
```
body,html,.parent{height: 100%;overflow: hidden;}
.top{position: absolute;left: 0;top: 0;right:0; height: 100px;background: #e38daf;}
.left{position: absolute;top: 100px;left: 0;width: 200px;bottom: 100px;background: #ccdfac;}
.right{position: absolute;top: 100px;left: 200px;right: 0px;bottom:100px;background: #123dff;}
.right-small{max-height: 405px;overflow: auto;}
.bottom{position: absolute;left: 0;right: 0;bottom: 0;height: 100px;background: #156dac;}er;}
```
利用定位来实现布局，并且右侧right-small会出现滚动条。给父级加overflow是防止页面整体出现滚动条。
此方法兼容性较好，可以撑满整个浏览器，但是IE6不兼容。（古老的版本感觉应该被抛弃##）<br>
##### 2.结构二<br>
```
<div class="parent">
  <div class="top">top</div>
  <div class="middle">
  <div class="left">left</div>
    <div class="right ">
    <div class="right-small">right</div>
  </div>
  </div>
  <div class="bottom">bottom</div>
</div>
```
```
body,html,.parent{height: 100%;overflow: auto;margin: 0;padding: 0;} 
.parent{display: flex;flex-direction: column;}
.top{height: auto;background: pink;}
.bottom{height: auto;background: #666;}
.middle{flex:1;display: flex;}
.left{width: 100px;background: #adcafd;}
.right{flex: 1;overflow: auto;background: #c659f1}
```
此结构是利用弹性盒模型构建的自适应页面，top,bottom,right的高都随内容变化而变化。并且为页面加上滚动条,当页面内容高度超出屏幕时出现。








