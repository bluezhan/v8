
![](https://github.com/bluezhan/v8/raw/master/images/v8.png)

<p align="center">
  <img src="https://img.shields.io/badge/language-HTML--CSS--JavaScript-green.svg">
  <img src="https://img.shields.io/badge/v8-javascriptengine-ff69b4.svg">
  <img src="https://img.shields.io/badge/web-togo-orange.svg">
  <img src="https://img.shields.io/badge/license-MIT-ccc.svg">
  <img src="https://img.shields.io/badge/Don't-Panic-ff69b4.svg">
  <img src="https://img.shields.io/badge/stroller-0.1.1-ded76a.svg">
</p>

### 目录

#### 什么是V8  

V8 is Google’s open source high-performance JavaScript engine, written in C++. It is used in Google Chrome, the open source browser from Google, and in Node.js, among others. It implements ECMAScript as specified in ECMA-262, and runs on Windows 7 or later, macOS 10.12+, and Linux systems that use IA-32, ARM, or MIPS processors. V8 can run standalone, or can be embedded into any C++ application.

#### JavaScript引擎  

JavaScript引擎是一个执行JavaScript代码的程序或解释器。  

下面是实现了JavaScript引擎的一个热门项目列表：

- `V8`—开源，由Google开发，用C++编写的  
- `Rhino`—由Mozilla基金所管理，开源，完全用Java开发  
- `SpiderMonkey`—第一个JavaScript引擎，最早用在Netscape Navigator上，现在用在Firefox上。  
- `JavaScriptCore`—开源，以Nitro销售，由苹果公司为Safari开发  
- `KJS`—KDE的引擎最初由Harri Porten开发，用于KDE项目的Konqueror浏览器  
- `Chakra(JScript9)`—Internet Explorer  
- `Chakra(JavaScript)`—Microsoft Edge  
- `Nashor`— 开源为OpenJDK的一部分，由Oracle的Java语言和工具组开发  
- `JerryScript`— 是用于物联网的轻量级引擎  

#### 比较引擎的速度

![image](https://note.youdao.com/yws/api/personal/file/404E96F5BC4947A5B980E5161E539FFB?method=download&shareKey=6d3c9dbf92b59d563bbe470d6dc8048b)

[arewefastyet.com](http://arewefastyet.com)：是用来比较流行JavaScript引擎速度的，包括Firefox里面的TraceMonkey+JaegerMonkey，Google Chrome的V8和Webkit的Nitro（jsc），会展示三大JavaScript引擎最新代码版本的性能比较图表，现在看，Firefox的JS引擎已经超过Nitro，距离Chrome还有一定距离。:)

1. 首先确认一点，JavaScript一直都是解释执行；
2.  传统JavaScript引擎是在运行时将JavaScript解释编译成了字节码，V8而是编译成机器码，并引入JIT和内联缓存等特性且优化了GC，所以性能上有质的飞跃；
3.   决定一个语言是解释型还是编译型，其实是根据程序运行时还是编译时获得运行目标平台的代码来决定的，所以JavaScript一直都是解释型。
4.   传统引擎解释执行过程大概是这样的：JavaScript->AST->字节码->本地机器码(由解释器执行)；V8大概是这样：JavaScript源码 -> 抽象语法树 -> 本地机器码。但是这样的优点是可以极致的快，内存方面就飙升。所以新版的V8引擎打算改善这个问题： V8 5.9 引入Ignition 字节码解释器将默认启动 。

JavaScript度量方式从最初的微基准测试到后来的静态测试集，直到V8即将采用真实网页。

![](https://res.infoq.com/news/2017/01/V8-measure-performance-data/zh/resources/1.png)

和其他JavaScript引擎类似，V8也通过合成基准测试来度量性能指标。刚开始，引擎开发者使用诸如SunSpider、Kraken等微基准测试框架；随着浏览器市场的发展，基准测试进入了新纪元，诸如Octane、JetStream等更加大型的框架被使用，但是它们仍然属于合成基准测试引擎。

微基准和静态测试集有它们的优势：它们非常容易理解，运行方便，能够在任意浏览器中执行，容易进行对比。但它们也有很大的劣势：测试用例非常有限，难以模拟现实中众多复杂的网页；另外，基准测试需要经常变化，以满足不停进化的前端框架和前端技术；最后，基于基准测试分数的优化，对于真实用户或者前端开发者来说不一定有感知。

通过WebPageReplay和运行时调用状态来度量真实的网页性能

基于上述传统基准测试的缺陷，V8团队通过加载真实网站页面来度量真实性能。最终，他们通过基于Chrome的组件WebPageReplay来录制网页请求，并按照需求进行回放。

随后，配合WebPageReplay组件，他们又开发了称为运行时调用状态（Runtime Call Stats）的工具，以记录不同JavaScript代码在执行时实际使用到的V8组件。有了这个工具的帮助，不仅能够让使用真实网站来测试V8变得更加方便，同时能够完美展示为了V8在执行不同JavaScript代码时会表现的不同。

目前，V8团队已经使用了将近25个网站进行性能度量，来指导V8的优化。这些网站，是从Alexa前100名中，以使用JavaScript框架（React、Polymer、Angular、Ember等等）、地理位置分布以及开发团队与V8团队有合作等因素最终挑选出来。

想要深入了解网页和运行时调用状态的测试集开发详情，请收看 BlinkOn 6演讲：真实世界性能。读者也可以自己执行运行时调用状态工具。

和真实网页加载的区别

通过运行时调用状态工具，可以直观的观察到真实网站性能度量数据，并能够和传统基准测试进行比较，了解不同JavaScript执行时V8的内部情况。

从这些对比数据上，我们会发现性能基准测试工具Octane和实际25个测试网站相比差别很大。从下面的图表可以看出，Octane的颜色区域和其他网站测试结果相差很大。当运行Octane时，V8的瓶颈在于JavaScript代码执行，然而在处理真实网页时，V8的瓶颈却是解析和编译。如此大的差别最终会导致针对V8的优化效果不佳，甚至产生反效果。

![](https://res.infoq.com/news/2017/01/V8-measure-performance-data/zh/resources/2.png)

从这个图表还能发现，相比于Octane，Speedometer和现实数据更加接近。Speedometer是一个WebKit的基准测试库，其中包含使用了React、Angular、Ember等框架编写的测试用例，和实际的25个网站获得的数据比较匹配。

最终目的：更快的V8引擎

在过去一年中，基于真实网站的测试集和运行时调用状态工具已经帮助V8有了大约10%～20%的性能提升。由于之前的优化主要着关注于Chrome的页面加载优化，2位数的性能提升已经是个不错的成绩了。这些优化同样使得Speedometer性能基准测试的分数提升了20%～30%。

这些性能的提升，对于使用现代JavaScript框架（或者类似模式JavaScript代码）的网站会有明显反应。其他一些提升，如JavaScript内置功能Object.create和Function.prototype.bind的优化、围绕着对象工厂模式的优化、V8引擎的内联缓存功能实现、实时解析器优化等，对于普通JavaScript执行性能优化也有帮助。

V8团队将持续使用真实网站加载性能来指导V8引擎优化。

#### github v8

https://github.com/v8

#### 目前的情况

- [JavaScript的成本](https://juejin.im/post/5a253d746fb9a0451463e197)  
- [V8 JavaScript 引擎：高性能的 ES2015+](https://www.w3ctech.com/topic/2035) 

#### V8结构知识点

- [找出可能影响性能的代码（模式）](https://github.com/xitu/gold-miner/blob/master/TODO/tracing-patterns-hinder-performance.md)
- [前端程序员应该懂点 V8 知识](https://segmentfault.com/l/1500000008618265/play)
- [面向前端开发者的V8性能优化](https://juejin.im/post/59ba6420f265da065166e27d ) 
- [新 V8 为 NODE.JS 带来的性能变化](https://mp.weixin.qq.com/s/Kztct2x_pYBU7JN6P50uIw)
- [认识 V8 引擎](https://zhuanlan.zhihu.com/p/27628685)
- [JavaScript 是如何工作的：V8 引擎内部机制及5个诀窍编写优化代码的技巧](http://www.zcfy.cc/article/how-javascript-works-inside-the-v8-engine-5-tips-on-how-to-write-optimized-code-4033.html)

#### 编译 Google V8 引擎

翻墙、C++、GN  

- [编译和使用 Google V8 引擎](http://www.jikexueyuan.com/course/523.html)
- [编译 V8 源码](https://zhuanlan.zhihu.com/p/25120909?refer=v8core)

#### 视频

- [High Performance JS in V8 - Peter Marshall, Google](https://www.youtube.com/watch?v=YqOhBezMx1o)  
- [前端程序员应该懂点 V8 知识](https://segmentfault.com/l/1500000008618265/play)

http://www.clipconverter.cc/

#### 分析JS代码

由于NodeJS是搭建在V8之上的，所以NodeJS很多「新增语言特性」和「提高性能」等更新都需要依赖V8的发布日程。  
在NodeJS中，通过process.versions可以查看NodeJS依赖模块的版本号，V8就包含其中。

    d8 --trace-opt-verbose add-of-ints.js
    node --trace-opt-verbose add-of-ints.js
    
    d8 --trace-opt-verbose add-of-mixed.js
    node --trace-opt-verbose add-of-mixed.js
    
    d8 --trace-opt --trace-deopt add-of-mixed-dep.js
    node --trace-opt --trace-deopt add-of-mixed-dep.js

运行 d8 --help 可以查看所有的 d8 命令行参数。如果使用 node，直接运行 node --help 输出的是 node 的命令行参数，如果想查看 V8 的，需要使用 node --v8-options。执行node --harmony, 就可以进入开启了es6特性的repl。

    node --trace-opt test.js
    node --trace-bailout test.js 查找不能被优化的函数，重写
    node --trace-deopt test.js 查找不能优化的函数

后面章节会介绍 V8 的 GC（命令行参数 --trace-gc）以及最有意思的 --allow-natives-syntax。


### 参考  

#### 书籍

《两周自制脚本语言》  
《深入浅出Node.js》  
《webkit技术内幕》  
《JavaScript高级程序设计》  
《你所不知道的JavaScript（上卷）》  

#### 文章

- [V8 性能优化杀手](https://juejin.im/post/5959edfc5188250d83241399)
- [V8如何度量真实数据性能](http://www.infoq.com/cn/news/2017/01/V8-measure-performance-data)
- [在 Windows 平台编译 Google V8 引擎](http://www.jikexueyuan.com/course/523_3.html?ss=1)  
- [前端程序员应该懂点 V8 知识](https://segmentfault.com/l/1500000008618265/play)
- [面向前端开发者的V8性能优化](https://juejin.im/post/59ba6420f265da065166e27d ) 
- [前端精读周刊](https://github.com/dt-fe/weekly)
- [新 V8 为 NODE.JS 带来的性能变化](https://mp.weixin.qq.com/s/Kztct2x_pYBU7JN6P50uIw)
- [认识 V8 引擎](https://zhuanlan.zhihu.com/p/27628685)
- [JavaScript 语法解析、AST、V8、JIT](https://cheogo.github.io/learn-javascript/201709/runtime.html)
- [V8找出可能影响性能的代码（模式）](https://juejin.im/post/59f95af951882574d1723e70)
- [JavaScript 是如何工作的：V8 引擎内部机制及5个诀窍编写优化代码的技巧](https://juejin.im/entry/5a1552aef265da4310480878)
- [翻译JavaScript的成本](https://juejin.im/post/5a253d746fb9a0451463e197)
- [Node.js 十问 ](https://zhuanlan.zhihu.com/p/29650110)
- [Node.js背后的V8引擎优化技术 ](http://geek.csdn.net/news/detail/52207)
- [JavaScript 开发者所需要知道的 V8（一）：V8 In NodeJS](https://segmentfault.com/a/1190000007484357)
- [Node 性能优化](https://segmentfault.com/a/1190000007621011)
- [NodeJS的代码调试和性能调优](http://www.barretlee.com/blog/2015/10/07/debug-nodejs-in-command-line/)
- [Node.js 调试 GC 以及内存暴涨的分析](https://blog.eood.cn/node-js_gc)
- [记一次 Node.js 应用内存暴涨分析](http://taobaofed.org/blog/2016/01/14/nodejs-memory-leak-analyze/)
