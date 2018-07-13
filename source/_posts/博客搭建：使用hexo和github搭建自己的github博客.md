---
title: 博客搭建：使用hexo和github搭建自己的github博客
date: 2018-07-13 09:29:55
tags: 博客搭建
---
# 使用hexo和github来搭建自己的github博客

#### 写这篇文章的原因：

1. 自己在搭建博客的时候，查看了网上很多教程，其中99%的博客基本上都是一样的，课件文档的质量着实一般。按照其办法，基本不可能完全部署成功。经过深入研究和尝试。自己初步搭建好一套，并且将尝试过程中所研究出来的原理记录下来。
2. 因为电脑使用的是公司的电脑，在搭建博客过程中所需要安装软件，还有自建hexo目录和修改对应的配置，怕日后换电脑，对相关操作遗忘，而造成不必要的麻烦，记录下来。

<!--more-->

### 0.准备条件

1.申请github账号

### 1.安装nodejs和git

这两个软件没啥可说的，自行百度或者google安装即可。需要注意的一点事nodejs安装的版本不能太低，记得当时是安装了4.x版本的nodejs导致后来出错，还排查了很长时间。因此建议安装较新的版本。

### 2.安装hexo

#### hexo是什么：

可以这么理解，hexo是一个可以帮助你完成博客搭建的一个工具。比如，你在本地写好一个md文件，hexo可以帮助你上传到github上去（具体上传到特定的github repo中，就是$username.github.io），然后渲染成好看的页面，最后通过http://$username.github.io就可以通过互联网来访问。

当然，如果不借助hexo来搭建可不可以？当然可以，不过你需要自己上传md文件，自己编写css、html文件来进行渲染，有想法的老哥可以自己尝试着做一下。不过这没什么必要，术业有专攻，专注于自己领域本身的问题就可以，既然有现成的工具，为什么不用呢。

当然除了hexo之外，还有很多其他的工具，比如jekyll。为什么选择hexo呢，因为刚开始俩都不熟悉，就学习了hexo，然后觉得hexo还不错，也没有学习jekyll了。工具嘛，有一个并且使用着方便就行。

#### 安装hexo


```
$ npm install hexo
```

等待一段时间，然后如果能得到以下结果，就可以了。


```
$ hexo version
hexo-cli: 1.1.0
os: Windows_NT 10.0.17134 win32 x64
http_parser: 2.7.0
node: 8.10.0
v8: 6.2.414.50
uv: 1.19.1
zlib: 1.2.11
ares: 1.10.1-DEV
modules: 57
nghttp2: 1.25.0
openssl: 1.0.2n
icu: 60.1
unicode: 10.0
cldr: 32.0
tz: 2017c
```

目前软件安装的工作已经完成，下面开始建立本地博客仓库和github repo。

### 3.创建github repo

在github上创建名字为$username.github.io的仓库，名字一定要严格按照这个来。然后根据提示进行一些配置，就可以了。

原理：为什么必须要严格按照这个名字来。这个是github为了方便开发者写博客，特意推出的一个功能，也就是每一个github账号只能创建一个对应这样特定名字的仓库，然后如果访问http://$username.github.io的话就会访问到该仓库里的一些文件，正如上面说的，如果不借助工具，就可以自己写html、css文件，上传到该仓库中。同样可以达到建站的目的。

### 4.创建本地hexo博客目录（重要）

这一步是你今后写的博客文件存放的目录，同样，如果相对博客进行一些定制化的改造，如使用主题等，也需要在该目录下进行配置的修改。

#### （1）创建hexo根目录。

选择一个合适的位置，创建一个目录，我是在f:的根目录下创建的hexo目录。然后进入到该目录中执行hexo init，执行完不报错，查看该目录的目录结构如下：


```
$ ll -ah
total 261K
drwxr-xr-x 1 user 197121    0 7月  13 08:48 ./
drwxr-xr-x 1 user 197121    0 7月  12 19:16 ../
drwxr-xr-x 1 user 197121    0 7月  13 09:22 .deploy_git/
-rw-r--r-- 1 user 197121   71 7月  12 19:17 .gitignore
-rw-r--r-- 1 user 197121 2.3K 7月  13 08:48 _config.yml
-rw-r--r-- 1 user 197121  174 7月  13 09:29 db.json
drwxr-xr-x 1 user 197121    0 7月  13 08:36 node_modules/
-rw-r--r-- 1 user 197121  611 7月  13 08:36 package.json
-rw-r--r-- 1 user 197121 135K 7月  13 08:36 package-lock.json
drwxr-xr-x 1 user 197121    0 7月  13 08:41 public/
drwxr-xr-x 1 user 197121    0 7月  12 19:17 scaffolds/
drwxr-xr-x 1 user 197121    0 7月  12 19:17 source/
drwxr-xr-x 1 user 197121    0 7月  12 19:19 themes/
```

介绍几个目录的作用

* _config.yml  该文件是整个站点的配置文件，其中包括你的站点title、站点所关联的github仓库等等
* source 该目录存放着你创建的所有的博客的源文件，即md文件，当然如果有相关images同样也在该目录下。
* public 该目录存放着渲染好的一些js、css、html文件，可以供本地起server来实时查看。
* themes 存放着主题目录，安装了几个主题，该目录下就存放着几个目录


进入到themes目录下查看：


```
user@HT-20171109NHBX MINGW64 /f/projects/hexo/themes
$ ls
landscape/  yilia/

user@HT-20171109NHBX MINGW64 /f/projects/hexo/themes
$ cd yilia/

user@HT-20171109NHBX MINGW64 /f/projects/hexo/themes/yilia (master)
$ ls
_config.yml  languages/  layout/  package.json  README.md  source/  source-src/  webpack.config.js

```

上述可以看出我是安装了两个主题，我目前使用的是yilia，可以看到主题目录下也有_config.yml文件。这个配置文件可以对主题进行一些定制化修改。

好了现在hexo 存放博客的目录已经完成。可以通过

```
$ hexo g
INFO  Start processing
INFO  Files loaded in 680 ms
INFO  Generated: content.json
INFO  Generated: archives/2018/index.html
INFO  Generated: archives/index.html
INFO  Generated: archives/2018/07/index.html
INFO  Generated: 2018/07/12/firstblog-md/index.html
INFO  Generated: index.html
INFO  Generated: tags/test/index.html
INFO  Generated: fonts/default-skin.b257fa.svg
INFO  Generated: fonts/iconfont.8c627f.woff
INFO  Generated: fonts/iconfont.45d7ee.svg
INFO  Generated: fonts/iconfont.16acc2.ttf
INFO  Generated: fonts/iconfont.b322fa.eot
INFO  Generated: img/default-skin.png
INFO  Generated: fonts/tooltip.4004ff.svg
INFO  Generated: img/scrollbar_arrow.png
INFO  Generated: img/preloader.gif
INFO  Generated: main.0cf68a.css
INFO  Generated: slider.e37972.js
INFO  Generated: main.0cf68a.js
INFO  Generated: 2018/07/13/博客搭建：使用hexo和github搭建自己的github博客/index.html
INFO  Generated: mobile.992cbe.js
INFO  21 files generated in 197 ms
```

通过


```
$ hexo s
INFO  Start processing
INFO  Hexo is running at http://localhost:4000/. Press Ctrl+C to stop.
```

来本地查看博客系统是什么样的。

### 5.本地hexo目录关联远程github仓库

上一步创建了本地的博客编写目录。还可以生成静态文件供本地预览等。那么写好的博客如何上传到github上。

配置相关的配置文件，打开hexo根目录下的_config.yml文件（注意，是hexo根目录的，不是主题的），在最后deploy模块处进行如下配置：


```
deploy:
  type: git
    repository: git@github.com:shuangyangqian/shuangyangqian.github.io.git
      branch: master

      ```

      仔细查看该配置文件，可以看到我们可以配置很多东西：

      * title
      * description
      * author
      * theme
      * ...

      因为可以配置的实在太多了，贴上一份自己初步配置的，供参考：


      ```
      $ cat _config.yml
      # Hexo Configuration
      ## Docs: https://hexo.io/docs/configuration.html
      ## Source: https://github.com/hexojs/hexo/

      # Site
      title: shuangyangqian
      subtitle:
      description: 业精于勤，荒于嬉。
      keywords:
      author: shuangyangqian
      language:
      timezone:

      # URL
      ## If your site is put in a subdirectory, set url as 'http://yoursite.com/child' and root as '/child/'
      url: http://yoursite.com
      root: /
      permalink: :year/:month/:day/:title/
      permalink_defaults:

      # Directory
      source_dir: source
      public_dir: public
      tag_dir: tags
      archive_dir: archives
      category_dir: categories
      code_dir: downloads/code
      i18n_dir: :lang
      skip_render:

      # Writing
      new_post_name: :title.md # File name of new posts
      default_layout: post
      titlecase: false # Transform title into titlecase
      external_link: true # Open external links in new tab
      filename_case: 0
      render_drafts: false
      post_asset_folder: false
      relative_link: false
      future: true
      highlight:
        enable: true
	  line_number: true
	    auto_detect: false
	      tab_replace:

	      # Home page setting
	      # path: Root path for your blogs index page. (default = '')
	      # per_page: Posts displayed per page. (0 = disable pagination)
	      # order_by: Posts order. (Order by date descending by default)
	      index_generator:
	        path: ''
		  per_page: 10
		    order_by: -date

		    # Category & Tag
		    default_category: uncategorized
		    category_map:
		    tag_map:

		    # Date / Time format
		    ## Hexo uses Moment.js to parse and display date
		    ## You can customize the date format as defined in
		    ## http://momentjs.com/docs/#/displaying/format/
		    date_format: YYYY-MM-DD
		    time_format: HH:mm:ss

		    # Pagination
		    ## Set per_page to 0 to disable pagination
		    per_page: 10
		    pagination_dir: page

		    # Extensions
		    ## Plugins: https://hexo.io/plugins/
		    ## Themes: https://hexo.io/themes/
		    theme: yilia

		    # Deployment
		    ## Docs: https://hexo.io/docs/deployment.html
		    deploy:
		      type: git
		        repository: git@github.com:shuangyangqian/shuangyangqian.github.io.git
			  branch: master
			  jsonContent:
			      meta: false
	                      pages: false
			      posts:
		                title: true
		                date: true
		                path: true
			        text: false
			        raw: false
			        content: false
			        slug: false
			        updated: false
			        comments: false
			        link: false
			        permalink: false
			        excerpt: false
			        categories: false
			        tags: true
```

关联github仓库的配置主要就是deploy那一部分，一定要填写自己github中名字的$username.github.io的仓库。

上述配置完成之后，执行如下发布博客。


```
# hexo g //生成静态文件
# hexo d //部署博客，主要是上传到github上

```

上述两条命令也可以合成一条

```
# hexo d -g
```

如果没出什么错误，就会提示成功commit等信息。这时候你去github仓库中发现，多了一个commit。
停大概十几分钟，访问http://$username.github.io就可以看到自己的博客了。


### 6.写博客的步骤

我们费这么大劲来搭建这一套系统，最终的目的还是要方便我们写博客，并且获得一个比较可观的成果。

本人总结出的一个步骤，可记录如下

##### #hexo new “博客名字”

执行完上述之后，会在source/_posts目录下出现一个博客名字.md的文件，我们可以在该文件内写自己想写的内容

打开本地md编辑器，写博客，我使用的是网易云笔记，md编辑器随便选择一个就行。

写完之后复制到博客名字.md中，就完成博客内容的编写了

##### #hexo g

生成本地静态文件

##### #hexo s

起本地server来预览，如果出现格式上或者内容上的错误，可以重新回去修改，如果没什么问题，就可以提交了

##### #hexo d

提交到github上。

### 7.一些小问题

##### 如何让首页的博客缩略显示而不是显示全部？

网上有很多种方法，有修改theme主题的源码的。我本人重上至简原则，使用hexo官方推荐的方法。
即在文章内部插入<!--more-->来进行分割。标记之前的部分会显示，之后的部分会隐藏。


##### 博客上传之后，去github仓库里找不到该md文件。

这个我也没整明白。是md文件直接上传，还是上传的是渲染好的静态文件。目前的理解是后者。但是，如果是后者，呢么本地的md文件丢失了，岂不是只剩下渲染后的文件了，之前的md文件已经找不到了。（该问题还需要研究）

### 8.后记

这篇文章简单记录了搭建博客的过程，一些问题交代的可能也不是很清楚，对hexo本身和主题的配置也少之又少。但是整个流程走通了，对于最基本的需求（写博客）也满足了。

后续会持续不断的对博客进行定制和优化，本篇文章也会不断的更新。


-----------------------------------------------------------------
---------------------我是一个分割线------------------------------

距离上次搭建博客已经很久了，哦， 不对，才过去不到一个小时时间。
上次遗留了一个比较重要的问题：


** 就是，hexo文件夹内是存放md源文件，还包括一些theme文件，渲染过的html、css、js等文件。hexo d部署完成的时候，只是把对应的public文件夹目录下的内容给推送到了github上$shuangyangqian.github.io上了，并且会覆盖。因此md文件的保存成为了一个问题，解决办法有两个：**


* 1.本地保存，再别的地方编辑，完成之后复制到hexo目录下，然后执行渲染，上传等操作。当然，这个别的地方可以仅仅是本地的一个目录，也可以是github的一个仓库（方便保管），what ever。
* 2.将hexo文件夹本身关联到github的一个仓库。

上述第一种方法是我刚开始用的，也不麻烦，但是存在一个问题：

* 把md文件单独存储，看似很方便，但是总感觉怪怪的。
* 一些config配置无法保存，如hexo的配置，theme的配置，一旦换电脑，如果丢失的话很难。
* 

那么针对第二种办法，记录一下我的操作：

##### 1.github上穿件一个仓库，对因关联本地的hexo目录，我创建的是shuangyangqian-hexo，原则上这个名字是无所谓的，随意起。

##### 2.在本地hexo根目录执行下述操作

```
# git init
# git config --global user.name shuangyangqian
# git config --global user.email qsyqian@gmail.com
# git remote add origin git@github.com:shuangyangqian/shuangyangqian-hexo.git
# git pull //该步骤是同步一下远程的文件
# git add .
# git commit //新建一个commit，把本地的source、theme、public等目录都上传上去
# git push --set-upstream origin master
```

这样本地的hexo目录也就是对应一个新建的github仓库，我们的md源文件和一些配置信息也不会丢失了。

##### 3.写博客的步骤

写博客的步骤和之前的大差不差，不过就是hexo本身成为一个repo，我们写完博客之后，执行hexo相关的命令deploy我们的博客。

然后还需要git相关的操作把我们的md源文件上传到shuangyangqian-hexo仓库中去。

#### 经过这次原理上的分析，基本上hexo的原理也差不多了。

hexo g：只负责渲染md文件到html、css等到public目录下；
hexo d：负责将public目录下的文件全部上传到$username.github.io的仓库中，执行的是覆盖操作。

hexo+github搭建博客基本上研究到这里，以后是一些定制化的配置。基本上不做深入研究。

#### 最重要的还是能够坚持写博客。
