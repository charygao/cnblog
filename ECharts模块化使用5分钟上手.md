ECharts模块化使用5分钟上手
========

### 什么是EChats？

一句话： 一个数据可视化(图表)Javascript框架，[详细？移步这里][1]，类似(推荐)的有 [HighCharts][4]，其他？ 嗯，先看看吧……
快速上手： [模块化单文件引入（推荐）、标签式单文件引入][2]
github issues ： [问题列表&新建问题][3]

### 起步推荐（模块化）

举个栗子：新建文件夹 ，结构

    -- echats-demo
    -- -- js
    -- -- css
    -- -- images 
    -- -- echarts
    -- -- -- src
    -- -- zrender
    -- -- -- src
    -- -- index.html

1、**模块加载器**： 这里以 [ESL为例][5]，why？百度自己家的；或者选择 [seajs ……][6]
引入 <code>esl.js</code> 下载(或 clone) esl-master 后找到 src 目录下的 esl.js 文件copy到js目录下

2、**zrender** （echats底层依赖）：

什么是 [ZRender][8] ，一句话：[MVC封装的Canvas框架][7]
下载(或clone) zrender-master 找到 src 目录 ，将整个目录放入 zrender

3、**echarts**

下载 [echats][9] ，找到 src 目录，同样将整个目录放入 echarts

4、**配置参考**

在 <kbd>echats-demo</kbd> 文件夹下新建 demo.html 在 script 中配置如下：

    //配置例子
    require.config({
        packages: [
            {
                name: 'echarts',
                location: 'echats/src',      
                main: 'echarts'
            },
            {
                name: 'zrender',
                location: 'zrender/src', // zrender与echarts在同一级目录
                main: 'zrender'
            }
        ]
    });


5、**起步**：

源码如下：


    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>echarts 5分钟上手 demo</title>
    </head>
    <body>
        <!-- 为ECharts准备一个具备大小（宽高）的Dom -->
        <div id="main" style="height:400px"></div>
        <script src="js/esl.js"></script>
        <script type="text/javascript">
        //配置例子
        require.config({
            packages: [
                {
                    name: 'echarts',
                    location: 'echarts/src',      
                    main: 'echarts'
                },
                {
                    name: 'zrender',
                    location: 'zrender/src', // zrender与echarts在同一级目录
                    main: 'zrender'
                }
            ]
        });

        option = {

            title:{
                text:"by highsea",
                subtext:"echarts 5分钟上手 demo",
                sublink:"http://www.cnblogs.com/highsea90/",
                link:"http://highsea90.com"
            },
            backgroundColor : "#eee",
            tooltip : {
                trigger: 'axis'
            },
            legend: {
                data:['蒸发量','降水量']
            },
            //工具箱，每个图表最多仅有一个工具箱。
            toolbox: {
                show : true,
                feature : {
                    mark : {show: true},
                    dataView : {show: true, readOnly: false},
                    magicType : {show: true, type: ['line', 'bar']},
                    restore : {show: true},
                    saveAsImage : {show: true}
                }
            },
            calculable : true,
            xAxis : [
                {
                    type : 'category',
                    data : ['1月','2月','3月','4月','5月','6月','7月','8月','9月','10月','11月','12月']
                }
            ],
            yAxis : [
                {
                    type : 'value',
                    splitArea : {show : true}
                }
            ],
            series : [
                {
                    name:'蒸发量',
                    type:'bar',
                    data:[2.0, 4.9, 7.0, 23.2, 25.6, 76.7, 135.6, 162.2, 32.6, 20.0, 6.4, 3.3]
                },
                {
                    name:'降水量',
                    type:'bar',
                    data:[2.6, 5.9, 9.0, 26.4, 28.7, 70.7, 175.6, 182.2, 48.7, 18.8, 6.0, 2.3]
                }
            ]
        };

        require(
            [
                'echarts',
                'echarts/chart/line',
                'echarts/chart/bar'
            ],
            function (ec) {
                var myChart = ec.init(document.getElementById('main'));
                myChart.setOption(option);
            }
        )
        </script>
    </body>
    </html>


案例源码下载：[轻点这里][10]




完
--------
扩展阅读：[百度商业体系前端团队][11]  [百度ECharts][12]



[1]: http://highsea90.com/t/echarts-2.1.10/                         "详细介绍Echats"
[2]: http://highsea90.com/t/echarts-2.1.10/doc/start.html           "快速上手"
[3]: https://github.com/ecomfe/echarts/issues?q=is%3Aopen           "问题列表&新建问题"
[4]: http://www.highcharts.com/                                     "highcharts"
[5]: https://github.com/ecomfe/esl                                  "前端模块管理ESL"
[6]: http://seajs.org/docs/                                         "SeaJs"
[7]: https://github.com/ecomfe/zrender                              "ZRender GitHub"
[8]: http://ecomfe.github.io/zrender/                               "ZRender 官网"
[9]: http://echarts.baidu.com/build/echarts-2.1.10.zip              "echarts 2.1.10"
[10]: http://pan.baidu.com/s/1c0leOjY                               "案例源码下载"
[11]: http://ecomfe.github.io/                                      "百度商业体系前端团队"
[12]: http://echarts.baidu.com/index.html                           "百度ECharts"