---
title: Linux_Basics_10
date: 2020-10-02 19:43:34
---
Welcome to [Hexo](https://hexo.io/)! This is your very first post. Check [documentation](https://hexo.io/docs/) for more info. If you get any problems when using Hexo, you can find the answer in [troubleshooting](https://hexo.io/docs/troubleshooting.html) or you can ask me on [GitHub](https://github.com/hexojs/hexo/issues).

	曾经沧海难为水，除却巫山不是云。
	取次花丛懒回顾，半缘修道半缘君。

[Linux命令手册][Linux]

[Linux]: https://man.linuxde.net/

[阿里云][aliyun]

[aliyun]: http://www.aliyun.com

[菜鸟教程][Runoob]

[Runoob]: http://www.runoob.com/

[百度][Baidu]

[Baidu]: http://www.baidu.com

[Google][Google]

[Google]: http://google.com.hk

# Yum项目上线实战 （网站运维）
	一、编译安装与卸载Nginx
	Nginx：是一款比较流行的web服务器软件，类似于Apache。

![](/hexo-private-blog-website/images/项目.png)

	1、安装nginx
	①下载nginx
	下载地址：https://nginx.org/en/download.html
	使用在服务器端下载的方式进行下载（此处不使用filezilla）：
	#wget 地址
	例如当前需要下载nginx到“/usr/local/src”
	#wget https://nginx.org/download/nginx-1.13.11.tar.gz

![](/hexo-private-blog-website/images/项目1.png)

	②解压nginx安装包
	# tar -zxvf nginx-1.13.11.tar.gz

![](/hexo-private-blog-website/images/项目1.png)

	③进入nginx解压目录
	开始进行配置、编译、安装操作

	在配置时候报错：没有PCRE库

![](/hexo-private-blog-website/images/项目2.png)

	直接yum安装pcre-devel：
	#yum install pcre-devel

![](/hexo-private-blog-website/images/项目2.png)

	报错缺少zlib库：

![](/hexo-private-blog-website/images/项目2.png)

![](/hexo-private-blog-website/images/项目3.png)

	直接使用yum安装zlib库：
	#yum install zlib-devel

![](/hexo-private-blog-website/images/项目3.png)

![](/hexo-private-blog-website/images/项目4.png)

	还需要自己去下载一个zlib的源码包，然后解压出来：

![](/hexo-private-blog-website/images/项目4.png)

![](/hexo-private-blog-website/images/项目5.png)

	最终的nginx配置命令：
	#./configure --prefix=/usr/local/nginx --with-pcre --with-zlib=/usr/local/src/zlib-1.2.11

	开始安装：
	#make

![](/hexo-private-blog-website/images/项目5.png)

![](/hexo-private-blog-website/images/项目6.png)

	最后安装：
	#make install

![](/hexo-private-blog-website/images/项目6.png)

	安装好的目录：

![](/hexo-private-blog-website/images/项目6.png)

	④运行nginx
	先停止Apache，然后再运行nginx

![](/hexo-private-blog-website/images/项目6.png)

	#/usr/local/nginx/sbin/nginx			【启动命令】
	#/usr/local/nginx/sbin/nginx -s reload	【重载，重载配置文件】

	启动效果：

![](/hexo-private-blog-website/images/项目6.png)

![](/hexo-private-blog-website/images/项目7.png)

	⑤了解：卸载编译安装的软件
	#rm -rf 软件的安装目录
	注意：卸载一个编译安装的软件的时候必须先停止。

![](/hexo-private-blog-website/images/项目7.png)

![](/hexo-private-blog-website/images/项目8.png)

	二、关于LAMP
	LAMP：Linux + Apache + MySQL + PHP		LAMP架构（组合）
	LNMP：Linux + Nginx + MySQL + php-fpm  LNMP架构（组合）
	LNMPA：Linux + Nginx + MySQL + PHP + Apache		Nginx代理方式

![](/hexo-private-blog-website/images/项目8.png)

![](/hexo-private-blog-website/images/项目9.png)

	三、LAMP环境部署
	首先登录控制台获取需要连接的主机ip地址：

![](/hexo-private-blog-website/images/项目9.png)

![](/hexo-private-blog-website/images/项目10.png)

	后续可以进行远程登录。

![](/hexo-private-blog-website/images/项目10.png)

	在整个LAMP中需要自己安装的也就只有Apache + PHP + Mysql。后续以yum为例。
	1、PHP与Apache的安装
	#yum install php		【在安装好php的同时会一起顺带安装Apache】

![](/hexo-private-blog-website/images/项目10.png)

![](/hexo-private-blog-website/images/项目11.png)

	启动Apache：#service httpd start

![](/hexo-private-blog-website/images/项目11.png)

	此处会有一个警告，无法确定主机的FQDN，如果需要处理，则需要修改Apache的配置文件（/etc/httpd/conf/httpd.conf）
	# vim /etc/httpd/conf/httpd.conf
	在文件中搜索“ServerName”

![](/hexo-private-blog-website/images/项目11.png)

	将前面的“#”去除，保存退出，重启apache

![](/hexo-private-blog-website/images/项目11.png)

	测试访问，在地址栏中输入ip地址直接访问（关闭防火墙）：

![](/hexo-private-blog-website/images/项目12.png)

	测试php是否可以运行（默认的Apache站点目录：/var/www/html）：
	创建一个index.php文件

![](/hexo-private-blog-website/images/项目12.png)

	运行php看到页面：

![](/hexo-private-blog-website/images/项目12.png)

![](/hexo-private-blog-website/images/项目13.png)

	2、MySQL的安装与初始化
	#yum install mysql-server

![](/hexo-private-blog-website/images/项目13.png)

![](/hexo-private-blog-website/images/项目14.png)

	初始化操作：
	#service mysqld start		【启动】

![](/hexo-private-blog-website/images/项目14.png)

	# mysql_secure_installation

![](/hexo-private-blog-website/images/项目14.png)

	测试进行命令行登录：
	#mysql -uroot -p

	如果需要远程登录则需要修改登录主机：

![](/hexo-private-blog-website/images/项目14.png)

![](/hexo-private-blog-website/images/项目15.png)

	重启MYSQL或者刷新权限：
	Mysql> flush privileges

![](/hexo-private-blog-website/images/项目15.png)

	阿里云上的安全组端口放行：

![](/hexo-private-blog-website/images/项目15.png)

![](/hexo-private-blog-website/images/项目16.png)

	使用navicat进行登录：

![](/hexo-private-blog-website/images/项目16.png)

	3、项目上线
	解压项目包，将upload其中的内容上传到服务器站点目录（/var/www/html）

![](/hexo-private-blog-website/images/项目17.png)

	①使用filezilla上传需要的代码文件

![](/hexo-private-blog-website/images/项目17.png)

	②传完之后打开网站的首页，会运行DZ的安装向导
	a. 选择同意协议

![](/hexo-private-blog-website/images/项目17.png)

![](/hexo-private-blog-website/images/项目18.png)

	b. 赋予指定目录写权限

![](/hexo-private-blog-website/images/项目18.png)

	# chmod 777 -R /var/www/html

![](/hexo-private-blog-website/images/项目19.png)

	#yum install php-mysqli

![](/hexo-private-blog-website/images/项目19.png)

![](/hexo-private-blog-website/images/项目20.png)

	重启Apache：

![](/hexo-private-blog-website/images/项目20.png)

	重启之后保证所有的配置项都是绿色的勾才可以下一步。

![](/hexo-private-blog-website/images/项目20.png)

	c. 选择DZ的安装方式

![](/hexo-private-blog-website/images/项目20.png)

![](/hexo-private-blog-website/images/项目21.png)

	d. 填写数据库与管理员的信息

![](/hexo-private-blog-website/images/项目21.png)

![](/hexo-private-blog-website/images/项目22.png)

	e. 安装完成

![](/hexo-private-blog-website/images/项目22.png)

![](/hexo-private-blog-website/images/项目23.png)

	f. 首页

![](/hexo-private-blog-website/images/项目22.png)

![](/hexo-private-blog-website/images/项目23.png)


## 省钱利器：
![](/hexo-private-blog-website/images/淘宝客24.jpg)
![](/hexo-private-blog-website/images/淘宝客25.jpg)
![](/hexo-private-blog-website/images/淘宝客26.jpg)

### 内存分布：
![](/hexo-private-blog-website/images/内存分布.bmp)
![](/hexo-private-blog-website/images/内存分布.png)
### 指针数组:
![](/hexo-private-blog-website/images/整型指针数组.bmp)
![](/hexo-private-blog-website/images/字符指针数组.bmp)
### 二维数组:
![](/hexo-private-blog-website/images/二维数组.png)
![](/hexo-private-blog-website/images/二维数组1.png)
### 支付宝打赏:
![](/hexo-private-blog-website/images/alipay.jpg)
### 微信打赏:
![](/hexo-private-blog-website/images/wechat.jpg)
### 财付通打赏：
![qq.jpg](http://ww1.sinaimg.cn/large/006DnxC4gy1gfggwd0rvjj32ai2lxdrm.jpg)
