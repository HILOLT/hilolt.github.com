---
layout: post
keywords: create svn server in native
description: 在本地windows7建立svn服务器
title: "在本地windows7建立svn服务器"
categories: [Archive]
tags: [svn]
group: archive
icon: file-alt
---

##**需求**
----

有的公司要求的比较严,在做项目的时候,不开通网络,导致外网的svn也无法访问!那么,就只好建立本地的svn库了.

##**准备**
----

下载svn server安装介质

* svn服务器:http://subversion.apache.org/packages.html网页最下面:Win32Svn (32-bit client, server and bindings, MSI and ZIPs; maintained by David Darj)
* svn客户端:http://tortoisesvn.net/downloads.html
	
	安装好后,重启电脑,我没有重启,以防万一,还是重启下吧!
	
##**安装**
----

* 我的svn服务器是安装目录为:D:\Program Files\Subversion\,svn客户端随便安装在哪里都行.
	
##**配置**
----

* 首先,需要建立一个库,我是建立在这里:G:\ABC_INSURANCE\,
* 打开cmd控制台,cd /d D:\Program Files\Subversion\bin,
	* 输入:svnadmin create G:\ABC_INSURANCE\ABC_1,回车.其中ABC_1为库的名字.
	* 输入:svnserve --daemon --root G:\ABC_INSURANCE,回车.启动服务器,当cmd窗口关闭的时候,服务也随之关闭了.
	
OK,到现在位置,环境已经搭建完毕,我们右键任意文件夹,TortoiseSVN -> Repo-browser,输入svn://localhost/ABC_1/就可以看见目录结构了.别忘了将localhost更改为自己的ip地址.
	
##**配置项目**
----

* 右键任意文件夹,TortoiseSVN -> Repo-browser,右键库可以创建文件夹,也可以添加本地文件夹,如果想导入工程,可以添加本地文件夹.添加好项目,现在还不能checkout,因为还没有配置user和password.
* 打开G:\ABC_INSURANCE\ABC_1\conf,用文本编辑器编辑passwd和svnserve.conf:
	* 将svnserve.conf中**password-db = passwd**的#去掉,前面不能有空格;
	* 在passwd中增加用户和密码,格式为:顶格,不需#号, username = password ,每个用户占一行.
	* OK,到现在为止,就可以正常的chekout了,安全起见, 重启一下服务svnserve --daemon --root G:\ABC_INSURANCE.
----
	
	
	