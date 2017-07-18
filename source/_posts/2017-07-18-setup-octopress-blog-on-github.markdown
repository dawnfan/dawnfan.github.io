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

## 基本流程##

1. GitHub新建 repository 命名 username.github.io

2. 安装Ruby（官网下载Ruby-Installer for Windows）：直接最新版就好

3. 下载Octopress并安装相关项
   {% codeblock lang:PowerShell %} 
   git clone git://github.com/imathis/octopress.git octopress
   cd octopress
   gem install bundler
   bundler install
   rake install  
   {% endcodeblock %}

4. 配置到GitHub

   1. `rake setup_github_pages`
   2. 输入GitHub repo：e.g. `git@github.com:username/username.github.io.git`
   3. 生成并提交博客：
      `rake generate`
      `rake deploy`
   4. 提交源文件（备份到Github）：
      `git add .`
      `git commit -m 'your message'`
      `git push origin source:source`

5. 配置博客（`config.yml`）：
   {% codeblock lang:PowerShell %} 
   url: # For rewriting urls for RSS, etc
   title: # Used in the header and title tags
   subtitle: # A description used in the header
   author: # Your name, for RSS, Copyright, Metadata
   simple_search: # Search engine for simple site search
   description: # A default meta description for your site
   date_format: # Format dates using Ruby's date strftime syntax
   subscribe_rss: # Url for your blog's feed, defauts to /atom.xml
   subscribe_email: # Url to subscribe by email (service required)
   category_feeds: # Enable per category RSS feeds (defaults to false in 2.1)
   email: # Email address for the RSS feed if you want it.
   {% endcodeblock %}

6. 开始写博客：

   {% codeblock lang:PowerShell %}
   rake new_post["Zombie Ninjas Attack: A survivor's retrospective"]
   # Creates source/_posts/2011-07-03-zombie-ninjas-attack-a-survivors-retrospective.markdown
   {% endcodeblock %}

7. 修改主题：

   {% codeblock lang:PowerShell %}
   $ cd octopress
   $ git submodule add GIT_URL .themes/THEME_NAME
   $ rake install['THEME_NAME']
   $ rake generate
   {% endcodeblock %}


   可以自行修改主题内各文件来改变样式和框架。