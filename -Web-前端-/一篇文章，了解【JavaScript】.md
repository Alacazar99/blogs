【先来打个招呼】
![本页使用 firefox打开](https://upload-images.jianshu.io/upload_images/17476267-8baa22114c14272c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![简书交友~](https://upload-images.jianshu.io/upload_images/17476267-49f3af7caf6a6a37.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
---
【进入正题】
##### 关于【JavaScript】
【简介】JavaScript 是互联网上最流行的脚本语言，这门语言可用于 HTML 和 web，更可广泛用于服务器、PC、笔记本电脑、平板电脑和智能手机等设备。
- JavaScript 是一种轻量级的编程语言。

- JavaScript 是可插入 HTML 页面的编程代码。

- JavaScript 插入 HTML 页面后，可由所有的现代浏览器执行。
---
【先上代码-html】
```
<!DOCTYPE html>
<html>
<head>
	<title>javascript ES5\ES6</title>
	<meta charset="utf-8">
	<!-- node.js -->
	<script type="text/javascript" src="./script.js">
	</script>
</head>
<body>
	<button onclick="test()">点我，点我！</button>
</body>
</html>
```
**JavaScript 可以通过不同的方式来输出数据：**

- 使用 window.alert() 弹出警告框。
- 使用 document.write() 方法将内容写到 HTML 文档中。
- 使用 innerHTML 写入到 HTML 元素。
- 使用 console.log() 写入到浏览器的控制台。

【举个栗子：js-脚本】
```
function test(){
	// alert("你点击我...要干嘛");
	alert("你好，欢迎阅读我的简书");
	alert("我叫Zurich Alcazar，交个朋友吧~");

	var foo = '我叫Zurich Alcazar';
	console.info(foo);
	foo1="你好，欢迎阅读我的简书";
	console.info(foo1);
	const PI = 2019-7-18;
	console.info(PI);
}
```
【解释一下】：运行结果如上图所示。

---
#####【JavaScript】中的数据类型
主要有以下几类：
- 数字：整数、浮点数；
- 字符串：单引号、双引号；
- 布尔型：true、false；
- Null型：null；
- undefined型（未定义）：undefined;

#####【JavaScript】中的变量作用范围
js在默认情况下，变量以函数为范围。

变量 **加var与不加var** 的区别：

- 【加var】：局部变量；
- 【不加var】：全局变量；

【重点总结】
【关于常量】：一旦赋值，就不能再修改的量；
---
##### 关于【JavaScript】中的函数
在JavaScript中，**函数即对象**，可以随意地被程序操控，函数可以嵌套在其他函数中定义，这样可以访问它们被定义时所处的作用域中的任何变量。

【w3cshool上的解释】：函数是由**事件驱动**的或者当它被调用时执行的**可重复使用**的代码块。

【看一个小例子】：
```js
function outer(){
	var a = 'outter'

	function inner(){
		a = "inner"
		console.info(a)
	}
	console.info("===" + a)
	return inner;
}

console.info(outer()())
```
执行结果：
```
===outter 
inner 
undefined 
```
【解释一下】
**闭包**：函数嵌套函数。
内部的函数，可以使用外部函数的变量；

---

###### 关于【JavaScript】的数组
直接来看以下实例：
```js
var name = "zhang san";
console.info(name.length);

var reg = /^1\d{10}$/
console.info(reg.test('13492359245'))

var regname = /ZHANG/i
console.info(regname.test(name))

var a = 20;

console.info(isNaN(20 / 'ewe'))

console.info(typeof a )

输出：
9 [script.js:73:9]
true [script.js:76:9]
true
true
number
```
【插播内容】
###### 什么是正则表达式？
- 正则表达式是**由一个字符序列形成的搜索模式**。

当你在文本中搜索数据时，你可以用**搜索模式**来描述你要查询的内容。

-  正则表达式可以是一个简单的字符，或一个更复杂的模式。
- 正则表达式可用于所有文本搜索和文本替换的操作。

```
var a = 10;
var b = 20;
var c = 30;

var max = -Infinity;

console.info(max)
// 三目表达式：expr ? expr1 : expr2;
max1 = (a > b ? a:b) >c ? (a > b ? a:b) : c;

var a= 0;
var b = ++a +3;
console.info(a)
console.info(b)

a = 2;
switch(a){
	case 1:
		console.info('11111')
	case 2:
		console.info('22222')
}
```
输出：
```
-Infinity [script.js:108:9]

1 [script.js:114:9]

4 [script.js:115:9]

22222
```
---

####关于【JavaScript】的循环

主要的几种循环类型：
- for - 循环代码块一定的次数；
- for/in - 循环遍历对象的属性；
- while - 当指定的条件为 true 时循环指定的代码块；
- do/while - 同样当指定的条件为 true 时循环指定的代码块；
【看看这个实例】
```
// for in  循环
var arr=[5.20,2.1]
for (i in arr){
	console.info(arr[i])
}
// for 循环
for (var i = 0; i < arr.length; i ++){
	console.info(arr[i])
}

// while 循环
var i = 0;
while (i < arr.length){
	console.info(arr[i])
	i++;
}
// do...while 循环
var i = 0;
do{
	console.info(arr[i])
	i ++;
}while(i < arr.length);
```
和其他语言相通的，可以使用**break**： 跳出当前循环；**continue** ：跳出本次循环；
```
for(var i = 0; i < 10; i++){
	if(i % 2 ==0){
		continue;
	}
	console.info(i)
}
```

【注释】：但凡是有编程基础的朋友，以上内容都是最基础的。

---
##### 【JavaScript】面向对象（重点）
JavaScript 允许自定义对象。

**JavaScript 中，一切事物都是对象**

JavaScript 提供多个内建对象，比如 String、Date、Array 等等。 **对象只是带有属性和方法的特殊数据类型。**

- 数字型可以是一个对象；
- 字符串也可以是一个对象；
- 数学和正则表达式也是对象；
- 数组是一个对象；
- 函数也可以是对象

【总结】：对象只是一种特殊的数据。对象拥有属性和方法。

```
// 面向对象
var student = {
	name:'Zurich',
	age:27,
	sayhello:function(){
		// 方法的调用者；
		console.info(this.name)
	}
}
student.sayhello();
console.info(student.age)
console.info(student['age'])

// 对象:原型链
// 默认原型：
function Student(name,age){
	this.name = name;
	this.age = age;
}

Student.prototype.sayhello =function(){
		console.info(this.name + "说：hello！")
	} 

var s1 = new Student("Alcazar",100);
s1.sayhello();
var s2 = new Student("AS_Alcazar",100);
// console.info(s2);

function person(name,age){
	this.name = name;
	this.age = age;
}

person.prototype.shuo= function(){
	console.info('Hello WOrd!!')
}
function User(uname){
	this.uname = uname;
}
User.prototype =new person()

var u =new User('admin')
u['name'] = 'GGGGG'
console.info(u['name'])
u.shuo();
------
输出：
Zurich [script.js:162:11]
27 [script.js:166:9]
27 [script.js:167:9]
Alcazar说：hello！ [script.js:177:11]
GGGGG [script.js:200:9]
Hello WOrd!!
```
---

#### 关于 ES6
【简介】ES6 主要是为了解决 ES5 的先天不足，比如 JavaScript 里并没有类的概念，但是目前浏览器的 JavaScript 是 ES5 版本，大多数高版本的浏览器也支持 ES6，不过只实现了 ES6 的部分特性和功能。
- 箭头函数
- 类
- 继承
- 模板字符串
来看一个实例：
```
// 1.箭头函数
var a =( v, h ) => v + h;
console.info(a(5,8))

// 2.类
class Person1{
	constructor(name){
		this.name = name;
	}
}
// 3.继承
class User1 extends Person1{

}
// 模板字符串
var name = "aa";
var age = 20;

var introduce = `
   my name is ${name}, my age is ${age}
`;

console.info(introduce)
```
输出：
```
13
my name is aa, my age is 20

```
---

【总结】：此篇文章用于初步学习和了解【JavaScript】，如果简书朋友们希望能深入学习这门语言。可以看看【W3Cshool】上的教程[https://www.w3cschool.cn/javascript/](https://www.w3cschool.cn/javascript/)
