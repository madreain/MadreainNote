# Service 详解

---

> Service 是一种程序后台运行的方案,用于不需要用户交互,长期运行的任务，Service 并不是在单独进程中运行,也是运行在应用程序进程的主线程中,在执行具体耗时任务过程中要手动开启子线程,应用程序进程被杀死,所有依赖该进程的服务也会停止运行

## 目录

-[生命周期](#生命周期)

-[启动方式](#启动方式)

-[Service 通信](#Service通信)

-[IntentService](#IntentService)

-[ForegroundService](#ForegroundService)

## 生命周期

Service 有两种启动服务的方式，对应的生命周期也不一致

![Service生命周期](/Resource/Image/service_lifecycle.png)

**onCreate**
首次创建服务时，系统将调用此方法。如果服务已在运行，则不会调用此方法，该方法只调用一次

**onStartCommand**
当另一个组件通过调用 startService()请求启动服务时，系统将调用此方法

**onDestroy**
当服务不再使用且将被销毁时，系统将调用此方法

**onBind**
当另一个组件通过调用 bindService()与服务绑定时，系统将调用此方法

**onUnbind**
当另一个组件通过调用 unbindService()与服务解绑时，系统将调用此方法

**onRebind**
当旧的组件与服务解绑后，另一个新的组件与服务绑定，onUnbind()返回 true 时，系统将调用此方法

## 启动方式

通过上面的生命周期我们可知道，启动 Service 的方式有两种分别是 startService 和 bindService，接下来我们来分别介绍一下

###### startService

1.在 AndroidManifest.xml 注册我们所要写的 Service，我们这里以 AService 为例

```
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.madreain"
    >

    ……

        <service android:name="com.madreain.AService" >
        </service>
    </application>

</manifest>
```

2.AService 继承 Service 的相关代码

```
public class AService extends Service{

    @Override
    public void onCreate() {
        super.onCreate();
    }

    @Override
    public int onStartCommand(Intent intent, int flags, int startId) {
        return super.onStartCommand(intent, flags, startId);
    }

    @Nullable
    @Override
    public IBinder onBind(Intent intent) {
        return null;
    }

    @Override
    public void onDestroy() {
        super.onDestroy();
    }
}
```

我们还可以在相关的生命周期中打印 Thread.currentThread().getId(）及其相关值

3.在 MainActivity 中使用开启服务

```
Intent intent = new Intent(this, AService.class);
startService(intent);

```

4.多次调用 Service 来观察生命周期

```
//启动三个相同服务
Intent intent1 = new Intent(this, AService.class);
startService(intent1);

Intent intent2 = new Intent(this, AService.class);
startService(intent2);

Intent intent3 = new Intent(this, AService.class);
startService(intent3);

//停止服务
Intent intent4 = new Intent(this, AService.class);
stopService(intent4);

//再启动服务
Intent intent5 = new Intent(this, AService.class);
startService(intent5);

```

这时候我们可以在 Aservice 不同生命周期中打印生命周期执行函数及其 Thread ID 来分析得出：
1.Thread ID 打印出来是相同的，说明回调方法都在主线程执行的 2.多次调用 startService，onCreate 方法只被执行一次，每启动一次触发一次 onStartCommand,多次 startService 不会重复执行 onCreate 回调，但每次都会执行 onStartCommand 回调。

###### bindService

1.创建 BService

```
public class BService extends Service{

    //client 可以通过Binder获取Service实例
    public class MyBinder extends Binder {
        public BService getService() {
            return BService.this;
        }
    }

    //通过binder实现调用者client与Service之间的通信
    private MyBinder binder = new MyBinder();

    private final Random generator = new Random();

    @Override
    public void onCreate() {
        super.onCreate();
    }

    @Override
    public int onStartCommand(Intent intent, int flags, int startId) {
        return START_NOT_STICKY;
    }

    @Nullable
    @Override
    public IBinder onBind(Intent intent) {
        return binder;
    }

    @Override
    public boolean onUnbind(Intent intent) {
        return false;
    }

    @Override
    public void onDestroy() {
        super.onDestroy();
    }

    //getRandomNumber是Service暴露出去供client调用的公共方法
    public int getRandomNumber() {
        return generator.nextInt();
    }
}
```

2.在 BActivity 中使用

```
public class BActivity extends Activity implements Button.OnClickListener {

    private BService service = null;

    private boolean isBind = false;

    private ServiceConnection conn = new ServiceConnection() {
        @Override
        public void onServiceConnected(ComponentName name, IBinder binder) {
            isBind = true;
            BService.MyBinder myBinder = (BService.MyBinder)binder;
            service = myBinder.getService();
            //int num = service.getRandomNumber();
        }

        @Override
        public void onServiceDisconnected(ComponentName name) {
            isBind = false;
        }
    };

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_b);

        findViewById(R.id.btnBindService).setOnClickListener(this);
        findViewById(R.id.btnUnbindService).setOnClickListener(this);
        findViewById(R.id.btnFinish).setOnClickListener(this);
    }

    @Override
    public void onClick(View v) {
        if(v.getId() == R.id.btnBindService){
            //单击了“bindService”按钮
            Intent intent = new Intent(this, TestTwoService.class);
            intent.putExtra("from", "ActivityB");
            bindService(intent, conn, BIND_AUTO_CREATE);
        }else if(v.getId() == R.id.btnUnbindService){
            //单击了“unbindService”按钮
            if(isBind){
                unbindService(conn);
            }
        }else if(v.getId() == R.id.btnFinish){
            //单击了“Finish”按钮
            this.finish();
        }
    }
    @Override
    public void onDestroy(){
        super.onDestroy();
    }
}
```

再创建一个相同的 CActivity 去绑定 Service,然后我们去动手执行一下操作，然后在对应的生命周期中打印生命周期执行的方法以及 onServiceConnected 方法中打印调用情况
1.BActivity 去 bindService
2.CActivity 去 bindService
3.CActivity 中的 unbindService 按钮
4.BActivity 中的 unbindService 按钮

上面四种情况去操作可发现： 1.再次创建不会调用 onCreate、onBind 方法 2.只有都调用了 unbindService，BService 才会去执行 onUnbind 方法，再执行 onDestroy

补充：系统资源回收

```
public int onStartCommand(Intent intent, int flags, int startId) {
  return START_NOT_STICKY | START_STICKY | START_REDELIVER_INTENT;
}
```

START_NOT_STICKY
当系统因回收资源而销毁了 Service，当资源再次充足时不再自动启动 Service，除非有未处理的 Intent 准备发送。

START_STICKY
当系统因回收资源而销毁了 Service，当资源再次充足时自动启动 Service。而且再次调用 onStartCommand() 方法，但是不会传递最后一次的 Intent，相反系统在回调 onStartCommand() 的时候会传一个空 Intent，除非有未处理的 Intent 准备发送。

START_REDELIVER_INTENT
当系统因回收资源而销毁了 Service，当资源再次充足时自动启动 Service，并且再次调用 onStartCommand() 方法，并会把最后一次 Intent 再次传递给 onStartCommand()，相应的在队列里的 Intent 也会按次序一次传递。此模式适用于下载等服务。

说到这就要说一下保活后台服务的相关方法：
1).提高优先级

```
<!-- 为防止Service被系统回收，可以尝试通过提高服务的优先级解决，1000是最高优先级，数字越小，优先级越低 -->
android:priority="1000"
```

2).把 service 写成系统服务，将不会被回收：

在 Manifest.xml 文件中设置 persistent 属性为 true，则可使该服务免受 out-of-memory killer 的影响。但是这种做法一定要谨慎，系统服务太多将严重影响系统的整体运行效率。

3).将服务改成前台服务 foreground service：

重写 onStartCommand 方法，使用 StartForeground(int,Notification)方法来启动 service。

注：一般前台服务会在状态栏显示一个通知，最典型的应用就是音乐播放器，只要在播放状态下，就算休眠也不会被杀，如果不想显示通知，只要把参数里的 int 设为 0 即可。

同时，对于通过 startForeground 启动的 service，onDestory 方法中需要通过 stopForeground(true)来取消前台运行状态。

这个方案将在-[ForegroundService](#ForegroundService)细说

4).利用 Android 的系统广播

利用 Android 的系统广播检查 Service 的运行状态，如果被杀掉，就再起来，系统广播是 Intent.ACTION_TIME_TICK，这个广播每分钟发送一次，我们可以每分钟检查一次 Service 的运行状态，如果已经被结束了，就重新启动 Service。

## Service 通信

Activity 与 Service 进行通信有四种方式

######通过 Intent 传递参数

在 MainActivity 中传递参数

```
intent = new Intent(MainActivity.this, MyService.class);
intent.putExtra("data", object);
startService(intent);
```

在 MyService 的 onStartCommand 接受参数

```
@Override
public int onStartCommand(final Intent intent, int flags, int startId) {
    data = intent.getStringExtra("data");
    return super.onStartCommand(intent, flags, startId);
}

```

######Binder 类

MyService 中我们创建一个 Binder 类，让其实现 android.os.Binder 类，并且定义一个方法 setData，然后我们通过 onBind（）方法将其对象返回 MainActivity。

设置 MyService 中设置参数数据的方法

```
public class MyService extends Service {
    private String data;
    public MyService() {
    }

    @Override
    public IBinder onBind(Intent intent) {
       return new Binder();
    }

    public class Binder extends android.os.Binder{
        public void setData(String data){
            MyService.this.data = data;
        }
    }
    @Override
    public void onCreate() {
        super.onCreate();
    }

    @Override
    public void onDestroy() {
        super.onDestroy();
    }
    @Override
    public boolean onUnbind(Intent intent) {
        return super.onUnbind(intent);
    }

}
```

在 MainActivity 中实现 ServiceConnection 方法，获取到 MyService 中返回的 Binder 对象，接着我们通过 Binder 对象调用它的方法 setData 向其传递数据

```
public class MainActivity extends AppCompatActivity implements View.OnClickListener, ServiceConnection {

    private Intent intent;
    private MyService.Binder myBinder = null;//①

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        intent = new Intent(MainActivity.this, MyService.class);
        findViewById(R.id.btySend).setOnClickListener(this);

    }

    @Override
    public void onClick(View view) {
        switch (view.getId()) {
            case R.id.btySend://想MyService传递数据
                if (myBinder != null) {
                    myBinder.setData("传递能想传递的数据");//③
                }
                break;
            default:
                break;
        }
    }
    //一旦绑定成功就会执行该函数
    @Override
    public void onServiceConnected(ComponentName componentName, IBinder iBinder) {
        myBinder = (MyService.Binder) iBinder;//②
    }

    @Override
    public void onServiceDisconnected(ComponentName componentName) {

    }

}

```

注意 ①②③ 的顺序

######接口回掉 CallBack
在 MyService 设置 CallBack，可用于 Service 向 activity 传递参数，监听服务中的进程的变化

```
public class MyService extends Service {
    private Boolean myflags = false;
    private String data ;
    private Callback callback;
    private static final String TAG = "MyService";
    public MyService() {
    }

    @Override
    public IBinder onBind(Intent intent) {
       return new Binder();
    }

    public class Binder extends android.os.Binder{
        public void setData(String data){
            MyService.this.data = data;
        }
        public MyService getMyService(){
            return MyService.this;
        }
    }

    @Override
    public void onCreate() {
        super.onCreate();
        myflags = true;
        new Thread(){
            @Override
            public void run() {
                super.run();
                int i =1;
                while(myflags){
                    //System.out.println("程序正在运行.....");
                    try {
                        String str = i + ":" +data;
                        Log.d(TAG, str);
                        if (callback != null){
                            callback.onDataChange(str);
                        }
                        sleep(1000);
                        i++;
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                        Toast.makeText(MyService.this, "出错了", Toast.LENGTH_SHORT).show();
                    }
                }
                Log.d(TAG, "服务器已停止");
            }
        }.start();
    }

    @Override
    public int onStartCommand(final Intent intent, int flags, int startId) {
        data = intent.getStringExtra("data");
        return super.onStartCommand(intent, flags, startId);
    }

    @Override
    public void onDestroy() {
        super.onDestroy();
        myflags = false;
    }

    @Override
    public boolean onUnbind(Intent intent) {
        return super.onUnbind(intent);
    }

    public void setCallback(Callback callback) {
        this.callback = callback;
    }

    public Callback getCallback() {
        return callback;
    }

    public static interface Callback{
        void onDataChange(String data);
    }
}

```

在 MainActivity 中我们可以通过 onServiceConnected 方法中的 iBinder 对象来实现 MyService 中 Callback 的接口，在 Android 中有一个安全机制，辅助线程是不能直接修改主线程中的数据的，因此我们需要再定义一个 Hander 对象。

```
public class MainActivity extends AppCompatActivity implements  ServiceConnection {

    private MyService.Binder myBinder = null;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
    }

    //一旦绑定成功就会执行该函数
    @Override
    public void onServiceConnected(ComponentName componentName, IBinder iBinder) {
        myBinder = (MyService.Binder) iBinder;
        myBinder.getMyService().setCallback(new MyService.Callback(){
            @Override
            public void onDataChange(String data) {
                Message msg = new Message();
                Bundle b = new Bundle();
                b.putString("data",data);
                msg.setData(b);
                hander.sendMessage(msg);
            }
        });
    }

    @Override
    public void onServiceDisconnected(ComponentName componentName) {

    }

    private Handler hander = new Handler(){
        @Override
        public void handleMessage(Message msg) {
            super.handleMessage(msg);
            //获取变化的数据 msg.getData().getString("data")
        }
    };
}

```

######broadcast

通过广播的的形式，可参考[BroadcastReciver 详解](10004-BroadcastReciver详解.md)

## IntentService

IntentService 是继承并处理异步请求的一个类，在 IntentService 内有一个工作线程来处理耗时操作，启动 IntentService 的方式和启动传统的 Service 一样，同时，当任务执行完后，IntentService 会自动停止，而不需要我们手动去控制或 stopSelf()。另外，可以启动 IntentService 多次，而每一个耗时操作会以工作队列的方式在 IntentService 的 onHandleIntent 回调方法中执行，并且，每次只会执行一个工作线程，执行完第一个再执行第二个，以此类推。

查看 IntentService 类的部分源码

```
@Override
    public void onCreate() {
        // TODO: It would be nice to have an option to hold a partial wakelock
        // during processing, and to have a static startService(Context, Intent)
        // method that would launch the service & hand off a wakelock.

        super.onCreate();
        HandlerThread thread = new HandlerThread("IntentService[" + mName + "]");
        thread.start();

        mServiceLooper = thread.getLooper();
        mServiceHandler = new ServiceHandler(mServiceLooper);
    }


ServiceHandler相关代码

private final class ServiceHandler extends Handler {
        public ServiceHandler(Looper looper) {
            super(looper);
        }

        @Override
        public void handleMessage(Message msg) {
            onHandleIntent((Intent)msg.obj);
            stopSelf(msg.arg1);
        }
    }
```

从源码中看，onCreate 中 thread.start()开启工作线程，thread.getLooper()单独的消息队列，通过 handler 中的 handleMessage 方法调用 onHandleIntent 执行处理任务

在实际应用中我们继承 IntentService，重写 onHandleIntent 方法来执行异步耗时操作

```
public class MIntentService extends IntentService {

    public MIntentService(){
        super("MIntentService");
    }

    /**
     * Creates an IntentService.  Invoked by your subclass's constructor.
     * @param name Used to name the worker thread, important only for debugging.
     */
    public MIntentService(String name) {
        super(name);
    }

    @Override
    public void onCreate() {
        super.onCreate();
    }

    @Override
    public int onStartCommand(Intent intent, int flags, int startId) {
        return super.onStartCommand(intent, flags, startId);
    }

    @Override
    protected void onHandleIntent(Intent intent) {
        //TODO耗时操作  实际应用有后台静默上传数据

    }

    @Override
    public void onDestroy() {
        super.onDestroy();
    }
}
```

## ForegroundService

ForegroundService 前台服务是那些被认为用户知道（用户所认可的）且在系统内存不足的时候不允许系统杀死的服务。前台服务必须给状态栏提供一个通知，它被放到正在运行(Ongoing)标题之下——这就意味着通知只有在这个服务被终止或从前台主动移除通知后才能被解除

应用场景：在一般情况下，Service 几乎都是在后台运行，一直默默地做着辛苦的工作。但这种情况下，后台运行的 Service 系统优先级相对较低，当系统内存不足时，在后台运行的 Service 就有可能被回收。
那么，如果我们希望 Service 可以一直保持运行状态且不会在内存不足的情况下被回收时，可以选择将需要保持运行的 Service 设置为前台服务。我们可以以常见的音乐播放器举例说明

创建音乐服务

```
public class MusicPlayerService extends Service {
　　
　　private static final String TAG = MusicPlayerService.class.getSimpleName();
　　
　　@Override
　　public void onCreate() {
  　　  super.onCreate();
    　　Log.d(TAG, "onCreate()");
　　}
　　
　　@Override
　　public int onStartCommand(Intent intent, int flags, int startId) {
　　　　Log.d(TAG, "onStartCommand()");
　　}
　　
　　@Override
　　public IBinder onBind(Intent intent) {
    　　　Log.d(TAG, "onBind()");
    　　　// TODO: Return the communication channel to the service.
　　　　throw new UnsupportedOperationException("Not yet implemented");
　　}
｝
```

然后创建 Notification，在 Service 的 onStartCommand 中添加如下代码：

```
@Override
public int onStartCommand(Intent intent, int flags, int startId) {
　　Log.d(TAG, "onStartCommand()");
　　// 在API11之后构建Notification的方式
　　Notification.Builder builder = new Notification.Builder
　　　　(this.getApplicationContext()); //获取一个Notification构造器
　　Intent nfIntent = new Intent(this, MainActivity.class);
　　
　　builder.setContentIntent(PendingIntent.
　　　　getActivity(this, 0, nfIntent, 0)) // 设置PendingIntent
　　　　.setLargeIcon(BitmapFactory.decodeResource(this.getResources(),
　　　　　　R.mipmap.ic_large)) // 设置下拉列表中的图标(大图标)
　　　　.setContentTitle("下拉列表中的Title") // 设置下拉列表里的标题
　　　　.setSmallIcon(R.mipmap.ic_launcher) // 设置状态栏内的小图标
　　　　.setContentText("要显示的内容") // 设置上下文内容
　　　　.setWhen(System.currentTimeMillis()); // 设置该通知发生的时间
　　
　　Notification notification = builder.build(); // 获取构建好的Notification
　　notification.defaults = Notification.DEFAULT_SOUND; //设置为默认的声音
}
```

在完成 Notification 通知消息的构建后，在 Service 的 onStartCommand 中可以使用 startForeground 方法来让 Android 服务运行在前台。

```
// 参数一：唯一的通知标识；参数二：通知消息。
startForeground(110, notification);// 开始前台服务

```

如果需要停止前台服务，可以使用 stopForeground 来停止正在运行的前台服务。

```
@Override
public void onDestroy() {
　　Log.d(TAG, "onDestroy()");
　　stopForeground(true);// 停止前台服务--参数：表示是否移除之前的通知
　　super.onDestroy();
}

```
