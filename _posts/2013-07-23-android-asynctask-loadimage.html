---
layout: post
keywords: android,asynctask,异步加载图片
description: android异步下载网络图片
title: "android异步下载网络图片"
categories: [android]
tags: [android, asyntask]
icon: file-alt
---
{% include hilolt/setup %}
##**AsyncTask：**
------------------------------------------------
	继承AsyncTask后，要实现三个重要的方法：
	{% highlight java %}
	 /**
	 * 在异步任务执行执行调用的方法，此处不能设置耗时的操作，否则会ANR异常。
	 */
	@Override
	protected void onPreExecute() {
		super.onPreExecute();
	}

	/**
	 * 在异步任务执行完毕后调用的方法，此处不能设置耗时的操作，否则会ANR异常。
	 */
	@Override
	protected void onPostExecute(Bitmap result) {
		super.onPostExecute(result);
		
	}
	/**
	 * 后台异步线程，执行耗时的操作，比如检查网络，下载文件，上传文件
	 */
	@Override
	protected Bitmap doInBackground(String... params) {

	}
	{% endhighlight %}


### LoadImageAsyncTask.java
{% highlight java %}

public class LoadImageAsyncTask extends AsyncTask<String, Void, Bitmap> {
	private LoadImageCallBack mLoadImageCallBack;
	
	public LoadImageAsyncTask(LoadImageCallBack mLoadImageCallBack) {
		this.mLoadImageCallBack = mLoadImageCallBack;
	}


	@Override
	protected void onPreExecute() {
		super.onPreExecute();
		//在执行doInBackground()之前，我们可以给imageview设置一个默认的图片，类似于weibo和网易客户端
		mLoadImageCallBack.beforeImageLoad();
	}

	
	@Override
	protected void onPostExecute(Bitmap result) {
		super.onPostExecute(result);
		//给imageview设置下载好的图片，result为下载好的图片
		mLoadImageCallBack.afterImageLoad(result);
		
	}

	@Override
	protected Bitmap doInBackground(String... params) {
		try {
			String iconpath = params[0];
			URL url = new URL(iconpath);
			HttpURLConnection conn = (HttpURLConnection) url.openConnection();
			conn.setRequestMethod("GET");
			conn.setConnectTimeout(5000);

			InputStream is = conn.getInputStream();
			Bitmap bitmap = BitmapFactory.decodeStream(is);
			return bitmap;
		} catch (Exception e) {
			e.printStackTrace();
			return null;
		}

	}

}
{% endhighlight %}

### LoadImageCallBack.java
{% highlight java %}
/**
 * 回调接口,如果要使用LoadImageAsyncTask这个类，必须要实现beforeImageLoad()和afterImageLoad()这两个接口。
 * @author HILOLT
 *
 */
public interface LoadImageCallBack{
	void beforeImageLoad();
	void afterImageLoad(Bitmap result);
}
{% endhighlight %}

### Demo.java
{% highlight java %}
public class Demo extends Activity{
	ImageView image ;
	
	@Override
	protected void onCreate(Bundle savedInstanceState) {
		super.onCreate(savedInstanceState);
		setContentView(R.layout.main);
		image = (ImageView) findViewById(R.id.image);
		
		LoadImageAsyncTask mTask = new LoadImageAsyncTask(new LoadImageCallBack() {
			
			@Override
			public void beforeImageLoad() {
				//设置默认图片
				image.setBackgroundDrawable(getResources().getDrawable(R.drawable.ic_launcher));
			}
			
			@Override
			public void afterImageLoad(Bitmap result) {
				LogUtils.logError("result", result);
				if(result!=null)
				{
					image.setImageBitmap(result);
				}else
				{
					//如果不成功，设置默认图片
					image.setBackgroundDrawable(getResources().getDrawable(R.drawable.ic_launcher));
				}
			}
			
			
		});
		mTask.execute("http://jekyllrb.com/img/octojekyll.png");
	}
{% endhighlight %}
[示例]
![示例](/assets/images/Screenshot_2013-07-23-17-26-56.png)
