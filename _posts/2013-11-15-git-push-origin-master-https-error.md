---
layout: post
keywords: git HTTP request failed
description: blog
title: "git push origin master 错误:error: SSL peer certificate or SSH remote key was not OK while accessing ... fatal: HTTP request failed"
categories: [git]
tags: [git error]
group: archive
icon: file-alt
---

##**今天在添加新文章,git push origin master 时,出错啦,错误信息:**
	
	G:\hilolt.github.com>git push origin master
	error: SSL peer certificate or SSH remote key was not OK while accessing https:/
	/github.com/HILOLT/hilolt.github.com.git/info/refs?service=git-receive-pack
	fatal: HTTP request failed

我再次访问 https://github.com/HILOLT/时,会跳转到https://help.github.com/articles/why-did-i-get-redirected-to-this-page页面,
根据页面提示:
	Please remove anything matching github.com from the file located at %SystemRoot%\system32\drivers\etc\hosts.
我删除了%SystemRoot%\system32\drivers\etc\hosts中有github.com域名的条目,就可以正常提交了.

原因是之前我修改过hosts文件,因为那时候github.com还没有开放....你懂的!

-----------------------------------------

#如何修改%SystemRoot%\system32\drivers\etc\hosts文件?

	点击开始-所有程序-附件-右键记事本以管理员身份打开-文件-打开%SystemRoot%\system32\drivers\etc\hosts 进行修改,修改后即可保存.

