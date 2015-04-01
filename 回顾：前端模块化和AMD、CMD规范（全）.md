回顾：前端模块化和AMD、CMD规范（全）
====

先列举下一些著名言论： 

*“我想定义一个 each 方法遍历对象，但页头的 util.js 里已经定义了一个，我的只能叫 eachObject 了，好无奈。”*

*“RequireJS 是没有明显的 bug，SeaJS 是明显没有 bug。”*

*“在用SeaJS，除了打包非常痛苦外，其他的还好”*

*“你变了精彩的魔术，我们会为你喝彩。但你想让我们信任你，你得主动解释魔术的奥秘。否则我会觉得自己被耍了。”*

*“这两个加载器和标准没有优劣之分，只有差别。具体还是要根据实际情况进行选择；”*

……

## 前端模块化

这个是个老掉牙的话题，我就不再误人子弟了，欢迎阅读这篇由 [玉伯][1] 大大执笔的《 [前端模块化开发的价值][2] 》

#### 模块化能解决：

+ **模块的版本管理** 。通过别名等配置，配合构建工具，可以比较轻松地实现模块的版本管理。

+ **提高可维护性** 。模块化可以让每个文件的职责单一，非常有利于代码的维护。Sea.js 还提供了 nocache、debug 等插件，拥有在线调试等功能，能比较明显地提升效率。

+ **前端性能优化** 。Sea.js 通过异步加载模块，这对页面性能非常有益。Sea.js 还提供了 combo、flush 等插件，配合服务端，可以很好地对页面性能进行调优。

+ **跨环境共享模块** 。CMD 模块定义规范与 Node.js 的模块规范非常相近。通过 Sea.js 的 Node.js 版本，可以很方便实现模块的跨服务器和浏览器共享。

#### [sea.js 举例][6]

[sea.js是一个专门为解决浏览器模块化开发的方案][3]，SeaJS定义了一个全局函数 **define** 它用来定义一个模块，SeaJS提倡：文件即模块。

+ **先贴一段模块代码**

	// 所有模块都通过 define 来定义
	define(function(require, exports, module) {

		// 通过 require 引入依赖 注意 .js 可以省略
		var $ = require('jquery');
		// 你也可以引入自己的函数依赖
		var Spinning = require('./yourFunction');
		var util = {};
		util.sayHello = function(){
			return 'seajs向你问好';
		}
		// 通过 exports 对外提供接口
		// exports 和 module.exports 区别见下文：
		//exports.doSomething = function() {

		//};
		// 或者通过 module.exports 提供整个接口
		module.exports = util;

	});
	// 看完这一段 你应该明白了 require, exports, module 三个参数

+ **如何学习**
如果你顺手 NodeJS ，那么你会爱上它，如果你真爱上了它，那你可以移步这里： [5 分钟上手 Sea.js][6] 或者是这里： [一步步学会使用 Sea.js 2.0][4]，或者先看下 [2.0的更新][7] 。但是有个问题需要你慎重 **打包** ！ **SeaJS的打包确实有门槛哦~** （ 不论 [grunt][18] 还是 [spm][17] ）
打个广告：自然 SeaJS 也欢迎你的 issues，不过 提问是一门学问： [如何向开源社区提问题][8]


#### 前端模块化开发的历史

既然是 **回顾** 那就应该好文不断： 《 [前端模块化开发那点历史][5] 》


## AMD规范及其代表： [RequireJS][10] 

#### AMD规范

+ **中文版在这里** ： [AMD (中文版)][9]，或者： [鸟语原版][11]

+ **简单说**： 就是 异步模块定义（Asynchronous Module Definition），它是 **依赖前置** (因为依赖必须一开始就写好)会先 **尽早地执行(依赖)模块** , 相当于所有的require都被提前了，它的 require 分全局和局部， **一个API当多个用** 。

+ **define() 函数**

看下它的具体参数说明

	define(
	    //这个模块的名称
	    "types/Manager",
	    //依赖的模块
	    ["types/Employee"],
	    //函数执行时，所有依赖项加载。这个函数的参数依赖上述模块。
	    function (Employee) {
	        function Manager () {
	            this.reports = [];
	        }
	        //开始执行
	        Manager.prototype = new Employee();
	        //返回经理构造函数可以由其他模块的应用。
	        return Manager;
	    }
	);

#### 一些例子

创建一个名为"alpha"的模块，使用了require，exports，和名为"beta"的模块:

	define("alpha", ["require", "exports", "beta"], function (require, exports, beta) {
	   exports.verb = function() {
	       return beta.verb();
	       //或者:
	       //return require("beta").verb();
	   }
	});

一个返回对象的匿名模块：

	define(["alpha"], function (alpha) {
	   return {
	     verb: function(){
	       return alpha.verb() + 2;
	     }
	   };
	});

一个使用了简单CommonJS转换的模块定义：

	define(function (require, exports, module) {
		var a = require('a'),
		 	b = require('b');

		exports.action = function () {};
	});


## CMD规范及其代表： [SeaJS][3] 

#### CMD规范

+ **中文版在这里** ： [CMD 模块定义规范][12] ，也有 [鸟语版][13]

+ **简单说** ：  CMD（Common Module Definition）更贴近 CommonJS Modules/1.1 和 Node Modules 规范，一个模块就是一个文件；它推崇 **依赖就近** 想什么时候 require 就什么时候加载，实现了  懒加载， **延迟执行** (as lazy as possible) ；也没有全局 require， **每个API都简单纯粹** 。

+ **define() 函数**类似，看定义即可

#### module.exports 和 exports 区别

经常使用的 API 只有 define, require, require.async, exports, module.exports 这五个。其他 API 有个印象就好。

传给 factory 构造方法的 exports 参数是 module.exports 对象的一个引用。只通过 exports 参数来提供接口，有时无法满足开发者的所有需求。 比如当模块的接口是某个类的实例时，需要通过 module.exports 来实现：

	define(function(require, exports, module) {
		// exports 是 module.exports 的一个引用
		console.log(module.exports === exports); // true
		// 重新给 module.exports 赋值
		module.exports = new SomeClass();
		// exports 不再等于 module.exports
		console.log(module.exports === exports); // false
	});

对 module.exports 的赋值需要同步执行，不能放在回调函数里。下面这样是不行的：

	// x.js
	define(function(require, exports, module) {
		// 错误用法
		setTimeout(function() {
			module.exports = { a: "hello" };
		}, 0);

	});

在 y.js 里有调用到上面的 x.js:

	// y.js
	define(function(require, exports, module) {

		var x = require('./x');

		// 无法立刻得到模块 x 的属性 a
		console.log(x.a); // undefined

	});


## AMD（RequireJS）和CMD（SeaJS）异同

#### 看看执行流程

**当我们看到RequireJS** 的接口，

	require(['a','b'],function(){
		//Do something
	})

实际做的事情是：
1、require 函数检查依赖的模块，根据配置文件，获取js文件的实际路径
2、根据js文件实际路径，在dom中插入script节点，并绑定onload事件来获取该模块加载完成的通知。
3、依赖script全部加载完成后，调用回调函数


**Sea.js在调用时** ：

	define('a',function(require,exports,modules){
		var b = require('b')
	})


1、通过回调函数的Function.toString函数，使用正则表达式来捕捉内部的require字段，找到require('jquery')内部依赖的模块jquery
2、根据配置文件，找到jquery的js文件的实际路径
3、在dom中插入script标签，载入模块指定的js，绑定加载完成的事件，使得加载完成后将js文件绑定到require模块指定的id（这里就是jquery这个字符串）上
4、回调函数内部依赖的js全部加载（暂不调用）完后，调用回调函数
5、当回调函数调用require('jquery')，即执行绑定在'jquery'这个id上的js文件，即刻执行，并将返回值传给var b

**注意：**  SeaJS 这种用 ** 正则表达式 捕捉require内部依赖模块的方式** ，使得无法利用尚未执行的回调函数中的js运行环境，导致require函数的内部只能将依赖的模块名称硬编码，就不能写下面这样的代码了

	define('a',function(require,exports,modules){
		//错误
	    var b = require('Us'+'er');
	})

而只能写成

	define('a',function(require,exports,modules){
		//正确
	    var b = require('User');
	})



其他的区别其实介绍时已经说明了，上文~ 小总结：[玉伯：与 RequireJS 的异同][14]

#### 相同之处

RequireJS 和 Sea.js 都是模块加载器，倡导模块化开发理念，核心价值是让 JavaScript 的模块化开发变得简单自然。

#### 不同之处

两者的主要区别如下：

* 定位有差异。RequireJS 想成为浏览器端的模块加载器，同时也想成为 Rhino / Node 等环境的模块加载器。Sea.js 则专注于 Web 浏览器端，同时通过 Node 扩展的方式可以很方便跑在 Node 环境中。

* 遵循的规范不同。RequireJS 遵循 AMD（异步模块定义）规范，Sea.js 遵循 CMD （通用模块定义）规范。规范的不同，导致了两者 API 不同。Sea.js 更贴近 CommonJS Modules/1.1 和 Node Modules 规范。

* 推广理念有差异。RequireJS 在尝试让第三方类库修改自身来支持 RequireJS，目前只有少数社区采纳。Sea.js 不强推，采用自主封装的方式来“海纳百川”，目前已有较成熟的封装策略。

* 对开发调试的支持有差异。Sea.js 非常关注代码的开发调试，有 nocache、debug 等用于调试的插件。RequireJS 无这方面的明显支持。

* 两者代码质量有差异。RequireJS 是没有明显的 bug，SeaJS 是明显没有 bug。

* 插件机制不同。RequireJS 采取的是在源码中预留接口的形式，插件类型比较单一。Sea.js 采取的是通用事件机制，插件类型更丰富。

还有不少差异，涉及具体使用方式和源码实现，欢迎有兴趣者研究并发表看法。

总之，如果说 RequireJS 是 Prototype 类库的话，则 Sea.js 致力于成为 jQuery 类库。

#### 还不够

这里有一个图文并茂的 “栗子” [SeaJS与RequireJS最大的区别][15]，当然楼主的观点更赞成这个“栗子”： [LABjs、RequireJS、SeaJS 哪个最好用？为什么？][16]

结论： 这两个加载器和标准没有优劣之分，只有差别。具体还是要根据实际情况进行选择；


----
完

[1]: https://github.com/lifesinger 					"lifesinger(玉伯)"
[2]: https://github.com/seajs/seajs/issues/547 		"发表在《程序员》杂志 2013 年 3 月刊"
[3]: https://github.com/seajs/seajs 				"A Module Loader for the Web"
[4]: http://hi.baidu.com/liuda101/item/54bcf8d0b6a65602d68ed057 "一步步学会使用 Sea.js 2.0"
[5]: https://github.com/seajs/seajs/issues/588 		"前端模块化开发那点历史"
[6]: http://seajs.org/docs/#quick-start 			"5 分钟上手 Sea.js"
[7]: https://github.com/seajs/seajs/issues/451 		"发布 Sea.js 2.0.0"
[8]: https://github.com/seajs/seajs/issues/545 		"如何向开源社区提问题"
[9]: https://github.com/amdjs/amdjs-api/wiki/AMD-(中文版) "AMD (中文版)"
[10]: https://github.com/jrburke/requirejs			"RequireJS"
[11]: http://wiki.commonjs.org/wiki/Modules/AsynchronousDefinition "Modules/AsynchronousDefinition"
[12]: https://github.com/seajs/seajs/issues/242 	"CMD 模块定义规范"
[13]: https://github.com/cmdjs/specification/blob/master/draft/module.md "Common Module Definition / draft"
[14]: https://github.com/seajs/seajs/issues/277 	"与 RequireJS 的异同"
[15]: http://www.douban.com/note/283566440/ 		"SeaJS与RequireJS最大的区别"
[16]: http://www.zhihu.com/question/20342350 		"LABjs、RequireJS、SeaJS 哪个最好用？为什么？"
[17]: https://www.npmjs.com/package/spm 			"spm"
[18]: http://www.gruntjs.net/docs/getting-started/ 	"grunt 快速入门"