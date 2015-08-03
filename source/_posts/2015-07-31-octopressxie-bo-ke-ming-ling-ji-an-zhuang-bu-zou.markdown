---
layout: post
title: "Octopress写博客命令及安装步骤"
date: 2015-07-31 18:20:22 +0800
comments: true
categories: 命令
---
##  Octopress关于写博客的命令

**创建新文章**

```
$ rake new_post['title']
```

生成的文件在`~/source/_posts`目录下


**编辑文章**

```
$ rake generate     //生成博客

$ git add .
$ git commit -m "comment" 
$ git push origin source

$ rake deploy       //部署


$ rake preview #本地生成浏览 localhost:4000
```


## 安装Octopress过程

安装Octopress主要需要两个东西

* `git`

* `ruby`

git:mac上都默认安装

ruby:mac默认安装.不过按官方要求是需要1.9.3的版本

**1.安装Homebrew**

```
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

**2.安装Rbenv**

```
brew update
brew install rbenv
brew install ruby-build
```

**3.安装低版本ruby**

```
rbenv install 1.9.3-p125
rbenv local 1.9.3-p125
rbenv rehash
```

使用`ruby --version`命令查看版本号

**开始安装octopress**

```
$ git clone git://github.com/imathis/octopress.git octopress
$ cd octopress

**安装依赖**
$ gem install bundler
$ rbenv rehash
$ bundle install

**安装octopress默认主题**
$ rake install
```

**设置github和Octopress的关联**

第一步要做的是去github创建一个`username.github.io`的repo
运行如下命令,按照提示完成关联

```
$ rake setup_github_pages
```


**如果github没有添加SSH keys**

首先创建**SSH keys**,终端敲入:`ssh-keygen`,根据系统提示进行设置.

生成的**SSH keys**保存在: `~/.ssh`拷贝生成的文件内容,将其添加至github帐号管理的SSH key中,就可以克隆github上的代码库了.

