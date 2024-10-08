---
title: GithubPages生成静态博客
date: 2024-10-08
excerpt: 使用GithubPages写Blog
---

记录一下我使用Github Pages写博客的经历，最开始是使用 [jekyll](https://jekyllrb.com/) 来生成，前两篇就是用它生成的，但我重新安装了一次系统，环境需要重新安装，第一次安装的时候就很麻烦，很慢，第二次安装好`ruby`后运行项目发现有些莫名其妙的错误，搞了一段时间果断放弃，之后又寻找到了[Hexo](https://github.com/hexojs/hexo)，也就是我现在使用的，虽然用`npm`来安装也不是特别快，但还可以，内容主要参考[这篇文章](https://blog.csdn.net/yaorongke/article/details/119089190)，

![jekyll](/images/jekyll.png)

## 安装

```bash
# 全局安装hexo脚手架，要使用npm，需要安装Node.js
npm install hexo-cli -g
# 查看安装的版本
hexo -v

# 使用hexo初始化一个项目，blog为自定义
hexo init blog
# 进入该项目
cd blog
# 安装所需依赖
npm install
# 创建一个要发布的内容，名为Hello Hexo
hexo new "Hello Hexo"

# 生成静态文件
hexo g / hexo generate

# 本地启动
hexo server
```

在 http://localhost:4000 可以访问到blog

## 主题

[Hexo 社区](https://hexo.io/themes/)有很多主题供选择使用, 以[Fluid](https://github.com/fluid-dev/hexo-theme-fluid)为例进行更换主题，不想换的话用默认的主题其实也可以

下载 [Fluid](https://github.com/fluid-dev/hexo-theme-fluid/releases) 主题到`themes`目录，修改`_config.yml`文件使用`Fluid`主题

```
theme: <themes文件夹下那个文件夹名字>
```

## 写作

在`/source/_posts`下存放写作的`Markdown`源文件，在`/source/images`下存放写作用到的图片，在`/source/adout`下存放`index.md`文件，里边的内容就是主页点击关于所展示的内容

```bash
# 普通要发布的文章头部添加如下内容
# 指定文章标题、节选内容、日期、布局
---
title: HelloWorld
excerpt: HelloWorld
date: 2024-04-18
layout: post
---

# about下的index.md
---
title: about
date: 2024-04-08
layout: about
---

# 当然还可以指定标签Tags， 类别之类的在官方文档上都有介绍
```



## 部署到Github

首先你要在你的`Github`有一个`你的用户名.github.io`仓库，这个也就是将来访问博客的链接，然后将`Hexo`生成的`public`文件夹下所有内容推送到这个仓库，也可以将`source`文件夹复制到`public`目录下，这样也能保存你的源文件，不过每次都需要手动更新下



## 总结

以上只是简单的配置，如果有需求可以按照官网的文档自己去定制，一般博客的功能基本也都有，让我们开始愉快的写作吧，记录下自己的点滴。
