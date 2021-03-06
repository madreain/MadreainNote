# Intent 与 IntentFilter 详解

---

> 在前面的章节我们介绍到了 Activity、Service、BroadCastReceiver，这三者的启动、数据的传递都用到了 Intent，足以可见 Intent 在 Andorid 的重要性。Intent 这个英语单词的本意是“目的、意向、意图”。Intent 是一种运行时绑定（runtime binding)机制，它能在程序运行的过程中连接两个不同的组件。通过 Intent，你的程序可以向 Android 表达某种请求或者意愿，Android 会根据意愿的内容选择适当的组件来响应。

## 目录

-[显示](#显示)

-[隐示](#隐示)

-[属性](#属性)

-[匹配规则](#匹配规则)

-[PendingIntent](#PendingIntent)

Intent 分为两种类型，分别为显示和隐示，下面将分别介绍

## 显示

显式 Intent，可以通过类名来找到相应的组件，在应用中用显式 Intent 去启动一个组件，通常是因为我们知道这个组件（Activity、Service）的名字。如下代码，我们知道具体的 Activity 的名字，要启动一个新的 Activity，下面就是用的显示 Intent。

```
Intent intent = new Intent(context，AActivity.class);
startActivity(intent);
```

## 隐示

隐式 Intent，不指定具体的组件，但是它会声明将要执行的操作，从而匹配到相应的组件。最简单的 Android 中调用系统拨号页面准备打电话的操作，就是隐式 Intent。

```
Intent intent = new Intent(Intent.ACTION_DIAL);
Uri data = Uri.parse("tel:" + "13888888888");
intent.setData(data);
startActivity(intent);
```

使用隐式 Intent 的时候，系统通过将 Intent 对象中的 IntentFilter 与组件在 AndroidManifest.xml 或者代码中动态声明的 IntentFilter 进行比较，从而找到要启动的相应组件。如果组件的 IntentFilter 与 Intent 中的 IntentFilter 正好匹配，系统就会启动该组件，并把 Intent 传递给它。如果有多个组件同时匹配到了，系统则会弹出一个选择框，让用户选择使用哪个应用去处理这个 Intent，比如有时候点击一个网页链接，会弹出多个应用，让用户选择用哪个浏览器去打开该链接，就是这种情况。隐式 Intent 启动固定 Activity。

AndroidManifest.xml 中注册

```
<activity
    android:name=".SecondActivity">
    <intent-filter>
        <action android:name="com.madreain.intent.MY_ACTION"/>
        <category android:name="android.intent.category.DEFAULT" />
    </intent-filter>
</activity>
```

隐式 Intent 启动 Activity 代码

```
Intent intent = new Intent();
//设置动作（实际action属性就是一个字符串标记而已）
intent.setAction("com.madreain.intent.MY_ACTION");
tartActivity(intent);

```

上面隐式 Intent 启动 Activity 提到了 action 属性，接下来来详细介绍一下 Intent 的相关属性

## 属性

**component(组件)**
目的组件的名称，这个只有显式 Intent 有，隐式 Intent 没有。例如：com.madreain.DemoActivity。该属性可以通过 setComonentName()、setClass()、setClassName()或者 Intent 的构造函数来设置。

**action（动作）**
用来表现意图的行动,这个可以用户自定义也可以使用系统中自带的 Action 值。例如：com.madreain.intent.MY_ACTION"该属性可以通过 setAction()方法或者 Intent 的构造函数来设置。

系统中常用的 Action 值：
ACTION_MAIN，标识 Activity 为一个程序的开始
ACTION_VIEW，当有一些信息需要展示出来
ACTION_SEND，发送邮件
Action_CALL，呼叫指定的电话号码
ACTION_DIAL，拨打电话
ACTION_EDIT，编辑某些文件
ALL_APPS，列出所有的应用
ACTION_ANSWER，处理呼入的电话

**category（类别）**
用来表现动作的类别，它是一个 ArraySet 类型的容器，所以可以向里面添加任意数量的补充信息，同时，Intent 没有设置这个属性不会影响解析组件信息。可以通过 addCategory()方法来设置该属性

常用的 Category 的值：

CATEGORY_LAUNCHER，应用启动的初始 Activity，这个 Activity 会被添加到系统启动 launcher 当中。
CATEGORY_BROWSABLE，设置 Category 为该值后，在网页上点击图片或链接时，系统会考虑将此目标 Activity 列入可选列表，供用户选择以打开图片或链接。
CATEGORY_APP_EMAIL，用来启动邮件应用程序

**data（数据）**
表示与动作要操纵的数据，它是待操作数据的引用 URI 或者数据 MIME 类型的 URI，它的值通常与 Intent 的 Action 有关联。实际应用打开指定网页

```
Intent intent = new Intent(Intent.ACTION_VIEW);
intent.setData(Uri.parse("http://www.baidu.com"));
startActivity(intent);
```

**type（数据类型）**
对于 data 范例的描写，当Intent不指定Data属性时，Type属性才会起作用，否则Android系统将会根据Data属性值来分析数据的类型，所以无需指定Type属性。data和type属性一般只需要一个，通过setData方法会把type属性设置为null，相反设置setType方法会把data设置为null，如果想要两个属性同时设置，要使用Intent.setDataAndType()方法

常用type类型：
intent.setType(“image/*”);//选择照片
intent.setType(“audio/*”); //选择音频
intent.setType(“video/*”); //选择视频 （mp4 3gp 是android支持的视频格式）
intent.setType(“video/*;image/*”);//同时选择视频和图片

**extras（扩展信息）**
扩展信息，以key-value键值对的形式来存储组件执行操作过程中需要的额外信息，可以调用putExtra()方法来设置该属性，这个方法接受两个参数，一个是key，一个是value。也可以通过实例化一个储存额外信息的Bundle对象，然后调用putExtras()方法将我们实例化的Bundle添加到Intent中。

**Flags（标志位）**
期望这个意图的运行模式，这个属性可以指示系统如何启动一个Activity，以及启动之后如何处理

补充：
Intent.createChooser()：可用于启动网页强制每一次唤起选择框
接受隐式Intent：我们也可以设置我们可以接受的文件的type，然后隐式Intent能够匹配到任意一个过滤器就能被启动了

## 匹配规则

上面介绍了相关属性，我们知道当我们发送一个隐式Intent后，系统会将它与设备中的每一个组件的过滤器进行匹配，匹配属性有Action、Category、Data三个，需要这三个属性都匹配成功才能唤起相应的组件。接下来分别介绍Action、Category、Data的匹配规则

######  Action匹配规则

一个过滤器可以不声明Action属性也可以声明多个Action属性。隐式Intent中的Action属性，与组件中的某一个过滤器的Action能够匹配（如果一个过滤器声明了多个Action属性，只需要匹配其中一个就行），这样就算匹配成功。如果过滤器没有声明Action属性，那么只有没有设置Action属性的隐式Intent才能匹配成功。

######  Category匹配规则

一个过滤器可以不声明Category属性也可以声明多个Category属性。隐式Intent中声明的Category必须全部能够与某一个过滤器中的Category匹配才算匹配成功。比如说一个Category属性设为CATEGORY_BROWSABLE的隐式Intent也可以通过上面的过滤器，也就是说，过滤器的Category属性内容必须是大于或者等于隐式Intent的Category属性时候，隐式Intent才能匹配成功。如果一个隐式Intent没有设置Category属性，那么它可以通过任何一个过滤器的Category匹配。

######  Data匹配规则

一个过滤器可以不声明Data属性也可以声明多个Data属性。每个Data属性都可以指定数据的URI结构和数据MIME类型。URI包括scheme、host、port 和path四个部分，host和port合起来也成authority（host:port）部分。

## PendingIntent

说到了Intent，我们就再来说说PendingIntent，PendingIntent是对Intent的一种封装。用于处理即将发生的事情。比如在通知Notification中用于跳转页面，但不是马上跳转。 

实例应用：Notification，SmsManager,AlarmManager等

