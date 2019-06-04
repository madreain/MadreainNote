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



[activity 属性解释](https://developer.android.google.cn/guide/topics/manifest/activity-element.html)

###### meta-data

```
<meta-data android:name="string"
           android:resource="resource specification"
           android:value="string" />
```

[meta-data 属性解释](https://developer.android.google.cn/guide/topics/manifest/meta-data-element.html)

###### service

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

[service 属性解释](https://developer.android.google.cn/guide/topics/manifest/service-element.html)

###### receiver

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

[receiver 属性解释](https://developer.android.google.cn/guide/topics/manifest/receiver-element.html)

###### provider

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

[provider 属性解释](https://developer.android.google.cn/guide/topics/manifest/provider-element.html)
