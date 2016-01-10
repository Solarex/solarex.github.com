---
layout: post
title: "android-custom-view"
date: 2015-05-05 11:45
comments: true
categories: 
---
<p><center><img src="/images/android_training.jpg"/></center></p>

<!-- more --> 

<p><center><iframe width="560" height="315" src="https://www.youtube.com/embed/NYtB6mlu7vA" frameborder="0" allowfullscreen></iframe></center></p>

//todo romain guy custom view notes

While viewgroups dont generally draw any content of their own,there are many situations where this could be useful,there are help instances where we can ask viewgroup to do some drawing.

the first is inside the method ``dispatchDraw()`` after ``super.dispatchDraw`` has been called,at this stage,child views have already been drawn and we have an opportunity to do additional drawing on top.

the second is using the same ``onDraw()`` callback as in ``view``.Anything we draw here will be drawn before the child view and thus will show us underneath them,this can be helpful for drawing any type of dynamic backgrounds or selector states if you wish to put cod into the ``onDraw`` of a ViewGroup.Dont forget to enable drawing callbacks of ViewGroup by ``setWillNotDraw(false)``,otherwise ViewGroup's ``onDraw`` callback will never be triggered,this is because ViewGroup by default has its ``onDraw`` callback disabled.