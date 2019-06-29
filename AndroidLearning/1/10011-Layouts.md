# Layouts介绍

---

> Android系统布局的介绍，将简单介绍一下RelativeLayout、LinearLayout、FrameLayout、Constraintlayout、TableLayout、AbsoluteLayout、GridLayout

## 目录


-[LinearLayout](#LinearLayout)

-[RelativeLayout](#RelativeLayout)

-[FrameLayout](#FrameLayout)

-[Constraintlayout](#Constraintlayout)

-[TableLayout](#TableLayout)

-[AbsoluteLayout](#AbsoluteLayout)

-[GridLayout](#GridLayout)

-[通用属性](#通用属性)

## LinearLayout

线性布局在开发中使用居多，具有垂直方向与水平方向的布局方式，通过设置属性“android:orientation”控制方向，属性值垂直（vertical）和水平(horizontal)，默认水平方向。

相关常用属性：

android:gravity：内部控件对齐方式，常用属性值有center、center_vertical、center_horizontal、top、bottom、left、right等。

android:layout_weight：权重，用来分配当前控件在剩余空间的大小。使用权重一般要把分配该权重方向的长度设置为零，比如在水平方向分配权重，就把width设置为零。️weightSum权重总值⚠️用android:layout_weight，会导致布局重新绘制，非特殊情况一般不使用 

android:layout_gravity：控制子组件在夫容器里的对其方式



## RelativeLayout

相对布局可以让子控件相对于兄弟控件或父控件进行布局，可以设置子控件相对于兄弟控件或父控件进行上下左右对齐。RelativeLayout能替换一些嵌套视图，当我们用LinearLayout来实现一个简单的布局但又使用了过多的嵌套时，就可以考虑使用RelativeLayout重新布局。相对布局就是一定要加Id才能管理。

常用属性：
相对于父控件：
例子：android:layout_alignParentTop=“true”
android:layout_alignParentTop      控件的顶部与父控件的顶部对齐;
android:layout_alignParentBottom  控件的底部与父控件的底部对齐;
android:layout_alignParentLeft      控件的左部与父控件的左部对齐;
android:layout_alignParentRight     控件的右部与父控件的右部对齐;

相对给定Id控件：
例子：android:layout_above=“@id/**”
android:layout_above 控件的底部置于给定ID的控件之上;
android:layout_below     控件的底部置于给定ID的控件之下;
android:layout_toLeftOf    控件的右边缘与给定ID的控件左边缘对齐;
android:layout_toRightOf  控件的左边缘与给定ID的控件右边缘对齐;
android:layout_alignBaseline  控件的baseline与给定ID的baseline对齐;
android:layout_alignTop        控件的顶部边缘与给定ID的顶部边缘对齐;
android:layout_alignBottom   控件的底部边缘与给定ID的底部边缘对齐;
android:layout_alignLeft       控件的左边缘与给定ID的左边缘对齐;
android:layout_alignRight      控件的右边缘与给定ID的右边缘对齐;

居中方式：
例子：android:layout_centerInParent=“true”
android:layout_centerHorizontal 水平居中;
android:layout_centerVertical    垂直居中;
android:layout_centerInParent  父控件的中央;

## FrameLayout

帧布局，从屏幕左上角按照层次堆叠方式布局，后面的控件覆盖前面的控件。例子：设计地图

常用属性：

android:foreground:设置改帧布局容器的前景图像 

android:foregroundGravity:设置前景图像显示的位置

## Constraintlayout

约束布局ConstraintLayout 是一个ViewGroup，可以在Api9以上的Android系统使用它，它的出现主要是为了解决布局嵌套过多的问题，以灵活的方式定位和调整小部件。从 Android Studio 2.3 起，官方的模板默认使用 ConstraintLayout。

常用属性：
相对定位：
app:layout_constraintLeft_toLeftOf
app:layout_constraintLeft_toRightOf
app:layout_constraintRight_toLeftOf
app:layout_constraintRight_toRightOf
app:layout_constraintTop_toTopOf
app:layout_constraintTop_toBottomOf
app:layout_constraintBottom_toTopOf
app:layout_constraintBottom_toBottomOf
app:layout_constraintBaseline_toBaselineOf
app:layout_constraintStart_toEndOf
app:layout_constraintStart_toStartOf
app:layout_constraintEnd_toStartOf
app:layout_constraintEnd_toEndOf

角度定位：
app:layout_constraintCircle="@+id/TextView1"
app:layout_constraintCircleAngle="120"（角度）
app:layout_constraintCircleRadius="150dp"（距离）

边距：
android:layout_marginStart
android:layout_marginEnd
android:layout_marginLeft
android:layout_marginTop
android:layout_marginRight
android:layout_marginBottom
app:layout_goneMarginStart
app:layout_goneMarginEnd
app:layout_goneMarginLeft
app:layout_goneMarginTop
app:layout_goneMarginRight
app:layout_goneMarginBottom

居中设置：
app:layout_constraintBottom_toBottomOf="parent"
app:layout_constraintLeft_toLeftOf="parent"
app:layout_constraintRight_toRightOf="parent"
app:layout_constraintTop_toTopOf="parent"

偏移：（赋值为0-1）
layout_constraintHorizontal_bias 水平偏移
layout_constraintVertical_bias 垂直偏移

尺寸：
当控件的高度或宽度为wrap_content时，可以使用下列属性来控制最大、最小的高度或宽度：
android:minWidth 最小的宽度
android:minHeight 最小的高度
android:maxWidth 最大的宽度
android:maxHeight 最大的高度

宽高比：
app:layout_constraintDimensionRatio="H,2:3"指的是 高:宽=2:3
app:layout_constraintDimensionRatio="W,2:3"指的是 宽:高=2:3

链：
app:layout_constraintHorizontal_chainStyle
app:layout_constraintVertical_chainStyle
里面可设置三种：
CHAIN_SPREAD —— 展开元素 (默认)
CHAIN_SPREAD_INSIDE —— 展开元素，但链的两端贴近parent
CHAIN_PACKED —— 链的元素将被打包在一起

Constraintlayout布局辅助控件：
layout_optimizationLevel 优化
android.support.constraint.Barrier 辅助线
android.support.constraint.Group  组
android.support.constraint.Placeholder  占位符
android.support.constraint.Guideline 辅助线

## TableLayout

表格布局，适用于多行多列的布局格式，每个TableLayout是由多个TableRow组成，一个TableRow就表示TableLayout中的每一行，这一行可以由多个子元素组成。实际上TableLayout和TableRow都是LineLayout线性布局的子类。但是TableRow的参数android:orientation属性值固定为horizontal，且android:layout_width=MATCH_PARENT，android:layout_height=WRAP_CONTENT。所以TableRow实际是一个横向的线性布局，且所以子元素宽度和高度一致。
⚠️注意：在TableLayout中，单元格可以为空，但是不能跨列，意思是只能不能有相邻的单元格为空。

TableLayout常用属性：
android:shrinkColumns：设置可收缩的列，内容过多就收缩显示到第二行
android:stretchColumns：设置可伸展的列，将空白区域填充满整个列
android:collapseColumns：设置要隐藏的列

列的索引从0开始，shrinkColumns和stretchColumns可以同时设置。

子控件常用属性：
android:layout_column：第几列
android:layout_span：占据列数

## AbsoluteLayout

绝对布局中将所有的子元素通过设置android:layout_x 和 android:layout_y属性，将子元素的坐标位置固定下来，即坐标(android:layout_x, android:layout_y) ，layout_x用来表示横坐标，layout_y用来表示纵坐标。屏幕左上角为坐标(0,0)，横向往右为正方，纵向往下为正方。实际应用中，这种布局用的比较少，因为Android终端一般机型比较多，各自的屏幕大小。分辨率等可能都不一样，如果用绝对布局，可能导致在有的终端上显示不全等。

## GridLayout

作为android 4.0 后新增的一个布局,与前面介绍过的TableLayout(表格布局)其实有点大同小异;
不过新增了一些东西：跟LinearLayout(线性布局)一样,他可以设置容器中组件的对齐方式；容器中的组件可以跨多行也可以跨多列(相比TableLayout直接放组件,占一行相比较)，因为是android 4.0新增的,API Level 14,在这个版本以前的sdk都需要导入项目。这里不解释。

常用属性:
android:orientation=""     vertical(竖直,默认)或者horizontal(水平)
android:layout_gravity=""   center,left,right,buttom
android:rowCount="4"        //设置网格布局有4行
android:columnCount="4"    //设置网格布局有4列

子组件属性
android:layout_row = "1"   //设置组件位于第二行 
android:layout_column = "2"   //设置该组件位于第三列
android:layout_rowSpan = "2"     //纵向横跨2行
android:layout_columnSpan = "3"     //横向横跨2列

## 通用属性

android:id —— 为控件指定相应的ID

android:layout_width —— 定义本元素的宽度

android:layout_height —— 定义本元素的高度

android:background —— 指定该控件所使用的背景色,RGB命名法 

android:orientation="vertical"     ----horizontal为控件默认横向显示,vertical为竖着显示,不设置该属性默认为横向显示

android:gravity　

android:gravity属性是对该view 内容的限定．比如一个button 上面的text. 你可以设置该text 在view的靠左，靠右等位置．以button为例，android:gravity="right"则button上面的文字靠右

android:layout_gravity

android:layout_gravity是用来设置该view相对与起父view 的位置．比如一个button 在linearlayout里，你想把该button放在靠左、靠右等位置就可以通过该属性设置．以button为例，android:layout_gravity="right"则button靠右

***属性值为具体的像素值***

 外间距:

外间距是指控件与控件之间的距离:

android:layout_margin 本元素离父元素上下左右间的距离

android:layout_marginBottom 离父元素底边缘的距离

android:layout_marginLeft 离父元素左边缘的距离

android:layout_marginRight 离父元素右边缘的距离

android:layout_marginTop 离父元素上边缘的距离

android:layout_marginStart本元素里开始的位置的距离(相当于离父元素左边缘的距离)

android:layout_marginEnd本元素里结束位置的距离(相当于离父元素右边缘的距离)

内间距:

内间距是指控件之内的内容与该控件的距离间隔:

android:padding指定布局与子布局的间距

android:paddingLeft指定布局左边与子布局的间距

android:paddingTop指定布局上边与子布局的间距

android:paddingRight指定布局右边与子布局的间距

android:paddingBottom指定布局下边与子布局的间距

android:paddingStart指定布局左边与子布局的间距与android:paddingLeft相同

android:paddingEnd指定布局右边与子布局的间距与android:paddingRight相同

其它属性

android:fadingEdgeLength 设置边框渐变的长度

android:minHeight最小高度

android:minWidth最小宽度

android:translationX 水平方向的移动距离

android:translationY垂直方向的移动距离

android:transformPivotX相对于一点的水平方向偏转量

android:transformPivotY相对于一点的垂直方向偏转量

***滚动条的设置***

android:fadeScrollbars —— 滚动条自动隐藏

android:scrollbarSize —— 设置滚动条大小

android:scrollbars —— 设置滚动条的状态

android:scrollbarStyle —— 设置滚动条的样式

android:scrollbarFadeDuration —— 设置滚动条淡入淡出时间

android:scrollbarDefaultDelayBeforeFade —— 设置滚动条N毫秒后开始淡化，以毫秒为单位。

android:fadingEdge —— 设置拉滚动条时 ,边框渐变的放向

android:verticalScrollbarPosition —— 设置垂直滚动条的位置

android:scrollbarThumbHorizontal —— 设置水平滚动条的drawable。

android:scrollbarThumbVertical —— 设置垂直滚动条的drawable

android:scrollbarTrackHorizontal —— 设置水平滚动条背景（轨迹）的色drawable

android:scrollbarTrackVertical —— 设置垂直滚动条背景（轨迹）的色drawable

android:scrollbarAlwaysDrawHorizontalTrack —— 设置水平滚动条是否含有轨道

android:scrollbarAlwaysDrawVerticalTrack —— 设置垂直滚动条是否含有轨道

android:requiresFadingEdge —— 定义滚动时边缘是否褪色

***常用设置***

android:descendantFocusability —— 控制子布局焦点获取方式 常用于listView的item中包含多个控件 点击无效

android:visibility —— 定义布局是否可见

android:clickable —— 定义是否可点击

android:longClickable —— 定义是否可长点击

android:saveEnabled —— 设置是否在窗口冻结时（如旋转屏幕）保存View的数据

android:filterTouchesWhenObscured —— 所在窗口被其它可见窗口遮住时,是否过滤触摸事件

android:keepScreenOn —— 设置屏幕常亮

android:soundEffectsEnabled —— 点击或触摸是否有声音效果

android:hapticFeedbackEnabled —— 设置触感反馈

***其他***

android:fitsSystemWindows —— 设置布局调整时是否考虑系统窗口(如状态栏)

android:drawingCacheQuality —— 设置绘图时半透明质量

android:OverScrollMode —— 滑动到边界时样式

android:alpha —— 设置透明度

android:rotation —— 旋转度数

android:rotationX —— 水平旋转度数

android:rotationY —— 垂直旋转度数

android:scaleX —— 设置X轴缩放

android:scaleY —— 设置Y轴缩放

android:layerType —— 设定支持

android:layoutDirection —— 定义布局图纸的方向

android:textDirection —— 定义文字方向

android:textAlignment —— 文字对齐方式

android:importantForAccessibility —— 设置可达性的重要行

android:labelFor —— 添加标签

android:persistentDrawingCachehua —— 定义绘图的高速缓存的持久性   

android:layoutAnimation —— 定义布局显示时候的动画

android:tag —— 为布局添加tag方便查找与类似

android:nextFocusLeft —— 设置左边指定视图获得下一个焦点

android:nextFocusRight —— 设置右边指定视图获得下一个焦点

android:nextFocusUp —— 设置上边指定视图获得下一个焦点

android:nextFocusDown —— 设置下边指定视图获得下一个焦点

android:nextFocusForward —— 设置指定视图获得下一个焦点

android:contentDescription —— 说明

android:OnClick —— 点击时从上下文中调用指定的方法

android:animateLayoutChanges —— 布局改变时是否有动画效果

android:clipToPadding —— 定义布局间是否有间距

android:animationCache —— 定义子布局也有动画效果

android:alwaysDrawnWithCache —— 定义子布局是否应用绘图的高速缓存

android:addStatesFromChildren —— 定义布局是否应用子布局的背景

android:splitMotionEvents —— 定义布局是否传递touch事件到子布局

android:focusableInTouchMode —— 定义是否可以通过touch获取到焦点

android:isScrollContainer —— 定义布局是否作为一个滚动容器 可以调整整个窗体

android:duplicateParentState —— 是否从父容器中获取绘图状态(光标,按下等)

注意⚠️️️⚠️⚠️布局的优化很重要，我们将在后面的文章中介绍