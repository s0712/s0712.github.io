---
title: call/apply/bind区别和用法
date: 2018-04-24 10:51:54
tags: call/apply/bind
categories: JS
---

## apply和call的区别

ECMAScript规范给所有函数都定义了call与apply方法,他们的应用非常广泛,他们的作用也是一模一样的,只是传参的方式不同而已.


#### apply(thisArg, [argsArray])
apply方法传入两个参数: 一个是作为函数上下文的对象,另外一个是作为函数参数所组成的数组.

注意:thisArg
可选的。在 func 函数运行时使用的 this 值。需要注意的是，使用的 this 值并不一定是该函数执行时真正的 this 值，如果这个函数处于非严格模式下，则指定为 null 或 undefined 时会自动替换为指向全局对象（浏览器中就是window对象），同时值为原始值（数字，字符串，布尔值）的 this 会指向该原始值的包装对象


```
var obj ={
		name:'sxy'
}
function func(firstname,lastname){
	console.log(firstname+''+this.name+''+lastname)
}
func.apply(obj,['A','B']) //A,sxy,B
```

可以看到,obj是作为函数上下文的对象,函数func中的this指向了obj这个对象,参数A和B是放在数组中传入func函数,分别对应func参数的列表元素.

#### call()
call方法第一个参数也是作为函数上下文的对象,但是后面传入的事一个参数列表,而不是单个数组.

```
var obj={
	name:'sxy'
}
function func(firstname,lastname){
	console.log(firstname+''+this.name+''+lastname)
}
func.call(obj,'C','D')  //C sxy D
```

通过call和apply我们也可以实现对象的继承,示例:

```
var Parent=funtion(){
	this.name='sxy';
	this.age=22;
}
var child={};
console.log(child);  //object {},空对象
Parent.call(child);
console.log(child);//object{name:'sxy',age:22}
```

对比apply我们可以看到区别,C和D是作为单独的参数传给func函数,而不是放到数组中.
对于什么时候该用什么方法,其实不用纠结,如果你的参数本来就存在一个数组中,那自然就用apply,如果参数比较散乱,相互之间没什么关联,那么就用call.

### apply和call的用法
1.改变this指向

```
var obj={
	name:'sxy'
}
function func(){
	console.log(this.name)
}
func.call(obj);		//sxy
```
我们知道,call方法的第一个参数是作为函数上下文的对象,这里吧obj作为参数传给了func,此时函数里的this便指向了obj对象,此处func函数里其实相当于

```
function func(){
	console.log(obj.name)
}

```

2.借用别的对象的方法
先看例子

```
var Person1=function(){
	this.name='sxy'
}
var Person2=function(){
	this.getname=function(){
		console.log(this.name)
	}
	Person1.call(this)
}
var person=new Person2();
person.getname();   //sxy
```

从上面我们看到,Person2实例化出来的对象person通过getname方法拿到了Person1中的name. 因为在Person2中,Person1.call(this)的作用就是使用Person1对象代替this对象,那么Person2就有了Person1中所有的属性和方法了,相当于Person2继承了Person1的属性和方法.

3.调用函数
apply和call方法都会是函数立即执行,因此他们也可以用来调用函数

```
function func(){
	console.log('sxy')
}
func.call();   //sxy
```

## call和bind的区别
在ES5中扩展了叫bind的方法,在低版本的IE汇中不兼容.它和call很相似,接受的参数有两部分,第一个参数是作为函数上下文,第二部分参数是个列表,可以接收多个参数.他们之间的区别有一下两点:
1.bind返回值是函数

```
var obj={
	name:'sxy'
}
function func(){
	console.log(this.name)
},
var func1=func.bind(obj);
func1()  //sxy
```

bind方法不会立即执行,而是返回一个改变了上下文this后的函数,而原函数func中的this并没有被改变,依旧指向全局对象window.

2.参数的使用

```
function func(a,b,c){
	console.log(a,b,c)
}
var func1=func.bind(null,'sxy');
func('A','B','C')   // A B C
func1('A','B','C')  //sxy A B
func1('B','C')   //sxy  B C
func.call(null,'sxy')   //sxy,udf,udf
```
call是把第二个及以后的参数作为func方法的实参传进去,而func1方法的实参实则在bind参数的基础上再往后排.

在低版本浏览器中没有bind方法,但我们可以自己实现一个

```
if(!Function.prototype.bind){
	Function.prototype.bind=function(){
		var self=this,						//保存原函数
		context=[].shift.call(arguments),//保存需要绑定的this上下文
		args=[].slice.call(arguments);//剩余的参数转为数组
		return function(){
			self.apply(context,[].concat.call(args,[].slice.call(arguments)))
		}
	}
},

```