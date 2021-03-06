# Activity详解

---

>Activity 是一个应用组件，用户可与其提供的屏幕进行交互，以执行拨打电话、拍摄照片、发送电子邮件或查看地图等操作。 每个 Activity 都会获得一个用于绘制其用户界面的窗口。窗口通常会充满屏幕，但也可小于屏幕并浮动在其他窗口之上。 

## 目录

-[生命周期](#生命周期)

-[启动模式](#启动模式)

-[onNewIntent](#onNewIntent)

-[Intent Filter](#IntentFilter)

-[onConfigurationChanged](#onConfigurationChanged)

-[数据保存与恢复](#数据保存与恢复)

-[URL Scheme](#URL_Scheme)

-[startActivityForResult](#startActivityForResult)

-[源码分析](#源码分析)

## 生命周期

在介绍Activity生命周期之前，我们先来了解一下Activity的四种基本状态：

**Active/Running:**
Activity处于活动状态，此时Activity处于栈顶，是可见状态，可与用户进行交互。 

**Paused：** 
当Activity失去焦点时，或被一个新的非全屏的Activity，或被一个透明的Activity放置在栈顶时，Activity就转化为Paused状态。但我们需要明白，此时Activity只是失去了与用户交互的能力，其所有的状态信息及其成员变量都还存在，只有在系统内存紧张的情况下，才有可能被系统回收掉。 

**Stopped：** 
当一个Activity被另一个Activity完全覆盖时，被覆盖的Activity就会进入Stopped状态，此时它不再可见，但是跟Paused状态一样保持着其所有状态信息及其成员变量。 

**Killed：** 
当Activity被系统回收掉时，Activity就处于Killed状态。 
Activity会在以上四种形态中相互切换，至于如何切换，这因用户的操作不同而异。了解了Activity的4种形态后，我们就来聊聊Activity的生命周期。

![Activity生命周期](/Resource/Image/activity_lifecycle.png)

**onCreate**

Activity被创建时的第一个方法，表示正在被创建，一些初始化工作在这里（加载布局资源，初始化所需要的数据等），如果有耗时任务需开异步线程去完成。

**onStart**

Activity正在被启动，在这个状态下Activity还在加载其他资源，正在向我们展示，用户还无法看到，不能交互

**onResume**

Activity创建完成，用户可看见界面，可交互

**onPause**

Activity正在暂停，正常情况下接着会执行onStop()。这时可以做数据的存储、动画停止的操作，但是不能太耗时，要不然影响新Activity的创建

**onStop**

Activity即将停止，这时可以做一些回收工作，一样不能太耗时

**onDestory**

Activity即将被销毁，可以做一些工作和资源的回收（Service、BroadCastReceiver、Map、Bitmap回收等）

**onRestart**

Activity正在重新启动，一般时当前Activity从不可见到可见状态时会执行这个方法，例如：用户按下Home键（锁屏）或者打开新Activity再返回这个Activity

思考问题：（实际操作打印生命周期执行顺序）

1.启动AActivity
2.AActivity创建完成，按Home键回到主屏
3.按Home键再主屏，再次点击App回到AActivity
4.AActivity去打开BActivity
5.在BActivity点击Back键回退
6.熄屏、亮屏
7.系统配置改变（横竖屏切换）
8.内存不足导致低优先级的Activity被杀死


## 启动模式

**standard**

Standard模式是Android的默认启动模式，你不在配置文件中做任何设置，那么这个Activity就是standard模式，这种模式下，Activity可以有多个实例，每次启动Activity，无论任务栈中是否已经有这个Activity的实例，系统都会创建一个新的Activity实例

**singleTop**

SingleTop模式和standard模式非常相似，主要区别就是当一个singleTop模式的Activity已经位于任务栈的栈顶，再去启动它时，不会再创建新的实例,如果不位于栈顶，就会创建新的实例，现在把配置文件中FirstActivity的启动模式改为SingleTop，我们的应用只有一个Activity，FirstActivity自然处于任务栈的栈顶。

**SingleTask**

SingleTask模式的Activity在同一个Task内只有一个实例，如果Activity已经位于栈顶，系统不会创建新的Activity实例，和singleTop模式一样。但Activity已经存在但不位于栈顶时，系统就会把该Activity移到栈顶，并把它上面的activity出栈。修改上面的程序，新建一个SecondActivity,将FirstActivity设置为singleTask启动模式，并让它启动SecondActivity，再让SecondActivity来启动FirstActivity。

**singleInstance**

singleInstance模式也是单例的，但和singleTask不同，singleTask只是任务栈内单例，系统里是可以有多个singleTask Activity实例的，而singleInstance Activity在整个系统里只有一个实例，启动一singleInstanceActivity时，系统会创建一个新的任务栈，并且这个任务栈只有他一个Activity。

SingleInstance模式并不常用，如果我们把一个Activity设置为singleInstance模式，你会发现它启动时会慢一些，切换效果不好，影响用户体验。它往往用于多个应用之间，例如一个电视launcher里的Activity，通过遥控器某个键在任何情况可以启动，这个Activity就可以设置为singleInstance模式，当在某应用中按键启动这个Activity，处理完后按返回键，就会回到之前启动它的应用，不影响用户体验。

## onNewIntent

避免Activity之间的跳转传值多次实体化

![onNewIntent什么时候被执行](/Resource/Image/activity_lifecycle1.png)

如果在 AndroidManifest.xml 中，将 Activity 的 launchMode 设置成了 "singleTop" 模式，或者在调用 startActivity(Intent) 时，设置了 
FLAG_ACTIVITY_SINGLE_TOP 标识，那么，当该 Activity 再次被启动时，如果它依然存在于 Activity 栈中，并且刚好处于栈的最顶层时，那么它将不会被重新创建，而是直接使用原来的实例，此时，onNewIntent(Intent) 将会被调用，后续生命周期中的其它方法，就可以使用 onNewIntent(Intent) 传递过来的新的 Intent 参数了。（也就是说，其它方法可以使用更新后的 Intent 参数） 

调用顺序如下： 
onNewIntent() -> onRestart() -> onStart() -> onResume() 

需要特别注意的是， 如果在 onNewIntent(Intent) 中，不调用 setIntent(Intent) 方法对 Intent 进行更新的话，那么之后在调用 getIntent() 方法时得到的依然是最初的值。 

## IntentFilter

意图过滤器，Activity之间通过intent实现通信，intent-filter就是用来注册Activity,Service和Broadcast Receiver 使Android知道那个应用程序（或组件）能用来响应intent请求使其可以在一片数据上执行那个动作。为了注册一个应用程序组件为intent处理者，在其组件的manifest节点中添加一个intent-fillter标签。

1.动作测试（action）

一个intent对象只能指定一个action，而一条<intent-filter>元素至少应该包含一个<action>，否则任何Intent请求都不能和该<intent-filter>匹配；

一个intent对象的action必须和intent-filter中的某一个actiong匹配，才能通过；

如果intent对象不指定action且intent-filter的action列表不为空，则通过；

2.类别测试（category）

简单的说就是种类匹配Intent-Filter必须包含所有在解析的Intent中定义的种类。一个没有特定种类的Intent Filter只能与没有种类的Intent匹配,对于 IntentFilter中多余的<category>声明并不会导致匹配失败。有一个重要的点就是如果intent 是implicit intent（隐式意图），android默认给加上一个CATEGORY_DEFAULT，这样的话如果intent filter中没有android.intent.category.DEFAULT这个category的话，匹配测试就会失败，换句话说就是必须加上这个category。

3：数据测试（data）

data有两部分构成，一个是数据类型，另一个是URI。每个URI包括四个属性参数(scheme,host, port, path)，形如：scheme://host:port/path， 其中，用setData()设定的Inteat请求的URI数据类型和scheme必须与IntentFilter中所指定的一致。若IntentFilter中还指定了authority或path，它们也需要相匹配才会通过测试。 Intent filter和Intent相互配合，实现了Android系统四大组件之间的信使功能。

URL_Scheme就用到了data，将在后面阐述-[URL Scheme](#URL_Scheme)

## onConfigurationChanged

在一些特殊的情况中，你可能希望当一种或者多种配置改变时避免重新启动你的activity。你可以通过在manifest中设置android:configChanges属性来实现这点。
你可以在这里声明activity可以处理的任何配置改变，当这些配置改变时不会重新启动activity，而会调用activity的
onConfigurationChanged(Resources.Configuration)方法。如果改变的配置中包含了你所无法处理的配置（在android:configChanges并未声明），
你的activity仍然要被重新启动，而onConfigurationChanged(Resources.Configuration)将不会被调用。

其次：android:configChanges=""中可以用的值：keyboard|mcc|mnc|locale|touchscreen|keyboardHidden|navigation|orientation……
Configuration 类中包含了很多种信息，例如系统字体大小，orientation，输入设备类型等等.
比如:android:configChanges="orientation|keyboard|keyboardHidden"

## 数据保存与恢复

系统配置改变时，会用到Activity中数据保存与恢复（onSaveInstanceState和onRestoreInstanceState）

## URL_Scheme

scheme是一种页面内跳转协议，是一种非常好的实现机制，通过定义自己的scheme协议，可以非常方便跳转app中的各个页面；通过scheme协议，服务器可以定制化告诉App跳转那个页面，可以通过通知栏消息定制化跳转页面，可以通过H5页面跳转页面等。

一个完整的Scheme的协议格式由 scheme、userInfo、host、port、path、query和fragment 组成。结构如下：

scheme://userInfo@host:port/path?query#fragment

## startActivityForResult

Activity在finish的时候需要向上一个Activity传值的时候，我们就可以使用startActivityForResult。比如打开相册选取照片，拍照，关闭页面需要返回一个参数给打开我们的页面。

使用startActivityForResult(Intent intent, int requestCode)方法打开新的Activity，新Activity关闭前需要向前面的Activity返回数据需要使用系统提供的setResult(int resultCode, Intent data)，前面的Activity中通过onActivityResult(int requestCode, int resultCode, Intent data)方法接受回传值

## 源码分析

[Activity源码解析](/AndroidLearning/3/30026-Activity源码解析.md)







