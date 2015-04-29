---
layout: post
keywords: blog
description: blog
title: "javascript preview"
categories: [Archive]
tags: [Archive]
group: archive
icon: file-alt
---

``` javascript
var box = ‘LT’;
function setBox(){
	var box = ‘LJ’;		//不能改变box的值，应为是局部变量
	// box = ‘LJ’;		//去掉var 就可以了
}
alert(box);
setBox();
```




