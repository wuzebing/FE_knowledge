### <font color=#0099ff >babel把ES6转成ES5或者ES3之类的原理</font>


#### <font color=#0099ff >前言</font>

当我们还在沉迷于ES5的时候，殊不知ES6早就已经发布几年了。时代在进步，WEB前端技术也在日新月异，是时候做些改变了！

ECMAScript 6(ES6)的发展速度非常之快，但现代浏览器对ES6新特性支持度不高，所以要想在浏览器中直接使用ES6的新特性就得借助别的工具来实现。

Babel是一个广泛使用的转码器，babel可以将ES6代码完美地转换为ES5代码，所以我们不用等到浏览器的支持就可以在项目中使用ES6的特性。

babel 6与之前版本的区别：

之前版本只要安装一个babel就可以用了，所以之前的版本包含了一大堆的东西，这也导致了下载一堆不必要的东西。但在babel 

6中，将babel拆分成两个包：babel-cli和babel-core。如果你想要在CLI(终端或REPL)

使用babel就下载babel-cli，如果想要在node中使用就下载babel-core。 babel 6已结尽可能的模块化了，如果还用babel 

6之前的方法转换ES6，它会原样输出，并不会转化，因为需要安装插件。
如果你想使用箭头函数，那就得安装箭头函数插件npm install 

babel-plugin-transform-es2015-arrow-functions。


#### <font color=#0099ff >Babel转码</font>

```
var a = (msg) => () => msg;
 
var bobo = {
  _name: "BoBo",
  _friends: [],
  printFriends() {
    this._friends.forEach(f =>
      console.log(this._name + " knows " + f));
  }
};
```

实际上，上面的这段代码通过Babel转换后，会变成：

```
"use strict";
 
var a = function a(msg) {
  return function () {
    return msg;
  };
};
 
var bobo = {
  _name: "BoBo",
  _friends: [],
  printFriends: function printFriends() {
    var _this = this;
 
    this._friends.forEach(function (f) {
      return console.log(_this._name + " knows " + f);
    });
  }
};
```

#### <font color=#0099ff >配置package.json 和 .babelrc</font>

```
{
  "devDependencies": {
    "babel-cli": "^6.22.2"
  }
}
```

```
{
  "presets": [],
  "plugins": []
}
```

```
# ES2015转码规则
$ npm install --save-dev babel-preset-es2015
 
# react转码规则
$ npm install --save-dev babel-preset-react
 
# ES7不同阶段语法提案的转码规则（共有4个阶段），选装一个
$ npm install --save-dev babel-preset-stage-0
$ npm install --save-dev babel-preset-stage-1
$ npm install --save-dev babel-preset-stage-2
$ npm install --save-dev babel-preset-stage-3
```

```
{
    "presets": [
      "es2015",
      "react",
      "stage-2"
    ],
    "plugins": []
  }
```

#### <font color=#0099ff >Stage-X (实验阶段 Presets)</font>

Stage-x preset 中的任何转换都是对未被批准为 JavaScript 版本一部分的语言的变化(如 es6 / es2015 )。

>不稳定
>这些提案可能会有所改动，因此请谨慎使用，尤其是第 3 阶段以前的提案。我们计划会在每次 TC39 会议结束后更新对应的 stage-x preset。

TC39 将提案分为以下几个阶段:

* Stage 0 - 稻草人: 只是一个想法，可能是 babel 插件。
* Stage 1 - 提案: 初步尝试。
* Stage 2 - 初稿: 完成初步规范。
* Stage 3 - 候选: 完成规范和浏览器初步实现。
* Stage 4 - 完成: 将被添加到下一年度发布。
欲了解更多信息，请务必查看当前 TC39 提案及其文档流程.


![图片](/img/babel_01.jpg)

#### <font color=#0099ff >babel插件地址</font>

https://www.babeljs.cn/docs/plugins/




