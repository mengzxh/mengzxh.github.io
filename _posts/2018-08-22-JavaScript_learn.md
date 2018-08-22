---
layout: post
title: 'Javascript学习'
subtitle: ''
date: 2018-08-22
categories: 技术
cover: '/cover_img/javascript.jpg'
tags: Javascript
---





[TOC]

### JavaScript简介

* JavaScript是一种运行在浏览器中的解释型的脚本编程语言 
* 如果浏览器不支持Js，可以使用<noscript>你的浏览器不支持Javascript，请更换浏览器</noscript>

### Javascript语法

#### Javascript基础语法

* Javscript的执行顺序是按照在HTML文件中出现的顺序依次执行
* 严格区分大小写
* Javascript默认忽略空白符号和换行符
* 以;结束，建议加分号
* 可以使用{}括成一个语句组，形成一个快block
* 通过使用\使用折行操作
* 单行注释//，多行注释/\**....\*/
* document.write()、console.log()、alert()调试
* JavaScript错误
  * 语法错误：通过控制台调试
  * 逻辑错误：通过alert()进行调试

#### 变量

##### 声明变量

```javascript
<script type="text/javascript">
	var a;
	var b,c;
	var d="this is a test";
	var e=1;
	var f=true;
	var g=h=i=j=1;
	alert(a);
	alert(d);
</script>
```

* 变量重名会产生覆盖
* 变量名称严格区分大小写

##### 变量在内存中存储与释放

* 收集方式
* 收集内容
* 回收算法

##### 数据类型

* Infinity(正无穷大)、-Infinity(负无穷大)
* NaN(0/0, isNaN()检测)
* true/false布尔类型
* undefined 无定义的数据类型(声明变量未赋值，赋予不存在属性值)
* 空值null
* alert(null==undefined)为true，alert(null\=\=\=undefined)为false

##### 数据类型转换

typeof 得到变量类型

###### 隐式转换

* 转化为布尔型
  * undefined -> false
  * null -> false
  * 0/0.0/NaN -> false
  * '' -> false
  * 其他对象 -> true
  * 1 -> true
  * '1' > true
* 转化为数值型
  * undefined -> NaN
  * null -> 0
  * true > 1| false ->0
  * 内容为数字 -> 数字，否则转化为NaN（**注意这种情况的加法运算**）
  * 其他对象 -> NaN
* 转化为字符串
  * undefined -> 'undefined'
  * null -> 'null'
  * NaN -> 'NaN'
  * true -> 'true' | false -> 'false'
  * 数值型 -> NaN、0或者与数值对应的字符串
  * 其他对象，如果存在这个对象则转换为toString()方法的值，否则转化为undefined

###### 显式转换

* 转换为布尔型(var a=Boolean())
  * 0、NaN、undefined -> false
  * '' -> false
  * null -> false
* 转化为字符串(var a=String())
* 转化为数字(var a=Number())

```javascript
parseInt(0xabc,10)
parseInt("3abc")
parseInt(true)
parseInt("3 5 6")
parseFloat("1213adf")
parseFloat("adfads")
parseFloat("2e3a")
```

###### 算术运算符号

特殊情况

```javascript
alert(3%-8) -> 3
alert(-3%-8) -> -3
alert(3+8+"3a") -> 113a
alert(''+3+8+"3a") -> 113a
var num =1;
alert(num++);
alert(num);
var num1=1;
alert(++num);
var num2=true;
alert(++num2);
var num3="3b";
alert(++num3);
```

字符串支持++(取决是否支持转换)， num支持(0)，undefined不支持

```javascript
=、+=、-=、*=、/*、>、<、==、!=、==(值相等)、===(值和类型相等)、!==、&&、||
exp1?exp2:exp3(三元运算符，if类似)
```

短路现象举例:

```javascript
var i=0,j=1;
if (i-- && j++){
    alert("hello");
}else{
    alert("world");
}
alert(i);
alert(j);
```

逗号表达式

```javascript
var z=(n=1,m=2,p=3)
```

**void运算符代表表达式没有返回结果**



#### 流程控制

```javascript
if (exp1){
    exp2;
}else{
    exp3;
};

if (exp1){
    exp2;
}else if(exp3){
    exp4;
}else if(exp5){
    exp6;
}else{
  	exp7  
};

switch(exp1){
    case 值1:
        执行代码段;
        break;
    case 值2:
        执行代码段;
        break;
    case 值3:
        执行代码段;
        break;
    case 值4:
        执行代码段;
        break;
    default:
    	没有匹配到;
    	break
}
```

#### 循环语句

```javascript
for(exp1;exp2;exp3){
    循环体;
};
/*
    exp1:无条件的执行第一个表达式
    exp2:是判断是否能执行循环体的条件
    exp3:做增量的操作
*/
```

习题1：求和100内的奇数和偶尔分别的和

```javascript
<!DOCTYPE html>
<html>
<head>
	<title>100以内奇数和偶数求和</title>
</head>
<body>

		<script type="text/javascript">
			var sum_odd=0, sum_even=0; 
			for(var i=0; i<100; i++){
				if(i%2==0){
					sum_even += i;
				}else{
					sum_odd += i;
				};
			};
			document.write("奇数之和为:" + sum_odd);
			document.write('<hr color="red"/>')
			document.write("偶数之和为:" + sum_even);
		</script>
</body>
</html>
```



习题2：打印99乘法表

```javascript
<!DOCTYPE html>
<html>
<head>
	<title>打印99乘法表</title>
</head>
<body>
	<script type="text/javascript">
		document.write('<table border="1" width="80%">');
		for(var i=1; i<=9; i++){
			document.write("<tr>")
			for (var j=1; j<=i; j++){
						document.write("<td>" + i + "*" + j + "=" + i*j + "</td>");
				};
			document.write("</tr>");
			};
		document.write("</table>");
	</script>
	
</body>
</html>
```

倒99乘法表

```javascript
<!DOCTYPE html>
<html>
<head>
	<title>打印倒99乘法表</title>
</head>
<body>
	<script type="text/javascript">
		document.write('<table border="1" width="80%">');
		var row=0;
		for(var i=1; i<=9; i++){
			row += 1;
			document.write("<tr>")
			for (var j=1; j<=9; j++){
				if(j >= row){
						document.write("<td>" + i + "*" + j + "=" + i*j + "</td>");
					}else{
						document.write("<td></td>");
					};
				};
			document.write("</tr>");
			};
		document.write("</table>");
	</script>
	
</body>
</html>
```



习题3：通过for循环实现百钱买百鸡的题目，其中公鸡5元每只，母鸡3元每只、小鸡3只1元，100元可以买100只鸡，求每种鸡的数量（输出多种情况）

```javascript
<!DOCTYPE html>
<html>
<head>
	<title>百元买百鸡</title>
</head>
<body>
	<script type="text/javascript">
		for(var i=0; i<=20; i++){
			for(var j=0; j<=33; j++){
				k = 100 -i -j;
				sum = 5 * i + 3 * j + k/3
				if(sum == 100){
					document.write("公鸡:" + i + "母鸡:" + j + "小鸡:" + k + "<br/>");
				};
			};
		};
	</script>
</body>
</html>
```

while循环

```javascript
while(exp){
    循环体;
};
```

do while循环

```javascript
do {
    循环体;
}while(exp);
```

#### 函数

* 函数名称严格区分大小写
* 函数名称重复会产生覆盖
* 函数通过return 返回值，如果没有return返回值为undefined
* window.函数名称()进行调用
* 函数可以有参数也可以没有参数，如果定义了参数，在调用函数的时候没有传值，默认设置为undefined
* 在调用函数时如果传递参数超过定义时参数，js会忽略多余的参数
* js中不能直接写默认值，可以通过arguments对象来实现默认值效果。
* 尽量使用var定义变量

```javascript
function 函数名称([参数,...]){
    代码段;
    return 返回值;
};

// 默认值写法1
function calc(num1, num2){
    num1 = num1 || 1;
    num2 = num2 || 2;
    return num1 + num2
};

// 默认值写法2
function calc(x, y) {
	x = x===undefined?0:x
	y = y===undefined?0:y
	return x+y
};
alert(calc());
alert(calc(1, 3));

// 默认值写法3
function calc(x, y) {
	x = arguments[0]?arguments[0]:0
	y = arguments[1]?arguments[1]:0
	return x + y
};
alert(calc())
alert(calc(1, 3))

// 不定参数使用
function calc() {
	var count = arguments.length;
	var sum = 0;
	for(var i=0; i<count; i++){
		sum += arguments[i];
	};
	return sum
};
alert(calc(1,2,3,5,6));

// 作用域的一个陷阱
var x=1, y=2;
function calc(x, y) {
	document.write("函数体内a值:" + a + "<br/>"); //此处a没有定义但是没有报错，是因为下面a有 var 定义，如果下面没有var a程序报错;
	document.write("函数体内x值:" + x + "<br/>");
	document.write("函数体内y值:" + y + "<br/>");
	// var x, y;
	x = x + y;
	z = x + 1;
	var a =198;
};
calc(x, y)
document.write("函数体外x值" + x + "<br/>");
document.write("函数体外y值" + y + "<br/>");
document.write("函数体外z值" + z + "<br/>");
```

全局函数

* parseInt()
* parseFloat()
* isFinite()
* isNaN()
* encodeURI()
* decodeURI()
* encodeURIComponent()
* decodeURIComponent()
* escape()
* unescape()
* eval()
* Number() --> Number(new Date()) 时间戳
* String()

匿名函数

```javascript
function calc(x, y) {
	return x() + y();
	};
alert(calc(function(){return 5;}, function(){return 10;}))
```

函数回调

```javascript
function calc(a, b, c, callback){
	var i, arr=[];
	for(i=0; i<3; i++){
		arr[i] = callback(arguments[i]);
	};
	return arr;
};
alert(calc(5, 6, 7, function(i){return i * 2 + 1}))
```

call和apply

```javascript
function calc(a, b){
    return a*b;
};
alert(calc.call(calc, 5, 10));
var arr=[3, 4];
alert(calc.apply(calc, arr))
```

自调用

不会产生任何全局变量，无法重复执行，适合执行一些一次性的或者初始化的任务。

```javascript
(
	function(i, j){
      alert("this is a test");  
      alert(i+j)
	}(1, 2);
)
```





