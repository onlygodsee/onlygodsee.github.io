---
title: Git Pages + Hexo 搭建个人博客记录
date: 2020-04-26 14:01:52
tags:
- 博客
- Hexo
- Git
categories:
- 技术
- Hexo
---

首先交代一下为什么使用Hexo而不是Jekyll。因为电脑环境有问题导致第一次安装Ruby失败了，然后懒得弄了。

开发环境：Mac

感谢 **[muzilan](https://blog.csdn.net/muzilanlan/article/details/81542917)** 兄台的分享



### 设置Git Pages

 创建一个新仓库，命名规则为 **{\*username\*}.github.io**

 注意这里的 **“\*username\*”** 是 Github 的用户名

 创建成功后创建 ***index.html\*** 文件，内容随意

 之后使用浏览器打开pages页面查看是否创建成功

### Hexo

 [Hexo模版地址](https://hexo.io/themes/)

 [Hexo中文文档](https://hexo.io/zh-cn/docs/)

 [Next主题文档](http://theme-next.iissnan.com/getting-started.html)

 Hexo 依赖 Node.js 环境，我这里已经安装过了便不再叙述。



#### 安装 Hexo

```shell
npm install -g hexo-cli
```

 初始化 Hexo 目录

```shell
mkdir hexofolder
hexo init hexofolder
cd hexofolder
npm install
```

#### Hexo 常用命令

```shell
hexo g # 生成模版
hexo s # 启动本地服务 http://localhost:4000
hexo d # 部署
hexo new post [postname] # 创建指定名称的文章文件,md格式
hexo new page [pagename] # 创建新的页面，eg：tags， categories
hexo d -g # 生成部署
hexo s -g # 生成预览
```

#### 安装主题

```shell
hexo clean
cd hexofolder/themes
git clone [theme地址] [themename] # 将theme模版以自定义name保存到本地
```

#### 应用主题

```shell
cd hexofolder/themes/[themename] # 进入主题目录
git pull
hexo g
hexo s
```

#### 部署至 Git Pages

 安装插件

```shell
npm install hexo-deployer-git --save
```

 修改 ***站点配置文件\*** ***_config.yml\***

```shell
deploy:
  type: git
  repo: [Git Pages 地址]
  branch: master
```

 在 Hexo 中执行部署命令

```shell
hexo d
```

至此 模版部署完成

### Hexo 添加分类、标签

#### 新建分类页面

```shell
hexo new page categories
```

此时会在 ***source/categories\*** 下生成一个 ***index.md\*** 文件,对他进行编辑

```shell
---
title: 分类
date: 2020-04-26 12:11:21
type: "categories"
comments: false
---
```

***comments\*** 为评论开关

修改 ***主题配置文件 _config.yml\*** ，将分类与标签的注释打开

```shell
menu:
  home: / || fa fa-home
  #about: /about/ || fa fa-user
  tags: /tags/ || fa fa-tags
  categories: /categories/ || fa fa-th
  archives: /archives/ || fa fa-archive
  #schedule: /schedule/ || fa fa-calendar
  #sitemap: /sitemap.xml || fa fa-sitemap
  #commonweal: /404/ || fa fa-heartbeat
```

标签的设置方式同分类

#### 文章中添加标签和分类等

新建文章

```shell
hexo new post [articlename]
```

修改文章头部信息

```shell
---
title: [标题名称]
catalog: true
date: 2018-09-29 14:23:53
subtitle: "[子标题]"
header-img: "[imgpath]"
tags:
- [tag1]
- [tag2]
categories:
- [categorie1]
- [categorie2]
---
```

### 装饰博客

#### 配置网站

打开 ***站点配置文件 _config.yml\***, 修改参数

```shell
# Site
title: 太液池 # 网站标题
subtitle: ‘’ # 网站副标题
description: ''
keywords:
author: 青桑 # 您的名字
language: zh-CN # 网站使用的语言。参考你的主题的文档自行设置，常见的有 zh-Hans和 zh-CN
timezone: ''
```

#### 设置网站的图标Favicon.ico和头像

在 **source** 下创建文件夹 **images**， 将图片文件保存在 **images** 目录下，然后修改 ***主题配置文件 _config.yml\***

```shell
favicon:
  small: /images/favicon-16x16-next.png
  medium: /images/favicon.ico # 这里修改要生效的图标
  apple_touch_icon: /images/apple-touch-icon-next.png
  safari_pinned_tab: /images/logo.svg
  #android_manifest: /images/manifest.json
  #ms_browserconfig: /images/browserconfig.xml
avatar:
  # Replace the default image and set the url here.
  url: /images/blog_head.png # 这里修改为要生效的头像
  # If true, the avatar will be dispalyed in circle.
  rounded: false
  # If true, the avatar will be rotated with the cursor.
  rotated: false
```

#### 配置站内搜索

采用 **Local Search** 方式，添加百度/谷歌/本地 自定义站点内容搜索

安装 `hexo-generator-searchdb` ，在站点的根目录下执行以下命令：

```shell
npm install hexo-generator-searchdb --save
```

在 ***站点配置文件\*** ***_config.yml\*** 末尾添加如下信息：

```shell
search:
  path: search.xml
  field: post
  format: html
  limit: 10000
```

在 ***主题配置文件\*** ***_config.yml\*** 中启用本地搜索：

```shell
# Local search
local_search:
  enable: true
```

#### 统计

**不蒜子统计**

修改 ***主题配置文件\*** ***_config.yml\*** 中 ***busuanzi_count\*** 配置

```shell
busuanzi_count:
  enable: true # true 为启用状态
  total_visitors: true # 统计访客数
  total_visitors_icon: fa fa-user
  total_views: true # 统计访问量
  total_views_icon: fa fa-eye
  post_views: true # 统计阅读数
  post_views_icon: fa fa-eye
```

### 利用Shell脚本按序号递增创建文章

创建shell脚本

```shell
vim crt_hexo_article.sh
# 文件名命名示例 article-1.md"
NEW_FILE=`ls Hexo文章目录 | sed '/sh/d' | tail -n1`
echo $NEW_FILE
var=`echo ${NEW_FILE}|awk -F '-' '{print $2}'|awk -F '.' '{print $1}'`
echo "max index now -> $var"

let var+=1
echo "create article index -> $var"
hexo new post article-${var}
# 使用 typora 打开刚刚创建的md文件
open -a typora Hexo文章路径
```

在 ***.bash_profile\*** 中添加别名

```shell
# shell 脚本别名
alias crtart="/bin/bash /Users/woo/crt_hexo_article.sh" 

# typora 打开文件别名
alias typora="open -a typora"
```

保存退出后执行

```shell
source .bash_profile
```

进入站点目录输入别名 ***crtart\***

### 文章图片

### Github 图床

在Github中新建了一个图床仓库，将图片push到仓库中，然后在文本中引用图片的地址就可以

#### 本地图片

将图片保存至站点目录下的 **source/images/[articlename]** 中，然后在文本中引用如下链接：

**http://onlygodsee.top/images/[articlename]/img.png**

#### hexo-asset-image 插件

安装 hexo-asset-image

```shell
npm install hexo-asset-image --save
```

修改 ***站点配置文件\***

```shell
post_asset_folder: true
```

之后每次new命令创建文章的时候就会生成同名的资源文件夹，部署的时候就会把资源文件同步上传到文章目录下

在发布文章时，先把我们要用到的图片放到文章目录下面的同名目录中

然后markdown中的图片链接直接填入图片名称即可

#### 图片预览

想要让博文中的图片有放大预览功能需要借助插件 **fancybox**

```shell
cd theme/next/source/lib

git clone https://github.com/theme-next/theme-next-fancybox3 fancybox
```

修改 ***主题配置文件\***

```shell
fancybox: true
```

重新部署后就可以看到效果了

### 遇到的问题

#### 部署后没有更新

解决方案：

```shell
hexo clean
hexo d -g
```

[参考地址](https://blog.csdn.net/nzjdsds/article/details/81194116)