---
title: 使用hexo与fluid搭建自己的blog
excerpt: 使用hexo进行创建blog，并使用fluid进行美化
date: 2024-01-19
index_img: /img/hexo-head配置blog.jpg
tags: [blog,网页]
categories: [网页]
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


```powershell
npm -v
```


检测 **node.js** 环境是否配置完成

{% endnote %}


可使用 **npm** 进行安装 **hexo**


```powershell
npm install -g hexo-cli
```


### 1.2初始化blog


在自己喜欢的文件夹中运行


```powershell
hexo init <folder>      # 使用hexo初始化blog根目录，相当于新建文件夹
cd <folder>             # 进入<folder>文件夹中
npm install             # 安装npm依赖库
```


完成上述命令后，Hexo就会自动在站点根目录中生成一系列用于生成博客的文件，再输入以下命令，就可以在本地浏览博客：


```powershell
hexo g   # 生成博客，等同于 hexo generate
hexo s   # 本地预览，等同于 hexo server
```


在浏览器中访问`http://localhost:4000`就可本地访问自己的blog

此时`folder`中的文件目录为这样


```fold
|-- _config.yml
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
若在`source`新写一篇文章，请运行`hexo clean`以清除缓存
{% endnote %}

### 2.1 将本地博客部署到GitHubPages

最直接的方式使用git将本地博客文件夹关联到GitHub的远程仓库，并且把本地文件push到对应的仓库中。hexo提供了一种更为简洁的方式，只需要在`_config.yml`文件中进行配置并在命令行中输入相应命令就可以将本地博客发布到GitHubPages上。 首先，打开`_config.yml`文件，在deployment配置项下设置如下参数：

```yml
deploy:
    type: git
    repo: git@github.com:你GitHub的用户名/你GitHub的用户名.github.io.git
    branch: master
```

然后，安装以下插件：

```powershell
npm install hexo-deployer-git -save
```

最后运行

```powershell
hexo d     #相当于hexo deploy
```

此时，在浏览器中输入`github用户名.github .io`，就可以在互联网上看到本地的博客了

## fluid

初看**hexo**自带的主题真的不怎么样，但是**hexo**可以使用大量的第三方主题。在这里我使用的就是**fluid**为主题

### 3.1安装fliud

官方提供了两种安装fluid的方式

#### 使用**npm**
在控制台中输入
```powershell
npm install --save hexo-theme-fluid
```
#### 直接下载
直接下载[最新的release版本](https://github.com/fluid-dev/hexo-theme-fluid/releases)，解压到themes目录，并将解压出来的文件夹改名为`fluid `

### 3.2指定主题
如下修改 Hexo 博客目录中的 `_config.yml`
```yml
theme: fluid     # 指定主题
language: zh-CN  # 指定语言，会影响主题显示的语言，按需修改
```
### 3.3创建关于页
首次使用主题的「关于页」需要手动创建：

```powershell
hexo new page about
```
创建成功后修改 `/source/about/index.md`，添加 `layout` 属性。

修改后的文件示例如下：

```markdown
---
title: 标题
layout: about
---

这里写关于页的正文，支持 Markdown, HTML
```
{% note warning %}
{% label warning @**WARNING** %}

layout: about 必须存在，并且不能修改成其他值，否则不会显示头像等样式。
{% endnote %}

### 3.4更新主题

#### 方法一

{% note %}
适用于npm安装的用户
{% endnote %}
在博客目录下执行命令：

```powershell
npm update --save hexo-theme-fluid
```

#### 方法二

{% note %}
适用于通过 Release 压缩包安装主题，且没有自行修改任何代码的情况。
{% endnote %}

1. 先将原文件夹重命名为别的名称，例如 fluid-bkp，用于升级失败进行回退；
2. 按照安装步骤，重新下载 [release](https://github.com/fluid-dev/hexo-theme-fluid/releases)并解压重命名为 `fluid`；
3. 如果某些配置发生了变化（改名或弃用），release 的说明里会特别提示，同步修改原配置文件即可.

#### 方法三

{% note %}
适用于自定义了一些代码，或想体验其他分支的情况，以 dev 分支为例。
{% endnote %}

1. 确定自己的 fluid 目录已经开启 git，并且所有改动都已 commit；
2. 把 fluid 仓库的 develop 分支拉取到自己当前的分支上（也可新建一个分支再拉取）：
```powershell
git pull https://github.com/fluid-dev/hexo-theme-fluid.git develop
```
3. 解决代码冲突，保留自己修改的部分（如何解决冲突可自行搜索）。


