---
layout: post
title: "Android Handler Looper Messagequeue机制"
date: 2019-10-10 00:00
comments: true
categories: 
- dev
- android
---

```java
 com.android.internal.os.Zygote$MethodAndArgsCaller.run(Zygote.java:327) 
 at java.lang.reflect.Method.invoke(Native Method) 
 at android.app.ActivityThread.main(ActivityThread.java:6944) 
 at android.os.Looper.loop(Looper.java:142) 
 at android.os.MessageQueue.next(MessageQueue.java:325) 
 at android.os.MessageQueue.nativePollOnce(Native Method) 
 at android.view.InputEventReceiver.dispatchInputEvent(InputEventReceiver.java:197) 
 at android.view.ViewRootImpl$WindowInputEventReceiver.onInputEvent(ViewRootImpl.java:7963) 
 at android.view.ViewRootImpl.enqueueInputEvent(ViewRootImpl.java:7753) 
 at android.view.ViewRootImpl.doProcessInputEvents(ViewRootImpl.java:7792) 
 at android.view.ViewRootImpl.deliverInputEvent(ViewRootImpl.java:7852) 
 at android.view.ViewRootImpl$InputStage.deliver(ViewRootImpl.java:4995) 
 at android.view.ViewRootImpl$InputStage.apply(ViewRootImpl.java:5022) 
 at android.view.ViewRootImpl$InputStage.forward(ViewRootImpl.java:5014) 
 at android.view.ViewRootImpl$InputStage.onDeliverToNext(ViewRootImpl.java:5048) 
 at android.view.ViewRootImpl$InputStage.deliver(ViewRootImpl.java:4995) 
 at android.view.ViewRootImpl$AsyncInputStage.apply(ViewRootImpl.java:5208) 
 at android.view.ViewRootImpl$InputStage.apply(ViewRootImpl.java:5022) 
 at android.view.ViewRootImpl$AsyncInputStage.forward(ViewRootImpl.java:5151) 
 at android.view.ViewRootImpl$InputStage.forward(ViewRootImpl.java:5014) 
 at android.view.ViewRootImpl$InputStage.onDeliverToNext(ViewRootImpl.java:5048) 
 at android.view.ViewRootImpl$InputStage.deliver(ViewRootImpl.java:4995) 
 at android.view.ViewRootImpl$ViewPostImeInputStage.onProcess(ViewRootImpl.java:5502) 
 at android.view.ViewRootImpl$ViewPostImeInputStage.processPointerEvent(ViewRootImpl.java:5707) 
 at android.view.View.dispatchPointerEvent(View.java:12800) 
 at com.android.internal.policy.DecorView.dispatchTouchEvent(DecorView.java:575) 
 at androidx.appcompat.view.WindowCallbackWrapper.dispatchTouchEvent(WindowCallbackWrapper.java:69)  
 at android.app.Activity.dispatchTouchEvent(Activity.java:3384) 
 at com.android.internal.policy.PhoneWindow.superDispatchTouchEvent(PhoneWindow.java:1871) 
 at com.android.internal.policy.DecorView.superDispatchTouchEvent(DecorView.java:613) 
 at android.view.ViewGroup.dispatchTouchEvent(ViewGroup.java:2844) 
 at android.view.ViewGroup.dispatchTransformedTouchEvent(ViewGroup.java:3159) 
 at android.view.ViewGroup.dispatchTouchEvent(ViewGroup.java:2844) 
 at android.view.ViewGroup.dispatchTransformedTouchEvent(ViewGroup.java:3159) 
 at android.view.ViewGroup.dispatchTouchEvent(ViewGroup.java:2844) 
 at android.view.ViewGroup.dispatchTransformedTouchEvent(ViewGroup.java:3159) 
 at android.view.ViewGroup.dispatchTouchEvent(ViewGroup.java:2844) 
 at android.view.ViewGroup.dispatchTransformedTouchEvent(ViewGroup.java:3159) 
 at android.view.ViewGroup.dispatchTouchEvent(ViewGroup.java:2844) 
 at android.view.ViewGroup.dispatchTransformedTouchEvent(ViewGroup.java:3159) 
 at android.view.ViewGroup.dispatchTouchEvent(ViewGroup.java:2844) 
 at android.view.ViewGroup.dispatchTransformedTouchEvent(ViewGroup.java:3159) 
 at android.view.ViewGroup.dispatchTouchEvent(ViewGroup.java:2844) 
 at android.view.ViewGroup.dispatchTransformedTouchEvent(ViewGroup.java:3159) 
 at android.view.View.dispatchTouchEvent(View.java:12552) 
 at android.widget.TextView.onTouchEvent(TextView.java:11327)
 ```
