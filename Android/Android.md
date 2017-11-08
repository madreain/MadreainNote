# AndroidNote

Android development notes

[English](Android.md) | [中文](Android-zh.md)

## Contents

-[Framework](#framework)

-[Network Request](#network-request)

-[Data Analysis](#data-analysis)

-[Database](#database)

-[Database Util](#database-util)

-[Code Template](#code-template)

-[Asynchronous](#asynchronous)

-[Indicator](#indicator)

-[Adaptor](#adaptor)

-[Refresh the controls](#refresh-the-controls)

-[Photo](#photo)

-[Messaging](#messaging)

-[Permissions](#permissions)

-[Chart](#chart)

-[UI Framework](#ui-ramework)

-[Audio and video](#audio-and-video)

-[Qr code](#qr-code)

-[Memory](#memory)

-[Animation](#animation)

-[Dialog](#dialog)

-[utils](#Utils)

-[Hot update](#hot-update)

-[Custom view look-up table](#custom-view-look-up-table)

-[Custom view advanced reference information](#custom-view-advanced-reference-information)

-[Auxiliary tool](#auxiliary-tool)

-[Android advanced approach](#android-advanced-approach)

## Framework

* [Android-CleanArchitecture](https://github.com/android10/Android-CleanArchitecture)

* [android-architecture](https://github.com/googlesamples/android-architecture/tree/master)

* [iosched](https://github.com/google/iosched)

## Network Request

* [Retrofit](https://github.com/square/retrofit)

* [okHttp](https://github.com/square/okhttp)

## Data Analysis

* [gson](https://github.com/google/gson)

* [fastjson](https://github.com/alibaba/fastjson)

## Database

* [realm](https://github.com/realm/realm-java)

* [ormlite-android](https://github.com/j256/ormlite-android)

* [greenDAO](https://github.com/greenrobot/greenDAO)

## Database Util

* [stetho](http://facebook.github.io/stetho/)

## Code Template

* [butterknife](https://github.com/JakeWharton/butterknife)

* [androidannotations](https://github.com/androidannotations/androidannotations)

## Asynchronous

* [RxAndroid](https://github.com/ReactiveX/RxAndroid)

## Indicator

* [ViewPagerIndicator](https://github.com/JakeWharton/ViewPagerIndicator)

## Adaptor

* [BaseRecyclerViewAdapterHelper](http://www.recyclerview.org/)

## Refresh the controls

### 1.SmartRefreshLayout

GitHub 刚开源的，最近热火朝天的，它的优点：

1.) 把下拉刷新的效果做的很酷炫，用户体验好

2.) 支持所有的view及其多层嵌套的视图结构

炫酷的效果直接去看github
[SmartRefreshLayout github地址](https://github.com/scwang90/SmartRefreshLayout)

### 2.TwinklingRefreshLayout

RefreshLayout that support for OverScroll and better than iOS.
支持下拉刷新和上拉加载的 RefreshLayout,自带越界回弹效果，支持 RecyclerView,AbsListView,ScrollView,WebView

[TwinklingRefreshLayout](https://github.com/lcodecorex/TwinklingRefreshLayout)

### 3.android-Ultra-Pull-To-Refresh

Ultra Pull to Refresh for Android. Support all the views.
该项目只包含下拉刷新，可以包裹任何控件，如果需要添加上拉加载，[看这里](https://github.com/liaohuqiu/android-cube-app)

[android-Ultra-Pull-To-Refresh](https://github.com/liaohuqiu/android-Ultra-Pull-To-Refresh)

### 4.SwipeRefreshLayout0
google官方，项目里面可以直接使用，但是再国内并不受欢迎

### 5.Android-PullToRefresh

PullToRefresh是一套实现非常好的下拉刷新库，它支持：
ListView
ExpandableListView
GridView
WebView
ScrollView
HorizontalScrollView
ViewPager
等多种常用的需要刷新的View类型，而且使用起来也十分方便。

缺点：没有加载更多，还需要直接修改

[Android-PullToRefresh](https://github.com/chrisbanes/Android-PullToRefresh)

### 6.CommonPullToRefresh

在android-Ultra-Pull-To-Refresh的基础上增加了加载更多的支持
下拉刷新支持大部分view：ListView、ScrollView、WebView等，甚至一个单独的TextView
加载更多目前支持ListView、RecyclerView、GridView、SwipeRefreshLayout
支持自定义header以及footer
增加SwipeRefreshLayout刷新方式，同样支持加载更多

缺点：嵌套时存在着滑动冲突

[CommonPullToRefresh](https://github.com/Chanven/CommonPullToRefresh)

### 7.ActionBar-PullToRefresh

看名字就能知道这是一个在 actionbar 上增加下拉的效果

[ActionBar-PullToRefresh](https://github.com/chrisbanes/ActionBar-PullToRefresh)

### 8.android-PullRefreshLayout

This component like SwipeRefreshLayout, it is more beautiful than SwipeRefreshLayout.就是比Google的漂亮,完美的虐了Google的亲儿子

[android-PullRefreshLayout](https://github.com/baoyongzhang/android-PullRefreshLayout)

### 9.FlyRefresh

创意漫天飞的下拉刷新，适合看看，估计没有APP刷新要这个飞机效果

[FlyRefresh](https://github.com/race604/FlyRefresh)

### 10.JellyRefreshLayout

A pull-down-to-refresh layout inspired by Lollipop overscrolled effects

material设计深入开发者的杰作

[JellyRefreshLayout](https://github.com/imallan/JellyRefreshLayout)

### 11.SuperSwipeRefreshLayout

A custom SwipeRefreshLayout to support the pull-to-refresh featrue.You can custom your header view and footer view. RecyclerView，ListView，GridView，NestedScrollView，ScrollView are supported.

[SuperSwipeRefreshLayout](https://github.com/nuptboyzhb/SuperSwipeRefreshLayout)

### 12.Phoenix

Yalantis公司开源出来的，动画效果是杠杠的

[Phoenix](https://github.com/Yalantis/Phoenix)

### 13.CircleRefreshLayout

圆动画效果的下拉刷新

[CircleRefreshLayout](https://github.com/tuesda/CircleRefreshLayout)

### 14.BGARefreshLayout-Android

多种下拉刷新效果、上拉加载更多、可配置自定义头部广告位

[BGARefreshLayout-Android](https://github.com/bingoogolapple/BGARefreshLayout-Android)

### 15.AJWaveRefreshForAndroid

这个作者做了Android与iOS的刷新控件，iOS效果更好一些

[AJWaveRefreshForAndroid](https://github.com/alienjun/AJWaveRefreshForAndroid)

### 16.XRecyclerView

a RecyclerView that implements pullrefresh and loadingmore featrues.you can use it like a standard RecyclerView

[XRecyclerView](https://github.com/jianghejie/XRecyclerView)

### 17.ChromeLikeSwipeLayout

Pull down, and execute more action!效果赞

[ChromeLikeSwipeLayout](https://github.com/ashqal/ChromeLikeSwipeLayout)

### 18.FunGameRefresh

好玩的下拉刷新控件，让我们一起来回味童年,一边下拉刷新，一边打游戏，估计过一会服务器该哭了

[FunGameRefresh](https://github.com/Hitomis/FunGameRefresh)

### 19.BeerSwipeRefresh  WaveSwipeRefreshLayout

啤酒和水滴的下拉刷新，估计是一个爱酒之人

[BeerSwipeRefresh](https://github.com/recruit-lifestyle/BeerSwipeRefresh)

[WaveSwipeRefreshLayout](https://github.com/recruit-lifestyle/WaveSwipeRefreshLayout)


[github上刷新加载控件](https://github.com/search?l=Java&o=desc&q=refresh&s=stars&type=Repositories&utf8=%E2%9C%93)很多，选择一个适合自己项目的，并且让产品满意的才是王道


## Photo

* [glide](https://github.com/bumptech/glide)

* [picasso](https://github.com/square/picasso)

* [fresco](https://github.com/facebook/fresco)

* [PhotoView](https://github.com/chrisbanes/PhotoView)

* [Matisse](https://github.com/zhihu/Matisse)

* [CircleImageView](https://github.com/hdodenhof/CircleImageView)


## Messaging

* [EventBus](https://github.com/greenrobot/EventBus)

## Permissions

* [PermissionsDispatcher](https://github.com/permissions-dispatcher/PermissionsDispatcher)

* [easypermissions](https://github.com/googlesamples/easypermissions)

* [RxPermissions](https://github.com/tbruyelle/RxPermissions)

* [AndPermission](https://github.com/yanzhenjie/AndPermission)

## Chart

* [MPAndroidChart](https://github.com/PhilJay/MPAndroidChart)

* [SmallChart](https://github.com/Idtk/SmallChart)

* [WilliamChart](https://github.com/diogobernardino/WilliamChart)

## UI Framework

* [vlayout](https://github.com/alibaba/vlayout)

* [RapidView](https://github.com/Tencent/RapidView)

## Audio and video

* [ijkplayer](https://github.com/Bilibili/ijkplayer)

* [JiaoZiVideoPlayer](https://github.com/lipangit/JiaoZiVideoPlayer)

## Qr code

* [zxing](https://github.com/zxing/zxing)

## Memory

* [leakcanary](https://github.com/square/leakcanary)

## Animation

* [lottie-android](https://github.com/airbnb/lottie-android)

* [SVGAPlayer-Android](https://github.com/yyued/SVGAPlayer-Android)

* [Material-Animations](https://github.com/lgvalle/Material-Animations)

## Dialog

* [material-dialogs](https://github.com/afollestad/material-dialogs)

## Utils

* [AndroidUtilCode](https://github.com/Blankj/AndroidUtilCode)

## Hot update

* [tinker](https://github.com/Tencent/tinker)

## Custom view look-up table

### Canvas常用操作速查表

| 操作分类       | 相关API                                    | 备注                                       |
| ----------    | ---------------------------------------- | ---------------------------------------- |
| 绘制颜色       | drawColor, drawRGB, drawARGB             | 使用单一颜色填充整个画布 |
| 绘制基本形状     | drawPoint, drawPoints, drawLine, drawLines, drawRect, drawRoundRect, drawOval, drawCircle, drawArc | 依次为 点、线、矩形、圆角矩形、椭圆、圆、圆弧 |
| 绘制图片       | drawBitmap, drawPicture                  | 绘制位图和图片) |
| 绘制文本       | drawText,    drawPosText, drawTextOnPath | 依次为 绘制文字、绘制文字时指定每个文字位置、根据路径绘制文字 |
| 绘制路径       | drawPath                                 | 绘制路径，绘制贝塞尔曲线时也需要用到该函数 |
| 顶点操作       | drawVertices, drawBitmapMesh             | 通过对顶点操作可以使图像形变，drawVertices直接对画布作用、 drawBitmapMesh只对绘制的Bitmap作用 |
| 画布剪裁       | clipPath,    clipRect                    | 设置画布的显示区域                                |
| 画布快照       | save, restore, saveLayerXxx, restoreToCount, getSaveCount | 依次为 保存当前状态、 回滚到上一次保存的状态、 保存图层状态、 会滚到指定状态、 获取保存次数 |
| 画布变换       | translate, scale, rotate, skew           | 依次为 位移、缩放、 旋转、错切|
| Matrix(矩阵) | getMatrix, setMatrix, concat             | 实际画布的位移，缩放等操作的都是图像矩阵Matrix，只不过Matrix比较难以理解和使用，故封装了一些常用的方法。|

| 操作类型       | 相关API                                    | 备注                                       |
| ---------- | ---------------------------------------- | ---------------------------------------- |
| 基础方法       | getDensity, getWidth, getHeight，getDrawFilter，isHardwareAccelerated(API 11)，getMaximumBitmapWidth，getMaximumBitmapHeight，getDensity，quickReject，isOpaque，setBitmap，setDrawFilter | 使用单一颜色填充画布                               |
| 绘制颜色       | drawColor, drawRGB, drawARGB，drawPaint   | 使用单一颜色填充画布                               |
| 绘制基本形状     | drawPoint, drawPoints, drawLine, drawLines, drawRect, drawRoundRect, drawOval, drawCircle, drawArc | 依次为 点、线、矩形、圆角矩形、椭圆、圆、圆弧                  |
| 绘制图片       | drawBitmap, drawPicture                  | 绘制位图和图片                                  |
| 绘制文本       | drawText,    drawPosText, drawTextOnPath | 依次为 绘制文字、绘制文字时指定每个文字位置、根据路径绘制文字          |
| 绘制路径       | drawPath                                 | 绘制路径，绘制贝塞尔曲线时也需要用到该函数                    |
| 顶点操作       | drawVertices, drawBitmapMesh             | 通过对顶点操作可以使图像形变，drawVertices直接对画布作用、 drawBitmapMesh只对绘制的Bitmap作用 |
| 画布剪裁       | clipPath,    clipRect， clipRegion，getClipBounds | 画布剪裁相关方法                                 |
| 画布快照       | save, restore, saveLayer, saveLayerXxx, restoreToCount, getSaveCount | 依次为 保存当前状态、 回滚到上一次保存的状态、 保存图层状态、 回滚到指定状态、 获取保存次数 |
| 画布变换       | translate, scale, rotate, skew           | 依次为 位移、缩放、 旋转、错切                         |
| Matrix(矩阵) | getMatrix, setMatrix, concat             | 实际画布的位移，缩放等操作的都是图像矩阵Matrix，只不过Matrix比较难以理解和使用，故封装了一些常用的方法。 |

### Path常用操作速查表

API21以上，很不爽，得吐槽

作用            | 相关API        | 备注
----------------|-----------------|------------------------------------------
移动起点        | moveTo          | 移动下一次操作的起点位置
设置终点        | setLastPoint    | 重置当前path中最后一个点位置，如果在绘制之前调用，效果和moveTo相同
连接直线        | lineTo          | 添加上一个点到当前点之间的直线到Path
闭合路径        | close           | 连接第一个点连接到最后一个点，形成一个闭合区域
添加内容        | addRect, addRoundRect,  addOval, addCircle, 	addPath, addArc, arcTo | 添加(矩形， 圆角矩形， 椭圆， 圆， 路径， 圆弧) 到当前Path (注意addArc和arcTo的区别)
是否为空        | isEmpty         | 判断Path是否为空
是否为矩形      | isRect          | 判断path是否是一个矩形
替换路径        | set             | 用新的路径替换到当前路径所有内容
偏移路径        | offset          | 对当前路径之前的操作进行偏移(不会影响之后的操作)
贝塞尔曲线      | quadTo, cubicTo | 分别为二次和三次贝塞尔曲线的方法
rXxx方法        | rMoveTo, rLineTo, rQuadTo, rCubicTo | **不带r的方法是基于原点的坐标系(偏移量)， rXxx方法是基于当前点坐标系(偏移量)**
填充模式        | setFillType, getFillType, isInverseFillType, toggleInverseFillType   | 设置,获取,判断和切换填充模式
提示方法        | incReserve      | 提示Path还有多少个点等待加入**(这个方法貌似会让Path优化存储结构)**
布尔操作(API19) | op              | 对两个Path进行布尔运算(即取交集、并集等操作)
计算边界        | computeBounds   | 计算Path的边界
重置路径        | reset, rewind   | 清除Path中的内容<br/> **reset不保留内部数据结构，但会保留FillType.**<br/> **rewind会保留内部的数据结构，但不保留FillType**
矩阵操作        | transform       | 矩阵变换

### Matrix常用操作速查表

| 方法类别     | 相关API                                    | 备注                         |
| -------- | ---------------------------------------- | -------------------------- |
| 基本方法     | equals hashCode toString toShortString   | 比较、 获取哈希值、 转换为字符串          |
| 数值操作     | set reset setValues getValues            | 设置、 重置、 设置数值、 获取数值         |
| 数值计算     | mapPoints mapRadius mapRect mapVectors   | 计算变换后的数值                   |
| 设置(set)  | setConcat setRotate setScale setSkew setTranslate | 设置变换                       |
| 前乘(pre)  | preConcat preRotate preScale preSkew preTranslate | 前乘变换                       |
| 后乘(post) | postConcat postRotate postScale postSkew postTranslate | 后乘变换                       |
| 特殊方法     | setPolyToPoly setRectToRect rectStaysRect setSinCos | 一些特殊操作                     |
| 矩阵相关     | invert isAffine(API21) isIdentity        | 求逆矩阵、 是否为仿射矩阵、 是否为单位矩阵 ... |


### 贝塞尔曲线常用操作速查表

> **表格中演示动画均来自维基百科**

贝塞尔曲线               | 对应的方法 | 演示动画
:-----------------------:|------------|------------------------------------------------------------------------------
 一阶曲线<br/>(线性曲线) | lineTo     | ![](https://upload.wikimedia.org/wikipedia/commons/0/00/B%C3%A9zier_1_big.gif)
 二阶曲线                | quadTo     | ![](https://upload.wikimedia.org/wikipedia/commons/3/3d/B%C3%A9zier_2_big.gif)
 三阶曲线                 | cubicTo    | ![](https://upload.wikimedia.org/wikipedia/commons/d/db/B%C3%A9zier_3_big.gif)
 四阶曲线                 | 无         | ![](https://upload.wikimedia.org/wikipedia/commons/a/a4/B%C3%A9zier_4_big.gif)


## Custom view advanced reference information

### 绘制机制
先好好的理解一下绘制流程 [公共技术点之 View 绘制流程](http://a.codekk.com/detail/Android/lightSky/%E5%85%AC%E5%85%B1%E6%8A%80%E6%9C%AF%E7%82%B9%E4%B9%8B%20View%20%E7%BB%98%E5%88%B6%E6%B5%81%E7%A8%8B)

#### [GcsSloop---自定义View系列自定义](https://github.com/GcsSloop/AndroidNote/tree/master/CustomView/README.md)

* Basic Article
    * [Android custom View base - Coordinate System](https://github.com/GcsSloop/AndroidNote/blob/master/CustomView/Base/%5B01%5DCoordinateSystem.md)
    * [Android custom View base - Angle And Radian](https://github.com/GcsSloop/AndroidNote/blob/master/CustomView/Base/%5B02%5DAngleAndRadian.md)
    * [Android custom View base - Color](https://github.com/GcsSloop/AndroidNote/blob/master/CustomView/Base/%5B03%5DColor.md)
* Advanced Article
    * [Android custom View advanced - Classification and Process](https://github.com/GcsSloop/AndroidNote/blob/master/CustomView/Advance/%5B01%5DCustomViewProcess.md)
    * [Android custom View advanced - Draw basic graphics](https://github.com/GcsSloop/AndroidNote/blob/master/CustomView/Advance/%5B02%5DCanvas_BasicGraphics.md)
    * [Android custom View advanced - Canvas Operation](https://github.com/GcsSloop/AndroidNote/blob/master/CustomView/Advance/%5B03%5DCanvas_Convert.md)
    * [Android custom View advanced - Picture Words](https://github.com/GcsSloop/AndroidNote/blob/master/CustomView/Advance/%5B04%5DCanvas_PictureText.md)
    * [Android custom View advanced - Path basic operation](https://github.com/GcsSloop/AndroidNote/blob/master/CustomView/Advance/%5B05%5DPath_Basic.md)
    * [Android custom View advanced - Bessel curve](https://github.com/GcsSloop/AndroidNote/blob/master/CustomView/Advance/%5B06%5DPath_Bezier.md)
    * [Android custom View advanced - Final Path](https://github.com/GcsSloop/AndroidNote/blob/master/CustomView/Advance/%5B07%5DPath_Over.md)
    * [Android custom View advanced - Path Figure(PathMeasure)](https://github.com/GcsSloop/AndroidNote/blob/master/CustomView/Advance/%5B08%5DPath_Play.md)
    * [Android custom View advanced - Matrix Principle](https://github.com/GcsSloop/AndroidNote/blob/master/CustomView/Advance/%5B09%5DMatrix_Basic.md)
    * [Android custom View advanced - Matrix](https://github.com/GcsSloop/AndroidNote/blob/master/CustomView/Advance/%5B10%5DMatrix_Method.md)
    * [Android custom View advanced - Matrix Camera](https://github.com/GcsSloop/AndroidNote/blob/master/CustomView/Advance/%5B11%5DMatrix_3D_Camera.md)
    * [Android custom View advanced - Principle of event distribution mechanism](https://github.com/GcsSloop/AndroidNote/blob/master/CustomView/Advance/%5B12%5DDispatch-TouchEvent-Theory.md)
    * [Android custom View advanced - The event distribution mechanism is detailed](https://github.com/GcsSloop/AndroidNote/blob/master/CustomView/Advance/%5B15%5DDispatch-TouchEvent-Source.md)
    * [Android custom View advanced - MotionEvent](https://github.com/GcsSloop/AndroidNote/blob/master/CustomView/Advance/%5B16%5DMotionEvent.md)
    * [Android custom View advanced - Special shape control event handling scheme](https://github.com/GcsSloop/AndroidNote/blob/master/CustomView/Advance/%5B17%5Dtouch-matrix-region.md)
    * [Android custom View advanced - Multi-touch detailed solution](https://github.com/GcsSloop/AndroidNote/blob/master/CustomView/Advance/%5B18%5Dmulti-touch.md)
    * [Android custom View advanced - Signal detection(GestureDetector)](https://github.com/GcsSloop/AndroidNote/blob/master/CustomView/Advance/%5B19%5Dgesture-detector.md)

* [ViewSupport - Customize the View toolkit](https://github.com/GcsSloop/ViewSupport)



#### [爱哥的---自定义View其实很简单](http://blog.csdn.net/column/details/androidcustomview.html)

 *  [自定义控件其实很简单1/12](http://blog.csdn.net/column/details/androidcustomview.html)
 *  [自定义控件其实很简单1/6](http://blog.csdn.net/column/details/androidcustomview.html)
 *  [自定义控件其实很简单1/4](http://blog.csdn.net/column/details/androidcustomview.html)
 *  [自定义控件其实很简单1/3](http://blog.csdn.net/column/details/androidcustomview.html)
 *  [自定义控件其实很简单5/12](http://blog.csdn.net/column/details/androidcustomview.html)
 *  [自定义控件其实很简单1/2](http://blog.csdn.net/column/details/androidcustomview.html)
 *  [自定义控件其实很简单7/12](http://blog.csdn.net/column/details/androidcustomview.html)
 *  [自定义控件其实很简单2/3](http://blog.csdn.net/column/details/androidcustomview.html)
 *  [自定义控件其实很简单3/4](http://blog.csdn.net/column/details/androidcustomview.html)

#### [刘某人程序员---Android绘图机制](http://blog.csdn.net/qq_26787115/article/details/50457413)

 * [Android绘图机制（一）——自定义View的基础属性和方法](http://blog.csdn.net/qq_26787115/article/details/50457413)
 * [Android绘图机制（二）——自定义View绘制形, 圆形, 三角形, 扇形, 椭圆, 曲线,文字和图片的坐标讲解](http://blog.csdn.net/qq_26787115/article/details/50466655)
 * [Android绘图机制（三）——自定义View的实现方式以及半弧圆新控件](http://blog.csdn.net/qq_26787115/article/details/50485698)
 * [Android绘图机制（四）——使用HelloCharts开源框架搭建一系列炫酷图表，柱形图，折线图，饼状图和动画特效，抽丝剥茧带你认识图表之美](http://blog.csdn.net/qq_26787115/article/details/50492743)

#### [Carson_Ho](http://www.jianshu.com/p/e9d8420b1b9c)

 * [Canvas类的最全面详解 - 自定义View应用系列](http://www.jianshu.com/p/762b490403c3)
 * [Path类的最全面详解 - 自定义View应用系列](http://www.jianshu.com/p/2c19abde958c)
 * [自定义View基础 - 最易懂的自定义View原理系列（1）](http://www.jianshu.com/p/146e5cec4863)
 * [自定义View Measure过程 - 最易懂的自定义View原理系列（2）](http://www.jianshu.com/p/1dab927b2f36)
 * [自定义View Layout过程 - 最易懂的自定义View原理系列（3）](http://www.jianshu.com/p/158736a2549d)
 * [自定义View Draw过程- 最易懂的自定义View原理系列（4）](http://www.jianshu.com/p/95afeb7c8335)
 * [手把手教你写一个完整的自定义View](http://www.jianshu.com/p/e9d8420b1b9c)
 * [最易懂的自定义View---自定义view讲解集合](http://www.jianshu.com/nb/9976005)

### 事件传递机制


还是先来理解事件传递机制[公共技术点之 View 事件传递](http://a.codekk.com/detail/Android/Trinea/%E5%85%AC%E5%85%B1%E6%8A%80%E6%9C%AF%E7%82%B9%E4%B9%8B%20View%20%E4%BA%8B%E4%BB%B6%E4%BC%A0%E9%80%92)

 * [Carson_Ho Android事件分发机制详解：史上最全面、最易懂](http://www.jianshu.com/p/38015afcdb58)
 * [Kelin  图解 Android 事件分发机制](http://www.jianshu.com/p/e99b5e8bd67b)
 * [milter  可能是讲解Android事件分发最好的文章](http://www.jianshu.com/p/2be492c1df96)
 * [Idtk  更简单的学习Android事件分发](https://github.com/Idtk/Blog/blob/master/Blog/11%E3%80%81TouchEvent.md)
 * [希尔瓦娜斯女神  Android View事件机制 21问21答](http://www.cnblogs.com/punkisnotdead/p/5179115.html)

### 属性动画

把动画基础了解好来，差不多就出师了[公共技术点之 Android 动画基础](http://a.codekk.com/detail/Android/lightSky/%E5%85%AC%E5%85%B1%E6%8A%80%E6%9C%AF%E7%82%B9%E4%B9%8B%20Android%20%E5%8A%A8%E7%94%BB%E5%9F%BA%E7%A1%80)

 * [DylanAndroid  Android属性动画上手实现各种动画效果，自定义动画，抛物线等](http://blog.csdn.net/linglongxin24/article/details/53084234)
 * [MrXI  Android全套动画使用技巧](https://juejin.im/post/58fbfb93a22b9d00659c655f)
 * [IAM四十二  Android 动画总结](http://www.jianshu.com/p/420629118c10)

### 自定义view简短篇

 [教你步步为营掌握自定义View](http://www.jianshu.com/p/d507e3514b65)
 [自定义View，有这一篇就够了](http://blog.csdn.net/huachao1001/article/details/51577291)


### 自定义view开源项目练习

 1.  [NumberProgressBar](https://github.com/daimajia/NumberProgressBar)(代码家)
 这个项目可以熟练掌握如何控制view在界面中的位子

 2.  [SmallChart](https://github.com/Idtk/SmallChart)
 项目包括折线图、曲线图(可填充)、柱状图、扇形图、雷达图的绘制，让你熟练使用draw()相关类。

 3.  [CircleImageView](https://github.com/hdodenhof/CircleImageView)
  一个圆形的ImageView

 4.  [PhotoView](https://github.com/chrisbanes/PhotoView)
 对ImageView支持各种手势操作，缩放、移动、旋转...熟练掌握手势操作。

 5.  [AndroidSwipeLayout](https://github.com/daimajia/AndroidSwipeLayout)
 综合

 6.  [MPAndroidChart](https://github.com/PhilJay/MPAndroidChart)
 MPAndroidChart是一款基于Android的开源图表库，MPAndroidChart不仅可以在android设备上绘制各种统计图表，而且可以对图表进行拖动和缩放操作，应用起来非常灵活。MPAndroidChart同样拥有常用的图表类型：线型图、饼图、柱状图和散点图。

 7.  [Side-Menu.Android](https://github.com/Yalantis/Side-Menu.Android)
 分类侧滑菜单,吊炸天的效果

 8.  [WilliamChart](https://github.com/diogobernardino/WilliamChart)
 绘制图表的库，支持LineChartView、BarChartView和StackBarChartView三中图表类型

 9.  [circular-progress-button](https://github.com/dmytrodanylyk/circular-progress-button)
 带进度显示的Button,让操作更炫酷

 10. [Context-Menu.Android](https://github.com/Yalantis/Context-Menu.Android)
 可以方便快速集成漂亮带有动画效果的上下文菜单

 11. [ToggleButton](https://github.com/zcweng/ToggleButton)
 状态切换的 Button，类似 iOS，用 View 实现

 12. [InstaMaterial](https://github.com/frogermcs/InstaMaterial)
 Instagram的一组Material 风格的概念设计

 13. [sweet-alert-dialog](https://github.com/pedant/sweet-alert-dialog)
 一个带动画效果的自定义对话框样式

 ## Auxiliary tool

* [json在线解析](http://www.json.cn/)

* [颜色转换工具](http://tool.css-js.com/rgba.html)

* [颜色转换工具](http://www.sioe.cn/yingyong/yanse-rgb-16/)

* [Genymotion 虚拟机](https://www.genymotion.com/#!/)

* [icon在线制作工具](http://romannurik.github.io/AndroidAssetStudio/index.html)

* [Android 代码搜索工具](https://www.codota.com/)

* [charles截取请求](https://www.charlesproxy.com/)

## Android advanced approach

* Android订阅

  *[干货集中营](http://gank.io/)

  *[Android开发技术周报](http://www.androidweekly.cn/)

  *[Android 科学院](https://zhuanlan.zhihu.com/andlib)

  *[移动开发每周阅读清单](http://mobilefrontier.github.io/)

  *[Android Blog 周刊 ](http://androidblog.cn/)

  *[Android 开发经验谈](http://www.jianshu.com/c/5139d555c94d)

  *[Android Weekly 中文翻译版](https://zhuanlan.zhihu.com/android-weekly)

  *[DiyCode](https://www.diycode.cc/topics)

  *[极客导航](http://www.jikedaohang.com/)

* Android体系化学习网站／博客

  *[极客学院](http://www.jikexueyuan.com/path/android)

  *[Android 知识梳理](https://juejin.im/post/587dbaf9570c3522010e400e)

  *[Android Studio中文社区](http://www.android-studio.org/index.php)

  *[Android技术体系](http://lib.csdn.net/aqi00/347554/chart/Android%E6%8A%80%E6%9C%AF%E4%BD%93%E7%B3%BB)

  *[Android开源库集锦](http://lib.csdn.net/zhhy88/286182/chart/Android%E5%BC%80%E6%BA%90%E5%BA%93%E9%9B%86%E9%94%A6)

* Android问题解决网站

  *[Stack Overflow](https://stackoverflow.com/)

  *[GitHub](https://github.com/)

  *[CSDN](http://www.csdn.net/)

  *[掘金](https://juejin.im/timeline)

  *[简书](http://www.jianshu.com/)

  *[Google Groups: Android Developers](https://groups.google.com/forum/#!forum/android-developers)（需翻墙）

*  Android开源分享网站

  *[GitHub](https://github.com/)

  *[开源中国](http://www.oschina.net/)

  *[安卓巴士](http://www.apkbus.com/)

  *[DevStore](http://www.devstore.cn/)

  *[eoeandroid](http://www.eoeandroid.com/forum.php)

**[⬆ Top](#contents)**
