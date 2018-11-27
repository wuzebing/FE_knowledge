### <font color=#0099ff >px、em、rem、%、vw、vh、vm</font>


#### <font color=#0099ff >px</font>
像素（pixel）

#### <font color=#0099ff >% </font>
百分比


#### <font color=#0099ff >rem</font>（重点）
rem（font size of the root element）<font color=red>是相对于根元素html，</font>这样就意味着，我们只需要在根元素确定一个参考值，可以设计HTML为大小为10px，到时设置1.2rem就是12px.以此类推。

```
html{ font-size:16px;}

//12px 对应于 16px  就是 0.75rem
```
那么我们这里的1rem就应该这么来计算：1x16=16px=1rem；<font color=red>浏览器默认为16px</font>可能造成rem计算上的麻烦和多位小数，所以，我们也可以进行这种方式的初始化根元素：
```
html{
   font-size=62.5% //这里就是10/16x100%=62.5% 也就是默认10px的字号
}
//12px 对应于 10px  就是 1.2rem
```
利用10的倍数，这样初始化之后，我们来进行rem计算的时候，就会减少许多的麻烦。
```
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no" />

```
这个做法不是完美兼容，不同分辨率的手机，效果是一样的。

##### <font color=green >完美兼容</font>（依赖设计图尺寸）
注意：这里面需要设计到设计图尺寸大小<br/>
一般我们做页面，肯定都会有设计图，移动端页面，一般情况下，UI出图都会定宽为640px，这也是移动端的标准尺寸；但是，我们也不能排除可能有其他特殊的情况可能需要做其他大小的设计图。所以，我们可以先定一个基准，然后来看看isux团队的整理出来的一个表格：
* 1、基于CSS<br/>
<!-- ![图片](/img/rem_01.jpg) -->

```
@media (min-width: 320px) {
    html {
        font-size: 14.22px;
    }
}

@media (min-width: 360px) {
    html {
        font-size: 16px;
    }
}

@media (min-width: 375px) {
    html {
        font-size: 16.67px;
    }
}

@media (min-width: 1440px) {
    html {
        font-size: 64px;
    }
}

// CSS单位使用rem
p.intro {
    font-size: 1rem;
}


```
* 2、基于JS进行屏幕分辨率计算
```
(function (doc, win) {
    var docEl = doc.documentElement,
        resizeEvt = 'orientationchange' in window ? 'orientationchange' : 'resize',
        recalc = function () {
            var clientWidth = docEl.clientWidth;<br>　　　　　　　window.innerWidth>max ?  window.innerWidth : max;
            if (!clientWidth) return;
            docEl.style.fontSize = 100 * (clientWidth / 640) + 'px';
        };
 
    if (!doc.addEventListener) return;
    win.addEventListener(resizeEvt, recalc, false);
    doc.addEventListener('DOMContentLoaded', recalc, false);
})(document, window);
```
<font color=red>100 和 640 是设计稿的尺寸基准，其实最重要的是基准计算100/640</font>

[参考链接](http://www.mamicode.com/info-detail-1816919.htmls)

[参考链接](https://www.cnblogs.com/noobfly/p/6207832.html)

推荐 flexible.js


#### <font color=#0099ff >em</font>
em（font size of the root element）<font color=red>参考物是父元素的font-size</font>，具有继承的特点。如果自身定义了font-size按自身来计算（浏览器默认字体是16px），整个页面内1em不是一个固定的值。<br/>
<font color=red>特点是1. em的值并不是固定的； 2. em会继承父级元素的字体大小。</font><br/>
<font color=red>优点是，只需要设置根目录的大小就可以把整个页面的成比例的调好。</font>

#### <font color=#0099ff >vw</font>

css3新单位，view width的简写，是指可视窗口的宽度。假如宽度是1200px的话。那10vw就是120px

举个例子：浏览器宽度1200px, 1 vw = 1200px/100 = 12 px。
#### <font color=#0099ff >vh</font>
css3新单位，view height的简写，是指可视窗口的高度。假如高度是1200px的话。那10vh就是120px

举个例子：浏览器高度900px, 1 vh = 900px/100 = 9 px。

#### <font color=#0099ff >vmin</font>
vw和vh中较小的那个。

#### <font color=#0099ff >vmax</font>
vw和vh中较大的那个。


<font color=red>vw, vh, vmin, vmax：IE9+局部支持，chrome/firefox/safari/opera支持，iOS safari 8+支持，Androidbrowser4.4+支持，chrome for android39支持</font>

#### <font color=#0099ff >vm</font>（可忽略）
css3新单位，相对于视口的宽度或高度中较小的那个。其中最小的那个被均分为100单位的vm 举个例子：浏览器高度900px，宽度1200px，取最小的浏览器高度，1 vm = 900px/100 = 9 px。

<font color=red>兼容性太差 ，现在基本上没人用，我试了一下Chrome就用不了。</font>
