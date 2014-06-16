---
layout: post
keywords: android lock screen
description: blog
title: "android lock screen"
categories: [android]
tags: [android]
group: android
icon: file-alt
---

## 实现原理:自己定义一个广播接收者,继承DeviceAdminReceiver,申请系统管理员权限,其实就是激活系统的一个管理员组件的过程.

### 1, 自定义一个广播接收者,继承DeviceAdminReceiver:

package sample.reboot;

import android.app.admin.DeviceAdminReceiver;
import android.content.Context;
import android.content.Intent;
import android.util.Log;

	public class MyAdminReicever extends DeviceAdminReceiver{
		
		   @Override
			public void onEnabled(Context context, Intent intent) {
				// TODO Auto-generated method stub
				Log.e("TAG000","------onEnabled-------");
				
				super.onEnabled(context, intent);
			}

			@Override
			public void onReceive(Context context, Intent intent) {
				// TODO Auto-generated method stub
				Log.e("TAG000","--------onReceive-----");
				
				super.onReceive(context, intent);
			}

	}

	
### 2,在清单文件中注册广播

	<?xml version="1.0" encoding="utf-8"?>
	<manifest xmlns:android="http://schemas.android.com/apk/res/android"
		package="sample.reboot"
		android:sharedUserId="android.uid.system"
		android:versionCode="1"
		android:versionName="1.0" >

		<application
			android:icon="@drawable/ic_launcher"
			android:label="@string/app_name" >
			<activity
				android:name="sample.reboot.DemoActivity"
				android:label="@string/app_name" >
				<intent-filter>
					<action android:name="android.intent.action.MAIN" />

					<category android:name="android.intent.category.LAUNCHER" />
				</intent-filter>
			</activity>

			<receiver android:name="sample.reboot.MyAdminReicever" >
				<meta-data
					android:name="android.app.device_admin"
					android:resource="@xml/my_admin" />

				<intent-filter>
					<action android:name="android.app.action.DEVICE_ADMIN_ENABLED" />
				</intent-filter>
			</receiver>
		</application>

	</manifest>
	
### 3,xml/my_admin.xml
	
		<?xml version="1.0" encoding="utf-8" ?>
	<device-admin xmlns:android="http://schemas.android.com/apk/res/android">
		<!-- 
		limit-password  设置密码的规则
		watch-login 监控屏幕解锁尝试次数
		reset-password 更改屏幕解锁密码
		force-lock 设备自动锁屏
		wipe-data 删除全部的数据
		
		 -->
		<uses-policies>
			<limit-password/>
			<watch-login />
			<reset-password />
			<force-lock />
			<wipe-data />
		</uses-policies>
	</device-admin>

### 4, activity:

	package sample.reboot;

	import android.annotation.SuppressLint;
	import android.app.Activity;
	import android.app.admin.DevicePolicyManager;
	import android.content.ComponentName;
	import android.content.Context;
	import android.content.Intent;
	import android.os.Bundle;
	import android.view.View;
	import sample.reboot.R;

	public class DemoActivity extends Activity {
		@Override
		public void onCreate(Bundle savedInstanceState) {
			super.onCreate(savedInstanceState);
			setContentView(R.layout.main);
		}

		@SuppressLint("NewApi")
		public void click(View view) {
			// 设备安全管理服务 2.2之前的版本是没有对外暴露的 只能通过反射技术获取
			DevicePolicyManager devicePolicyManager = (DevicePolicyManager) getSystemService(Context.DEVICE_POLICY_SERVICE);

			// 申请权限
			ComponentName componentName = new ComponentName(this, MyAdminReicever.class);
			// 判断该组件是否有系统管理员的权限
			boolean isAdminActive = devicePolicyManager.isAdminActive(componentName);
			if (isAdminActive) {

				devicePolicyManager.lockNow(); // 锁屏

				devicePolicyManager.resetPassword("123", 0); // 设置锁屏密码
				// devicePolicyManager.wipeData(0); 恢复出厂设置 (建议大家不要在真机上测试) 模拟器不支持该操作

			} else {
				Intent intent = new Intent();
				// 指定动作名称
				intent.setAction(DevicePolicyManager.ACTION_ADD_DEVICE_ADMIN);
				// 指定给哪个组件授权
				intent.putExtra(DevicePolicyManager.EXTRA_DEVICE_ADMIN, componentName);
				startActivity(intent);
			}

		}
	}
	
<img src="/assets/images/lock_screen/1.png"></img>

[激活设备]
<img src="/assets/images/lock_screen/2.png"></img>

[响应监听]
<img src="/assets/images/lock_screen/3.png"></img>

[再次点击锁屏按钮,锁定屏幕成功,输入设定的密码解锁]
<img src="/assets/images/lock_screen/4.png"></img>
	
