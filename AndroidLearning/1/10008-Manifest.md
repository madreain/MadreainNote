# Manifest 详解

---

> AndroidManifest 是应用清单（manifest 意思是货单），每个应用的根目录中都必须包含一个，并且文件名必须一模一样。这个文件中包含了 APP 的配置信息，系统需要根据里面的内容运行 APP 的代码，显示界面。AndroidManifest.xml 是 Android 应用程序的最重要的文件之一，

## 目录

-[相关属性介绍](#相关属性介绍)

## 相关属性介绍

介绍常用的 manifest、uses-permission、application、activity、meta-data、service、receiver、provider

###### manifest

```
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
          xmlns:tools="http://schemas.android.com/tools"
          package="string"
          android:sharedUserId="string"
          android:sharedUserLabel="string resource"
          android:versionCode="integer"
          android:versionName="string"
          android:installLocation=["auto" | "internalOnly" | "preferExternal"] >
    . . .
</manifest>
```

常用属性：

**xmlns:android**
定义 Android 命名空间。此属性应始终设置为“ http://schemas.android.com/apk/res/android”。

**xmlns:tools**
属性也是固定的，在布局 xml 中也会出现，主要用于 xml 中的错误处理、xml 预览（例如：tools:text="1111"，可生成预览、运行时就不存在）、资源压缩。

**package**
Android 应用程序的完整 Java 语言样式包名称。名称可能包含大写或小写字母（'A'到'Z'），数字和下划线（'\_'）。但是，单个包名称部分可能只以字母开头。
注： Google Play 禁止使用 com.example 和 com.android 命名空间。

**android:versionCode**
内部版本号。此数字仅用于确定一个版本是否比另一个版本更新，更高的数字表示更新的版本。这不是向用户显示的版本号; 该数字由 versionName 属性设置。
该值必须设置为整数，例如“100”。您可以根据需要定义它，只要每个连续版本具有更高的数字即可。例如，它可以是内部版本号。或者，您可以通过在低 16 位和高 16 位分别编码“x”和“y”将“xy”格式的版本号转换为整数。或者，每次发布新版本时，您只需将数字增加一即可。

**android:versionName**
显示给用户的版本号。此属性可以设置为原始字符串或字符串资源的引用。该字符串没有其他目的，只能显示给用户。该 versionCode 属性包含内部使用的重要版本号。

现在 android:versionCode、android:versionName 一般都在 build.gradle 中设置，对应的具体版本号和版本名称在 config.gradle 中设置

build.gradle 中

```
versionCode rootProject.ext.versionCode
versionName rootProject.ext.versionName
```

config.gradle 中

```
ext{
    versionCode = 1
    versionName = "1.0.0"
}
```

不常用属性：

**android:sharedUserId**
将与其他应用程序共享的 Linux 用户标识的名称。默认情况下，Android 会为每个应用分配自己唯一的用户 ID。但是，如果此属性设置为两个或多个应用程序的相同值，则它们将共享相同的 ID - 前提是它们的证书集相同。具有相同用户 ID 的应用程序可以访问彼此的数据，并且如果需要，可以在同一进程中运行。

**android:sharedUserLabel**
共享用户标识的用户可读标签。必须将标签设置为对字符串资源的引用; 它不能是原始字符串。
此属性是在 API 级别 3 中引入的。仅在 sharedUserId 设置了属性时才有意义 。

**android:installLocation**
该应用的默认安装位置。
“ internalOnly” 该应用必须仅安装在内部设备存储上。如果设置了此选项，则永远不会在外部存储上安装该应用程序。如果内部存储空间已满，则系统将不会安装该应用程序。如果您没有定义，这也是默认行为 android:installLocation。
“ auto” 该应用程序可能安装在外部存储上，但系统默认会在内部存储上安装该应用程序。如果内部存储已满，则系统会将其安装在外部存储上。安装后，用户可以通过系统设置将应用程序移动到内部或外部存储。
“ preferExternal” 该应用程序更喜欢安装在外部存储（SD 卡）上。无法保证系统会遵守此请求。如果外部媒体不可用或已满，则应用程序可能会安装在内部存储上。安装后，用户可以通过系统设置将应用程序移动到内部或外部存储。

###### uses-permission

请求应用正确运行所必须获取的权限

```
<uses-permission android:name="string"
        android:maxSdkVersion="integer" />
```

**android:name**
权限的名称。可以是应用通过 <permission> 元素定义的权限、另一个应用定义的权限，或者一个标准系统权限（例如 "android.permission.CAMERA" 或 "android.permission.READ_CONTACTS"）。如这些示例所示，权限名称通常以软件包名称为前缀。

附：常用权限

| 功能         |                     权限                     |                                                  注释 |
| ------------ | :------------------------------------------: | ----------------------------------------------------: |
| 访问网络     |         android.permission.INTERNET          |                                          访问网络连接 |
| 网络状态     |   android.permission.ACCESS_NETWORK_STATE    |                    允许程序访问网络状态，如是否能联网 |
| 程序缓存     | android.permission.MOUNT_UNMOUNT_FILESYSTEMS |                  允许挂载和反挂载可移动存储文件系统， |
| SD 卡写操作  |  android.permission.WRITE_EXTERNAL_STORAGE   |                允许程序写入外部存储，如 SD 卡上写文件 |
| 拨打电话     |        android.permission.CALL_PHONE         |    允许一个程序初始化一个电话拨号不需通过拨号用户界面 |
| 读取联系人   |       android.permission.READ_CONTACTS       |                          允许应用访问联系人通讯录信息 |
| 处理拨出电话 |  android.permission.PROCESS_OUTGOING_CALLS   |                        允许程序监视、修改有关播出电话 |
| 发送短信     |         android.permission.SEND_SMS          |                                              发送短信 |
| 接收短信     |        android.permission.RECEIVE_SMS        |              允许程序监控一个将收到短信息，记录或处理 |
| 读取短信内容 |         android.permission.READ_SMS          |                                    允许程序读取短信息 |
| 粗略位置     |  android.permission.ACCESS_COARSE_LOCATION   |   通过 WiFi 或基站获取粗略经纬度，精度误差:30~1500 米 |
| 精确位置     |   android.permission.ACCESS_FINE_LOCATION    |        通过 GPS 芯片接收卫星的定位信息,精度达 10 米内 |
| wifi 状态    |     android.permission.ACCESS_WIFI_STATE     |          获取当前 WiFi 接入的状态以及 WLAN 热点的信息 |
| 打开摄像头   |          android.permission.CAMERA           |                                允许访问摄像头进行拍照 |
| 闪光灯       |        android.permission.FLASHLIGHT         |                                            控制闪光灯 |
| 电量统计     |       android.permission.BATTERY_STATS       |                                  获取电池电量统计信息 |
| 开机启动     |  android.permission.RECEIVE_BOOT_COMPLETED   |                                  允许程序开机允许服务 |
| 使用蓝牙     |         android.permission.BLUETOOTH         |                          允许程序连接配对过的蓝牙设备 |
| 管理蓝牙     |      android.permission.BLUETOOTH_ADMIN      |                    允许程序进行发现和配对新的蓝牙设备 |
| 闹钟         |         android.permission.SET_ALARM         |                                          设置闹铃提醒 |
| 应用大小     |     android.permission.GET_PACKAGE_SIZE      |                                    获取应用的文件大小 |
| 声音设置     |   android.permission.MODIFY_AUDIO_SETTINGS   |                                      修改声音设置信息 |
| 录音         |       android.permission.RECORD_AUDIO        |                          录制声音通过手机或耳机的麦克 |
| 振动         |          android.permission.VIBRATE          |                                              手机振动 |
| 桌面壁纸     |       android.permission.SET_WALLPAPER       |                                          设置桌面壁纸 |
| 系统设置     |      android.permission.WRITE_SETTINGS       |                                    允许读写系统设置项 |

**android:maxSdkVersion**
此权限应授予应用的最高 API 级别。如果应用需要的权限从某个 API 级别开始不再需要，则设置此属性很有用。

###### uses-feature

声明应用使用的单一硬件或软件功能。

```
<uses-feature
  android:name="string"
  android:required=["true" | "false"]
  android:glEsVersion="integer" />
```

**android:name**
以描述符字符串形式指定应用使用的单一硬件或软件功能。

[硬件功能和软件功能属性值参考](https://developer.android.google.cn/guide/topics/manifest/uses-feature-element.html)

**android:required**
表示应用是否需要 android:name 中所指定功能的布尔值。
当您为某项功能声明 android:required="true" 时，即是规定当设备不具有该指定功能时，应用无法正常工作，或设计为无法正常工作。
当您为某项功能声明 android:required="false" 时，则意味着如果设备具有该功能，应用会在必要时优先使用该功能，但应用设计为不使用该指定功能也可正常工作。
如果未予声明，android:required 的默认值为 "true"。

**android:glEsVersion**
应用需要的 OpenGL ES 版本。高 16 位表示主版本号，低 16 位表示次版本号。 例如，要指定 OpenGL ES 2.0 版，您需要将其值设置为“0x00020000”；要指定 OpenGL ES 3.2，则需将其值设置为“0x00030002”。
应用应在其清单中至多指定一个 android:glEsVersion。 如果指定不止一个，将使用数值最高的 android:glEsVersion，任何其他值都会被忽略。

如果应用不指定 android:glEsVersion 属性，则系统假定应用只需要 OpenGL ES 1.0，即所有 Android 设备都支持的版本。

应用可以假定，如果平台支持给定 OpenGL ES 版本，也同样支持所有数值更低的 OpenGL ES 版本。 因此，同时需要 OpenGL ES 1.0 和 OpenGL ES 2.0 的应用必须指定它需要 OpenGL ES 2.0。

能够使用几种 OpenGL ES 版本中任一版本的应用只应指定它需要的数值最低的 OpenGL ES 版本。 （它可以在运行时检查是否有更高版本的 OpenGL ES 可用）。

###### application

申请应用的声明。此元素包含子元素，这些子元素声明每个应用程序的组件，并具有可影响所有组件的属性

```
<application android:allowTaskReparenting=["true" | "false"]
             android:allowBackup=["true" | "false"]
             android:allowClearUserData=["true" | "false"]
             android:backupAgent="string"
             android:backupInForeground=["true" | "false"]
             android:banner="drawable resource"
             android:debuggable=["true" | "false"]
             android:description="string resource"
             android:directBootAware=["true" | "false"]
             android:enabled=["true" | "false"]
             android:extractNativeLibs=["true" | "false"]
             android:fullBackupContent="string"
             android:fullBackupOnly=["true" | "false"]
             android:hasCode=["true" | "false"]
             android:hardwareAccelerated=["true" | "false"]
             android:icon="drawable resource"
             android:isGame=["true" | "false"]
             android:killAfterRestore=["true" | "false"]
             android:largeHeap=["true" | "false"]
             android:label="string resource"
             android:logo="drawable resource"
             android:manageSpaceActivity="string"
             android:name="string"
             android:networkSecurityConfig="xml resource"
             android:permission="string"
             android:persistent=["true" | "false"]
             android:process="string"
             android:restoreAnyVersion=["true" | "false"]
             android:requiredAccountType="string"
             android:resizeableActivity=["true" | "false"]
             android:restrictedAccountType="string"
             android:supportsRtl=["true" | "false"]
             android:taskAffinity="string"
             android:testOnly=["true" | "false"]
             android:theme="resource or theme"
             android:uiOptions=["none" | "splitActionBarWhenNarrow"]
             android:usesCleartextTraffic=["true" | "false"]
             android:vmSafeMode=["true" | "false"] >
    . . .
</application>
```

常用属性

**android:allowClearUserData**
是否给以用户删除用户数据的权限.如果为 true 应用管理者就拥有清除数据的权限；false 没有。默认为 true。

**android:allowTaskReparenting**
应用定义的 activities 是否可以被从启动的任务转移到和他有相同并且将被带到前台的任务。true 他们可以被转移，如果为 false，他们必须和启动他们的任务保持在一起。默认为 false。

**android:backupAgent**
实现应用的备份代理的类名，BackupAgent 的子类。这个属性的名称因该是全限定类名(如，”com.madreain.project.MyBackupAgent”)。但是，如果名称的首字母被设置为点号，也可以为类名(如，”.MyBackupAgent”)，他将被追加到在<manifest/>元素中定义的包名后。没有默认值。

**android:debuggable**
应用是否可以使用 debug，甚至运行在用户模式下。true 可以，false 不能。默认为 false。

**android:description**
用户可读的，比应用标签更长、更多的应用描述。此值必须是一个引用字符串。不像标签，他不能被设置为硬编码字符串。没有默认值。

**android:enabled**
Android 系统是否可以实例化应用的组件。如果为 true 可以，如果为 false 不可以。如果为 true，每个组件的 enabled 属性决定了此组件。如果为 false，他重写了组件指定值，所有的组件将不还用。默认为 true。

**android:hasCode**
应用是否包含代码。true 表示包含，false 表示不包含。当值为 false 时，在启动组件是系统不会试着加载应用的任何代码。默认为 true。

**android:icon**
整个应用的图标，还是每个组件的默认图标。这个属性值 必须 被设置为 drawable 资源的引用。没有默认值。

**android:killAfterRestore**
在整型系统重置操作中，当他的设置被重置后，应用是否应该被终止。单个包的重置操作不会引起应用被关闭。整个系统的恢复操作仅代表性的发生一次，当电话第一次被设置时。第三方应用将不会经常使用此属性。默认值为 true，意思是，当整个系统被恢复时，应用运行完他的数据后，将会终止。

**android:label**
一个易读的应用标签，并且还是应用的每个组件的默认标签。这个标签应该被设置为引用字符串资源，当然他也可以像其他字符串一样在用户接口中指定。但是为了方便，在应用开发时，可以被设置未定义字符串。

**android:name**
为这个应用实现的 Application 子类的全限定名称。当应用启动时，这个类将在应用的其他组件之前被实例化。这个子类是可选的；大多数应用不需要。在缺省时，Android 使用基本 Application 类的实例。

**android:permission**
客户为了和应用交互必须设置的许可的名称。这个属性是一个便利的途径为应用的组件设置许可。他可以被组件的 permission 属性重写。

**android:persistent**
应用是否在所有时间下都保持运行。true 是，false 不是。默认为 false。通常情况下应用不应该设置此标识。持久模式仅仅被几个系统应用指定。

**android:process**
为应用下的组件定一个运行进程名称。每个组件可以定义自己的进程名称通过设置自己的 process 属性。在默认情况下，Android 为应用创建一个进程，当应用的第一个组件需要运行时。所有的组件在同一个进程下运行。这个进程的名称和在<manifest/>元素设置的 backage 属性名相同。通过设置这个属性在可以在其他应用中共享，你可以协调应用的组件在同一个进程中运行，但是只有两应用也共享用户 ID 和签订相同的证书。如果这个属性的名称一个冒号(“:”)开始，一个新的私有的进程将被创建。如果一个进程的名称以小写字母开头，一个公共的进程将被创建。一个公共的进程可以被其他应用共享，来减少资源的使用。

**android:restoreAnyVersion**
表明这个应用准备尝试恢复所有的备份数据集合，甚至如果备份数据是比当前安装的应用高的编号存储的。设置为 true 将允许备份管理者去尝试恢复当版本不匹配，意思是数据冲突。要小心使用。默认为 false。

**android:taskAffinity**
提供给应用下所有组件的类同名称，除了设置了自己的 taskAffinity 属性的组件。默认情况下所有的组件使用相同的 affinity。Affinity 的名称和在<manifest/>元素中设置的包名相同。

**android:theme**
为应用下的组件定义一个引用自样式资源的主题。个别的 activities 可以设置自己的主题，通过设置自己的 theme 属性。

**android:allowBackup**
它表示是否允许应用程序参与备份。如果将该属性设置为 false,则即使备份整个系统,也不会执行这个应用程序的备份操作。而整个系统备份能导致所有应用程序数据通过 ADB 来保存。该属性必须是一个布尔值，或为 true，或为 false。默认值为 true。

**android:largeHeap**
应用程序是否使用一个比较大的堆创建。它是一个布尔值，在没有配置的情况下，它的默认值是 false。

**android:manageSpaceActivity**
一个 Activity 子类的全限定名称，这个 Activity 可以被系统启动让用户管理此应用占有的存储空间。这个 Activity 也应该用<activity/>元素声明。

非常用属性可查看[application 属性解释](https://developer.android.google.cn/guide/topics/manifest/application-element.html)

###### activity

应用组件的申明，申明后系统才可以访问

```
<activity android:allowEmbedded=["true" | "false"]
          android:allowTaskReparenting=["true" | "false"]
          android:alwaysRetainTaskState=["true" | "false"]
          android:autoRemoveFromRecents=["true" | "false"]
          android:banner="drawable resource"
          android:clearTaskOnLaunch=["true" | "false"]
          android:configChanges=["mcc", "mnc", "locale",
                                 "touchscreen", "keyboard", "keyboardHidden",
                                 "navigation", "screenLayout", "fontScale",
                                 "uiMode", "orientation", "screenSize",
                                 "smallestScreenSize"]
          android:documentLaunchMode=["intoExisting" | "always" |
                                  "none" | "never"]
          android:enabled=["true" | "false"]
          android:excludeFromRecents=["true" | "false"]
          android:exported=["true" | "false"]
          android:finishOnTaskLaunch=["true" | "false"]
          android:hardwareAccelerated=["true" | "false"]
          android:icon="drawable resource"
          android:label="string resource"
          android:launchMode=["standard" | "singleTop" |
                              "singleTask" | "singleInstance"]
          android:maxRecents="integer"
          android:multiprocess=["true" | "false"]
          android:name="string"
          android:noHistory=["true" | "false"]
          android:parentActivityName="string"
          android:permission="string"
          android:process="string"
          android:relinquishTaskIdentity=["true" | "false"]
          android:resizeableActivity=["true" | "false"]
          android:screenOrientation=["unspecified" | "behind" |
                                     "landscape" | "portrait" |
                                     "reverseLandscape" | "reversePortrait" |
                                     "sensorLandscape" | "sensorPortrait" |
                                     "userLandscape" | "userPortrait" |
                                     "sensor" | "fullSensor" | "nosensor" |
                                     "user" | "fullUser" | "locked"]
          android:stateNotNeeded=["true" | "false"]
          android:supportsPictureInPicture=["true" | "false"]
          android:taskAffinity="string"
          android:theme="resource or theme"
          android:uiOptions=["none" | "splitActionBarWhenNarrow"]
          android:windowSoftInputMode=["stateUnspecified",
                                       "stateUnchanged", "stateHidden",
                                       "stateAlwaysHidden", "stateVisible",
                                       "stateAlwaysVisible", "adjustUnspecified",
                                       "adjustResize", "adjustPan"] >
    . . .
</activity>

```

**android:name**
组建名，Activity 源文件所在的相对路径

**android:screenOrientation**
Activity 在屏幕当中显示的方向

**android:configChanges**
Activity 捕捉设备状态变化。当所指定属性(Configuration Changes)发生改变时，回调 onConfigurationChanged()函数。1. 设备旋转(orientation),2. 键盘隐藏(keyboardHidden), 3. 屏幕尺寸(screenSize)

**android:exported**
表示该组件是否能够被其它应用程序组件调用或者交互。该属性默认值依赖于它所包含得过滤器(intent-filter)。如果没有 intent-filter，则表明该组件只能通过显示 Intent 调用，所以默认值为 false。如果含有 intent-filter,则表明该组件能通过隐式 Intent 调用，所以默认值为 true。

**android:launchMode**
activity 启动模式。四种：Standard,SingleTop,SingleTask,SingleInstance

**android:taskAffinity**
Activity 所吸附的任务栈，该属性一般在 lauchMode 为 singleTask 模式才生效。默认 taskAffinity 于根 Activity 相同，即应用的包名。

**android:windowSoftInputMode**
键盘弹出后，界面调整模式 1. 当获取焦点时是否弹出键盘 2. 是否减少 Activity 主窗口大小以腾出空间给软键盘

**android:allowTaskReparenting**
当某个拥有相同 affinity 任务栈即将返回前台时，当前 Activity 是否能从启动它得任务栈中转移到与它相同 Affinity 相同任务栈中去。默认值，false，只能停留在启动它得任务栈。 利用该值的特性，可以强行让一个不再显示的 Activity 转移到另外的任务栈中。典型的应用，就是让一个程序的 Activity 转移到另外的应用程序中。

**android:alwaysRetainTaskState**
系统是否一直维持任务栈的状态。该属性只对任务栈根 Activity 有效，默认值 false。通常，用户从 launch 界面重新打开应用的时候，系统会清空任务栈中根 Activity 以上所有 Activity，比如说，用户停留在主界面很长时间没有操作了。当将该值设置为 true 的时候，总是返回任务的最后状态，比如说，浏览器应用打开了多个页面，用户不期望下次打开浏览器的时候，之前的页面全部清空了。

**android:clearTaskOnLaunch**
每次从 launch 启动应用时，是否清楚根 Activity 以上所有的 Activity，默认值 false

**android:enabled**
该组件是否能够被实例化。默认 true

**android:excludeFromRecents**
该 Activity 是否排除在用户最近任务列表。默认值 false。

**android:finishOnTaskLaunch**
当从 launch 再次启动任务时，是否 finish 该实例，默认值 false。

**android:multiprocess**
是否可以把 Activity 新实例放入到启动它的组件所在的进程中。

其他属性可查看[activity 属性解释](https://developer.android.google.cn/guide/topics/manifest/activity-element.html)

###### meta-data

可以提供给父组件的其他任意数据项的名称 - 值对。组件元素可以包含任意数量的<meta-data>子元素。所有这些值都收集在一个 Bundle 对象中，并作为 PackageItemInfo.metaData 字段提供给组件 。

```
<meta-data android:name="string"
           android:resource="resource specification"
           android:value="string" />
```

**android:name**
元数据项的名字，为了保证这个名字是唯一的，采用 java 风格的命名规范，如 com.woody.project.fried

**android:resource**
资源的一个引用，指定给这个项的值是该资源的 id。该 id 可以通过方法 Bundle.getInt()来从 meta-data 中找到。

**android:value**
指定给这一项的值。可以作为值来指定的数据类型并且组件用来找回那些值的 Bundle 方法：[getString],[getInt],[getFloat],[getString],[getBoolean]

###### service

声明服务（Service 子类）作为应用程序的组件之一。与活动不同，服务缺乏可视化用户界面。它们用于实现长时间运行的后台操作或可由其他应用程序调用的丰富通信 API。
所有服务必须由<service>清单文件中的元素表示。任何未在此处声明的内容都不会被系统看到并且永远不会被运行。

```
<service android:description="string resource"
         android:directBootAware=["true" | "false"]
         android:enabled=["true" | "false"]
         android:exported=["true" | "false"]
         android:icon="drawable resource"
         android:isolatedProcess=["true" | "false"]
         android:label="string resource"
         android:name="string"
         android:permission="string"
         android:process="string" >
    . . .
</service>
```

常用属性：

**android:exported**
代表是否能被其他应用隐式调用，其默认值是由 service 中有无 intent-filter 决定的，如果有 intent-filter，默认值为 true，否则为 false。为 false 的情况下，即使有 intent-filter 匹配，也无法打开，即无法被其他应用隐式调用。

**android:name**
对应 Service 类名

**android:permission**
是权限声明

**android:process**
是否需要在单独的进程中运行,当设置为 android:process=”:remote”时，代表 Service 在单独的进程中运行。注意“：”很重要，它的意思是指要在当前进程名称前面附加上当前的包名，所以“remote”和”:remote”不是同一个意思，前者的进程名称为：remote，而后者的进程名称为：App-packageName:remote。

**android:isolatedProcess**
设置 true 意味着，服务会在一个特殊的进程下运行，这个进程与系统其他进程分开且没有自己的权限。与其通信的唯一途径是通过服务的 API(bind and start)。

**android:enabled**
是否可以被系统实例化，默认为 true 因为父标签 也有 enable 属性，所以必须两个都为默认值 true 的情况下服务才会被激活，否则不会激活。

###### receiver

声明广播接收器（BroadcastReceiver 子类）作为应用程序的组件之一。广播接收器使应用程序能够接收由系统或其他应用程序广播的意图，即使应用程序的其他组件未运行也是如此。

```
<receiver android:directBootAware=["true" | "false"]
          android:enabled=["true" | "false"]
          android:exported=["true" | "false"]
          android:icon="drawable resource"
          android:label="string resource"
          android:name="string"
          android:permission="string"
          android:process="string" >
    . . .
</receiver>
```

**android:directBootAware**
广播接收器是否可直接启动 ; 也就是说，它是否可以在用户解锁设备之前运行。默认值为"false"。

**android:enabled**
广播接收机是否可以由系统实例化 - “ true”如果它可以，并且“ false”如果不是。默认值为“ true”。
该<application>元素具有自己的 enabled 属性，适用于所有应用程序组件，包括广播接收器。在 <application>和 <receiver>属性都必须是“ true”为启用广播接收机。如果其中一个是“ false”，则禁用; 它无法实例化。

**android:exported**
广播接收机是否可以从其应用程序之外的来源接收消息 - “ true”如果可以，则“和 false”如果不能。如果是“ false”，则广播接收器可以接收的唯一消息是由相同应用程序的组件或具有相同用户 ID 的应用程序发送的消息。

**android:icon**
表示广播接收器的图标。必须将此属性设置为对包含图像定义的可绘制资源的引用。

**android:label**
广播接收器的用户可读标签。如果未设置此属性，则使用整个应用程序的标签集

标签应设置为对字符串资源的引用，以便它可以像用户界面中的其他字符串一样进行本地化。但是，为了方便您开发应用程序，它也可以设置为原始字符串。

**android:name**
实现广播接收器的类的名称，是其子类 BroadcastReceiver。这应该是一个完全限定的类名（例如“ com.example.project.ReportReceiver”）。但是，作为简写，如果名称的第一个字符是句点（例如，“ . ReportReceiver”），则它将附加到<manifest>元素中指定的包名称。
发布应用程序后，不应更改此名称（除非您已设置 android:exported="false"）。

**android:permission**
广播公司必须具有向广播接收器发送消息的权限的名称。如果未设置此属性，则<application>元素 permission 属性设置的权限 适用于广播接收器。如果两个属性均未设置，则接收方不受权限保护。

**android:process**
广播接收器应运行的进程的名称。通常，应用程序的所有组件都在为应用程序创建的默认进程中运行。它与应用程序包具有相同的名称。该 <application>元素的 process 属性可以为所有的组件不同的默认。但是每个组件都可以使用自己的 process 属性覆盖默认值，从而允许您跨多个进程分布应用程序。

###### provider

声明内容提供程序组件。内容提供者是其子类 ContentProvider，提供对应用程序管理的数据的结构化访问。必须<provider>在清单文件的元素中定义应用程序中的所有内容提供者 ; 否则，系统不知道它们并且不运行它们。

```
<provider android:authorities="list"
          android:directBootAware=["true" | "false"]
          android:enabled=["true" | "false"]
          android:exported=["true" | "false"]
          android:grantUriPermissions=["true" | "false"]
          android:icon="drawable resource"
          android:initOrder="integer"
          android:label="string resource"
          android:multiprocess=["true" | "false"]
          android:name="string"
          android:permission="string"
          android:process="string"
          android:readPermission="string"
          android:syncable=["true" | "false"]
          android:writePermission="string" >
    . . .
</provider>

```

**android:authorities**
一个或多个URI权限的列表，用于标识内容提供商提供的数据。通过用分号分隔它们的名称来列出多个权限。为避免冲突，权限名称应使用Java样式的命名约定（例如com.example.provider.cartoonprovider）。通常，它ContentProvider是实现提供程序的子类 的名称。没有默认值。必须至少指定一个权限。

**android:enabled**
内容提供者是否可以由系统实例化 - “ true”如果可以，以及“ false”如果不是。默认值为“ true”。
该<application>元素具有自己的 enabled属性，适用于所有应用程序组件，包括内容提供程序。在 <application>和<provider> 属性都必须是“true”（因为它们都是由默认值）启用内容提供商。如果其中一个是“ false”，则提供者被禁用; 它无法实例化。

**android:directBootAware**
内容提供者是否可以直接启动 ; 也就是说，它是否可以在用户解锁设备之前运行。默认值为"false"。

**android:exported**
内容提供商是否可供其他应用程序使用：
true：提供程序可用于其他应用程序。任何应用程序都可以使用提供程序的内容URI来访问它，具体取决于为提供程序指定的权限。
false：提供程序不可用于其他应用程序。设置 android:exported="false"为限制对应用程序的提供程序的访问。只有与提供者具有相同用户ID（UID）的应用程序才能访问它。

**android:grantUriPermissions**
无论那些谁通常不会有权限访问内容提供商的数据可以被获准这样做，暂时克服强加的限制 readPermission， writePermission以及 permission属性- “ true”如果权限可以授予，而“ false”如果没有。如果是“ true”，则可以向任何内容提供商的数据授予权限。如果为“ false”，则只能授予子<grant-uri-permission>元素中列出的数据子集的权限 （如果有）。默认值为“ false”。

**android:icon**
表示内容提供商的图标。必须将此属性设置为对包含图像定义的可绘制资源的引用。如果未设置，则使用为整个应用程序指定的图标

**android:initOrder**
相对于由同一进程托管的其他内容提供程序，应实例化内容提供程序的顺序。当内容提供者之间存在依赖关系时，为每个提供者设置此属性可确保按照这些依赖关系所需的顺序创建它们。该值是一个简单的整数，首先初始化更高的数字。

**android:label**
提供的内容的用户可读标签。如果未设置此属性，则使用整个应用程序的标签集
标签应设置为对字符串资源的引用，以便它可以像用户界面中的其他字符串一样进行本地化。但是，为了方便您开发应用程序，它也可以设置为原始字符串。

**android:multiprocess**
如果应用程序在多个进程中运行，则此属性确定是否创建了内容提供程序的多个实例。如果true，每个应用程序的进程都有自己的内容提供程序对象。如果 false，应用程序的进程只共享一个内容提供程序对象。默认值为false。
将此标志设置为true可以通过减少进程间通信的开销来提高性能，但它也会增加每个进程的内存占用量。

**android:name**
实现内容提供程序的类的名称，是其子类 ContentProvider。这应该是一个完全限定的类名（例如“ com.example.project.TransportationProvider”）。但是，作为简写，如果名称的第一个字符是句点，则将其附加到<manifest>元素中指定的包名称 。没有默认值。必须指定名称。

**android:permission**
客户端必须具有读取或写入内容提供者数据的权限的名称。此属性是为读取和写入设置单个权限的便捷方式。但是， readPermission和 writePermission属性优先于此属性。如果readPermission 还设置了该属性，则它控制查询内容提供程序的访问权限。如果writePermission设置了该属性，它将控制用于修改提供者数据的访问权限。

**android:process**
内容提供程序应运行的进程的名称。通常，应用程序的所有组件都在为应用程序创建的默认进程中运行。它与应用程序包具有相同的名称。该 <application>元素的 process 属性可以为所有的组件不同的默认。但是每个组件都可以使用自己的process属性覆盖默认值，从而允许您跨多个进程分布应用程序。
如果分配给此属性的名称以冒号（'：'）开头，则在需要时创建一个专用于应用程序的新进程，并在该进程中运行活动。如果进程名称以小写字符开头，则活动将在该名称的全局进程中运行，前提是它具有执行此操作的权限。这允许不同应用程序中的组件共享进程，从而减少资源使用。

**android:readPermission**
客户端必须具有查询内容提供者的权限。

**android:syncable**
内容提供者控制下的数据是否与服务器上的数据同步 - “ true”如果要同步，“ false”如果不同步。

**android:writePermission**
客户端必须具有的权限才能更改内容提供程序控制的数据。


