# BroadcastReciver 详解

---

> BroadcastReciver 位于 android.content 包下，主要用于对广播消息(Intent)的过滤并响应的控件。可以理解为全局的监听器。BroadcastReceiver 自身并不实现图形用户界面，但是当它收到某个广播消息后，BroadcastReceiver 可以启动 Activity 作为响应，或者启动 Service 服务等等。

## 目录

-[广播分类](#广播分类)

-[静态广播](#静态广播)

-[动态广播](#动态广播)

-[广播发送](#广播发送)

-[广播接收器](#广播接收器)

-[8.0 静态广播](#8.0静态广播)

-[带权限的广播](#带权限的广播)

-[本地广播](#本地广播)

## 广播分类

    Broadcast（广播）是Android的四大组件之一，用于进程/线程间通信。按照不同的方式分类有不同的归类

######发送方式

1.标准广播

完全异步执行的广播，没有先后顺序，不可被拦截，效率高

2.有序广播

同步执行的广播，有先后顺序，发送出去的广播被广播接收者按照先后顺序接收，并且前面的广播接收器还可以截断正在传递的广播，这样后面的广播接收器就无法接收广播消息了。

######注册方式

1.静态广播

不管应用程序是否处于活动状态，都会进行监听。每次触发都会建立新的 Receiver 对象。

2.动态广播

代码中进行注册，动态注册的广播一定要取消注册才行，通常是在 onDestroy()方法中调用 unregisterReceiver()方法来实现。从开始创建直到其被解除注册会使用同一个 Receiver，无论这个广播被触发几次。

######定义方式

1.系统广播

Android 系统中内置了多个系统广播，每个系统广播都具有特定的 IntentFilter，其中主要包括具体的 Action，系统广播发出后，将被相应的 BroadcastReceiver 接收。系统广播在系统内部当特定事件发生时，由系统自动发出。

2.自定义广播

开发者自己定义的广播

######范围方式

1.全局广播

发出的广播可以被其他任意的应用程序接收，或者可以接收来自其他任意应用程序的广播。

2.本地广播

只能在本应用程序的内部进行传递的广播，广播接收器也只能接收内部的广播，不能接受其他应用程序的广播。

介绍完广播按照不同方式的不同归类，接下来来讲解一下广播的使用

## 静态广播

静态广播的注册是在 AndroidMainfest.xml 中定义的

```
<receiver android:name=".ui.broadcast.MyReceiver"
          android:enabled="true"
          android:exported="true">
    <intent-filter>
        <!-- 接收系统开机广播 -->
        <action android:name="android.intent.action.ACTION_BATTERY_LOW" />
        <!-- 接收自定义的广播 -->
        <action android:name="com.madreain.broadcast.MyReceiverFilter" />
    </intent-filter>
</receiver>

```

enabled="true"代表能够接受到广播信息。exported="true"代表能够接收到外部 APK 发送的广播信息

## 动态广播

动态广播只需要在使用的代码中注册

```
MyReceiver myReceiver = new MyReceiver();

IntentFilter intentFilter = new IntentFilter();
// 系统广播 action 接受网络变化
intentFilter.addAction(ConnectivityManager.CONNECTIVITY_ACTION);
// 自定义的 action
intentFilter.addAction(MyReceiver.ACTION);
registerReceiver(myReceiver, intentFilter);

在onDestory方法中注销广播
unregisterReceiver(myReceiver);
```

## 广播发送

无论静态广播还是动态广播，发送广播的方式都是一样的

```
Intent intent = new Intent();
// 自定义的 action
intent.setAction(MyReceiver.ACTION);
sendBroadcast(intent);
```

## 广播接收器

广播的接受，创建广播接收器来实现广播的接受

```

public class MyReceiver extends BroadcastReceiver {

  private static final String ACTION = "com.madreain.broadcast.MyReceiverFilter";

  @Override
  public void onReceive(Context context, Intent intent) {
    if (intent.getAction().equals(ACTION) ){
              //接收固定广播进行处理

        }
  }
}

```

## 8.0 静态广播

Android8.0 引入了新的广播接收器限制，因此您应该移除所有为隐式广播 Intent 注册的广播接收器。将它们留在原位并不会在构建时或运行时令应用失效，但当应用运行在 Android8.0 上时它们不起任何作用。显式广播 Intent（只有您的应用可以响应的 Intent）在 Android8.0 上仍以相同方式工作。这个新增限制有一些例外情况。如需查看在以 Android 8.0 为目标平台的应用中仍然有效的隐式广播的列表，请参阅[隐式广播例外](https://developer.android.google.cn/guide/components/broadcast-exceptions)。

8.0 发送广播需修改,需设置 ComponetName，ComponetName(“自定义广播的包名”， “自定义广播的路径”)

```
Intent intent = new Intent(STATICACTION);
intent.setComponent(new ComponentName("com.madreain.broadcast", "com.madreain.broadcast.MyReceiver"));
sendBroadcast(intent);

```

## 带权限的广播

在广播的实际应用中，我们为了避免别的应用监听我们的广播，或者别人的应用冒充我们的应用进行发送广播。我们在使用中可以给广播增加权限来避免这些安全问题

```
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    package="com.madreain">

  <!-- 自定义一个自己的权限 -->
  <permission android:name="com.madreain.permissions.MY_BROADCAST"/>

  <!-- 使用自定义的权限 -->
  <uses-permission android:name="com.madreain.permissions.MY_BROADCAST"/>

  <application ...>

    <!-- 添加权限 -->
    <receiver android:name=".ui.broadcast.MyReceiver"
              android:permission="com.madreain.broadcast.MY_BROADCAST"
              android:enabled="true"
              android:exported="true">
      <intent-filter>
        <!-- 例如：接收自定义的广播 -->
        <action android:name="com.madreain.broadcast.MyReceiverFilter" />
      </intent-filter>
    </receiver>

  </application>

</manifest>
```

在发送广播时，我们也得指定权限，让具有权限的应用才能接受到此广播

```
Intent intent = new Intent();
intent.setAction(MyReceiver.ACTION);
// 发送广播，添加权限
sendBroadcast(intent, "com.madreain.permissions.MY_BROADCAST");

```

其实为了解决 Android 的广播安全问题,Android 还引入了另一个 LocalBroadcastManger 类,接下来的本地广播来介绍

## 本地广播

本地广播用于应用内部传递消息，比 BroadcastReceiver 更加高效，它只在应用内部有效，不需要考虑安全问题。本地广播的创建仍然是继承 BroadcastReceiver 创建子类，并实现父类的 onReceive() 方法。在注册、发送、注销广播时使用 LocalBroadcastManager 来进行相关操作。

发送广播

```
Intent intent = new Intent(MyReceiver.ACTION));
LocalBroadcastManager.getInstance(this).sendBroadcast(intent);

```

接受广播

```
MyReceiver myReceiver = new MyReceiver();
IntentFilter intentFilter = new IntentFilter();
intentFilter.addAction(MyReceiver.ACTION);
LocalBroadcastManager.getInstance(this).registerReceiver(myReceiver, intentFilter);
```

注销广播

```
LocalBroadcastManager.getInstance(this).unregisterReceiver(myReceiver);
```

在此推荐一个第三方消息传递的库[EventBus](https://github.com/greenrobot/EventBus)