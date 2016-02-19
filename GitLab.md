GitLab 之 Linux十分钟快装.md

先把 Shell 命令贴出来，
楼主以 CentOS release 6.5 (Final) 64位 为例：

	//配置系统防火墙,把HTTP和SSH端口开放.
	sudo yum install curl openssh-server postfix cronie
	sudo service postfix start
	sudo chkconfig postfix on
	sudo lokkit -s http -s ssh

	//下载rpm安装包
	sudo curl -O https://downloads-packages.s3.amazonaws.com/centos-6.6/gitlab-ce-7.10.0~omnibus.2-1.x86_64.rpm
	sudo rpm -ivh gitlab-ce-7.10.0~omnibus.2-1.x86_64.rpm 

	//这一步也可以用管道的方式安装：
	//sudo curl http://packages.gitlab.cc/install/gitlab-ce/script.rpm.sh | sudo bash
	//sudo yum install gitlab-ce

	sudo rpm -ivh gitlab-ce-7.10.0~omnibus.2-1.x86_64.rpm 
	//修改 自带的nginx配置，以及邮件提醒配置
	vim /var/opt/gitlab/nginx/conf/gitlab-http.conf
	vim /etc/gitlab/gitlab.rb
	
	//保存配置
	sudo gitlab-ctl reconfigure
	//启动运行，以及查看状态
	sudo gitlab-ctl start //stop
	sudo gitlab-ctl status

怎样，10分钟应该搞定了吧，接下来楼主开启废话模式，你可以选择 继续看 或者 继续看……

GIT，SVN，GitHub，[GitLab][15] 的区别这里就不展开了，有兴趣的同学可以浏览下面的文章：

* [SVN（subversion）简介][4]
* [GIT简介][5]
* [GIT与SVN的五处大区别][1]
* [GitHub 的替代产品有哪些][2]
* [GitHub GitLab 区别][3]

###### 楼主曰（读 yue）：

SVN 是一个完美的 **集中式的版本控制系统** ；
GIT 是 **分布式** 更快捷安全；
GitHub 是给用户 **提供GIT服务的网站** ，他将程序员的 协同，沟通 等工作提供了解决方案（代码社交）；
GitLab 是一个GIT的 **项目管理工具** （私有化），也有Github的类似功能


## 安装方式

#### BitNami 一键式

[BitNami][6] 有一键安装包的解决方案 不论你是 Linux，Windows，Mac OSX 系统 [点此下载适合你的安装包][7]

###### 题外话：据说 [GitLab ControliPhone版][8] 也很好玩。

#### 编译安装 高逼格

编译安装流程比较繁琐，还要下载各种依赖包，甚至还有的是被墙的；当然他的优势是随意搭配服务环境，随意选择数据库，随意更改各种配置……
如果你熟悉 [ROR环境][9] 当然推荐用这个，楼主也是搞web开发的，但是主要以 （NodeJs｜PHP） ＋（Linux｜MacOSX｜Windows）＋（MongoDB｜Mysql）＋（Nginx｜Apache） 环境为主，所以就放弃了这种方式安装，如果你依然想要这个，那推荐看看这个方案：[一键Shell指令安装][10]

#### rpm包安装 省时省力

如开头，楼主采用的是这种方式，这种方式虽然简便，但是重复安装了很多依赖，比如，Nginx，邮件收发系统之类的，楼主使用的服务器上还安装着 Redmine，RAP，Tomcat，JavaMail，Node……

这里再强调下 我用rpm包地址：

	curl -O https://downloads-packages.s3.amazonaws.com/centos-6.6/gitlab-ce-7.10.0~omnibus.2-1.x86_64.rpm

如果下载不下来可以尝试：[清华大学镜像][11] 或者 [翻墙试一试][12]

## 修改配置

#### Nginx

这个 rpm自带了 Nginx ，如果你找不到位置，你可以搜索下名称

	find / -name gitlab-http.conf
	sudo vim /var/opt/gitlab/nginx/conf/gitlab-http.conf

server_name 很重要哦，设置监听端口之前请先查看端口有无占用 <code>netstat -anpt | grep 8181</code> 然后再改

	server {
	  listen *:8181; ##这里注意
	  server_name gitlab.mycloudedu.net; ##这里注意
	  server_tokens off; ## Don't show the nginx version number, a security best practice
	  root /opt/gitlab/embedded/service/gitlab-rails/public;

	  ## Increase this if you want to upload large attachments
	  ## Or if you want to accept large git objects over http
	  client_max_body_size 250m;
	……


#### Email

这里提一下 <code>unicorn.rb</code> 文件，该文件会影响 gitlab-ctl 指令，如果你改动了则需要重新运行配置，指令：

	sudo gitlab-ctl reconfigure

你可以通过 <code>cat /var/opt/gitlab/gitlab-rails/etc/unicorn.rb</code>指令查看该文件，
接下来是修改邮件收发的配置：

	vim /etc/gitlab/gitlab.rb

smtp设置 很重要哦

	###################################
	# GitLab CI email server settings #
	###################################
	## see https://gitlab.com/gitlab-org/omnibus-gitlab/tree/629def0a7a26e7c2326566f0758d4a27857b52a3/doc/settings/smtp.md#smtp-settings

	##以下注意
	gitlab_ci['smtp_enable'] = true
	gitlab_ci['smtp_address'] = "smtp.exmail.qq.com"
	gitlab_ci['smtp_port'] = 465
	gitlab_ci['smtp_user_name'] = "admin@xx.com"
	gitlab_ci['smtp_password'] = "xxx"
	gitlab_ci['smtp_domain'] = "qq.com"
	gitlab_ci['smtp_authentication'] = "login"
	gitlab_ci['smtp_enable_starttls_auto'] = true
	# gitlab_ci['smtp_tls'] = false
	# gitlab_ci['smtp_openssl_verify_mode'] = false


还要改一下 <code>external_url</code> 对外显示的URL

	## Url on which GitLab will be reachable.
	## For more details on configuring external_url see:
	## https://gitlab.com/gitlab-org/omnibus-gitlab/blob/629def0a7a26e7c2326566f0758d4a27857b52a3/README.md#configuring-the-external-url-for-gitlab
	external_url 'http://gitlab.mycloudedu.net:8181'



改完记得运行 <code>sudo gitlab-ctl reconfigure</code>

#### Hosts

由于楼主没有 解析公司域名权限，如果你也碰巧如此的话 改下Hosts

	121.43.226.85 gitlab.mycloudedu.net


## 管理 GitLab 常用指令

这点，我要吐槽下，本来Linux很方便的有 <code>man</code> 指令来查看某个工具的指令，结果输入 <code>man gitlab-ctl</code> 后，提示竟然找不到说明文件 0.0

	//启动
	sudo gitlab-ctl start
	//查看运行状态
	sudo gitlab-ctl status
	//停止
	sudo gitlab-ctl stop
	//查看错误信息
	sudo gitlab-ctl tail
	//保存配置
	sudo gitlab-ctl reconfigure


最后，如果是编译安装的默认管理员账号密码是：<code>admin@local.host｜5iveL!fe</code>，如果是 rpm包安装则管理员账号密码是<code>root｜5iveL!fe</code> 登录后会提醒你重设密码；
还有[端口号][13] 之类需要与其他软件统一修改Nginx配置，就日后，再设置吧， [点此访问][14]。 
最后，记得关闭注册哦

![关闭用户自主注册][16]


完
----

[1]: http://boxysystems.com/index.php/5-fundamental-differences-between-git-svn/ "GIT与SVN的五处大区别"
[2]: http://www.zhihu.com/question/19573222 "Github 的替代产品有哪些"
[3]: https://www.upwork.com/hiring/development/gitlab-vs-github-how-are-they-different/ "GitHub GitLab 区别"
[4]: http://blog.csdn.net/mh942408056/article/details/7629036 "SVN（subversion）简介"
[5]: http://blog.csdn.net/wuyao721/article/details/8524190 "git简介"
[6]: https://bitnami.com/stacks "BitNami"
[7]: https://bitnami.com/stack/gitlab "BitNami安装gitlab"
[8]: https://itunes.apple.com/cn/app/gitlab-control/id654747119 "GitLab ControliPhone版"
[9]: http://rubyonrails.org/ "Ruby on Rails"
[10]: https://github.com/mattias-ohlsson/gitlab-installer "一键Shell指令安装"
[11]: http://mirror.tuna.tsinghua.edu.cn/help/gitlab-ce/ "Gitlab Community Edition 镜像使用"
[12]: https://github.com/highsea/Hosts "修改Host即可"
[13]: http://121.43.226.85:8181/ "端口号日后再去"
[14]: http://gitlab.mycloudedu.net:8181/ "统一配置"
[15]: https://about.gitlab.com/gitlab-ce-features/ "GitLab简介"
[16]: http://images2015.cnblogs.com/blog/531703/201602/531703-20160215210444689-681285519.png "关闭用户自主注册"

