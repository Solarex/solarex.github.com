---
layout: post
title: "Android Context"
date: 2014-10-14 10:11
comments: true
categories: 
- dev
- android
---
<center><p><img src="/images/android_robot.png" width="255" height="300"></p></center>

Context is probably the most used element in Android applications…it may also be the most misused.Context objects are so common, and get passed around so frequently, it can be easy to create a situation you didn’t intend.  Loading resources, launching a new Activity, obtaining a system service, getting internal file paths, and creating views all require a Context (and that’s not even getting started on the full list!) to accomplish the task.  What I’d like to do is provide for you some insights on how Context works alongside some tips that will (hopefully) allow you to leverage it more effectively in your applications.

<!-- more -->

##Context Types

Not all Context instances are created equal.  Depending on the Android application component, the Context you have access to varies slightly:

+ **Application** – is a singleton instance running in your application process.  It can be accessed via methods like getApplication() from an Activity or Service, and getApplicationContext() from any other object that inherits from Context.  Regardless of where or how it is accessed, you will always receive the same instance from within your process.

+ **Activity/Service** – inherit from ContextWrapper which implements the same API, but proxies all of its method calls to a hidden internal Context instance, also known as its base context.  Whenever the framework creates a new Activity or Service instance, it also creates a new ContextImpl instance to do all of the heavy lifting that either component will wrap.  Each Activity or Service, and their corresponding base context, are unique per-instance.

+ **BroadcastReceiver** – is not a Context in and of itself, but the framework passes a Context to it in onReceive() each time a new broadcast event comes in.  This instance is a ReceiverRestrictedContext with two main functions disabled; calling registerReceiver() and bindService().  These two functions are not allowed from within an existing BroadcastReceiver.onReceive().  Each time a receiver processes a broadcast, the Context handed to it is a new instance.

+ **ContentProvider** – is also not a Context but is given one when created that can be accessed via ``getContext()``.  If the ``ContentProvider`` is running local to the caller (i.e. same application process), then this will actually return the same Application singleton.  However, if the two are in separate processes, this will be a newly created instance representing the package the provider is running in.

##Saved References
The first issue we need to address comes from saving a reference to a ``Context`` in an object or class that has a lifecycle that extends beyond that of the instance you saved.  For example, creating a custom singleton that requires a ``Context`` to load resources or access a ``ContentProvider``, and saving a reference to the current ``Activity`` or ``Service`` in that singleton.

Bad Singleton

```java
public class CustomManager {
    private static CustomManager sInstance;
 
    public static CustomManager getInstance(Context context) {
        if (sInstance == null) {
            sInstance = new CustomManager(context);
        }
 
        return sInstance;
    }
 
    private Context mContext;
 
    private CustomManager(Context context) {
        mContext = context;
    }
}
```

The problem here is we don’t know where that ``Context`` came from, and it is not safe to hold a reference to the object if it ends up being an ``Activity`` or a ``Service``.  This is a problem because a singleton is managed by a single static reference inside the enclosing class.  This means that our object, and ALL the other objects referenced by it, will never be garbage collected.  If this ``Context`` were an Activity, we would effectively hold hostage in memory all the views and other potentially large objects associated with it; creating a leak.

To protect against this, we modify the singleton to always reference the application context:

Better Singleton

```java
public class CustomManager {
    private static CustomManager sInstance;
 
    public static CustomManager getInstance(Context context) {
        if (sInstance == null) {
            //Always pass in the Application Context
            sInstance = new CustomManager(context.getApplicationContext());
        }
 
        return sInstance;
    }
 
    private Context mContext;
 
    private CustomManager(Context context) {
        mContext = context;
    }
}
```

Now it doesn’t matter where our ``Context`` came from, because the reference we are holding is safe.  The application context is itself a singleton, so we aren’t leaking anything by creating another static reference to it.  Another great example of places where this can crop up is saving references to a ``Context`` from inside a running background thread or a pending Handler.

So why can’t we always just reference the application context?  Take the middleman out of the equation, as it were, and never have to worry about creating leaks?  The answer, as I alluded to in the introduction, is because one ``Context`` is not equal to another.

##Context Capabilities
The common actions you can safely take with a given ``Context`` object depends on where it came from originally.  Below is a table of the common places an application will receive a ``Context``, and in each case what it is useful for:

<table border="1" width="90%" align="center">
<thead>
<tr>
<th></th>
<th align="center">Application</th>
<th align="center">Activity</th>
<th align="center">Service</th>
<th align="center">ContentProvider</th>
<th align="center">BroadcastReceiver</th>
</tr>
</thead>
<tbody>
<tr>
<td>Show a Dialog</td>
<td align="center">NO</td>
<td align="center">YES</td>
<td align="center">NO</td>
<td align="center">NO</td>
<td align="center">NO</td>
</tr>
<tr>
<td>Start an Activity</td>
<td align="center">NO<sup>1</sup></td>
<td align="center">YES</td>
<td align="center">NO<sup>1</sup></td>
<td align="center">NO<sup>1</sup></td>
<td align="center">NO<sup>1</sup></td>
</tr>
<tr>
<td>Layout Inflation</td>
<td align="center">NO<sup>2</sup></td>
<td align="center">YES</td>
<td align="center">NO<sup>2</sup></td>
<td align="center">NO<sup>2</sup></td>
<td align="center">NO<sup>2</sup></td>
</tr>
<tr>
<td>Start a Service</td>
<td align="center">YES</td>
<td align="center">YES</td>
<td align="center">YES</td>
<td align="center">YES</td>
<td align="center">YES</td>
</tr>
<tr>
<td>Bind to a Service</td>
<td align="center">YES</td>
<td align="center">YES</td>
<td align="center">YES</td>
<td align="center">YES</td>
<td align="center">NO</td>
</tr>
<tr>
<td>Send a Broadcast</td>
<td align="center">YES</td>
<td align="center">YES</td>
<td align="center">YES</td>
<td align="center">YES</td>
<td align="center">YES</td>
</tr>
<tr>
<td>Register BroadcastReceiver</td>
<td align="center">YES</td>
<td align="center">YES</td>
<td align="center">YES</td>
<td align="center">YES</td>
<td align="center">NO<sup>3</sup></td>
</tr>
<tr>
<td>Load Resource Values</td>
<td align="center">YES</td>
<td align="center">YES</td>
<td align="center">YES</td>
<td align="center">YES</td>
<td align="center">YES</td>
</tr>
</tbody>
</table>

- An application CAN start an ``Activity`` from here, but it requires that a new task be created.  This may fit specific use cases, but can create non-standard back stack behaviors in your application and is generally not recommended or considered good practice.
- This is legal, but inflation will be done with the default theme for the system on which you are running, not what’s defined in your application.
- Allowed if the receiver is null, which is used for obtaining the current value of a sticky broadcast, on Android 4.2 and above.


##User Interface

You can see from looking at the previous table that there are a number of functions the application context is not properly suited to handle; all of them related to working with the UI.  In fact, the only implementation equipped to handle all tasks associated with the UI is ``Activity``; the other instances fare pretty much the same in all categories.

Luckily, these three actions are things an application doesn’t really have any place doing outside the scope of an ``Activity``; it’s almost like the framework was designed that way on purpose.  Attempting to show a Dialog that was created with a reference to the application context, or starting an ``Activity`` from the application context will throw an exception and crash your application…a strong indicator something has gone wrong.

The less obvious issue is inflating layouts.  If you read my last piece on layout inflation, you already know that it can be a slightly mysterious process with some hidden behaviors;  using the right ``Context`` is linked to another one of those behaviors.  While the framework will not complain and will return a perfectly good view hierarchy from a ``LayoutInflater`` created with the application context, the themes and styles from your app will not be considered in the process.  This is because ``Activity`` is the only ``Context`` on which the themes defined in your manifest are actually attached.  Any other instance will use the system default theme to inflate your views, leading to a display output you probably didn’t expect.

##The Intersection of these Rules

Invariably, someone will arrive at the conclusion that these two rules conflict.  There is a case in the application’s current design where a long-term reference must be saved and we must save an ``Activity`` because the tasks we want to accomplish include manipulation of the UI.  If that is the case, I would urge you to reconsider your design, as this would be a textbook instance of fighting the framework.

##The Rule of Thumb
In most cases, use the ``Context`` directly available to you from the enclosing component you’re working within.  You can safely hold a reference to it as long as that reference does not extend beyond the lifecycle of that component. As soon as you need to save a reference to a ``Context`` from an object that lives beyond your ``Activity`` or ``Service``, even temporarily, switch that reference you save over to the application context.

Context对象是最常见的对象，经常用于参数传递，因此也会出现一些你意想不到的情况。加载资源文件，启动一个新的Activity，获取一个系统服务，获取内部文件路径和创建view全部（这些仅仅是一部分）都需要一个Context对象来完成这些操作。我们想做的是给你展示Context如何工作，以及提供一些建议会（希望会）让你在开发中更合理的使用Context。

##Context类型

并不是所有的Context对象都相同，根据Android应用组件的不同，可以分为以下几种：

+ **Application**：它是应用程序的一个单例，它可以通过``Activity``或``Service``的``getApplication()``方法获取，也可以在任何继承``Context``类的的对象中通过``getApplicationContext()``来获取。不管它是怎么获取的，这些方法返回的都是App中同一个实例。
+ **Activity/Service**：它们继承自``ContextWrapper``，``ContextWrapper``实现了``Context``同样的API，但是隐藏了内部``Context``对象的方法调用，``Context``也是``ContextWrapper``的父类。每当系统创建一个``Activity``或``Service``对象的时候，它也为它们创建了新的``ContextWrapper``对象。每个``Activity``或``Service``对象，包括他们对应的context对象都是唯一的。
+ **BroadcastReceiver**：它并不拥有``Context``对象，但是系统在一个新的广播到来的时候通过``onReceiver()``方法传入一个``Context``对象，这是一个``ReceiverRestrictedContext``，它的两个主要方法，``registerReceiver()``和``bindService()``都被禁用了。每一次receiver处理一个广播，传入的``Context``对象都是一个新的实例。
+ **ContentProvider**：同样也不是一个``Context``对象，但是在创建的时候会通过``getContext()``方法传入一个context对象。如果``ContentProvider``是在本地调用的话（在同一个进程中），那么这会返回一个应用单例。然而，如果是在不同的进程中调用的话，它会新建一个context对象表示当前provider运行的进程。

##Saved References

第一个问题是，我们想在一个对象中保存一个Context对象的引用，并且这个对象的生命周期超过了你保存的Context对象。比如：创建一个需要一个Context对象的单例来加载文件资源或访问一个``ContentProvider``，并且在这个单例中保存当前``Activity``或``Service``的引用。

Bad Singleton这里的问题在于，我们并不知道Context从哪里来，并且如果单例保存了Activity或Service的引用，如果它们被销毁了，这样是不安全的。这个问题是因为单例在类里面保存了一个静态引用。这就意味着那个对象，以及这个对象引用的所有对象都不会被gc回收。如果Context对象是一个Activity，我们就会始终持有这个Activity的所有View以及其他可能很大的对象，最终导致内存泄露。

为了防止出现这种情况，我们可以修改这个类让它持有Application Context,Better Singleton：现在，不管context对象是从哪里传入的，因为现在单例持有的是Application Context，这个是安全的，因为Application Context 本身就是一个单例，因此不会造成内存泄露。还有一个类似的问题就是在一个后台线程（background thread）或一个延时Handler中持有一个对Context的引用。
既然Application Context有那么多好处，我们为什么不用Application Context来处理一切呢？这个问题的答案就是，前面提到过的，是因为**这些Context并不都是相同的。**

从上文中可以知道，Context有多种来源，而不同来源的Context所具有的通用操作也不一样，下表列出了各种不同Context 的作用域：这几个Context只有Activity的Context是“看的见的”，其他组件的Context都是“看不见的”。因此，如果你想创建一个比如Dialog，Activity等“看的见”的组件就必须要用Activity的Context。比如，你想调用getString或getResource方法获取res文件夹下的资源时，所有的Context对象都可以使用。因为这些东西都是“看不见”的。

##用户界面

你可以从上面的表格中看到Application Context有很多事情是做不了的，它不能做的事情都与UI有关。事实上，只有Activity才能够处理与UI有关的任务，其他的Context都是非常相似的（不能处理与UI有关的任务）。
这3个任务（“Show a Dialog”，“Start a Activity”，“Layout Inflation”）似乎就是Android系统就是这么设计的，让Activity来处理这些与UI有关的任务。想要使用Application Context对象来新建一个Dialog或者启动一个Activity系统就会抛出异常，然后程序就会崩溃。
Infalting layouts是一个容易被忽略的问题，如果你读过这篇文章[layout inflation](http://www.doubleencore.com/2013/05/layout-inflation-as-intended/)，你就会明白这里面隐藏着一些坑…使用不同的Context就是会带你走向不同坑。当你使用LayoutInflator，并且使用Application Context后，它会返回一个View，但是这个View的主题和样式就会被忽略。这是因为，Activity 才是系统配置文件中的唯一持有主题和样式的Context。其他所有的Context都会使用系统默认的主题来渲染你的xml来生成View，最终就导致了界面并不是你想要的。

##结论
很多情况下，你可以在一个组件内部使用Context对象，你可以很安全的持有Context的引用，前提就是你的对象生命周期小于Context的生命周期。如果你的对象需要持有一个比Context生命周期要长的Context引用时，即使你的对象也是一个临时对象，也请你考虑保存Application Context 的引用！

REF:[Context](http://www.doubleencore.com/2013/06/context/)
