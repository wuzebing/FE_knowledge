### <font color=#0099ff >闭包</font>

#### <font color=#0099ff >什么是闭包</font>

闭包，官方对闭包的解释是：一个拥有许多变量和绑定了这些变量的环境的表达式（通常是一个函数），因而这些变量也是该表达式的一部分。闭包的特点：

　　1. 作为一个函数变量的一个引用，当函数返回时，其处于激活状态。

　　2. 一个闭包就是当一个函数返回时，一个没有释放资源的栈区。

　　简单的说，Javascript允许使用内部函数---即函数定义和函数表达式位于另一个函数的函数体内。而且，这些内部函数可以访问它们所在的外部函数中声明的所有局部变量、参数和声明的其他内部函数。当其中一个这样的内部函数在包含它们的外部函数之外被调用时，就会形成闭包。

#### <font color=#0099ff >闭包，一睹为快</font>

```
//第1种写法  
function user () {
 var name = 'wangxi'
 return function getName () {
 	return name
 }
}
var userName = user()() // userName 变量中始终保持着对 name 的引用
console.log(userName) // wangxi
userName = null // 销毁闭包，释放内存
```

把这5步操作总结成一句话就是：

<strong>函数A的内部函数B被函数A外的一个变量 C 引用。</strong>

把这句话再加工一下就变成了闭包的定义：

<strong>当一个内部函数被其外部函数之外的变量引用时，就形成了一个闭包。</strong>

因此，当你执行上述5步操作时，就已经定义了一个闭包！

这就是闭包。


#### "JavaScript中的函数运行在它们被定义的作用域里，而不是它们被执行的作用域里."(重要理解)

```
var name = 'Schopenhauer'
function getName () {
 	console.log(name)
}
function myName () {
 	var name = 'wangxi'
 	getName()
}
myName() // Schopenhauer
```

```
var name = 'Schopenhauer'
function getName () {
	var name = 'Aristotle'
	var intro = function() { // 这是一个闭包
  		console.log('I am ' + name)
	}
	return intro
}
function showMyName () {
 	var name = 'wangxi'
 	var myName = getName()
 	myName()
}
showMyName() // I am Aristotle
```



#### <font color=#0099ff >为什么需要闭包？</font>

局部变量无法共享和长久的保存，而全局变量可能造成变量污染，所以我们希望有一种机制既可以长久的保存变量又不会造成全局污染。

#### <font color=#0099ff >特点</font>

占用更多内存

不容易被释放

何时使用？

变量既想反复使用，又想避免全局污染

#### <font color=#0099ff >如何使用？</font>

定义外层函数，封装被保护的局部变量。

定义内层函数，执行对外部函数变量的操作。

外层函数返回内层函数的对象，并且外层函数被调用，结果保存在一个全局的变量中。