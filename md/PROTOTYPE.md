### <font color=#0099ff >prototype/__proto__/constructor</font>

我们创建的每一个函数都有一个prototype（原型）属性，这个属性是一个指针，指向一个对象，而这个对象的用途是包含可以由特定类型的所有实例共享的属性和方法

<font color=red>引用类型才具有prototype属性</font>，包含：

1.Object

2.Function

3.Array

4.Date

5.String

6.RegExp

```
1 var fn=new String("text");
2 String.prototype.value="val";
3 console.log(fn.value);  //val
```


直接上例子

```
function　Person(){
}
Person.prototype.name = "Nicholas";
Person.prototype.age = 29;
Person.prototype.job = "Software Engineer";
Person.prototype.sayName = function(){
    console.log(this.name)
};
var person1 = new Person();
var person2 = new Person();
person2.sayName();  //Nicholas
console.log(person1.sayName == person2.sayName); //true
```

```
console.log(Person.prototype); //Object {name: "Nicholas", age: 29, job: "Software Engineer"}  
console.log(Person.prototype.isPrototypeOf(person1)) //true;
console.log(Object.getPrototypeOf(person1).name) //Nicholas;
console.log(Person.prototype.constructor == Person); //true
```

简单解释

```
Person.prototype ==  person1.__proto__ ;
Person.prototype.constructor = Person ;
person1.__proto__.constructor = Person;

```


注意：  JS中的所有对象都是继承自Object对象，所以这里来源是Object {}

```
var student = {name:'aaron'}; 
console.log(student.__proto__);//Object {}
```
