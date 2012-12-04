---
layout: post
title: 使用git管理/etc和用户配置文件
tags: git,etc,config
---

{{ page.title }}
================

《Linux系统管理技术手册》（第二版）的第11章介绍了使用`RCS`来管理系统。记得以前总是把配置文件给搞乱，或者丢失，想想这是个挺让人兴奋的主意。这里我使用git来监视我的`/etc目录`和`用户空间的配置文件`。

	~/.gitignore # 对所有git的档案生效
	./.gitignore # 对当前repository生效，此文件可被track
	./.git/info/exclude # 对当前repository生效

这里采用第二种方法，好处多多，规则参看`man gitignore`。

/etc目录的.gitignore：

	*~
	*.swp # 交换文件
	*.log # 日志
	*.cache # 缓存
	*.bak # 备份
	*.db # 二进制数据
	*.old # 旧版本文件
	*.bin # 二进制文件
	*.dat # 二进制数据
	*.log.*
	*.cache-*
	
	thumbs/ # 缩略图
	log/ # 日志
	cache/ #缓存

~/目录的.gitignore：

	... # 和上面的相同
	/[^.]*/ # 非隐藏的文件夹
	/[^.]* # 非隐藏的文件
	# 与配置无关/占用空间大&配置无需关心
	/.cache/
	/.fontconfig/
	/.compiz-1/
	/.compiz/
	/.config/google-chrome/ # 占用空间大
	/.config/chromium/ # 同上
	/.config/libreoffice/ # 同上
	/.local/ # 都是些缓存文件，貌似
	/.thumbnails/ # 缩略图
	/.macromedia/
	/.gimp-2.6/
	/.eclipse/
	/.mozilla/
	/.thunderbird/
	/.nv/

接下来是分别在/etc和~/目录执行：

	$ git init
	$ git add .
	$ git commit -m "first commit"
