---
layout: post
keywords: create svn server in native
description: �ڱ���windows7����svn������
title: "�ڱ���windows7����svn������"
categories: [Archive]
tags: [svn]
group: archive
icon: file-alt
---

##**����**
----

�еĹ�˾Ҫ��ıȽ���,������Ŀ��ʱ��,����ͨ����,����������svnҲ�޷�����!��ô,��ֻ�ý������ص�svn����.

##**׼��**
----

-����svn server��װ����

	svn������:http://subversion.apache.org/packages.html��ҳ������:Win32Svn (32-bit client, server and bindings, MSI and ZIPs; maintained by David Darj)
	svn�ͻ���:http://tortoisesvn.net/downloads.html
	
	��װ�ú�,��������,��û������,�Է���һ,���������°�!
	
-��װ

	�ҵ�svn�������ǰ�װĿ¼Ϊ:D:\Program Files\Subversion\,svn�ͻ�����㰲װ�����ﶼ��.
	
-��ʼ����

	����,��Ҫ����һ����,���ǽ���������:G:\ABC_INSURANCE\,
	��cmd����̨,
	cd /d D:\Program Files\Subversion\bin,
	����:svnadmin create G:\ABC_INSURANCE\ABC_1,�س�.����ABC_1Ϊ�������.
	����:svnserve --daemon --root G:\ABC_INSURANCE,�س�.����������,��cmd���ڹرյ�ʱ��,����Ҳ��֮�ر���.
	
	OK,������λ��,�����Ѿ�����,�����Ҽ������ļ���,TortoiseSVN -> Repo-browser,����svn://localhost/ABC_1/�Ϳ��Կ���Ŀ¼�ṹ��.�����˽�localhost����Ϊ�Լ���ip��ַ.
	
-������Ŀ

	�Ҽ������ļ���,TortoiseSVN -> Repo-browser,�Ҽ�����Դ����ļ���,Ҳ������ӱ����ļ���,����뵼�빤��,������ӱ����ļ���.
	��Ӻ���Ŀ,���ڻ�����checkout,��Ϊ��û������user��password.
	��G:\ABC_INSURANCE\ABC_1\conf,���ı��༭���༭passwd��svnserve.conf,��svnserve.conf��**password-db = passwd**��#ȥ��,ǰ�治���пո�,��passwd�������û�������,��ʽΪ:����,����#��, username = password ,ÿ���û�ռһ��.OK,������Ϊֹ,�Ϳ���������chekout��,��ȫ���, ����һ�·���svnserve --daemon --root G:\ABC_INSURANCE.
	
	
	