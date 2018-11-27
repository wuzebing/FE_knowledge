### <font color=#0099ff >HTTP缓存机制<font>

#### <font color=#0099ff >缓存规则解析<font>

为方便大家理解，我们认为浏览器存在一个<font color=red>缓存数据库</font>,用于存储缓存信息。
在客户端第一次请求数据时，此时缓存数据库中没有对应的缓存数据，需要请求服务器，服务器返回后，将数据存储至缓存数据库中。<br/>
![图片](/img/cache_01.png)
<br/>
HTTP缓存有多种规则，根据是否需要重新向服务器发起请求来分类，我将其分为两大类<font color=red>(强制缓存</font>，<font color=red>对比缓存</font>)
在详细介绍这两种规则之前，先通过时序图的方式，让大家对这两种规则有个简单了解。

#### <font color=#0099ff >强制缓存</font>

已存在缓存数据时，仅基于强制缓存，请求数据的流程如下，__<font color=red>缓存命中不与服务器交互</font>__<br/>
![图片](/img/cache_02.png)


![图片](/img/cache_04.png)<br/>

* <font color=red>标识字段 Expires/Cache-Control</font> <br/>

Expires的值为服务端返回的到期时间，即下一次请求时，请求时间小于服务端返回的到期时间，直接使用缓存数据。不过Expires 是HTTP 1.0的东西，现在默认浏览器均默认使用HTTP 1.1，所以它的作用基本忽略。另一个问题是，到期时间是由服务端生成的，但是客户端时间可能跟服务端时间有误差，这就会导致缓存命中的误差。<font color=red>所以HTTP 1.1 的版本，使用Cache-Control替代</font>。<br/>


Cache-Control 是最重要的规则。常见的取值有private、public、no-cache、max-age，no-store，默认为private。<br/><font color=#974806>private:</font>客户端可以缓存<br/><font color=#974806>public:</font>客户端和代理服务器都可缓存（前端的同学，可以认为public和private是一样的）<br/><font color=#974806>max-age=xxx: </font>缓存的内容将在 xxx 秒后失效<br/><font color=#974806>no-cache:</font>需要使用对比缓存来验证缓存数据（后面介绍）<br/><font color=#974806>no-store:</font>所有内容都不会缓存，强制缓存，对比缓存都不会触发（对于前端开发来说，缓存越多越好，so...基本上和它说886）<br/>


![图片](/img/cache_05.png)
<br/>
图中Cache-Control仅指定了max-age，所以默认为private，缓存时间为31536000秒（365天）
也就是说，在365天内再次请求这条数据，都会直接获取缓存数据库中的数据，直接使用。

#### <font color=#0099ff >对比缓存</font> (分为两种)

已存在缓存数据时，仅基于对比缓存，请求数据的流程如下，__<font color=red>缓存命中不命中都需要与服务器交互</font>__ <br/>
![图片](/img/cache_03.png)

![图片](/img/cache_06.png)

* <font color=red>标识字段 Last-Modified  /  If-Modified-Since </font><br/>
![图片](/img/cache_07.png)

![图片](/img/cache_08.png)

再次请求服务器时，通过此字段通知服务器上次请求时，服务器返回的资源最后修改时间。
服务器收到请求后发现有头If-Modified-Since 则与被请求资源的最后修改时间进行比对。
若资源的最后修改时间大于If-Modified-Since，说明资源又被改动过，则响应整片资源内容，返回状态码200；
若资源的最后修改时间小于或等于If-Modified-Since，说明资源无新修改，则响应HTTP 304，告知浏览器继续使用所保存的cache。


* <font color=red>Etag  /  If-None-Match（优先级高于Last-Modified  /  If-Modified-Since）</font>
![图片](/img/cache_09.png)
![图片](/img/cache_10.png)

再次请求服务器时，通过此字段通知服务器客户段缓存数据的唯一标识。
服务器收到请求后发现有头If-None-Match 则与被请求资源的唯一标识进行比对，
不同，说明资源又被改动过，则响应整片资源内容，返回状态码200；
相同，说明资源无新修改，则响应HTTP 304，告知浏览器继续使用所保存的cache。


#### <font color=#0099ff >总结<font>
强制缓存（Expires / Cache-Control） > 对比缓存（Etag / If-None-Match） > 对比缓存（Lash-Modified / If-Modified-Since）

![图片](/img/cache_11.png)


> 对缓存机制不太了解的同学可能会问，基于对比缓存的流程下，不管是否使用缓存，都需要向服务器发送请求，那么还用缓存干什么？
这个问题，我们暂且放下，后文在详细介绍每种缓存规则的时候，会带给大家答案。

> 我们可以看到两类缓存规则的不同，强制缓存如果生效，不需要再和服务器发生交互，而对比缓存不管是否生效，都需要与服务端发生交互。
<font color=red>两类缓存规则可以同时存在，强制缓存优先级高于对比缓存</font>，也就是说，当执行强制缓存的规则时，如果缓存生效，直接使用缓存，不再执行对比缓存规则。