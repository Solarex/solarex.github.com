---
layout: post
title: "ACTION_CANCEL是什么时候触发的"
date: 2019-11-02 23:02
comments: true
categories: 
- dev
- android
---

我们在处理MotionEvent的时候，一般会去处理``ACTION_DOWN``，``ACTION_UP``,``ACTION_MOVE``，似乎很少跟``ACTION_CANCEL``打交道，那``ACTION_CANCEL``是什么时候被触发的呢，对于``ACTION_CANCEL``又该如何做出处理呢？

<!-- more -->



### reference

+ [onInterceptTouchEvent](https://developer.android.com/reference/android/view/ViewGroup.html#onInterceptTouchEvent)
+ [ACTION_CANCEL while touching](https://stackoverflow.com/questions/6018309/action-cancel-while-touching)

