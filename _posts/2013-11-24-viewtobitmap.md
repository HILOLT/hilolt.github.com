---
layout: post
keywords: android, view to bitmap 
description: android将view转换成bitmap,需要注意的问题
title: "android将view转换成bitmap,需要注意的问题"
categories: [android]
tags: [android,achartengine,view to bitmap]
icon: file-alt
---

-----------------------------------------------------
##在将view转换成bitmap的时候,遇到了以下问题:

###1.转换完成的bitmap为null .

###2.转换完成的bitmap为不能显示不可见的区域的view .

--------------------------------------------------



针对问题1 , 导致bitmap为null的主要原因是,view还没有真正的绘制完成,获得的view的尺寸为0 .
解决的办法有2种 , 至少目前我只找到2种,如果你想在不点击任何事件的情况下想自动获取bitmap , 那么你就得实时判断view的尺寸.
方法一: 自定义view,如果你的view是RelativeLayout, 那么就集成extends RelativeLayout,重写onMeasure()这个方法:

```
@Override
	protected void onMeasure(int widthMeasureSpec, int heightMeasureSpec) {
		
		if(!mIsInit && getWidth() != 0 && getHeight() != 0) { //画出来了
			Bitmap  bitmap = getBitmapFromView(this); //获取父类CustomRelativeLayout对应的Bitmap
			if(null == bitmap) {
				super.onMeasure(widthMeasureSpec, heightMeasureSpec);
				return;
			}
			mIsInit = true;
			CustomImageView myImage = new CustomImageView(context);
			myImage.setScaleType(ScaleType.MATRIX); //使用矩阵进行处理Bitmap
			myImage.setId(this.getId()); //使用父类的Id来标示CustomImageView
			myImage.setOnClickListener(new CustomImageViewListener(context)); //按键事件监听器
			myImage.setImageBitmap(bitmap);
			myImage.setLayoutParams(new RelativeLayout.LayoutParams(LayoutParams.MATCH_PARENT, LayoutParams.MATCH_PARENT));
			
			removeAllViews(); //清空xml中的所有子view
			setBackgroundDrawable(null); //父类 背景设置为空
			
			addView(myImage); //添加CustomImageView 到当前 CustomRelativeLayout
		}
	
		super.onMeasure(widthMeasureSpec, heightMeasureSpec);
	}
```
	
方法二:不用重写控件,直接给view添加监听事件:

```
view.getViewTreeObserver().addOnGlobalLayoutListener(new ViewTreeObserver.OnGlobalLayoutListener() {
			@Override
			public void onGlobalLayout() {
				savaBitmapToSdcard(scrollView);

				LogUtils.logError(getClass(), "onGlobalLayout ......");
			}
		});
```

其中getViewTreeObserver().addOnGlobalLayoutListener的onGlobalLayout这个方法会执行多次,具体执行的次数要根据布局来判断,添加了
此事件,即使view的尺寸还没有获得,也不会报错. 到目前为止,我只找到这个方法能在初始化layout的时候就去获得bitmap .

---------------------------------------

针对问题2,是默认获得bitmap的尺寸就是当前可见的区域,我们可以获取全部的view ,可以这样做:

```
public static Bitmap getBitmapFromView(View view) {
		view.setDrawingCacheEnabled(true);
		view.measure(MeasureSpec.makeMeasureSpec(0, MeasureSpec.UNSPECIFIED), MeasureSpec.makeMeasureSpec(0, MeasureSpec.UNSPECIFIED));
		view.layout(0, 0, view.getMeasuredWidth(), view.getMeasuredHeight());
		view.buildDrawingCache();
		Bitmap returnedBitmap = Bitmap.createBitmap(view.getWidth(), view.getHeight(), Bitmap.Config.ARGB_8888);
		Canvas canvas = new Canvas(returnedBitmap);
		Drawable bgDrawable = view.getBackground();
		if (bgDrawable != null)
			bgDrawable.draw(canvas);
		else
			canvas.drawColor(Color.WHITE);
		view.draw(canvas);
		return returnedBitmap;
	}
```

其中,

```
view.measure(MeasureSpec.makeMeasureSpec(0, MeasureSpec.UNSPECIFIED), MeasureSpec.makeMeasureSpec(0, MeasureSpec.UNSPECIFIED))
```

就是获得全部的view,即使你的界面很长,也可以全部转成bitmap 了.
