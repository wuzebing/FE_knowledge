### <font color=#0099ff >BFC</font>

#### <font color=#0099ff >块级格式化上下文(Block Fromatting Context) </font>

在一个Web页面的CSS渲染中，块级格式化上下文 (Block Fromatting Context)是按照块级盒子布局的。W3C对BFC的定义如下：

浮动元素和绝对定位元素，非块级盒子的块级容器（例如 inline-blocks, table-cells, 和 table-captions），以及overflow值不为“visiable”的块级盒子，都会为他们的内容创建新的BFC（块级格式上下文）。
为了便于理解，我们换一种方式来重新定义BFC。一个HTML元素要创建BFC，则满足下列的任意一个或多个条件即可：


<font color=red>1、float的值不是none。</font><br/>
<font color=red>2、position的值不是static或者relative。</font><br/>
<font color=red>3、display的值是inline-block、table-cell、flex、table-caption或者inline-flex</font><br/>
<font color=red>4、overflow的值不是visible</font><br/>

<br/>
BFC是一个独立的布局环境，其中的元素布局是不受外界的影响，并且在一个BFC中，块盒与行盒（行盒由一行中所有的内联元素所组成）都会垂直的沿着其父元素的边框排列。

#### 怎么创建BFC

要显示的创建一个BFC是非常简单的，只要满足上述4个CSS条件之一就行。例如：

```
<div class="container">
  你的内容
</div>
```

在类container中添加类似 overflow: scroll，overflow: hidden，display: flex，float: left，或 display: table 的规则来显示创建BFC。虽然添加上述的任意一条都能创建BFC，但会有一些副作用：

1、display: table 可能引发响应性问题

2、overflow: scroll 可能产生多余的滚动条

3、float: left 将把元素移至左侧，并被其他元素环绕

4、overflow: hidden 将裁切溢出元素

因而无论什么时候需要创建BFC，都要基于自身的需要来考虑。对于本文，将采用 overflow: hidden 方式：

```
.container {
    overflow: hidden;
}
```


#### <font color=#0099ff >外边距折叠</font>

常规流布局时，盒子都是垂直排列，两者之间的间距由各自的外边距所决定，但不是二者外边距之和。

```
<div class="container">
  <p>Sibling 1</p>
  <p>Sibling 2</p>
</div>
```

```
.container {
  background-color: red;
  overflow: hidden; /* creates a block formatting context */
}
p {
  background-color: lightgreen;
  margin: 10px 0;
}

```

渲染结果如图：

![图片](/img/bfc_01.jpeg)

在上图中，一个红盒子（div）包含着两个兄弟元素（p），一个BFC已经创建了出来。

理论上，两个p元素之间的外边距应当是二者外边距之和（20px）但实际上却是10px，这是外边距折叠(Collapsing Margins)的结果。

在CSS当中，相邻的两个盒子（可能是兄弟关系也可能是祖先关系）的外边距可以结合成一个单独的外边距。这种合并外边距的方式被称为折叠，并且因而所结合成的外边距称为折叠外边距。折叠的结果按照如下规则计算：

1、两个相邻的外边距都是正数时，折叠结果是它们两者之间较大的值。

2、两个相邻的外边距都是负数时，折叠结果是两者绝对值的较大值。

3、两个外边距一正一负时，折叠结果是两者的相加的和。

产生折叠的必备条件：margin必须是邻接的! 


<font color=red size=18>BFC可以做什么呢？(重点)</font>

#### <font color=#0099ff >利用BFC避免外边距折叠</font>

```
<div class="container">
    <p>Sibling 1</p>
    <p>Sibling 2</p>
    <div class="newBFC">
        <p>Sibling 3</p>
    </div>
</div>
```

```
.container {
  background-color: red;
  overflow: hidden; /* creates a block formatting context */
}
p {
  background-color: lightgreen;
  margin: 10px 0;
}
.newBFC {
    overflow: hidden;  /* creates new block formatting context */
}
```

现在的结果如图：

![图片](/img/bfc_02.png)


#### <font color=#0099ff >BFC包含浮动</font>（感觉可以称之为清除浮动）

浮动元素是会脱离文档流的(绝对定位元素会脱离文档流)。如果一个没有高度或者height是auto的容器的子元素是浮动元素，则该容器的高度是不会被撑开的。我们通常会利用伪元素(:after或者:before)来解决这个问题。BFC能包含浮动，也能解决容器高度不会被撑开的问题。

![图片](/img/bfc_03.jpeg)

```
<div class="container">
    <div>Sibling</div>
    <div>Sibling</div>
</div>
```

```
.container {
  background-color: green;
}
.container div {
  float: left;
  background-color: lightgreen;
  margin: 10px;
}
```

在上面这个例子中，容器没有任何高度，并且它包不住浮动子元素，容器的高度并不会被撑开。为解决这个问题，可以在容器中创建一个BFC：

```
.container {
    overflow: hidden; /* creates block formatting context */
    background-color: green;
}
.container div {
    float: left;
    background-color: lightgreen;
    margin: 10px;
}
```

<font color=red>现在容器可以包住浮动子元素，并且其高度会扩展至包住其子元素，在这个新的BFC中浮动元素又回归到页面的常规流之中了。</font>

#### <font color=#0099ff >使用BFC避免文字环绕</font>

![图片](/img/bfc_04.jpeg)

如上图所示，对于浮动元素，可能会造成文字环绕的情况(Figure1)，但这并不是我们想要的布局(Figure2才是想要的)。要解决这个问题，我们可以用外边距，但也可以用BFC。

![图片](/img/bfc_05.jpeg)

```
<div class="container">
    <div class="floated">
        Floated div
    </div>
    <p>
        Quae hic ut ab perferendis sit quod architecto, 
        dolor debitis quam rem provident aspernatur tempora
        expedita.
    </p>
</div>
```

```
p {
  width:200px;
}
.floated {
  float: left;
  width:100px;
}
```

使用BFC方式解决环绕问题

```
p {
  width:200px;
  overflow:hidden; // this
}
.floated {
  float: left;
  width:100px;
}
```

#### <font color=#0099ff >在多列布局中使用BFC</font>(例子不好，@todo)

如果我们创建一个占满整个容器宽度的多列布局，在某些浏览器中最后一列有时候会掉到下一行。这可能是因为浏览器四舍五入了列宽从而所有列的总宽度会超出容器。但如果我们在多列布局中的最后一列里创建一个新的BFC，它将总是占据其他列先占位完毕后剩下的空间。

```
<div class="container">
    <div class="column">column 1</div>
    <div class="column">column 2</div>
    <div class="column">column 3</div>
</div>
```

```
.column {
    width: 31.33%;
    background-color: green;
    float: left;
    margin: 0 1%;
}
/*  Establishing a new block formatting 
    context in the last column */
.column:last-child {
    float: none;
	overflow: hidden; 
}
```







