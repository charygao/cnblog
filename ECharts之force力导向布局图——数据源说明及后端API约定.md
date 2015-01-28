ECharts之force力导向布局图——数据源说明及后端API约定
========

### Echarts ?
关于 Echarts [请移步这里][1]

### force 力导向图

实现方式，如：

	function require_EC () {
		require(
		    [
		        'echarts',
		        //载入force模块
		        'echarts/chart/kforce'
		    ],
		    function (ec) {
		    	//确定需要绘制的DOM
		        setChats(ec);
		    }
		)
	}

	function setChats (ec) {
		var myChart = ec.init(document.getElementById('main'));
		myChart.setOption(option);
	}


### 数据源说明

主要三个数据源： **categories** （数据分类）、 **nodes** （图表中的节点名称）、 **links** （图表中节点之间的链接线），具体如下图：

[![force数据说明][2]][2]

### API开发

力导向图数据 API文档 (个人意见仅供参考)

<table>
	<tr>
		<td>实现功能</td>
		<td>1、categories、nodes、links、数组按需加载，减轻服务器压力；<br>2、对展示孤岛链接的优化<br>3、cache data 应用</td>
	</tr>
	<tr>
		<td>接口传递的主要参数以及主要值</td>
		<td>name、force、categories、nodes、links、cache、refresh</td>
	</tr>
	<tr>
		<td>返回码</td>
		<td>见 返回码附件图</td>
	</tr>
</table>

参数说明：

[![API参数说明][8]][8]

请求示例：（详情看图片）

<!-- table>tr*5>td{text $}*3 -->
<table>
	<tr>
		<td>示例 链接</td>
		<td>返回值</td>
		<td>含义</td>
	</tr>
	<tr>
		<td>force-api.php?name=demo1&force=nodes</td>
		<td> 
			<!-- [图片详情][3]  -->
			<a href="http://images.cnitblog.com/blog/531703/201501/201053062353068.gif">图片详情</a>
		</td>
		<td>获取了 名称为 ”demo1“的力导向图表的 nodes（节点） 数据</td>
	</tr>
	<tr>
		<td>force-api.php?name=demo1&force=categories</td>
		<td> 
			<a href="http://images.cnitblog.com/blog/531703/201501/201053282505627.gif">图片详情</a>
		</td>
		<td>获取了 名称为 ”demo1“的力导向图表的 categories（分类） 数据，【以此类推 links 不做举例】</td>
	</tr>
	<tr>
		<td>force-api.php?name=demo1_isolated_all&force=links</td>
		<td> 
			<a href="http://images.cnitblog.com/blog/531703/201501/201053490319942.gif">图片详情</a>
		</td>
		<td>【如何获取 孤岛链接？】将 孤岛链接组成的图表 当成一张新的图表 即可：如图， 获取了 demo1的所有孤岛链接（demo1_isolated_all）的 links 数组</td>
	</tr>
	<tr>
		<td>force-api.php?name=demo1_isolated_all&force=nodes&cache=refresh</td>
		<td> 
			<a href="http://images.cnitblog.com/blog/531703/201501/201054072196800.gif">图片详情</a>
		</td>
		<td>获取了 demo1的所有孤岛链接（demo1_isolated_all）的 node 数组 并做了 强制刷新</td>
	</tr>
</table>

注：第一次请求服务器，如果请求正确返回码将是

	code: "2200",message: "nodes success",

第二次请求服务器 将会返回 

	code: "3304",message: "cache:2015-01-19 15:14:43",

除非加上参数 <code>cache=refresh</code> 缓存时间3天

### 返回码约定

[![返回码约定][7]][7]


### 牛刀小试

[![牛刀小试 force 力导向图表（描述复杂网络节点）][10]][10]

如需查看此次 demo 源码，[移步GitHub][11]

完
--------
附上此次 API 接口源码php [（php随便写了下，轻拍……）：下载][9]



[1]: http://www.cnblogs.com/highsea90/p/4228656.html 	"ECharts模块化使用5分钟上手"
[2]: http://images.cnitblog.com/blog/531703/201501/201052496105956.gif "force数据说明"
[3]: http://images.cnitblog.com/blog/531703/201501/201053062353068.gif "示例请求1"
[4]: http://images.cnitblog.com/blog/531703/201501/201053282505627.gif "示例请求2"
[5]: http://images.cnitblog.com/blog/531703/201501/201053490319942.gif "示例请求3"
[6]: http://images.cnitblog.com/blog/531703/201501/201054072196800.gif "示例请求4"
[7]: http://images.cnitblog.com/blog/531703/201501/201054242507268.gif "返回码约定"
[8]: http://images.cnitblog.com/blog/531703/201501/201049011724101.gif "API参数说明"
[9]: http://pan.baidu.com/s/1dDdBuLV 					"API 接口示例 php"
[10]: http://images.cnitblog.com/blog/531703/201501/271722234258301.gif "牛刀小试 force 力导向图表（描述复杂网络节点）"
[11]: https://github.com/highsea/force-echarts "力导向示意图（描述复杂网络节点）"