---
layout: post
keywords: hilolt,android使用外部字体
description: android使用外部字体
title: "android使用外部字体"
categories: [android]
tags: [android fonts]
group: archive
icon: file-alt
---


#android 使用外部的字体很简单,就几行代码,但有的字体不能很好的支持中文,而有的字体有不能区分英文的大小写,本人推荐我们项目中生成pdf的字体:simsun.ttc,
能够很好的支持中英文.

---------------------

```java

package com.example.androidhive;

import android.app.Activity;
import android.graphics.Typeface;
import android.os.Bundle;
import android.widget.TextView;

public class AndroidExternalFontsActivity extends Activity {
    @Override
    public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.main);
        
        // Font path
        String fontPath = "fonts/simsun.ttc";
        
        // text view label
        TextView txtGhost = (TextView) findViewById(R.id.ghost);
        txtGhost.setText("北京朝阳123456789aAbB");
        
        // Loading Font Face
        Typeface tf = Typeface.createFromAsset(getAssets(), fontPath);
        
        // Applying font
        txtGhost.setTypeface(tf);
    }
}

```


