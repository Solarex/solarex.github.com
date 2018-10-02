---
layout: post
title: "数据结构与算法"
date: 2018-07-01 19:06
comments: true
categories: 
- algorithm
- notes
---

## ArrayList

+ 尾插效率高，支持随机访问
+ 中间插入或者删除效率低
+ 空间不够用时，自动增长为现有空间的1.5倍
+ 底层使用Object数组来存储数据，使用System.arrayCopy来移动元素

## LinkedList

+ 头插，中间插，删除效率高
+ 不支持随机访问
+ MessageQueue中Message根据msg.when来进行插入，mMessages指向头结点

## Vector

+ 底层使用数组实现，增长看``capacityIncrement``，若``capacityIncrement``小于0，翻倍增长
+ 方法有``synchronized``修饰，线程安全
+ ``Stack``栈底层实现使用的``Vector``