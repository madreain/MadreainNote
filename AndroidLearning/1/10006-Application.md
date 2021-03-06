# Application 详解

---

> 在 Android 中使用全局变量，除 public 静态变量，还有就是使用 android.app.Application。Android 系统 会为每个程序运行时创建一个 Application 类的对象且仅创建一个，所以 Application 可以说是单例 (singleton)模式的一个类所以在不同的 Activity、Service 中获得的对象都是同一个对象，所以通过 Application 来进行一些，数据传递，数据共享等，数据缓存等操作。在启动 Application 时，系统会创建一个 PID,就是 进程 ID，所有的 Activity 会在此进程上运行，在创建 Application 时初始化全局变量，同一个应用的所有 Activity 都会获取这些全局变量的值，可用于保存登录状态。Application 中的全局变量值会在 Activity 中被改变，其生命周期等于整个程序的生命周期

## 目录

-[生命周期](#生命周期)

-[Activity 的生命周期监听](#Activity的生命周期监听)

## 生命周期

介绍一下 Application 的生命周期，以及在这些生命周期方法中可以做那些操作

**onCreate**
程序创建的时候执行

第三方库的初始化（网络请求、推送、地图、分享等）、MultiDex（分包配置）、全局对象、环境配置变量、全局数据共享存储，不能做耗时操作，要不然影响 App 的启动速度

**onTerminate**
程序终止的时候执行

**onConfigurationChanged**
配置改变时触发这个方法。

屏幕旋转

**onLowMemory**
低内存的时候执行

照片资源（GlideApp 的使用）缓冲的清除

**onTrimMemory**
程序在进行内存清理时执行

照片资源（GlideApp 的使用）、后台服务，可根据不同的 level 来决定是否清除缓冲

补充：
registerComponentCallbacks，ComponentCallbacks2回调接口，里面会重写onTrimMemory、onLowMemory、onConfigurationChanged。
ComponentCallbacks回掉接口里面少一个onTrimMemory函数


## Activity 的生命周期监听

registerActivityLifecycleCallbacks 和 unregisterActivityLifecycleCallbacks 函数，registerActivityLifecycleCallbacks对应用程序内所有 Activity 的生命周期监听，当应用程序内 Activity生命周期发生变化时就会调用，ActivityLifecycleCallbacks接口里面有对应的方法返回相对应Activity的生命周期状态。unregisterActivityLifecycleCallbacks注销

