---
layout: post
keywords: hilolt,android ,ahibernate
description: android使用ahibernate管理数据库
title: "android使用ahibernate管理数据库"
categories: [android, ahibernate]
tags: [android, ahibernate]
icon: file-alt
---
<h3>感谢李坤[博客](http://blog.csdn.net/lk_blog)为我们提供的这么好的框架。
之前的项目一直使用Ahibernate做数据库管理，挺好用的，有的时候一张表会有几十个字段，我们不能一个一个去写，
现在只要添加上注解就OK了，节省了不少力气啊。</h3>


### Header 3

<h3>介绍</h3>


<h3>(一)支持功能: </h3>
<pre>
 1. 自动建表,支持属性来自继承类:可根据注解自动完成建表,并且对于继承类中的注解字段也支持自动建表.   
 2. 自动支持增删改,增改支持对象化操作:增删改是数据库操作的最基本单元,不用重复写这些增删改的代码,并且添加和更新支持类似于hibernate中的对象化操作.  
 3. 查询方式灵活:支持android框架提供的方式,也支持原生sql方式.  
 4. 查询结果对象化:对于查询结果可自动包装为实体对象,类似于hibernate框架.  
 5. 查询结果灵活:查询结果支持对象化,也支持结果为List<Map<String,String>>形式,这个方法在实际项目中很实用,且效率更好些.  
 6. 日志较详细:因为android开发不支持热部署调试,运行报错时可根据日志来定位错误,这样可以减少运行Android的次数.   
</pre>
<h3>(二)不足之处: </h3>

<pre>
1. id暂时只支持int类型,不支持uuid,在sqlite中不建议用uuid.
2. 现在每个方法都自己开启和关闭事务,暂时还不支持在一个事务中做多个操作然后统一提交事务. 
</pre>
<h3>(三)作者寄语:</h3>

<pre>
* 昔日有JavaScript借Java发展,今日也希望AHibernate借Hibernate之名发展.  
* 希望这个项目以后会成为开源社区的重要一员,更希望这个项目能给所有Android开发者带便利.  
* 欢迎访问我的博客:http://blog.csdn.net/lk_blog,这里有这个框架的使用范例和源码,希望朋友们多多交流完善这个框架,共同推动中国开源事业的发展,AHibernate期待与您共创美好未来!!!  
</pre>

<h3>使用</h3>


<p>1.建立一个android工程,导入源码.</p>
<p>2.建立modle(or bean,or domain, or vo ...)，添加注解,shift+alt+s ，generate grtters and setters：</p>
	@Table(name="T_GIRL")：建表，T_GIRL为表名。如果需要主键,可以添加@Id注解。
{% highlight java %}
@Table(name="T_GIRL")
public class Girl {
	@Column(name="girl_id")
	private int girl_id ;
	
	@Column(name="name")
	private String name;
	
	@Column(name="age")
	private int age;
	public int getGirl_id() {
		return girl_id;
	}
	public void setGirl_id(int girl_id) {
		this.girl_id = girl_id;
	}
	public String getName() {
		return name;
	}
	public void setName(String name) {
		this.name = name;
	}
	public int getAge() {
		return age;
	}
	public void setAge(int age) {
		this.age = age;
	}

}
{% endhighlight %}
<p>3.建立自己的helper类，我这里指定了数据库的位置，这样便于以后扩展，注意在manifest.xml里面加上相应的读写权限。</p>
{% highlight java %}
public class GirlDBHelper extends MyDBHelper{
	private static final String DBPATH=Environment.getExternalStorageDirectory().getAbsolutePath() + "/test/db/";
	private static final String DBNAME="girl.db";
	private static final int DB_VERSION=1;

	public GirlDBHelper(Context context) {
		super(context, DBPATH+DBNAME, null, DB_VERSION, new Class[]{Girl.class});
		Log.e("GirlDBHelper数据库建立了", "数据库建立了"+DBPATH+DBNAME);
		
	}
		static{
			File file = new File(DBPATH);
			if(!file.exists()){
				file.mkdirs();
			}
		}
}
{% endhighlight %}
<p>4.操作类，我这里没有写GirlDao接口类，就不用实现接口了。</p>
{% highlight java %}
public class GirlDaoImpl extends BaseDaoImpl<Girl>{

	public GirlDaoImpl(Context context) {
		super(new GirlDBHelper(context));
	}
	
}
{% endhighlight %}
-------------------------End---------------
再次感谢作者[李坤博客]。(http://blog.csdn.net/lk_blog)



