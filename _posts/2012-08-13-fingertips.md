---
layout: post
title: 指尖 - 这里的第一篇
tags: github,jekyll,markdown,disqus,simplegray
---

{{ page.title }}
================

[github]/[jekyll]博客终于建立起来，写下来留记。

0.	因为是新系统环境，丢失ssh-key，重新生成并添加到github里。参考[Generating SSH Keys](https://help.github.com/articles/generating-ssh-keys)

		$ mkdir .ssh
		$ cd .ssh
		$ ssh-keygen -t rsa -C "your_email@youremail.com"
		# 生成后，将rsa_pub的内容添加到github
		$ ssh -T git@github.com	# 测试

	git环境设置。参考[Set Up Git](https://help.github.com/articles/set-up-git)

		$ git config --global user.name "your_name"
		$ git config --global user.email "your_email@youremail.com"

1.	克隆[SimpleGray]，按里面的说明文档配置。几个地址：
	*	[Disqus]
	*	[Google CSE][Google Custom Search]
	*	[Gravatar]

2.	更改远程仓库为站点所在的仓库。

		$ git remote rm origin
		$ git remote add origin git@github.com:hueidou/hueidou.github.com.git # for ssh
		$ git remote -v # 查看

3.	编写post，名称格式`YEAR-MONTH-DAY-title.MARKUP`。
	Markdown中文参考[Markdown-Syntax-CN]，官网[Markdown]。

4.	提交。

		$ git commit -a -m "first commit"
		$ git push origin master

5.	本地测试/平时提交

		$ jekyll --server # 本地测试
		$ git add _post/yourpost.md
		$ git commit -m "some message"
		$ git push origin master
	

{% include references.md %}
