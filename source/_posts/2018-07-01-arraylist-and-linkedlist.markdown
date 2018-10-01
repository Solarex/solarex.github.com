---
layout: post
title: "[Algo]ArrayList vs LinkedList"
date: 2018-07-01 19:06
comments: true
categories: 
- algorithm
---

## ArrayList

+ 尾插效率高，支持随机访问
+ 中间插入或者删除效率低
+ 空间不够用时，自动增长为现有空间的1.5倍

## LinkedList

+ 头插，中间插，删除效率高
+ 不支持随机访问
+ MessageQueue中Message根据msg.when来进行插入，mMessages指向头结点