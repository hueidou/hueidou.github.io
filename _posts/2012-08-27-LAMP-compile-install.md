---
layout: post
title: LAMP编译安装
tags: LAMP linux apache mysql php compile install 编译 安装
---

{{ page.title }}
================

按着LAMP的顺序安装，Linux，Apache，Mysql，Php。

Linux
-----

安装一些开发包：

	aptitude install libtool // Apache
	aptitude install libxml2-dev
	aptitude install libmcrypt-dev // PHP
	aptitude install libc6-dev gcc g++ make
	...

PCRE
----

Perl Compatible Regular Expressions
地址：<http://www.pcre.org/>

	cd /tmp
	tar xf /path/to/pcre-8.31.tar.bz2
	cd pcre-8.31
	'./configure' \
	'--prefix=/usr/local/pcre'
	make
	make install

Apache2
-------

地址：<http://www.apache.org/>

	cd /tmp
	tar xf /path/to/httpd-2.4.3.tar.gz
	tar xf /path/to/httpd-2.4.3-deps.tar.gz // apr,apr-util
	cd httpd-2.4.3
	'./configure' \
	'--prefix=/usr/local/apache2' \
	'--with-pcre=/usr/local/pcre' \
	'--enable-module=so'
	make
	make install

Mysql
-----

地址：<http://www.mysql.com/>

	待写。

PHP
---

地址：<http://www.php.net/>

	cd /tmp
	tar xf /path/to/php-5.4.6.tar.bz2
	cd php-5.4.6
	'./configure' \
	'--prefix=/usr/local/php' \
	'--with-apxs2=/usr/local/apache2/bin/apxs' \
	'--with-mysql' \
	'--with-mysqli' \
	'--with-mysql-sock' \
	'--with-mcrypt' \
	'--enable-mbstring' \
	'--enable-mysqlnd' \
	'--enable-sockets'
	make
	make install
	cp php.ini-development /usr/local/php/lib/php.ini

/usr/local/apache2/conf/httpd.conf自动添加的行：

	LoadModule php5_module        modules/libphp5.so

/usr/local/apache2/conf/mime.types手动添加的行：

	application/x-httpd-php                         php phtml
	application/x-httpd-php-source                  phps

QA
--

phpMyAdmin:

Q：#2002 无法登录 MySQL 服务器  
A：`如果未指定指定主机名或指定了特殊的主机名localhost，将使用Unix套接字。`  

   >修改libraries/config.default.php，将  
   >$cfg['Servers'][$i]['host']='localhost';  
   >改为  
   >$cfg['Servers'][$i]['host']='127.0.0.1';  

   或者，  

   >查看mysql配置，my.cnf  
   >[client]  
   >socket = /var/run/mysqld/mysqld.sock  
   >修改/usr/local/php/lib/php.ini：  
   >mysql.default_socket = /var/run/mysqld/mysqld.sock  

   或者，最合适的方法，重新编译PHP让其支持sockets。
