# Fragment 详解

---

> Fragment 表示 Activity 中的行为或用户界面部分。您可以将多个片段组合在一个 Activity 中来构建多窗格 UI，以及在多个 Activity 中重复使用某个片段。您可以将片段视为 Activity 的模块化组成部分，它具有自己的生命周期，能接收自己的输入事件，并且您可以在 Activity 运行时添加或移除片段（有点像您可以在不同 Activity 中重复使用的“子 Activity”）。

## 目录

-[生命周期](#生命周期)

-[简单使用](#简单使用)

-[Fragment 通信](#Fragment通信)

-[源码分析](#源码分析)

## 生命周期

Fragment 和 activity 一样，也是有四种状态

**Resumed**
当前 Fragment 位于前台，用户可见，可以获得焦点；

**Paused**
另一个 Activity 处于前台并拥有焦点, 但是该 Fragment 所在的 Activity 仍然可见(前台 Activity 局部透明或者没有覆盖整个屏幕)，不过不能获得焦点；

**Stopped**
要么是宿主 Activity 已经被停止, 要么是 Fragment 从 Activity 被移除但被添加到回退栈中；停止状态的 Fragment 仍然活着(所有状态和成员信息被系统保持着)。 然而, 它对用户不再可见, 并且如果 Activity 被销毁，它也会被销毁；

**Destroyed**
只能等待被回收。

![Fragment生命周期](/Resource/Image/fragment_lifecycle.png)

附上 Activity 生命周期进行对比

![Activity生命周期](/Resource/Image/activity_lifecycle.png)

Fragment 比 Activity 多了几个额外的生命周期回调方法：

onAttach（） 当碎片和活动建立关联时调用。（获得 activity 的传递的值）
onCreateView（） 为碎片创建视图调用就是加载布局时。
onActivityCreated（） 确保与碎片相关联的活动一定已经创建完毕的时候调用。
onDestroyView（） 当与碎片的视图被移除的时候调用。
onDetach（）当碎片与活动解除关联的时候调用。

## 简单使用

Fragment 添加到 Activity 中有两种方式一种是静态添加，一种是动态添加

#### 静态添加

在对应的 Activity 中的 xml 中添加，实例如下

```
<fragment
    android:id="@+id/a_fragment"
    android:name="com.madreain.AFragment"
    android:layout_width="match_parent"
    android:layout_height="match_parent">
</fragment>
```

#### 动态添加

一般都是把 xml 布局中的 FrameLayout 作为 Fragment 显示的区域，然后通过 tab 栏按钮来进行多个 Fragment 的切换

[Fragment 具体动态添加可参考这个库](https://github.com/YoKeyword/Fragmentation)

有两篇文章很好的讲解了Fragment使用中遇到的问题

[Fragment 全解析系列（一）：那些年踩过的坑](http://www.jianshu.com/p/d9143a92ad94)

[Fragment 全解析系列（二）：正确的使用姿势](http://www.jianshu.com/p/fd71d65f0ec6)


FragmentTransaction有一些基本方法，下面给出调用这些方法时，Fragment生命周期的变化：

add(): onAttach()->…->onResume()。
remove(): onPause()->…->onDetach()。
replace(): 相当于旧Fragment调用remove()，新Fragment调用add()。
show(): 不调用任何生命周期方法，调用该方法的前提是要显示的 Fragment已经被添加到容器，只是纯粹把Fragment UI的setVisibility为true。
hide(): 不调用任何生命周期方法，调用该方法的前提是要显示的Fragment已经被添加到容器，只是纯粹把Fragment UI的setVisibility为false。
detach(): onPause()->onStop()->onDestroyView()。UI从布局中移除，但是仍然被FragmentManager管理。
attach(): onCreateView()->onStart()->onResume()。

## Fragment 通信

1.Fragment向Activity传递数据
1).接口回掉的方法
Fragment中定义接口，Activity中实现该接口。

AFragment
```
public interface OnFragmentCallback {
    void onCallback(Object value);
}
```

AActivity
```
public class AActivity extends AppCompatActivity implements AFragment.OnFragmentCallback{

    @Override
    public void onCallback(Object value){
        //todo获取Fragment 向 Activity传递的数据
    }

}

```
2).广播方式

广播具体实现参考
[BroadcastReciver详解](10004-BroadcastReciver.md)



2.Activity 向 Fragment 传递数据(Bundle 和 setArguments(bundle))

1).Fragment创建时传递数据

在AActivity调用时
```
AFragment aFragment = AFragment.newInstance(object);
aFragment.setStyle(DialogFragment.STYLE_NO_TITLE, 0);
aFragment.show(getSupportFragmentManager(), AFragment.class.getName());
```

在AFragment中
```
public class AFragment extends DialogFragment {

    private Object object;

    /**
    * 创建时传入的对象，可传入多个参数
    */
    public static AFragment newInstance(Object object) {
        AFragment fragment = new AFragment();
        Bundle bundle = new Bundle();
        bundle.putBoolean("object", object);
        fragment.setArguments(bundle);
        return fragment;
    }

    @Nullable
    @Override
    public View onCreateView(LayoutInflater inflater, @Nullable ViewGroup container, @Nullable Bundle savedInstanceState) {
        //省略其他代码
        Bundle bundle = getArguments();
        //获取到传入的值进行显示等使用
        object = bundle.getString("object");
        return v;
    }
}    
```

2).Fragment创建后设置数据

在AFragment中
```
public class AFragment extends DialogFragment {

    private Object object;

    public void setObject(Object object) { 
        this.object = object;
    }

}
```


在AActivity中设置值
```
afragment.setObject("object")

```

3).广播方式同上

3.Fragment之间通信

1).通过Activity作为中介,参考Fragment->Activity传递参数,然后ctivity->Fragment传递参数

2).广播方式同上

## 源码分析

[Fragment源码解析](/AndroidLearning/3/30027-Fragment源码解析.md)