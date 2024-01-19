---
title: 使用hexo与fluid优美的写一个自己的blog
date: 2024-01-19
index_img: /img/hexo-head配置blog.jpg
tags: [blog,网页]
categories: 网页
---

{% note success %}
**注意：**
本文文参考 hexo 与 fluid 的官方文档
具体内容请参考以下链接:
[hexo 官方文档](https://hexo.io/zh-cn/docs/)
[fluid 官方文档](https://fluid-dev.github.io/hexo-fluid-docs/start/)
{% endnote %}

## hexo

Hexo是一个快速、简洁且高效的博客框架。Hexo 使用 Markdown（或其他渲染引擎）解析文章，在几秒内，即可利用靓丽的主题生成静态网页。即把用户的markdown文件，按照指定的主题解析成静态网页。

### 1.1安装hexo

{% note warning %}
注意提前搭建 **node.js** 环境
可使用

```npm -v
```

检测 **node.js** 环境是否配置完成
{% endnote %}

可使用 **npm** 进行安装 **hexo**

```npm install -g hexo-cli
```

### 1.2初始化blog

在自己喜欢的文件夹中运行

```hexo init <folder>      # 使用hexo初始化blog根目录，相当于新建文件夹
cd <folder>             # 进入<folder>文件夹中
npm install             # 安装npm依赖库
```

完成上述命令后，Hexo就会自动在站点根目录中生成一系列用于生成博客的文件，再输入以下命令，就可以在本地浏览博客：

```hexo g   # 生成博客，等同于 hexo generate
hexo s   # 本地预览，等同于 hexo server
```

在浏览器中访问`http://localhost:4000`就可本地访问自己的blog
此时`folder`中的文件目录为这样

```|-- _config.yml
|-- node_modules
|-- package-lock.json
|-- package.json
|-- scaffolds
|-- source
|-- themes
|-- public
|-- db.json
```

{% note info %}
若在`source`新写一篇文章，请运行`hexo clean`以清楚缓存
{% endnote %}

### 2.1 将本地博客部署到GitHubPages

最直接的方式使用git将本地博客文件夹关联到GitHub的远程仓库，并且把本地文件push到对应的仓库中。hexo提供了一种更为简洁的方式，只需要在`_config.yml`文件中进行配置并在命令行中输入相应命令就可以将本地博客发布到GitHubPages上。 首先，打开`_config.yml`文件，在deployment配置项下设置如下参数：

```deploy:
    type: git
    repo: git@github.com:你GitHub的用户名/你GitHub的用户名.github.io.git
    branch: master```

然后，安装以下插件：

```npm install hexo-deployer-git -save
```

最后运行

```hexo d     #相当于hexo deploy
```

此时，在浏览器中输入`github用户名.github .io`，就可以在互联网上看到本地的博客了

## fluid

初看**hexo**自带的主题真的不怎么样，但是**hexo**可以使用大量的第三方主题。在这里我使用的就是**fluid**为主题
