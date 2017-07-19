---
layout: post
title: "Setup Octopress Blog on Github"
date: 2017-07-18 15:50:24 +0800
comments: true
categories: 
---

供参考博文：

[简书-搭建配置Octopress博客](http://www.jianshu.com/p/0ac2ac1a8e45)

[Markdown语法说明（简体中文）](http://wowubuntu.com/markdown)

本文主要参考：（Octopress官网及其GitHub内容）

[3rd Party Octopress Themes](https://github.com/imathis/octopress/wiki/3rd-Party-Octopress-Themes)

[Octopress Setup](http://octopress.org/docs/setup/)

环境：Win10，git

开始之前需要先安装git，有很多种方式，最简单的就是直接用GitHub客户端提供的GitShell。在此不多说。

## 基本流程

1. GitHub新建 repository 命名 username.github.io

2. 安装Ruby（[官网](https://rubyinstaller.org/downloads/)下载RubyInstaller和DevKit）：直接最新版就好

   其中RubyInstaller直接安装就好，勾选添加环境变量等选项。

   DevKit双击后选择解压文件夹，在该文件夹下执行两个安装命令：
   {% codeblock lang:PowerShell %}
   $ ruby dk.rb init 
   $ ruby dk.rb install{% endcodeblock %}

3. 下载Octopress并安装相关项
   
   为了防止发布之后被墙，需要在安装octopress之前修改一下Ruby gem的软件更新源。

   这里使用的是[RubyGems-Ruby China](https://gems.ruby-china.org/)提供的镜像。

   1. 替换软件源
   {% codeblock lang:PowerShell %}
   $ gem update --system # 这里请翻墙一下
   $ gem sources --add https://gems.ruby-china.org/
   $ gem sources --remove https://rubygems.org/
   $ gem sources -l #查看当前source
   # 确保只有 gems.ruby-china.org{% endcodeblock %}
   2. 下载安装octopress
   {% codeblock lang:PowerShell %}
   $ git clone git://github.com/imathis/octopress.git octopress
   $ cd octopress
   $ gem install bundler
   $ bundler install
   $ rake install{% endcodeblock %}
   
   注意在bundler install之前先把octopress下的Gemfile第一行source改为 https://gems.ruby-china.org/ 。

   这一步往后的操作都是在octopress（安装目录）下进行的。
4. 配置到GitHub

   1. `rake setup_github_pages`
   2. 按命令行要求输入GitHub repo的url：e.g. `git@github.com:username/username.github.io.git`
   3. 生成并提交博客：
      {% codeblock lang:PowerShell %}
   $ rake generate
   $ rake deploy{% endcodeblock %}
   4. 提交源文件（备份到Github）：
      {% codeblock lang:PowerShell %}
   $ git add .
   $ git commit -m 'your message'
   $ git push origin source:source{% endcodeblock %}

   第3步和第4步都是日常提交更新的操作。

   第3步生成博客然后提交修改到git上，第4步将源文件备份到git上。
5. 配置博客（修改 `_config.yml`）：
   {% codeblock lang:PowerShell %}
   url: # For rewriting urls for RSS, etc
   title: # Used in the header and title tags
   subtitle: # A description used in the header
   author: # Your name, for RSS, Copyright, Metadata
   simple_search: # Search engine for simple site search
   description: # A default meta description for your site
   date_format: # Format dates using Ruby's date strftime syntax
   subscribe_rss: # Url for your blog's feed, /atom.xml
   subscribe_email: # Url to subscribe by email
   category_feeds: # Enable per category RSS feeds
   email: # Email address for the RSS feed if you want it.{% endcodeblock %}

6. 开始写博客：

   {% codeblock lang:PowerShell %}
   $ rake new_post["New Post"]
   # Creates source/_posts/2011-07-03-new-post.markdown{% endcodeblock %}

   修改source/_posts下生成的Markdown文件。
7. 修改主题：

   {% codeblock lang:PowerShell %}
   $ cd octopress
   $ git submodule add GIT_URL .themes/THEME_NAME
   $ rake install['THEME_NAME']
   $ rake generate{% endcodeblock %}

   可以自行修改主题内各文件来改变样式和框架。
8. 本地预览博客：
   {% codeblock lang:PowerShell %}
   $ rake preview{% endcodeblock %}
   查看 http://localhost:4000 就可以在本地预览啦。