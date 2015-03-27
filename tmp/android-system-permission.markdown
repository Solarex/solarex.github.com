##Android System Permissions

[REF](file:///home/houruhou/Software/Apps/adt-bundle-linux-x86_64-20140702/sdk/docs/guide/topics/security/permissions.html)

Android is a privilege-separated operating system, in which each application runs with a distinct system identity (Linux user ID and group ID). Android以UID/GID区分应用从而将各个应用独立开来。Additional finer-grained security features are provided through a "permission" mechanism that enforces restrictions on the specific operations that a particular process can perform, and per-URI permissions for granting ad hoc access to specific pieces of data.除此之外还有permission机制来保证各个进程对特定操作的限制和per-URI permission来保证对特定数据访问的限制。

###Security Architecture
A central design point of the Android security architecture is that no application, by default, has permission to perform any operations that would adversely impact other applications, the operating system, or the user. This includes reading or writing the user's private data (such as contacts or emails), reading or writing another application's files, performing network access, keeping the device awake, and so on.Android安全架构的中心设计思想是默认没有应用可以执行影响其他应用，Android系统或用户的操作。

Because each Android application operates in a process sandbox, applications must explicitly share resources and data. They do this by declaring the permissions they need for additional capabilities not provided by the basic sandbox. Applications statically declare the permissions they require, and the Android system prompts the user for consent at the time the application is installed. Android has no mechanism for granting permissions dynamically (at run-time) because it complicates the user experience to the detriment of security.每个应用都运行在进程沙盒中，应用必须显式共享需要的资源或者数据。应用在manifest中声明他们需要的权限，Android系统在应用安装时提示给用户，Android系统没有在运行时动态授权的机制。

###Application Signing
All APKs (.apk files) must be signed with a certificate whose private key is held by their developer. This certificate identifies the author of the application. The certificate does not need to be signed by a certificate authority; it is perfectly allowable, and typical, for Android applications to use self-signed certificates. The purpose of certificates in Android is to distinguish application authors. This allows the system to grant or deny applications access to signature-level permissions and to grant or deny an application's request to be given the same Linux identity as another application.所有的apk都必须签名，签名的目的是为了区分应用的作者。区分应用作者可以让系统决定是否为相同签名的两个应用来分配相同的uid或者grant/deny signature-level permissions，[signature-level permissions](http://developer.android.com/guide/topics/manifest/permission-element.html#plevel)。

关于signature-level permission，在``<permission>``中``android:protectionLevel=signature``，A permission that the system grants only if the requesting application is signed with the same certificate as the application that declared the permission. If the certificates match, the system automatically grants the permission without notifying the user or asking for the user's explicit approval.

###User IDs and File Access
At install time, Android gives each package a distinct Linux user ID. The identity remains constant for the duration of the package's life on that device. On a different device, the same package may have a different UID; what matters is that each package has a distinct UID on a given device.应用在安装时被分配一个独一无二的UID。Because security enforcement happens at the process level, the code of any two packages cannot normally run in the same process, since they need to run as different Linux users. You can use the ``sharedUserId`` attribute in the AndroidManifest.xml's ``manifest`` tag of each package to have them assigned the same user ID. By doing this, for purposes of security the two packages are then treated as being the same application, with the same user ID and file permissions. Note that in order to retain security, only two applications signed with the same signature (and requesting the same sharedUserId) will be given the same user ID.只有在manifest文件中声明了``sharedUserId``，才可能被分配相同的user id。只有应用的签名相同，并且要求了sharedUserId，才可能被分配相同的user id。

###Using Permissions
A basic Android application has no permissions associated with it by default, meaning it cannot do anything that would adversely impact the user experience or any data on the device. To make use of protected features of the device, you must include in your AndroidManifest.xml one or more ``<uses-permission>`` tags declaring the permissions that your application needs.应用在manifest文件中使用``<uses-permission>``来申请权限。Often times a permission failure will result in a ``SecurityException`` being thrown back to the application. However, this is not guaranteed to occur everywhere. For example, the ``sendBroadcast(Intent)`` method checks permissions as data is being delivered to each receiver, after the method call has returned, so you will not receive an exception if there are permission failures. In almost all cases, however, a permission failure will be printed to the system log.通常情况下授权失败会抛出``SecurityException``异常。

A particular permission may be enforced at a number of places during your program's operation:特定权限检查可以在不同场景中。

+ At the time of a call into the system, to prevent an application from executing certain functions.
+ When starting an activity, to prevent applications from launching activities of other applications.
+ Both sending and receiving broadcasts, to control who can receive your broadcast or who can send a broadcast to you.
+ When accessing and operating on a content provider.
+ Binding to or starting a service.

The permissions provided by the Android system can be found at [Manifest.permission](http://developer.android.com/reference/android/Manifest.permission.html). You can see which permissions were added with each release in the [Build.VERSION_CODES](http://developer.android.com/reference/android/os/Build.VERSION_CODES.html) documentation.

###Declaring and Enforcing Permissions
To enforce your own permissions, you must first declare them in your AndroidManifest.xml using one or more ``<permission>`` tags.

```xml
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.me.app.myapp" >
    <permission android:name="com.me.app.myapp.permission.DEADLY_ACTIVITY"
        android:label="@string/permlab_deadlyActivity"
        android:description="@string/permdesc_deadlyActivity"
        android:permissionGroup="android.permission-group.COST_MONEY"
        android:protectionLevel="dangerous" />
    ...
</manifest>
```

You can look at the permissions currently defined in the system with the Settings app and the shell command ``adb shell pm list permissions``. ``adb shell pm list permissions -s``displays the permissions in a form similar to how the user will see them.

###Enforcing Permissions in AndroidManifest.xml
High-level permissions restricting access to entire components of the system or application can be applied through your AndroidManifest.xml. All that this requires is including an ``android:permission`` attribute on the desired component, naming the permission that will be used to control access to it.四大组件permission.

Activity permissions (applied to the <activity> tag) restrict who can start the associated activity. The permission is checked during ``Context.startActivity()`` and ``Activity.startActivityForResult()``; if the caller does not have the required permission then ``SecurityException`` is thrown from the call.

Service permissions (applied to the <service> tag) restrict who can start or bind to the associated service. The permission is checked during ``Context.startService()``, ``Context.stopService()`` and ``Context.bindService()``; if the caller does not have the required permission then ``SecurityException`` is thrown from the call.

BroadcastReceiver permissions (applied to the <receiver> tag) restrict who can send broadcasts to the associated receiver. The permission is checked after ``Context.sendBroadcast()`` returns, as the system tries to deliver the submitted broadcast to the given receiver. As a result, a permission failure will not result in an exception being thrown back to the caller; it will **just not deliver the intent**. In the same way, a permission can be supplied to ``Context.registerReceiver()`` to control who can broadcast to a programmatically registered receiver. Going the other way, a permission can be supplied when calling ``Context.sendBroadcast()`` to restrict which BroadcastReceiver objects are allowed to receive the broadcast (see below).

ContentProvider permissions (applied to the <provider> tag) restrict who can access the data in a ContentProvider. (Content providers have an important additional security facility available to them called **URI permissions** which is described later.) Unlike the other components, there are two separate permission attributes you can set: ``android:readPermission`` restricts who can read from the provider, and ``android:writePermission`` restricts who can write to it. Note that if a provider is protected with both a read and write permission, holding only the write permission does not mean you can read from a provider. The permissions are checked when you first retrieve a provider (if you don't have either permission, a ``SecurityException`` will be thrown), and as you perform operations on the provider. Using ``ContentResolver.query()`` requires holding the read permission; using ``ContentResolver.insert()``, ``ContentResolver.update()``, ``ContentResolver.delete()`` requires the write permission. In all of these cases, not holding the required permission results in a ``SecurityException`` being thrown from the call.

###Enforcing Permissions when Sending Broadcasts
In addition to the permission enforcing who can send Intents to a registered ``BroadcastReceiver`` (as described above), you can also specify a required permission when sending a broadcast. By calling ``Context.sendBroadcast()`` with a permission string, you require that a receiver's application must hold that permission in order to receive your broadcast.在``sendBroadcast``时加入权限会要求BroadcastReceiver有要求的权限才可以接收到广播。Note that both a receiver and a broadcaster can require a permission. When this happens, both permission checks must pass for the Intent to be delivered to the associated target.广播者和广播接收者都可以要求权限，如果双方都要求了权限，两边的权限检查都必须满足广播才可以被送达到接收者。

###Other Permission Enforcement

































