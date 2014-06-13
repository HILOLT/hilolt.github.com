---
layout: post
keywords: android onNewInten
description: onNewIntent
title: "android onNewIntent的用法"
categories: [android]
tags: [android,onNewInten]
group: archive
icon: file-alt
---

## 使用场景：当一个activity的lauchmode为singleTask的时候，我们再去启动这个activity的时候，不会再创建新的实例，直接调用这个activity的onNewIntent方法，而不是onCreate方法。

	@Override
	protected void onNewIntent(Intent intent) {
		super.onNewIntent(intent);
		
		setIntent(intent);//must store the new intent unless getIntent() will return the old one
	}
	
---

## 如果activity因为某种原因被kill掉，那么还是先调用onCreate 。