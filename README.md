#D3
##简介
* D3: Data Driven Documents
    * 把数据加载到浏览器的内存里
    * 把数据跟DOM元素绑定起来
    * 通过绑定的数据来设置DOM元素的属性 
```html
<!DOCTYPE html>
<html>
<head>
<script src="https://d3js.org/d3.v4.min.js"></script>
</head>
<body>
<script>
  d3.select("body").append("p").text("hello world!");
</script>
</body>
</html>
```
* 代码解释：
    * d3: 引用全局变量d3
    * select("body"): css选择器，返回第一个匹配的元素
    * append("p"): 在选择的元素内部创建一个新的元素
    * text("xxx"): 把字符串添加到新创建的元素标签内
       
* SVG：Scalable Vector Graphics
    * svg x和y坐标左上角为（0，0）,元素x坐标向右，y坐标向下增加
    * 通用的属性：fill, stroke, stroke-width, opacity
    * svg没有分层的概念，没有三维z-index属性，后来的image会覆盖之前的image, 使用rgba或者opacity来设置透明度
    * 现代浏览器都支持svg, react native有第三方库支持svg
```html
<!DOCTYPE html>
<html>
<body>
<svg width="500" height="500">
  <circle cx="25" cy="25" r="20" fill="rgba(128, 0, 128, 0.75)"
          stroke="rgba(0, 255, 0, 0.25)" stroke-width="10"/>
  <circle cx="65" cy="25" r="20" fill="rgba(128, 0, 128, 0.75)"
          stroke="rgba(0, 255, 0, 0.25)" stroke-width="10" opacity="0.5"/>
  <circle cx="105" cy="25" r="20" fill="rgba(128, 0, 128, 0.75)"
          stroke="rgba(0, 255, 0, 0.25)" stroke-width="10" opacity="0.2"/>

</svg>
</body>
</html>
```
##数据
* 使用selection.data()方法来在DOM元素上绑定数据
    * 数据
```javascript
var dataset = [1,2,3,4,5]
```
```javascript
d3.csv('food.csv', (data) => {console.log(data)})
```
```javascript
d3.json("food.json", function(json) {
    console.log(json);  //Log output to console
});
```
* 选择DOM元素
    * data(dataSet): 绑定数据
    * enter(): 如果dataSet里的数据比当前选择的DOM元素多的话，创建一个新的placeHolder元素，并把它传给下面的函数.
```javascript
<body>
<p></p>
<p></p>
<p></p>
<script>
  var dataSet = [1,2,3,4,5];
  d3.select("body").selectAll("p").data(dataSet).enter().append("p").text("hello");
  //只创建两个"hello" text
</script>
</body>
// on console:
// d3.selectAll("p")._groups[0][0].__data__
//   1
```
* 操作数据
```javascript
<script>
  var dataSet = [1,2,3,4,5];
  d3.select("body").selectAll("p").data(dataSet).enter().append("p").text(d => {return d;});
</script>
```
* 设置DOM元素的属性
```javascript
<script>
  var dataSet = [1,2,3,4,5];
  d3.select("body").selectAll("p").data(dataSet).enter().append("p")
    .text(d => {return d;})
    .style("color", (d) => {return d>1 ? 'red' : 'blue'})
</script>
```

