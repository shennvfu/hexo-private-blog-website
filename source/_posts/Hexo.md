---
title: Hexo
date: 2020-09-13 16:32:18
---
Welcome to [Hexo](https://hexo.io/)! This is your very first post. Check [documentation](https://hexo.io/docs/) for more info. If you get any problems when using Hexo, you can find the answer in [troubleshooting](https://hexo.io/docs/troubleshooting.html) or you can ask me on [GitHub](https://github.com/hexojs/hexo/issues).

	井底点灯深烛伊，共郎长行莫围棋。
	玲珑骰子安红豆，入骨相思知不知。

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

	本文档教程在线访问地址：https://lipengzhou.com/posts/b050421a/
	有任何疑问可以在文章的留言区提问，感谢。

![](/hexo-private-blog-website/images/hexo.png)

	Hexo 是一个静态网站生成器，GitHub Pages 可以免费帮我们托管静态网站，本文主要介绍如何结合两
	者搭建一个类似于本站点的博客网站。
# 1. Hexo 介绍
	1.1. 是什么
	Hexo 是一个基于 Node.js 开发的静态网页生成器，官网的说明是博客框架，但是我认为它不仅仅可以
	用于博客，还可以用于企业宣传站、产品展示、文本文档等以信息展示为主的网站。
	1.2. 特点
	超快速度

![](/hexo-private-blog-website/images/hexo.png)

	Node.js 所带来的超快生成速度，让上百个页面在几秒内瞬间完成渲染。
	支持 Markdown
	Hexo 支持 GitHub Flavored Markdown 的所有功能，甚至可以整合 Octopress 的大多数插
	件。
	一键部署
	只需一条指令即可部署到 GitHub Pages, Heroku 或其他平台。
	丰富的主题
	在众多美观、强大、可定制的主题中选择；使用任何兼容的模板引擎创建自己的主题
	丰富的插件支持
	强大的 API 带来无限的可能，与数种模板引擎（EJS，Pug，Nunjucks）和工具（Babel，
	PostCSS，Less/Sass）轻易集成

![](/hexo-private-blog-website/images/hexo.png)

## 2. 起步
	2.1. 安装 Hexo
	Hexo 依赖了 Node.js 和 Git，所以在安装 Hexo 之前必须确保安装了这两个工具。
	2.1.1. 安装 Node.js
	步骤：
	1. 下载
	2. 安装
	3. 确认是否安装成功

![](/hexo-private-blog-website/images/hexo1.png)

	下面是具体操作。
	1、下载：https://nodejs.org/en/download/
	2、安装

![](/hexo-private-blog-website/images/hexo2.png)

	点击 Next 下一步

![](/hexo-private-blog-website/images/hexo3.png)

	勾选同意协议，点击 Next 进入下一步

![](/hexo-private-blog-website/images/hexo3.png)
![](/hexo-private-blog-website/images/hexo4.png)

	点击 Next 进入下一步

![](/hexo-private-blog-website/images/hexo4.png)
![](/hexo-private-blog-website/images/hexo5.png)

	点击 Next 进入下一步

![](/hexo-private-blog-website/images/hexo5.png)
![](/hexo-private-blog-website/images/hexo6.png)

	点击 Next 进入下一步

![](/hexo-private-blog-website/images/hexo6.png)
![](/hexo-private-blog-website/images/hexo7.png)

	点击 Install 开始安装

![](/hexo-private-blog-website/images/hexo7.png)
![](/hexo-private-blog-website/images/hexo8.png)

	正在安装

![](/hexo-private-blog-website/images/hexo8.png)
![](/hexo-private-blog-website/images/hexo9.png)


	安装完成，点击 Finish 退出

![](/hexo-private-blog-website/images/hexo9.png)

	3、确认是否安装成功，在终端中输入 node --version ，如果能看到类似于下面的输出则证明安装成
	功了
	$ node --version v
	12.14.0

![](/hexo-private-blog-website/images/hexo9.png)

	2.1.2. 安装 Git
	1、下载：https://git-scm.com/downloads
	
![](/hexo-private-blog-website/images/hexo10.png)

	点击 Next 进入下一步

![](/hexo-private-blog-website/images/hexo11.png)
![](/hexo-private-blog-website/images/hexo12.png)

	点击 Next 进入下一步

![](/hexo-private-blog-website/images/hexo12.png)
![](/hexo-private-blog-website/images/hexo13.png)

	点击 Next 进入下一步

![](/hexo-private-blog-website/images/hexo13.png)
![](/hexo-private-blog-website/images/hexo14.png)

	点击 Next 进入下一步

![](/hexo-private-blog-website/images/hexo14.png)
![](/hexo-private-blog-website/images/hexo15.png)

	点击 Next 进入下一步

![](/hexo-private-blog-website/images/hexo15.png)
![](/hexo-private-blog-website/images/hexo16.png)

	点击 Next 进入下一步

![](/hexo-private-blog-website/images/hexo16.png)
![](/hexo-private-blog-website/images/hexo17.png)

	点击 Next 进入下一步

![](/hexo-private-blog-website/images/hexo17.png)
![](/hexo-private-blog-website/images/hexo18.png)

	点击 Next 进入下一步

![](/hexo-private-blog-website/images/hexo18.png)
![](/hexo-private-blog-website/images/hexo19.png)

	点击 Next 进入下一步

![](/hexo-private-blog-website/images/hexo19.png)
![](/hexo-private-blog-website/images/hexo20.png)

	点击 Install 开始安装

![](/hexo-private-blog-website/images/hexo20.png)
![](/hexo-private-blog-website/images/hexo21.png)

	正在安装...

![](/hexo-private-blog-website/images/hexo21.png)
![](/hexo-private-blog-website/images/hexo22.png)

	安装完成，点击 Finish 结束

![](/hexo-private-blog-website/images/hexo22.png)
![](/hexo-private-blog-website/images/hexo23.png)

	3、最后，在终端中输入 git --version ，如果能看到输出 Git 版本号，则证明安装成功了。
	$ git --version 
	git version 2.24.1.windows.2

![](/hexo-private-blog-website/images/hexo23.png)

	2.1.3. 安装 Hexo 
	1、在命令中执行下面的安装命令
	npm install -g hexo-cli

![](/hexo-private-blog-website/images/hexo24.png)

	2、确认是否安装成功
	hexo --version

	如何能看到类似于下面的输出则证明安装成功。
	$ hexo --version 
	hexo: 4.2.0 
	hexo-cli: 3.1.0 
	os: Windows_NT 10.0.18362 win32 x64 
	node: 12.13.0
	v8: 7.7.299.13-node.12 
	uv: 1.32.0 
	zlib: 1.2.11 
	brotli: 1.0.7 
	ares: 1.15.0 
	modules: 72 
	nghttp2: 1.39.2 
	napi: 5 
	llhttp: 1.1.4 
	http_parser: 2.8.0 
	openssl: 1.1.1d 
	cldr: 35.1 
	icu: 64.2 
	tz: 2019a 
	unicode: 12.1

![](/hexo-private-blog-website/images/hexo24.png)
![](/hexo-private-blog-website/images/hexo25.png)

	2.2. 使用 Hexo 生成本地网站
	在命令行中执行下面的命令用来创建本地网站。

![](/hexo-private-blog-website/images/hexo25.png)

	hexo init 网站目录名称 

	# 例如 
	hexo init my-blog

	创建成功的话会生成一个具有下面目录结构的网站项目。

	.
	 ├── _config.yml 
	 ├── package.json 
	 ├── scaffolds 
	 ├── source 
	 | ├── _drafts 
	 | └── _posts 
	 └── themes

	 2.3. 预览本地网站

	 在你的网站根目录下执行 npm run server ，它会启动一个 http 服务，用于本地预览。

	 $ npm run server 
	 > my-blog@0.0.0 server C:\Users\LPZ\Documents\Projects\my-blog 
	 > hexo server 

	 INFO Start processing 
	 INFO Hexo is running at http://localhost:4000 . Press Ctrl+C to stop.

	 访问控制台中提示的地址，如果能看到下面的网页则证明服务启动成功了。

![](/hexo-private-blog-website/images/hexo25.png)

	 2.4. 目录结构
	 .
	 ├── _config.yml    网站配置文件 
	 ├── package.json   包说明文件 
	 ├── scaffolds      文章模板目录 
	 ├── source         源码目录 
	 | ├── _drafts      草稿文章目录 
	 | └── _posts       发布文章目录 
	 └── themes         主题目录

![](/hexo-private-blog-website/images/hexo26.png)

	 2.5. 写文章
	 1、创建文章
	 hexo new 文章文件名称

	 2、写文章
	文章配置
		语法：YAML
		可配置项：https://hexo.io/docs/front-matter
	下面是一个示例：
	--- 
	title: Sublime Text 打开方式 
	categories: 
	  - 开发笔记 
	tags: 
	  - Sublime Text 
	date: 2019-12-09 00:00:00 
	---

![](/hexo-private-blog-website/images/hexo27.png)

	文章正文
	语法：Markdown
	编辑工具：推荐 Typora
	下面是一个示例：

![](/hexo-private-blog-website/images/hexo28.png)
![](/hexo-private-blog-website/images/hexo29.png)


	3、预览文章
	2.6. 发布
	自己搭建服务器
	使用第三方云服务
	使用第三方网站托管服务

![](/hexo-private-blog-website/images/hexo29.png)
![](/hexo-private-blog-website/images/hexo30.png)

	3. GitHub Pages
	Github Pages 是 Github 提供的一个免费的静态网页托管服务，可以用来托管博客、项目官网、文本文
	档等静态网页。
	3.1. 注册 GitHub 账号
	3.2. 创建仓库
	3.3. 提交文件
	3.4. 将仓库托管到 GitHub Pages
	3.5. 关于域名
	3.5.1. GitHub 提供的默认域名

![](/hexo-private-blog-website/images/hexo29.png)

	用户名.github.io
	https://lipengzhou.github.io/
	用户名.github.io/仓库名称
	https://lipengzhou.github.io/gh-pages-demo/ 
	https://lipengzhou.github.io/a/ 
	https://lipengzhou.github.io/b/ 
	3.5.2. 自定义域名
	买一个域名
	在你的域名后台配置 CNAME 到 GitHub
	在 GitHub 仓库中添加 CNAME 文件，其中要写入你的自定义域名网址

	4. 将本地网站部署到 GitHub Pages
	正常的方式是：
	编译构建
	将构建结果推送 GitHub 仓库
	开启 GitHub Pages
	每次更新都需要重复上面的流程，太过繁琐，不推荐。
	4.1. GitHub：创建仓库
	4.2. GitHub：生成 GitHub 访问 token

![](/hexo-private-blog-website/images/hexo30.png)
![](/hexo-private-blog-website/images/hexo31.png)

	点击右上角用户头像，选择 Settings 设置

![](/hexo-private-blog-website/images/hexo31.png)

	选择 Developer settings

![](/hexo-private-blog-website/images/hexo32.png)
![](/hexo-private-blog-website/images/hexo33.png)

	在 Personal access tokens 中点击 Generate new token

![](/hexo-private-blog-website/images/hexo33.png)
![](/hexo-private-blog-website/images/hexo34.png)

	Note：一个名字，英文即可，例如我这里是 my-blog
	勾选 repo 选项

![](/hexo-private-blog-website/images/hexo34.png)
![](/hexo-private-blog-website/images/hexo35.png)

	点击 Generate token 生成 token

![](/hexo-private-blog-website/images/hexo35.png)
![](/hexo-private-blog-website/images/hexo36.png)

	绿色背景中的就是生成的 token，复制保存起来后面需要使用（之后不能查看）。
	如果忘记了，就按照上面的方式重新生成。

![](/hexo-private-blog-website/images/hexo36.png)

	4.3. GitHub：将 token 配置到项目的 secrets 中

![](/hexo-private-blog-website/images/hexo36.png)
![](/hexo-private-blog-website/images/hexo37.png)

	在项目中仓库中找到并依次进入：Settings -> Secrets

![](/hexo-private-blog-website/images/hexo37.png)
![](/hexo-private-blog-website/images/hexo38.png)

	Name：起一个名字，例如我这里是 ACCESS_TOKEN ，如果你使用了别的名字，后面的脚本中也
	要对应修改
	Value：就是你上面得到的那个 token

![](/hexo-private-blog-website/images/hexo38.png)

	!!!确认你 Add secret 之后能看到添加的这个。

![](/hexo-private-blog-website/images/hexo38.png)
![](/hexo-private-blog-website/images/hexo39.png)

	4.4. 本地：配置网站根目录
	果你部署的域名是根路径就不需要做任何修改
	如果你部署的域名是子目录，就把 root 修改为那个子目录名称

![](/hexo-private-blog-website/images/hexo39.png)

	4.5. 本地：配置 GitHub Actions
	在项目中创建 .github/workflows/main.yml 并写入下面的配置内容。
	name: build and deploy 
	# 当 master 分支 push 代码的时候触发 workflow 
	on:
		push: 
			branches:
			- master

	jobs: 
	  build-deploy: 
	    runs-on: ubuntu-latest 
	    steps:
	    # 下载仓库代码
	    - uses: actions/checkout@v2

	    # 缓存依赖
	    - name: Cache dependencies
	      uses: actions/cache@v1
	      with:
	        path: ~/.npm
	        key: ${{ runner.os }}-node-${{ hashFiles('**/package-lock.json') }}
	        restore-keys: |
	          ${{ runner.os }}-node-

	    # 安装依赖
	    - run: npm ci

	    # 打包构建
	    - run: npm run build

	    # 发布到 GitHub Pages
	    - name: Deploy
	      uses: peaceiris/actions-gh-pages@v2
	      env:
	        PERSONAL_TOKEN: ${{ secrets.ACCESS_TOKEN }}
	        PUBLISH_BRANCH: gh-pages
	        PUBLISH_DIR: ./public

![](/hexo-private-blog-website/images/hexo39.png)
![](/hexo-private-blog-website/images/hexo40.png)

	   4.6. 本地：推送到远程仓库
	   git init 
	   git add 
	   git commit -m "" 

	   git push

	   提交之后要做的事情：
			查看代码是否提交到线上
			查看 GitHub Actions 是否已经开始工作
			查看 GitHub Actions 是否工作成果
			查看部署之后的网站

	   4.7. 之后怎么更新？

	   上面的一系列流程配置好以后，之后只需安安静静的写你的博客（也就是markdown）就好了，如果需
	   要发布更新，只需要使用 Git 提交推送项目的源码就可以了，它会自动触发 GitHub Actions 自动构建
	   部署。

	   5. 实践分享
	   不定期更新

![](/hexo-private-blog-website/images/hexo41.png)

	   5.1. 文章图片
	   本地
	     source/images
	     文章资源文件夹
	   在线图床（推荐）
	     新浪微博图床
	     七牛云
	     阿里云OSS
	     。。。
	   5.2. 草稿文章
	   5.2.1. 默认方式
	   新建草稿文章
	   hexo new draft <title>
	   预览草稿：
	   hexo server --draft
	   将草稿正式发布为文章：
	   hexo publish [layout] <filename>
	   若日后想将正式文章转为为草稿，只需手动将文章从 source/_posts 目录移动到
	    source/_drafts 目录即可。

![](/hexo-private-blog-website/images/hexo42.png)

	    5.2.2. hexo-hide-posts 插件
		hexo-hide-posts 可以帮我们更好的处理文章的草稿和发布状态。
		使用它的第一步就是把它安装到网站项目中。

		npm i hexo-hide-posts

		然后在不希望发布的文章中配置 hidden: true 即可。

		--- 
			hidden: true
		---

![](/hexo-private-blog-website/images/hexo43.png)

		5.3. 网站主题
		6. 更多内容
		Hexo 官网：https://hexo.io/
			文档教程：https://hexo.io/zh-cn/docs/
			API 说明：https://hexo.io/api/
			插件市场：https://hexo.io/plugins/
			主题市场：https://hexo.io/themes/
		Hexo 中文教学
			B站上的Hexo中文教学, 紧随官方文档
		awesome hexo

![](/hexo-private-blog-website/images/hexo44.png)





### 省钱利器：
![](/hexo-private-blog-website/images/淘宝客.jpg)
![](/hexo-private-blog-website/images/淘宝客1.jpg)
![](/hexo-private-blog-website/images/淘宝客2.jpg)

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
![wechat.jpg](http://ww1.sinaimg.cn/large/006DnxC4gy1gfggvv58jjj30u010sgnq.jpg)
### 财付通打赏：
![qq.jpg](http://ww1.sinaimg.cn/large/006DnxC4gy1gfggwd0rvjj32ai2lxdrm.jpg)











