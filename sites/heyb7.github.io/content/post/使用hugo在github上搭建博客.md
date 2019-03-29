---
title: "使用hugo在github上搭建博客"
date: 2019-03-28T13:24:04+08:00
draft: false
tags: ["hugo", "blog"]
---

本文是在windows 10上面使用hugo在github上面搭建个人博客的方法。

# 什么是Hugo
Hugo是一个开源的静态网站生成器，很适合用来搭建个人博客，而且支持Markdown语法。关于Hugo的更多信息，请看[这里](https://gohugo.io/).

# 装备工作
1. 安装git  
从[git官网](https://git-scm.com/)下载对应的安装包。然后直接安装即可。
2. 安装hugo  
从[Hugo的github仓库](https://github.com/gohugoio/hugo/releases)下载最新版本的安装包（当前最新的包是hugo_extended_0.54.0_Windows-64bit.zip），记得要下载extended版本的，不然在使用主题的时候会报错。

# 安装Hugo
假如把hugo安装到D:\hugo目录下，先在D盘下新建hugo目录，然后在hugo目录下再新建2个目录，bin和sites。目录结构如下：  
```
D:\hugo
├─bin
└─sites
```
把下载的hugo压缩包直接解压到bin目录下，然后把D:\hugo\bin路径添加到系统的PATH变量中。  
验证hugo是否安装成功，在新打开的cmder中输入 hugo version,显示下面的信息说明hugo安装成功了，如果出现 hugo: command not found，请检查PATH环境变量。
```
Hugo Static Site Generator v0.54.0/extended windows/amd64 BuildDate: unknown
```

# 使用hugo建立网站数据
1. 首先建立网站的数据目录结构
```
cd D:\hugo\sites
hugo new site myblog   # myblog是网站数据的目录，名字可以任意
```
这时进入sites目录，将会看到新生成的myblog目录，目录结构如下：
```
D:\HUGO\SITES\MYBLOG
│  config.toml
│
├─archetypes
│      default.md
│
├─content
├─data
├─layouts
├─static
└─themes
```
archetypes目录里可以放一些原型，用于hugo新建内容的配置属性。config.toml是网站的配置属性文件。content目录里放你网站的内容，例如你发布的博客文章。data目录是hugo使用的配置文件存放的地方。layout目录存放布局内容。static目录存放静态资源如图片、css等。themes目录存放hugo主题。  
                                    
2. 创建博客页面
```
cd myblog
hugo new post/first_blog.md
```
上面命令会在content目录下新生成一个post目录，在post目录下新生成一个first_blog.md文件。这个文件就是一个页面文件。打开md文件，里面已经自动生成了一些内容：
```
---
title: "Firest_blog"
date: 2019-03-28T14:35:12+08:00
draft: true
---
```
---里面的是一些配置信息，默认只生成这3项，可以自己在添加一些：
```
---
title: "Firest_blog"
date: 2019-03-28T14:35:12+08:00
draft: false
tags: ["tag1", "tag2", "tag3"]
---
```
其中title是博客文章的名字，date是时间，draft是true时不发布文章，要发布文章的话需要改成false。tags是文章的标签，可以添加多个，以逗号隔开。
这时可以往新生成的md文件中加入以下markdown格式的内容。 

3. 下载hugo主题   
hugo提供了很多主题，自己可以去[Hugo主题网站](https://themes.gohugo.io/)挑选一个自己喜欢的主题。我选择的主题是[jane](https://themes.gohugo.io/hugo-theme-jane/)。然后下载主题：
```
cd myblog
git clone https://github.com/xianmin/hugo-theme-jane.git themes/jane
```
这时主题将会下载到themes的jane目录中。

# 测试新下载的主题
* 把新下载主题中的配置文件(config.toml)复制到myblog的根目录下：
```
cd myblog
cp themes/jane/exampleSite/config.toml ./
```
配置文件(config.toml)是静态网站的一些配置信息，其中的内容可以自己看一下，里面有详细的描述。如果要发布到github中，必须把配置文件中的 **baseURL**设置成**"https://用户名.github.io/"**。
启动Hugo自带的服务器来测试：
```
cd myblog
hugo server
```   

* 如果上面没有复制主题的配置文件到myblog根目录下，也可以这样来启动hugo服务：
```
hugo server -t jane   # jane是themes中的主题名字
```
其中-t是指明使用那个主题，后面跟主题的名字。这种方式可以方面我们测试不同主题的效果，省去了频繁拷贝主题的配置文件到myblog根目录下。  
* hugo服务器启动后，在浏览器中输入：http://127.0.0.1:1313就可以看到效果。

# 发布到github
1. 首先要有github的账号，然后在github上创建一个新的仓库。仓库的名字必须是  **用户名.github.io**。记得把myblog目录下配置文件中的 **baseURL**改成**"https://用户名.github.io/"**。
2. 发布前先执行如下命令使用hugo进行编译：
```
cd myblog
hugo
```
如果没有配置好myblog下的配置文件可以用下面的命令：
```
hugo --theme jane --baseURL "https://用户名.github.io"
```
hugo将编译好的文件输出到myblog的public目录中，public目录中的文件就是我们要正式发布的站点数据。  

3. 发布
```
cd public
git init
git add .
git commit -m "commit info"
git remote add origin https://github.com/用户名/用户名.github.io.git
git push origin master
```
git push的时候会要求输入github的用户名和密码。push之后就可以通过浏览器访问自己的博客站点了。
