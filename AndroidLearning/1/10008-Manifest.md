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

功能|权限|注释
---|:--:|---:
访问网络| android.permission.INTERNET  |访问网络连接
网络状态 | android.permission.ACCESS_NETWORK_STATE | 允许程序访问网络状态，如是否能联网
程序缓存 | android.permission.MOUNT_UNMOUNT_FILESYSTEMS | 允许挂载和反挂载可移动存储文件系统，
SD卡写操作 | android.permission.WRITE_EXTERNAL_STORAGE | 允许程序写入外部存储，如SD卡上写文件
拨打电话 | android.permission.CALL_PHONE | 允许一个程序初始化一个电话拨号不需通过拨号用户界面
读取联系人 | android.permission.READ_CONTACTS | 允许应用访问联系人通讯录信息
处理拨出电话 | android.permission.PROCESS_OUTGOING_CALLS | 允许程序监视、修改有关播出电话
发送短信 | android.permission.SEND_SMS | 发送短信
接收短信 | android.permission.RECEIVE_SMS | 允许程序监控一个将收到短信息，记录或处理
读取短信内容 | android.permission.READ_SMS | 允许程序读取短信息
粗略位置 | android.permission.ACCESS_COARSE_LOCATION |  通过WiFi或基站获取粗略经纬度，精度误差:30~1500米
精确位置 | android.permission.ACCESS_FINE_LOCATION |  通过GPS芯片接收卫星的定位信息,精度达10米内
wifi状态 | android.permission.ACCESS_WIFI_STATE |  获取当前WiFi接入的状态以及WLAN热点的信息
打开摄像头 | android.permission.CAMERA |  允许访问摄像头进行拍照
闪光灯 | android.permission.FLASHLIGHT |  控制闪光灯
电量统计 | android.permission.BATTERY_STATS |  获取电池电量统计信息
开机启动 | android.permission.RECEIVE_BOOT_COMPLETED |  允许程序开机允许服务
使用蓝牙 | android.permission.BLUETOOTH | 允许程序连接配对过的蓝牙设备
管理蓝牙 | android.permission.BLUETOOTH_ADMIN | 允许程序进行发现和配对新的蓝牙设备
闹钟 | android.permission.SET_ALARM | 设置闹铃提醒
应用大小 | android.permission.GET_PACKAGE_SIZE | 获取应用的文件大小
声音设置 | android.permission.MODIFY_AUDIO_SETTINGS | 修改声音设置信息
录音 | android.permission.RECORD_AUDIO | 录制声音通过手机或耳机的麦克
振动 | android.permission.VIBRATE | 手机振动
桌面壁纸 | android.permission.SET_WALLPAPER | 设置桌面壁纸
系统设置 | android.permission.WRITE_SETTINGS | 允许读写系统设置项

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

硬件功能属性值


软件功能属性值



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

[application 属性解释](https://developer.android.google.cn/guide/topics/manifest/application-element.html)

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
