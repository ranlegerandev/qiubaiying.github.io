---
layout:     post
title:      Android中ViewPager实现左右滑动
subtitle:   ViewPager实现左右滑动
date:       2018-02-22
author:     然了个然
header-img: post-bg-ioses.jpg
catalog: true
tags:
    - Android
---

在开发中ViewPager使用率还是非常高的，今天就来看一下它的基本用法实现一个左右滑动的功能。

布局文件：activity_main.xml
```
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    tools:context="com.ranlegeran.viewpagerdemo.MainActivity">

    <android.support.v4.view.ViewPager
        android:id="@+id/ViewPager"
        android:layout_width="match_parent"
        android:layout_height="match_parent">
    </android.support.v4.view.ViewPager>

</LinearLayout>

```
layout_first.xml
```
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:gravity="center"
    android:orientation="vertical" >

    <ImageView
        android:layout_width="80dp"
        android:layout_height="80dp"
        android:src="@mipmap/ic_launcher"/>

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_gravity="center"
        android:text="第一个页面"
        android:textSize="18sp"/>

</LinearLayout>
```
layout_second.xml
```
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:gravity="center"
    android:orientation="vertical" >

    <ImageView
        android:layout_width="80dp"
        android:layout_height="80dp"
        android:src="@mipmap/ic_launcher"/>

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="第二个页面"
        android:textSize="18sp"/>

</LinearLayout>
```
layout_third.xml
```
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:gravity="center"
    android:orientation="vertical" >

    <ImageView
        android:layout_width="80dp"
        android:layout_height="80dp"
        android:src="@mipmap/ic_launcher"/>

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="第三个页面"
        android:textSize="18sp"/>

</LinearLayout>
```
代码部分：MainActivity
```
package com.ranlegeran.viewpagerdemo;

import android.support.annotation.NonNull;
import android.support.v4.view.PagerAdapter;
import android.support.v4.view.ViewPager;
import android.support.v7.app.AppCompatActivity;
import android.os.Bundle;
import android.view.LayoutInflater;
import android.view.View;
import android.view.ViewGroup;

import java.util.ArrayList;
import java.util.List;

public class MainActivity extends AppCompatActivity {
    private ViewPager mViewPager = null;
    private View mFirst;
    private View mSecond;
    private View mThird;

    private List<View> mList = new ArrayList<>();

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        initView();
    }

    private void initView() {
        mViewPager = (ViewPager) findViewById(R.id.ViewPager);
        //添加视图
        LayoutInflater layoutInflater = getLayoutInflater().from(this);
        mFirst = layoutInflater.inflate(R.layout.layout_first,null);
        mSecond = layoutInflater.inflate(R.layout.layout_second,null);
        mThird = layoutInflater.inflate(R.layout.layout_third,null);
        mList.add(mFirst);
        mList.add(mSecond);
        mList.add(mThird);
        mViewPager.setAdapter(new ViewAdapter(mList));
        mViewPager.setCurrentItem(1);//设置ViewPager的默认页面从0开始当前默认为第二个页面
    }

    public class ViewAdapter extends PagerAdapter {
        private List<View> mList;
        public ViewAdapter(List<View> mList) {
            this.mList = mList;
        }

        /**
         * 删除Item
         * @param container
         * @param position
         * @param object
         */
        @Override
        public void destroyItem(@NonNull ViewGroup container, int position, @NonNull Object object) {
            container.removeView(mList.get(position));
        }

        /**
         * 实例化Item
         * @param container
         * @param position
         * @return
         */
        @NonNull
        @Override
        public Object instantiateItem(@NonNull ViewGroup container, int position) {
            container.addView(mList.get(position));
            return mList.get(position);
        }

        /**
         * 返回的数量
         * @return
         */
        @Override
        public int getCount() {
            return mList.size();
        }

        @Override
        public boolean isViewFromObject(@NonNull View view, @NonNull Object object) {
            return view == object;
        }
    }
}

```