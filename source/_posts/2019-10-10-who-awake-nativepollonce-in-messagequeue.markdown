---
layout: post
title: "谁唤醒了MessageQueue中的nativePollOnce"
date: 2019-10-10 00:00
comments: true
categories: 
- dev
- android
---

在看Handler相关源码的时候，我们都知道在MessageQueue的nativePollOnce这里会阻塞，那什么时候主线程从这里被唤醒呢？

<!-- more -->
