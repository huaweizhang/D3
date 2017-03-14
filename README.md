# D3
## 简介
### D3: Data Driven Documents
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
代码解释：
* d3: 引用全局变量d3
* select("body"): css选择器，返回第一个匹配的元素
* append("p"): 在选择的元素内部创建一个新的元素
* text("xxx"): 把字符串添加到新创建的元素标签内
       
### SVG：Scalable Vector Graphics
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
## 数据: 使用selection.data()方法来在DOM元素上绑定数据
### 数据
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
### 选择DOM元素
* data(dataSet): 绑定数据
* enter(): 如果dataSet里的数据比当前选择的DOM元素多的话，创建一个新的placeHolder元素，并把它传给下面的函数.
```html
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
### 操作数据
```html
<script>
  var dataSet = [1,2,3,4,5];
  d3.select("body").selectAll("p").data(dataSet).enter().append("p").text(d => {return d;});
</script>
```
### 设置DOM元素的属性
```html
<script>
  var dataSet = [1,2,3,4,5];
  d3.select("body").selectAll("p").data(dataSet).enter().append("p")
    .text(d => {return d;})
    .style("color", (d) => {return d>1 ? 'red' : 'blue'})
</script>
```
## 长方形数据可以直接用html元素，不使用SVG
```html
<style>
  div.bar {
    margin-right: 2px;
    background-color: teal;
    width: 20px;
    display: inline-block;
  }
</style>
<script>
  var dataSet = [1,2,3,4,5];
  d3.select("body").selectAll("div").data(dataSet).enter().append("div")
    .attr("class", "bar")
    .style("height", (d) => {return `${d*5}px`})
</script>
``` 
## 对于不规则形状，可以使用SVG元素来画图
```html
<script>
  var dataSet = [1, 3, 5, 7, 9];
  var svg = d3.select('body')
    .append('svg')
    .attr('width', 500)
    .attr('height', 100);

  svg.selectAll('rect')
    .data(dataSet)
    .enter()
    .append('rect')
    .attr('fill', 'teal')
    .attr('x', (d, i) => 21 * i)
    .attr('y', d => 100 - 10 * d)
    .attr('width', 20)
    .attr('height', d => 10 * d)
  
</script>
```
## 对于文字处理
```javascript
svg.selectAll("text")
			   .data(dataset)
			   .enter()
			   .append("text")
			   .text(d => d)
			   .attr("x", d => d)
			   .attr("y", d => d)
			   .attr("font-family", "sans-serif")
			   .attr("font-size", "11px")
			   .attr("fill", "red");
```
## scale 函数
