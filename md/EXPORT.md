### <font color=#0099ff >export和export default区别</font>

* export与export default均可用于导出常量、函数、文件、模块等
* 你可以在其它文件或模块中通过import+(常量 | 函数 | 文件 | 模块)名的方式，将其导入，以便能够对其进行使用
* 在一个文件或模块中，export、import可以有多个，**export default仅有一个**
* **通过export方式导出，在导入时要加{ }，export default则不需要**

#### <font color=#0099ff >export</font>

```
let name1 = '张三';
let name2 = '李四';
export {name1, name2}; //区别1

//import {name1, name2} from 'a.js' //区别2
```
or

```
let name1 = '张三';
let name2 = '李四';
export name1;
export name2;

//import {name1, name2} from 'a.js'
```

#### <font color=#0099ff >export default</font>

```
let name1 = '张三';
export default name1; //区别1

//import name1 from 'a.js' //区别2
```

