```java
/*
 * @Author: hzy
 * @Date: 2022-8-31 10:00
 * @LastEditTime: 2022-11-30 12.29
 * @LastEditors: hzy
 */
```

### 立即执行函数

```java
(function(){
	document.write("通常将匿名函数与事件处理程序一起使用")
})();
```
### input标签、rowspan

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <title>shiyan</title>
    <link rel="stylesheet" href="./page1.css">
</head>
<body>
    id 前台拿数据，name 后台拿数据
    时间：<input type="time" placeholder="请输入时间" autofocus><br>
    日期：<input type="date" placeholder="请输入日期"><br>
    数字：<input type="number" placeholder="输入数字" value="0" max="100" min="-100" step="20"><br>
    日期时间:<input type="datetime_local" placeholder="日期时间">
    搜索：<input type="search" required pattern="[0-9][a-z]{2}">!!!!!!!!!!!!!!
    邮箱：<input type="email">
    网址：<input type="url">
    滑块：<input type="range">
    颜色：<input type="color">
    音频:loop autopaly, muted
    (10.21)：<audio src="" controls></audio>
    视频：
    <video src="" controls></video>
    <table border="1" >
        <tr>
            <th>分类</th>
            <th>技能</th>
            <th>分数</th>
        </tr>
        <tr>
            <td rowspan="3">编程语言</td>
            <td>JavaScript</td>
            <td>8</td>
        </tr>
        <tr>
            <td>C++</td>
            <td>7</td>
        </tr>
        <tr>
            <td>Swift</td>
            <td>8</td>
        </tr>
        <tr>
            <td rowspan="2">产品设计</td>
            <td>PhotoShop</td>
            <td>6</td>
        </tr>
        <tr>
            <td>Axure</td>
            <td>5</td>
        </tr>
        <tr>
            <td rowspan="2">销售</td>
            <td>社交媒体</td>
            <td>3</td>
        </tr>
        <tr>
            <td>C++</td>
            <td>7</td>
        </tr>
    </table>
</body>
</html>
```

### 选择器、伪类选择器a:hover

```html
<!DOCTYPE html>
<html>

<head>
    <style>
        p {
            background-color: orange;
        }

        #p2 {
            color: red;
            background-color: cadetblue;
        }

        .para {
            color: blue;
        }

        p.para2 {
            color: aliceblue;
        }

        #div h2{
            color:aqua;
            background:gray;
        }
        /* 未访问前 */
        a:link{
            color:pink;
        }
        /* 访问以后 */
        a:visited{
            color:aqua;
        }
        /* 鼠标悬停 */
        a:hover{
            color:firebrick;
            font-size: 30px;
        }
        /* 点中时 */
        a:active{
            color:red;
        }
        
    </style>
</head>

<body>
    
    <p id="p1">这是一个段落</p>
    <hr>
    <p class="para" id="p2">这是第二个段落</p>
    <hr>
    <p class="para2">这是三个段落</p>
    <div id="div">
        <h2>标题1</h2>
        <h2>标题1</h2>
        <h2>标题1</h2>
        <h2>标题1</h2>
    </div>
    <a href="https://www.baidu.com">百度链接</a>
</body>

</html>
```

### 函数

```html
<!DOCTYPE html>
<html>
<head>
	<meta charset="utf-8">
	<title></title>
</head>
<body>
	函数
	1、函数定义 与调用
	2、函数参数 、arguments
	3、return 返回值 结束
	4、作用域  ：全局变量 、局部变量<br>
	<button name="button" id='btn1'>点击</button><br><br>
	<script>
		//1 定义 方法1 
		// document.write("123");
		//方法2  函数表达式  (匿名函数)
		var w=function(){
			document.write("通常将匿名函数与事件处理程序一起使用")
		}
		// w();
		// document.write("123")
		//通常将匿名函数与事件处理程序一起使用
		btn1.onlick=function(){
			w();
		};
		//自调用函数-立即执行函数
		(function(){
			document.write("通常将匿名函数与事件处理程序一起使用")
		})();
		document.write("<br>")
		//2 函数参数  直接写形参名字
		function write(name1, name){
			document.write("hello,"+name1+"<br>");
			document.write("hello,"+name+"<br>");
		}
		write(123,23)
		
		//实参个数多于形参:只出现前面的
		// write(1,2,3,4)
		//实参少于形参:出现undefined
		// write(123)
		// arguments 对象
		// 函数有一个名为 arguments 的内置对象。实参赋给形参使用这个参数来接收.
		//arguments 对象类似于数组的结构（可以遍历，可以用下标访问）。
		//如果函数调用的参数太多，则可以使用 arguments 对象来得到这些参数。
		function f(a, b){
			// document.write(arguments.length+"<br>")
			// document.write(arguments[0]+"<br>")
			// document.write(arguments[1]+"<br>")
			// document.write(arguments[3])
			var max=arguments[0]
			for(var i=1; i<arguments.length; i++){
				if(arguments[i]>max)
					max=arguments[i]
			}
			document.write(max)
		}
		f(1,2,234,3)
		document.write("<br>")
		var m = new Map([[1, 'x'], [2, 'y'], [3, 'z']]);
		m.forEach(function (value, key, map) {
			document.write(value+key+map+"<br>");
		});
	</script>
	
</body>
</html>

```

### 变量作用域

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title></title>
	</head>
	<body>
		JavaScript 中，变量的作用域有 全局作用域 和 局部作用域 两种。
		1、全局变量   函数外定义的变量   没有用 var定义的变量(let)   window对象定义的变量(window.c=1)
		2、局部变量  函数内用var定义的变量  
		            形参
		变量的生命周期
			全局变量活得和您的应用程序（窗口、网页）一样久。
			局部变量活得不长。它们在函数调用时创建，在函数完成后被删除。			
		<script>			
			//1  全局变量
			
			var str1=" 1我是全局"
			document.write(str1)
			
			function fn1(){
				var str2="我是局部"  //2  局部变量  使用var在函数内部定义的变量
				let s="123"
			}
			fn1();
			console.log(str1);
			// console.log(s);
			//6  以下程序运行结果是:
			//对Js而言，只要变量是在同一个范围（函数）里，就视为已经声明，
			//哪怕是在变量声明前就使用
			/*	 var num1=10;
				function fn2(){

					//因为num1是局部的 fn1内部的。所以提升到fn1函数最前面声明
					console.log(num1);//输出？undefined

					var num1=20;
				}

			fn2()*/

			var a="1111"
			function fn3(){				
				console.log(a);
				var a="2222"
				console.log(a);					
			}
			fn3();
			
		</script>

	</body>
</html>

```

### 获取元素

```html
<!DOCTYPE html>
<html>

<head>
	<meta charset="utf-8">
	<title></title>
</head>

<body>
	查找元素方法：
	1 通过 id 查找元素<br>
	<p id="intro">Hello!</p><button name="button" id='btn0'>确定</button>
	<script>
		var p = document.getElementById("para");
		// document.write();
		var btn0 = document.getElementById("btn0");
		btn0.click = function () {
			document.write("nihao");
		}
	</script>
	<hr>
	2、通过标签名查找元素<br>

	<ul>
		<li>【#回应何时开学#】</li>
		<li>【#回应何时开学#】</li>
		<li>【#回应何时开学#】</li>
		<li>【#回应何时开学#】</li>
		<li>【#回应何时开学#】</li>
		<li>【#回应何时开学#】</li>
	</ul>
	<script>
		var lis = document.getElementsByTagName('li');
		console.log(lis);
		for (var i = 0; i < lis.length; i++) {
			if (i % 2 == 0) {
				lis[i].style.background = "blue";
			}
		}
	</script>
	<hr>
	3 html5新增的 通过 CSS 选择器查找 HTML 元素:<br>
	querySelectorAll() 方法 :查找匹配指定 CSS 选择器（id、类名、标签等）的所有 HTML 元素<br>
	querySelector() 方法 :查找匹配指定 CSS 选择器的第一个 HTML 元素<br>
	<p id="p1">Hello!</p>
	<button name="button" id='btn1'>确定</button>
	<p class="imp">Hello!</p>
	<p class="imp">world!</p>
	<script>
		//querySelectorAll() 方法  获得class为imp的元素   .不能少
		var p1 = document.querySelectorAll('.imp')
		console.log(p1)
		//querySelector（）方法   获得id为btn1的元素 #不能少
		var p2 = document.querySelector('#btn1')
		console.log(p2)
		var ps = document.querySelectorAll(".imp");
		console.log(ps);
		ps[1].innerHTML = "123456789";
	</script>
</body>

</html>
```

### 面向对象

```html
<!DOCTYPE html>
<html>
<head>
	<meta charset="utf-8">
	<title>面向对象</title>
</head>
<body>
	1、什么是JavaScript对象？
		对象是有(属性)和(方法)的集合。
	2、创建对象的三种方式：<br>
	01、使用new object()方式
	<script>
		//创建
		var person = new Object();
		// console.log(person)
		// console.log(typeof(person))
		//设置属性和方法 
		person.name="123"
		person.age = 18;
		//设置方法 
		person.say=function(){
			console.log("say")
		}
		//访问属性
		// console.log(person.name)
		// console.log(person.age)		
		//调用方法
		// person.say()
	</script>
	<br> 
	01 使用字面量定义
	var 对象={
		//键值对的形式  键 属性名 ：值 属性值，
		属性名 ：值，
		//方法 冒号后面跟着一个匿名函数。
		方法:function(){
			
		}
	}
	<script>
		var person = {
			name:"123",
			age:123,
			say:function(){
				console.log("say")
			}
		}
		// person.say()
	</script>
	<br>
	3、使用自定义的构造函数声明多个类型特点一致的对象
	//构造函数的语法格式：
	// function 构造函数名(){
	// 	this.属性=值;
	// 	this.方法=function(){}
	// }
	//new 构造函数名(); 函数名首字母要大写
	<script>
		//1创建构造函数
		function Person(name, age){
			// console.log(this);
			this.name = name;
			this.age = age;
			this.say=function(){
				console.log("say")
			}
		}
		//2通过构造函数new对象
		var p1 = new Person("hzy", 18)
		var p2 = new Person("hhh", 21)
		// console.log(p1.say())
		// p2.say()
		//3属性访问访问2:['属性名']
		// console.log(p1['name'])
		//01什么时候 必须用['属性名']
		//属性名里包含特殊字符:
		var obj={	}
		obj['content-type']='text/css'
		// obj.c-c='123'//错误
		//属性名不确定
		var Name='123'
		obj.Name=20;
		// console.log(obj.Name)
		// for in 遍历属性
		//for(变量 in 对象){
		//}
		// for(var i in obj){
			// console.log('====', i, obj.i)//undefine
			// console.log('====', i, obj[i])
		// }
	</script>
	4、相关问题-对象赋值与引用
	内存：
		栈：局部变量  全局变量
		堆：对象
	<script>
		//01基本数据类型 在内存中的存储
		var a=10
		var b=a
		b=20;
		console.log(a)
	</script>
	<script>
		//02复杂数据类型 在内存中存储
		var obj1={name:"Tom"}
		var obj2=obj1
		obj1.name='123'
		console.log(obj2.name)
	</script>

	<script>
		//03基本数据类型作为函数参数
		
		
	</script>
	<script>
		//04复杂数据类型作为函数参数
		
		
	</script>
</body>
</html>
```

### Math库

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title></title>
	</head>
	<body>
		Math 对象
		Math 对象用于执行数学任务。<br>
		
		使用 Math 的属性和方法的语法：<br>
		var pi_value=Math.PI;
		var sqrt_value=Math.sqrt(15);<br><br>
		<hr>
		<script>			
			var i;
			i=Math.round(5/2) //四舍五入			
			//取整 向下取整
			i=Math.floor(5/2)
			//绝对值
			Math.abs(-100)
			//随机数 0-1之间
			i=Math.random()
			//产生0-10之间的随机数？
			i=Math.random()*10
			//产生5-10之间的随机数？
			i=Math.random()*5+5
			
			//公式：取x到y之间的随机数:;
			//Math.random()*(y-x)+x

			//问题：如何产生0到10之间的随机整数？
			
			//方法：先产生0-    之间的随机数 然后        .		
			
			document.write(i);
			//alert(i);
			
			/*练习  写一个随机点菜的小程序
			1 创建一个数组 
			2 随机产生数组下标
			3 将下标对应的数组元素 输出来.
			js中定义数组:var arr=["鱼头豆腐","红烧排骨","土豆丝","西红柿鸡蛋汤","鸦片鱼头","小鸡炖蘑菇"]
			*/
		</script>
		
		
	</body>
</html>
```

### 静态成员

```html
<!DOCTYPE html>
<html>

<head>
	<meta charset="utf-8">
	<title></title>
</head>

<body>
	1、静态成员
	<script>
		function Person(name, age) {
			this.name = name;
			this.age = age;
			//静态属性 类名.属性
			if (!Person.count) {
				Person.count = 0;
			}
			Person.count++;
		}

		/*设置一个属性 计算创建出多少对象
		  需要静态成员-判定在构造函数上的属性和方法
		*/
		Person.print = function(){
			alert("创建了"+Person.count+"对象")
		}
		var p1=new Person('qwe', 123)
		var p2 = new Person('qw', 1)
		console.log(Person.count)
		Person.print()
	</script>

</body>

</html>
```

### 特殊的引用类型：Boolean、Number 和 String

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title></title>
	</head>
	<body>
		为了便于操作基本类型值，js还提供了 3 个特殊的引用类型：Boolean、Number 和 String。
		Number 对象：基本包装类型  提供数字的对象（包括整数、浮点数等）
		1、NaN属性：非数字（(Not a Number)。）	
		
		2、isNaN（）函数：判断是否为非数字
		
		3、toString()：把数字转为字符串
		
		4、把变量转换为数值
		这三种 JavaScript 方法可用于将变量转换为数字：

			Number() 方法  可用于把其他类型转换为数值
			parseInt() 方法 解析一段字符串并返回整型数值。允许空格。只返回首个数字：
			parseFloat() 方法  解析一段字符串并返回数值。允许空格。只返回首个数字：
		<br><br><hr><br>
		<script>
			//1 NaN属性
			
				
			//document.writeln("===============<br>")	
			
			//2  isNaN 判断是否为非数字
				
				
			//3、toString()：把数字转为字符串	

			//n不是数字吗？怎么会有方法
				
				
			// 4 Number() 方法  可用于把 其它类型转换为数值
			    //布尔类型
			
			    //数字组成的字符串
						
				//日期对象
				
				 //字母+数字字符串
				 
				//5 parseInt() 方法 解析一段字符串并返回整型数值。允许空格。只返回首个数字
				
				
		</script>
	</body>
</html>

```

### 静态成员

```html
<!DOCTYPE html>
<html>

<head>
	<meta charset="utf-8">
	<title></title>
</head>

<body>
	1、静态成员
	<script>
		function Person(name, age) {
			this.name = name;
			this.age = age;
			//静态属性 类名.属性
			if (!Person.count) {
				Person.count = 0;
			}
			Person.count++;
		}

		/*设置一个属性 计算创建出多少对象
		  需要静态成员-判定在构造函数上的属性和方法
		*/
		Person.print = function(){
			alert("创建了"+Person.count+"对象")
		}
		var p1=new Person('qwe', 123)
		var p2 = new Person('qw', 1)
		console.log(Person.count)
		Person.print()
	</script>

</body>

</html>
```

### 原型prototype

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title></title>
	</head>
	<body>
		
		2、构造函数问题：存在浪费内存空间问题
		<script>
			function Person(name,age){
				this.name=name;
				this.age=age;
				this.say=function(){
					console.log('hello');
				}
			}
			//浪费内存空间问题
			var p1 = new Person('abc', 123)
			var p2 = new Person('ab', 12)
			// console.log(p1)
			// console.log(p2)
			
			//===严格相等
			//如果两个值都引用同一个对象或是函数，那么相等，否则不相等
			// console.log(p1.say===p2.say)
		</script>
		3 构造函数原型prototype 
		通过原型来实现所有对象共享方法。
		<script>
			function Person(name,age){
				this.name=name;
				this.age=age;				
			}
			//每个构造函数 都有一个prototype属性,指向另外一个对象.
			console.dir(Person)
			//我们可以把共有的方法,直接定义在prototype对象上,让所有对象实例共享.
			//通过原型添加方法
			Person.prototype.say = function(){
				alert("hello")
			}
			var p1=new Person("tom",10);
			var p2=new Person("jerry",10);
			
			console.log(p1.say===p2.say)
		</script>
		
		</script>
	</body>
</html>
```

### 基于原型链的继承

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title></title>
	</head>
	<body>
		1 基于原型链的继承
		<script>
			function Person(age){
				this.age=age				
			}
			Person.prototype.say=function(){
				alert('hello world');
			}
			
			function Student(){
			}
			//继承
			//1 构造父类的实例
			var p1=new Person(20)
			//2 设置为子类的原型对象
			Student.prototype=p1
			////问题：Student.prototype.constructor指向哪个构造函数
			// console.log(Student.prototype.constructor===p1.constructor)
			//3 修复子类函数原型的constructor指针即可
			Student.prototype.constructor=Student;

			var stu1=new Student()
			// stu1.say()
			// console.log(stu1.age)
		</script>
		<br>
		2 存在问题：实例共享引用类型，新创建的对象都有“玩游戏”这个变量，如p1、p2
		<script>
			function Parent() {
			  	this.newArr = ["唱歌", "跑步"];
			}
			function Child() {
			  	this.name = "abc";
			}
			Child.prototype = new Parent()
			Child.prototype.constructor = Child
			// 对于属性不希望继承
			var p1 = new Child()
			p1.newArr.push("玩游戏")
			// console.log(p1.newArr);///

			var p2 = new Child();
			// console.log(p2.newArr)
		</script>
		<br>
		3 call函数
		<script>
			//call函数
			var demo1={
				name:'好人:',
				showABC:function(param1,param2){
					console.log(this.name,param1,param2)
				}
			}
			demo1.showABC('张三','李四')
			var demo2={
				name:'坏人'
			}
			//call来让demo2对象 借用demo1的showABC方法
			demo1.showABC.call(demo2, "1", '2')
			
		</script>
		<br>
		4 利用构造函数继承属性
		<script>
			function Father(uname,age){
				this.uname=uname;
				this.age=age;
				console.log(uname, age)
			}
			// Son想要继承Father的属性
			function Son(name, age){
				Father.call(this, name, age)
			}
			console.log(Son(12, 2))
		</script>
		
	</body>
</html>

```



### 对象原型proto

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title></title>
	</head>
	<body>
		
		4 对象原型__proto__
			请问为什么对象 可以访问原型里的方法？		
			原因：每个对象都有一个属性__proto__指向原型prototype,
			所以可以使用原型对象里的方法
		<script>
			function Person(name,age){
				this.name=name;
				this.age=age;				
			}
			Person.prototype.sayHello = function(){
				alert("Hello")
			}
			var p1=new Person("tom",10);
			var p2=new Person("jerry",10);
			// 输出__proto__属性
			// console.log(p1.__proto__)
			// console.log(p1.__proto__===p2.__proto__)
		</script>
		<br>
		5 原型对象的constructor构造函数
			原型对象里面都有一个属性
			constructor属性,constructor我们称为构造函数
			因为它指回构造函数本身.
			<script>
				// console.log(Person.prototype.constructor)
				//有时候 我们需要用写代码让constructor指向原来的构造函数
				Person.prototype = {
					eat: function(){},
					study:function(){},
					toString:function(){}
				}
				// Person.prototype.toString = function(){
				// 	alert("alert")
				// }
				//打印 constructor指向了?
				// consoel.log(Person.prototype.constructor)
				
				//可以手动让constructor指向原来的构造函数
				
				// 父亲的父亲的父亲
				// null 继承
				console.log(p1.__proto__.toString()===p1.__proto__.__proto__.toString())
				console.log(p1.__proto__.__proto__.toString())
			</script>
			<br>
			6 原型对象的应用  扩展内置对象方法
			  数组也有原型对象
			<script>
				Array.prototype.sun = function(){
					var sum=0
					for(var i=0; i>this.length; i++){
						sum+=i;
					}
					console.log('sum: '+sum)
					return sum
				}
				var arr = [1, 2, 3, 4, 5]
				console.log(arr.__proto__)
			</script>
			
	</body>
</html>
```

### ES6的类

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>上课</title>
</head>
<body>
    1、箭头函数
    <script>
        var fn = function(){
            console.log("1")
        }
        var fn1=()=>{
            console.log("fn1")
        }
        // fn()
        // fn1()
    </script>
    <br>
    2、es6中标准的类
    <script>
        class Person{
            name="23"
            age="20"
            // say=function(){
            //     console.log(name, age)
            // }

            //将函数直接添加到原型Prototype上面
            say(){
                console.log(name, age)
            }
        }
        var p1 = new Person()
        // console.log(p1)
        // console.log(p1.prototype===p1.__proto__)

    </script>
    <br>
    3、静态成员
    <script>
        class Student{
            name="2"
            static count=0
            static doCount(){
                if(!Student.count){
                    Student.count=0
                }
                Student.count++
                console.log("当前对象的个数是"+Student.count)
            }
        }
        var s1 = new Student();
        // Student.doCount()
        var s1=new Student();
        // Student.doCount()
        // console.log(s1===s1)
    </script>
    <br>
    4、类中的构造函数
    <script>
        class Teacher{
            // name="tea"
            // age=999
            //私有属性 #+属性名
            #name
            #age
            getAge(){
                return this.#age
            }
            set age(age){
                if(age>0 && age<200)
                    this.#age=age
                else
                    console.log("数据错误")
            }
            get name(){
                return this.#name
            }
            set name(name){
                this.#name=name
            }
            constructor(name, age){//可以不用写属性
                this.#name=name
                this.#age=age
            }
            // Uncaught SyntaxError: A class may only have one constructor
            // constructor(name, age, ls){
            //     this.name=name
            //     this.age=age
            //     this.ls = ls
            // }
            say(){
                console.log(this.name, this.age, this.#name, this.#age)//
            }
        }
        var t1 = new Teacher("t", 123, "2",'23')
        // console.log(t1.getAge())
        // t1.say()
        // t1.age=10
        // t1.say()
        // console.log(t1.age2)
    </script>
    <br>
    5、继承
    <script>
        class S extends Teacher{
            constructor(name, age){
                super(name, age)
                
            }
            say(){
                console.log(this.age)
            }
        }
        var s = new S('2', '3')
        // s.say()
        console.log(S.prototype)
        console.log(S.__proto__)
        console.log(S.__proto__.__proto__)
        console.log(S.__proto__.__proto__.__proto__)
        console.log(S.__proto__.__proto__.__proto__.__proto__)

    </script>
</body>
</html>
```

### 定时器

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title></title>
		
	</head>
	<body>
		BOM:浏览器对象模型 window对象 顶级对象
		
		1、window对象的方法：setInterval(调用函数，毫秒数)  定时器
			方法可按照指定的周期（以毫秒计）来调用函数或计算表达式。
			注意：调用函数有3中写法：
						  函数        函数名       字符串（'函数名（）'）
		<br>
		<script>
			//1 函数 js函数可以作为参数
			//window可以省略
			// setInterval(function(){
			// 	console.log("hello")
			// }, 3000)
			//2 函数名
			function sayHello(){
				alert('hello');
			}
			// setInterval(sayHello, 3000)
			//3 字符串（'函数名（）')
			var tid = setInterval("alert('hello')", 3000)
			
		</script>
		
		2、取消定时器： clearInterval() 
			由 setInterval() 返回的 ID 值可用作 clearInterval() 方法的参数。<br>
			
			<button id='btn1'>开启</button>
			<button id='btn2'>关闭</button>
			<script>
				var btn2=document.getElementById("btn2");
				var btn1=document.querySelector()
				btn2.onclick=function(){
					clearInterval(tid)
				}
					
			</script>
		
	</body>
</html>

```

### 函数中this指针

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title></title>
	</head>
	<body>
		1、this是什么？
		函数中的this指函数的实际调用对象。<hr>
		<script>
			// console.log(this)
			// var obj={name:"123"}
			// obj.fn = function(){
			// 	console.log(this)
			// 	console.log(this.name)
			// }
			// // obj.fn()
			// // setTimeout(obj.fn, 1000)
			// console.log(typeof(obj))
		</script>
		2、箭头函数的this
		箭头函数没有自己的this,它的this是外层作用域的this<hr>
		<script>
			function fn1(){
				console.log("fn1-",this);
			}
			var fn2=()=>{
				console.log("fn2",this);
			}
			// fn1()
			// fn2()
			var obj1={
				name:"124",
				fn1,
				fn2,
				fn3:()=>{
					console.log(this)
				}
			}
			// obj1.fn1()//类的this
			// obj1.fn2()//外部的window
			// obj1.fn3()//同样也是外部的window
			var obj2={
				obj1
			}
			console.log(obj2.obj1.fn1())
			console.log(obj2.obj1.fn2())
			console.log(obj2.obj1.fn3())
		</script>
		3、 函数是Function类的实例
		可以通过new Function动态创建函数对象。
		<script>
			var add= new Function("x", "y", "return x+y")
			console.log(add(1, 2))
		</script>
	</body>
</html>
```

### 高阶函数

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title></title>
	</head>
	<body>
		1、js中函数可以作为参数传递<br>
		<script>
			function say (str) {
				alert(str);
			}
			//01方法函数作为参数
			// 高阶函数
			function fn(fun, str){
				fun(str)
			}
			// fn(say, "123")
			// setInterval(say, 3000, "1")
			//02直接将匿名函数作为参数传递给fn1方法
			
			// fn(function(str){
			// 	alert(str)
			// }, "1111111")
		</script>
		<br>
		2、函数作为返回值
		<script>
			/*希望test1函数执行时，可以记录一条日志
			开闭原则：对扩展开放，对修改关闭
			在不修改原函数的基础上，增加此功能。
			可以通过函数作为返回值 来动态生成一个新函数*/
			function test1(){
				var str="heollo"
				console.log(str)
			}
			function outer(fun){
				return ()=>{
					console.log("添加日志------------")
					fun()
				}//返回值是函数
			}
			outer(test1)//返回值是函数
			var f1=outer(test1)//接受返回值
			f1()
		</script>
		3、什么高阶函数<br>
		
		
	</body>
</html>
```

### 闭包

```html
<!DOCTYPE html>
<html>
<head>
	<meta charset="utf-8">
	<title></title>
</head>
<body>
	闭包
	01 什么是闭包：函数A中 有一个函数B，函数B中可以访问函数A中定义的变量
	或者数据，此时就形成了闭包。
	02 闭包的功能： 使函数内部的变量在函数执行完后，仍然存活在内存中
	（延长了局部变量的生命周期）
				让函数外部可以操作函数内部的数据
	<script>
		//1定义一个闭包
		function A(){
			var num=123
			function B(){
				console.log(num)
			}
			B()
		}
		// A()//打断点之后会出现，Closure (A)，闭包
		// console.log(num)

		//2 在f1 外部来访问n;
		function f1(){
			var n=10
			return function(){
				console.log(n)
				return n;
			}
		}
		// console.log(n);
		
		//console.log("函数外 输出局部变量n的值："+ );
		
		/*闭包的功能： 使函数内部的变量在函数执行完后，
		仍然存活在内存中（延长了局部变量的生命周期）
		让函数外部可以操作函数内部的数据*/
		
		//3 函数执行完后 局部变量就自动释放了 ？为什么还能访问？
		var f2=f1()
		f2()//延长了n的作用周期
		
	</script>
</body>
</html>
```

### 闭包案例

```html
<!DOCTYPE html>
<html>

<head>
	<meta charset="utf-8">
	<title></title>
</head>

<body>
	1、案例一<br>
	<script>
		/*	function f1(){
				var num=10;
				num++;
				console.log(num);
			}
			//运行结果是？
			f1()  //11
			f1()  //11 12			
			f1()*/

	</script>
	如何实现累加？-闭包
	<script>
		function f2() {
			var n = 10
			return function () {
				n++
				return n
			}
		}
		var ff = f2()
		// console.log(ff())
		// console.log(ff())
		// console.log(ff())
	</script>
	<hr>
	2、案例二：下来3个按钮，点击其中一个，提示"点击的是第n个按钮"：<br>
	<button>桔子1</button>
	<button>苹果2</button>
	<button>梨子3</button>
	<script>
		//html5 通过css选择器获得页面元素
		var btns = document.querySelectorAll("button")
		// console.log(btns)
		// for(var i=0; i<btns.length; i++){
		// 	btns[i].index=i+1;
		// 	// 注册事件
		// 	btns[i].onclick=function(){
		// 		alert("点击了第"+this.index+"个按钮")
		// 	}
		// }
		// console.log(i)
		//闭包来获得按钮的索引
		for (var i = 0; i < btns.length; i++) {
			(function (index) {
				btns[index].onclick = function () {
					alert("点击了第" + (index+1) + "个按钮")
				}
			})(i + 1);
		}
	</script>

</body>

</html>
```

闭包：实现可以持续分别点赞。

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <title></title>
    <style>
        h1{
            color: aquamarine;
        }
    </style>
</head>
<body>
    3、案例三<hr>
    <h1>请给下列新闻投票</h1>
    <ul>
        <li>北京冬奥会火种抵达北京<button>投票</button></li>
        <li>31省区市新增本土确诊17例。<button>投票</button></li>
        <li>互联网公司校招名额大幅增加 核心岗位首次开放<button>投票</button></li>
        <li>多地取暖用煤价格涨超2倍<button>投票</button></li>
    </ul>
    <script>
        var count = new Array()
        count[0]=0
        count[1]=0
        count[2]=0
        count[3]=0
        var buttons = document.querySelectorAll("button")
        // 闭包方法1
        // function clickCount(i){
        //     var n=count[i];
        //     return function add(){
        //         count[i]++;
        //         n++;
        //         return n
        //     }
        // }
        // for(var i=0; i<buttons.length; i++){
        //     let add1=clickCount(i)
        //     buttons[i].onclick=function(){
        //         this.innerHTML=add1()+"票"
        //     }
        // }
        // 闭包方法2
        // for(var i=0; i<buttons.length; i++){
        //     (function(i){
        //         var n=0
        //         buttons[i].onclick=function(){
        //             n++
        //             this.innerHTML=n+"票"
        //         }
        //     })(i)
        // }
        // 闭包方法3
        for(var i=0, j=1; i<buttons.length; i++){
            (function(num){
                buttons[i].onclick=function(){
                    this.innerHTML=num+"票"
                    num++;
                }
            })(j)
        }
    </script>
</body>
</html>
```

### 正则表达式

```js
<script>
        var rex = new RegExp(/abc/);
        var f1=rex.test('abc')
        var f2=rex.test("abcdd")
        console.log(f1, f2)

        var reg=/abb/
        var s=reg.exec('abbbbb')//搜索字符串并且返回已找到的文本
        console.log(s)

        var rg=/[a-z]/
        var reg=/[a-zA-Z]{2}/
        var s=reg.exec('a1a1v1b')
        console.log(s)
        var reg=/[zo*]/ //贪婪模式 ?结束贪婪模式
        var reg=/a{3}$/
        var reg=/s/
        var s=reg.exec("vaaa")
        console.log(s)
    </script>
```

### 事件监听

```html
<!DOCTYPE html>
<html>

<head>
	<meta charset="utf-8">
	<title></title>
	<style>
		#div1 {
			width: 200px;
			height: 200px;
			background: beige;
		}
	</style>
</head>

<body>
	1、event事件对象：是浏览器在事件触发时所创建的对象。
	这个对象封装了事件相关的各种信息。
	-通过事件对象 可以获取事件的各种信息。
	<hr>
	2、Dom中存在着多种不同类型的时间对象-他们的父对象都是Event
	<hr>

	<div id='div1'></div><br>
	<button id='btn1'>按钮1</button>
	<script>
		var div1 = document.getElementById('div1');
			var div1 = document.getElementById("div1")
			var btn1 = document.getElementById("btn1")
			// console.log(div1, btn1)		
			
			
			div1.onclick=function(event){
				// console.log(event)
				//target 触发事件的对象
				console.log(event.target)
				//this
				// console.log(this)
				div1.textContext=event.clientX+"  "+event.clientY
			}
			//事件的另外一种方式:监听事件 绑定多个方法
			btn1.addEventListener('click', function(){
				alert("1234")
			})
			btn1.addEventListener('click', function(){
				alert("12345")
			})
	</script>

</body>

</html>
```

### 事件冒泡

```html
<!DOCTYPE html>
<html>

<head>
	<meta charset="utf-8">
	<title></title>
	<style>
		#div1 {
			width: 300px;
			height: 300px;
			background: red;
		}
		#div2{
			width: 200px;
			height:100px;
			background-color: aqua;
		}
	</style>
</head>

<body>

	3、 事件冒泡:(bubble)是指事件向上传导
	当元素的事件被触发后，其父元素上的相同事件也同时触发。
	事件冒泡的存在简化了代码的编写，但是有时我们并不希望冒泡存在。
	取消事件冒泡：event.stopPropagation()


	<div id='div1'>
		<div id="div2"></div>
	</div><br>
	<script>
		var div1 = document.getElementById('div1');
		div1.onclick = function (ev) {
			alert("div1")
		}
		var div2 = document.getElementById('div2');
		div2.onclick = function (ev) {
			// 取消默认行为
			ev.stopPropagation()
			alert("div123")
		}
	</script>
	4 取消默认行为。
	<a id='a1' href="https://www.ahut.edu.cn">安工大</a>
	<script>
		var a1 = document.getElementById('a1');
		a1.onclick=function(ev){
			alert("安工大")
			
			//取消跳转的默认行为
			// 1.return false
			// 2.ev.preventDefault()
		}
	</script>
</body>

</html>
```

### 实现光标跟随效果

```html
<!DOCTYPE html>
<html>

<head>
  <meta charset="utf-8">
  <title>实验四</title>
  <style>
    div {
      width: 20px;
      height: 20px;
      background-color: bisque;
      position: absolute;
    }
  </style>
</head>

<body>
  <div class="div"></div>
  <div class="div"></div>
  <div class="div"></div>
  <div class="div"></div>
  <div class="div"></div>
  <div class="div"></div>
  <div class="div"></div>
  <div class="div"></div>
  <div class="div"></div>
  <div class="div"></div>
  <div class="div"></div>
  <div class="div"></div>
  <div class="div"></div>
  <div class="div"></div>
  <div class="div"></div>
  <div class="div"></div>
  <script>
    var divs=document.querySelectorAll(".div") 
    // console.log(divs.length)
    document.onmousemove = function (ev) {  
      divs[0].style.left=ev.clientX+"px"
      divs[0].style.top=ev.clientY+"px"
      for(var i=15; i>0; i--){
        divs[i].style.left=divs[i-1].style.left
        divs[i].style.top=divs[i-1].style.top
      }
      // for(var i=1; i<16; i++){
      //   divs[i].style.left=divs[i-1].style.left
      //   divs[i].style.top=divs[i-1].style.top
      // }
    }  
  </script>
  解题思路：
  
  1. 多个div跟随光标，第一个div的位置 始终是 光标移动到的位置。
  
  2. 第二个div，当移动的过程中，会到第一个div之前的位置，第三个div 会到第二个div 之前的位置。
  
  3.前一个div的位置赋给当前的div  :
  obj.offsetTop 指 obj 相对于父元素上侧位置，整型   
  obj.offsetLeft 指 obj 相对于父元素左侧位置，整型
           
  4.最后开始，在第一个div 还没移动的时候，第10个去到第9个，第9个去到第8个的位置......最后再把第一个移动到光标的位置。
  
  标签: JS

</body>

</html>
```
