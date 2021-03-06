title: 当我在谈论前端监控时我在谈什么
speaker: zhangxiang
url: https://github.com/ksky521/nodePPT
transition: slide3
files: /js/demo.js,/css/demo.css,/js/zoom.js
theme: moon
usemathjax: yes

[slide]
# 当我在谈论前端监控时我在谈什么 {:&.flexbox.vleft}
<small>share by：张翔</small>
> "我的源码被猫吃了" -- 出于对自己的代码, 功能负责的前提下, 应当为代码(项目)的可靠性可用性进行测试.

[slide]
## 性能监控
性能监控是一个成熟的前端开发过程中必不可少的步骤之一。它能够比较准确地让前端工程师知道网页性能的问题所在从而解决问题。

[slide]
## 为什么我们需要性能监控
很多时候我们的页面会在 wifi 或者 4G 等网络条件比较好的情况下进行测试, 这也是为了保证工作效率的手段之一, 但是
考虑到目前国内移动端网络情况其实并不是特别乐观的情况下, 我们需要优化我们的网页, 让很多 2/3 G 网络的用户也能
体验秒开.
如果我们不进行性能的监控, 清晰地了解到网页打开的每个步骤, 一旦产品上线，如果网页打开速度比较慢的话， 就会严重影响
用户体验，当情况反映到开发的工程师那里， 往往已经造成了成片的影响了.

![](https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1496941216670&di=8ea3f233690caea97c3e2a3d975dcd09&imgtype=0&src=http%3A%2F%2Fimg.zcool.cn%2Fcommunity%2F0141445553f56f0000009c50b35474.jpg)

[slide]
## 怎么做性能监控
业界已经有很多的 online 工具提供我们使用，例如 WebpageTest， YSlow 等等的这些工具，但是这些工具都有一些缺点让我们
无法大范围地使用它， 第一他们往往监控测试的指标比较单一， 例如只是检查页面 DOM 的大小与组件的大小， 第二，当我们在
内网测试网页的时候，就无法自如地使用这些 online 工具。因此，搭建性能监控是非常必要的。
做法是通过 window.performance 这个 API 去获取打开一个网页消耗的时间分布， 例如 DOM 解析的实践， TCP 建立连接的时间，
加载静态资源的数量与耗费时间，TTFB 等等的这些指标。编写客户端脚本，获取这些指标，然后通过 ajax 上传到后台系统中做分析。

[slide]
需要进行统计的指标有：
------
 * loadPage 时间 
 * domReady DOM 解析时间
 * ttfb 时间
 * DNS 查询时间
 * TCP 连接握手时间
 * ...

Navigation: 
------
![](http://ofsur12wi.bkt.clouddn.com/timgingfull.png)

[slide style="background-image: url('http://fex.baidu.com/img/build-performance-monitor-in-7-days/timing.png'); background-size: 100%;"]

[slide]
###后台系统数据

<pre>
<code class="markdown">
{
	"userAgent": "Mozilla/5.0 (Linux; U; Android 4.1; en-us; GT-N7100 Build/JRO03C) AppleWebKit/534.30 (KHTML, like Gecko) Version/4.0 Mobile Safari/534.30",
	"currentURL": "www.example.com/m/",
	"timestamp": 140000000,
	"pid": "456",
	"userId": "123",
    "perfData": {
		"sourceInfo": [
            {
                "name": "www.example.com/test.js",
                "type": "script",
                "DNSQuery": "100ms",
                "connectTime": "300ms",
                "loadTime": "200ms"
            },
            {
                "name": "www.example.com/test.js",
                "type": "script",
                "DNSQuery": "100ms",
                "connectTime": "300ms",
                "loadTime": "200ms"
            },
            {
                "name": "www.example.com/test.js",
                "type": "script",
                "DNSQuery": "100ms",
                "connectTime": "300ms",
                "loadTime": "200ms"
            }
		],
        "loadInfo": {
            "DNS Query": "100ms",
            "TCP Connect": "200ms",
            "Request Time": "12ms",
            "WhiteScreen Time": "1000ms",
            "DOM Analysis": "100ms",
            "Load Time": "1200ms"
        }
	}
}
</code>
</pre>

[slide]
## 异常监控
异常监控对于我们的调试也是非常重要的。能够比较迅速地让工程师知道错误的信息，方便复现 Bug，解决问题。

[slide]
## 项目反思
实际情景，在做 ME 移动端官网的时候，发现因为 Zepto 某些兼容性问题在 Samsung 手机浏览器上报错，但是需要真机调试加 fiddler来定位问题。如果我们有异常监控脚本的话，那么就可以在后台系统中获取到这些报错信息，迅速地定位问题。
![](https://pic4.zhimg.com/1d159b48ada0a69c81926897e0fbb1b7_b.jpg)

[slide]
### 为什么我们需要异常监控
后端服务通常都有服务器日志，异常记录等等的系统，可以帮助开发者快速定位系统的状态，追查 bug，了解异常情况，但是反观前端却没有这样的成熟的监控系统。
这样的话，一旦代码出现问题，只能通过测试或者用户反馈，这样的反馈效率就会显得有点慢。


[slide]
### 监控异常怎么做？
一个前端异常监控系统一般需要:
----
 * 前端 SDK 需要实现，主要是错误拦截，上报策略，API设计，以及日志接口。
 * 监控日志可视化管理后台的开发
 * 压缩后的单行文件如何定位源码错误

----
![](http://divio.qiniudn.com/FpwSmw5jQV8LdnVhvcbUyH6oCgER)

[slide]

### 怎么监控异常
前端 SDK 实现，实现前端主动上报数据到服务端监控日志, 使用 try catch 或者 window.onerror 进行监测，将 Javascript 的错误堆栈收集起来.
```
    // onerror
    window.onerror = function(msg, url, row, col, error) {
        // todo report
        report({
            msg: msg,
            url: url,
            row: row,
            col: col
        });
    };
```

[slide]
![](http://divio.qiniudn.com/FpQCHDStIIbTdG8LiJ3wh9rxaTlU)
----
 * 1、onerror 捕获 JavaScript 异常，对应跨域检测也有方案;
 * 2、addEventListener('error', handler, true)来捕获静态资源异常，包括js、img、css等；
 * 3、Resource Timing API 和 Performance Timing API来进行性能检测和内存检测;
 * 4、扩展XHR原型，检测返回的状态码，如404等，来检测ajax请求失败、错误;
 * 5、通过正则匹配等方式去检测DOM结构的合法性，如ID重复、文档类型未声明之类的;
 * 6、页面的死链接可以通过Nodejs的第三方模块，如request等，来检测。

[slide]
## 实现难点
上面所说的其实是常规的做法，如果想从异常中定位错误的话，那么就需要显示错误在第几行，但是一般生产环境的代码都是经过 uglify 的，压缩后的源文件无论
是什么报错都只会显示在第一行，所以对于我们的异常定位非常不利，因此需要一些特别的手段。
一般作为开发者，我们都会拥有网页脚本的源代码，那么这时候只需要将代码压缩后对应的 sourcemap 文件与压缩后的文件上传到后台，那么就可以显示出正确的
对应源文件的真实报错信息。
----
![](file:///media/jarvis/57ede06b-4ff7-4c56-bdc0-20fb50da7e49/jarvis/Workspace/ppt/publish/ppt.htm#16)

[slide]
## HTTP 劫持监控
### 运营商原罪
何为 HTTP 劫持？ 由于 HTTP 明文传输的特性，运营商通过一些精心设计的网络包的侵入，将我们原本的网页改造，嵌套在一个 iframe 标签中，
从而投放广告等等的信息。


[slide]
### 监控
<small>
虽然我们可以通过脚本让网页重定向到我们的真实 url ，但是如果再次重定向又被嵌套了怎么办？ 所以如果我们能够将这些错误的 url 收集起来, 可以一定程度上规避 HTTP 劫持。一般来说，运营商 ISP 的手法就是讲原来的页面嵌入到一个 iframe 中，我们可以监测 window.top 与 window.self 是否相同，如果不同很可能就是被 HTTP 劫持插入广告了。但是也有可能我们的页面本来就需要嵌套 iframe，所以可以通过一个白名单，如果不在这个白名单内的网页就上报。
</small>
----
![](https://user-gold-cdn.xitu.io/2016/11/29/96abbad701ff9b83e4f0674ccdd28e33)
----
HTTP 劫持一般都会通过 url 后面加上一个查询字符串形如 ```？hijack=1``` 这样的形式，所以我们可以将我们的 url 也加上这样类似的后缀来伪装已经被劫持。
通过分析劫持 url 后缀，不断加强我们的防护手段。

[slide]
## 页面监控
利用类似于 React 的 DOM diff 算法，将页面的 DOM 结构进行遍历，如果出现大范围大面积的 DOM 结构异常，就会进行报警。这里的页面监控其实属于异常监控的一种，将其分出来是为了将样式，DOM 结构的异常与脚本的异常进行区分。
### 所谓页面监控
所谓的页面监控是为了防止页面裸奔的情况出现，一般会有 CSS 样式文件出现 404 NOT Found 的情况。这样就会导致页面样式会有极大程度的改变。

[slide]
## 建立监控系统需要注意的细节

### 节流

#### 减少数据上传次数
比如设定一些条件，满足条件的才会上报，减轻服务器压力。或者设定一个队列，在一定时间内的报错合并后再提交
#### 压缩数据
一些数据为 0 的默认不上报，像性能监控那样如果 DNS 查询时间为零，那么在上传的时候将不会上报这个字段。

### 差异化
相同原因导致不同浏览器的异常可以合并起来，后台需要将这部分的信息进行合并，消除差异化，避免信息过于冗余。

[slide]
## Reference

 - [Javascript 异常文档](https://saijs.github.io/wiki/)
 - [BadJS Javascript Error](http://slides.com/loskael/badjs/fullscreen#/2)
 - [Div.io Javascript Error](http://div.io/topic/743)
 - [Javascript 错误堆栈处理](http://www.ctolib.com/topics-115283.html)
 - [HTTP 劫持](https://juejin.im/entry/57b3cd362e958a0056228410)

[slide ]

## Thanks

[slide]

## Q&A


