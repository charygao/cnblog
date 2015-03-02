PIGCMS 关闭聊天机器人(小黄鸡)
====

#### 无脑操作举例

1、找到 <code>WeixinAction.class.php</code> 文件，路径： <code> 你的版本\PigCms\Lib\Action\Home </code>

2、查询 <code>function chat</code> ，在 chat() 函数中修改 <code>return</code> 值

    return 'str_replace('highsea', 'hi', $str)';//举个例子

为，如下：

    return '亲，请等待客服回复~';


3、（可选）最后注释掉“小黄鸡”部分（可以不注释也可以）：

    /*小黄鸡*/
    /*$str  = 'http://api.bd001.com/iMicms_com/api.php?key=free&appid=0&msg=' . urlencode($name);
    $json = Http::fsockopenDownload($str);
    if ($json == false) {
        $json = file_get_contents($str);
    }
    $json = json_decode($json, true);
    $str  = str_replace('HighSea', $this->my, str_replace('提示：', $this->my . '提醒您:', str_replace('{br}', "\n", $json['content'])));*/

* 总结：该操作不会影响公众号其他关键字的回复，只针对回答不上来的问题（本来是请求 **小黄鸡** ）现在直接 回复：“亲，请等待客服回复~”；如需修改其他得操作源码

* 如果优化一下，可以这样：

先统一配置一个回答（或者请求 自己的接口服务器 ）,当访客回复如“小黄鸡”时再请求小黄鸡的接口…… 源码就不举例了~

#### 扩展阅读

[开发小黄鸡接口][1]
[小黄鸡官网][2]

完
----

[1]: http://www.cnblogs.com/txw1958/archive/2013/02/07/weixin-if8-simsimi.html "微信公众平台消息接口开发（8）小黄鸡(小贱鸡)机器人"
[2]: http://www.simsimi.com/language.htm '小黄鸡官网'