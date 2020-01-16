# 四大组件

- Activity
- Service
- BroadcastReceiver
- ContentProvider

## Activity
- **Activity提供一个界面, 让用户点击和各种滑动操作**

   以电子邮件应用为例
   - 一个显示新电子邮件列表的 Activity
   - 一个用于撰写电子邮件的 Activity
   - 一个用于阅读电子邮件的 Activity

- 生命周期
![Activity](http://ww1.sinaimg.cn/large/005Kyrj9ly1gaxn8z37a3j30f20iudg0.jpg)


  成对调用
  - onCreate() / onDestory()
  - onStart() / onStop()
  - onResume() / onPause()

- 4种状态

|状态|解释|
|----|----|
|active/running|表明Activity处于活动（完全可见）状态，用户可以与屏幕进行交互，此时，Activity处于栈顶|
|paused|表明Activity处于失去焦点的状态（例如：被非全屏的的Activity覆盖），此时用户无法与该Activity进行交互|
|stopped|表明activity处于不可见的状态（例如：被另一个Activity完全覆盖)|
|killed|表明Activity被系统回收了|

- 典型case
  - 启动Activity
    - onCreate() -> onStart() -> onResume()
    - onStart(): 前台可见, 但不能交互
    - onResume(): 前台可见且可交互

  - Activity退居后台： 当前Activity转到新的Activity或单击Home键
    - onPause() -> onStop()

  - ActivityA中打开一个新的ActivityB
    - A的onPause()执行完成后B的onCreate()才能开始，
    - A的onStop()则是在之后才会进行的
    - 不应该在onPause()中做耗时的操作，应该尽快让B显示出来进行操作才行

  - Activity返回前台
    - onRestart() -> onStart() -> onResume()

  - Activity退居后台，且系统内存不足， 系统会杀死这个后台状态的Activity，若再次回到这个Activity
    - onCreate() -> onStart() -> onResume()

  - 锁屏与解锁屏幕
    - 锁屏调用onPause()，不调用onStop()
    - 开屏调用onResume()

  - 单击back键, 退出activity
    - onPause() -> onStop() -> onDestory()


- 启动模式

|launch mode|说明|
|-----------|----|
|standard|默认的启动模式，每次启动Activity的时候都会创建一个新的实例。不会复用Activity，对内存消耗较大|
|singleTop|栈顶复用模式，如果在任务的栈顶正好存在该Activity的实例， 就重用该实例，否者就会创建新的实例并放入栈顶(即使栈中已经存在该Activity实例，只要不在栈顶，都会创建实例)|
|singletask|栈内复用模式, 如果在栈中已经有该Activity的实例，就重用该实例(会调用实例的onNewIntent())。重用时，会让该实例回到栈顶，因此在它上面的实例将会被移除栈。如果栈中不存在该实例，将会创建新的实例放入栈中|
|singleInstance|单一实例，独享一个任务栈，整个手机操作系统里面只有一个实例存在。用的较少|

## Service
- [Google官方](https://developer.android.com/guide/components/services?hl=zh-cn)
- Service是一种能在**后台执行耗时任务**的没有界面显示的组件
- Service运行在**主线程**(UI线程)的
- Service本身不能做耗时操作，而是通过**子线程**去完成


- 生命周期

![Service](http://ww1.sinaimg.cn/large/005Kyrj9ly1gay6jimw3kj30co0ekmyj.jpg)


如果service是被开启的，那么它的活动生命周期和整个生命周期一同结束。
如果service是被绑定的，它们它的活动生命周期是在onUnbind()方法返回后结束。

- 分类
  - 运行地点
    - startService() 启动本地服务Local Service
    - bindService() 启动远程服务Remote Service

  - 运行类型
    - 前台service
    - 后台service

  - 功能
    - 可通信service
    - 不可通信service

- 如要创建服务，必须创建 Service 的子类

- Service和Thread
  - 一般会将 Service 和 Thread联合着用，即在Service中再创建一个子线程（工作线程）去处理耗时操作逻辑

## BroadcastReceiver
- 广播接收器
  - Android 广播分为两个角色：广播发送者、广播接收者
  - 监听 / 接收 应用 App 发出的广播消息，并 做出响应
  - 广播广泛应用在应用程序之间**传递信息**的机制，使用了设计模式中的**观察者模式**，通过**intent**可以传递数据。

- 应用场景
  - **同一个app**具有多个进程**不同组件**之间的消息通信
  - **不同的app之间组件**的信息通信

- 分类
  - 普通广播（Normal Broadcast）
  - 系统广播（System Broadcast）
  - 有序广播（Ordered Broadcast）
  - 粘性广播（Sticky Broadcast）
  - App应用内广播（Local Broadcast）优点：高效、安全，其实它内部是通过Handler实现的发送message实现的。

- 注册方式
  - 静态注册：在AndroidManifest.xml里通过标签声明，注册完成就一直运行
  - 动态注册：在代码中调用Context.registerReceiver（）方法，跟随activity的生命周期。

- 内部机制

  - 自定义BroadcastReceiver，并override onReceive()方法
  - 通过Binder机制向AMS进行注册
  - 广播发送者通过Binder机制向AMS发送广播
  - AMS查找符合相应条件的BroadcastReceiver的BroadcastReceiver，将广播发送到相应的消息队列中
  - 消息循环执行拿到此广播，回调BroadcastReceiver的onReceive（)方法


![BroadcastReceiver](http://ww1.sinaimg.cn/large/005Kyrj9ly1gay7os6iegj30n20aptam.jpg)

- 使用
  - 继承BroadcastReceivre基类
  - override抽象方法onReceive()方法
  - 广播可能会导致内存泄露


## ContentProvider
- 内容提供者
  - 进程间 进行数据交互 & 共享，即跨进程通信
  - 允许把自己的应用数据根据需求开放给 其他应用 进行 增、删、改、查，而不用担心因为直接开放数据库权限而带来的安全问题
  - 本质上是一个标准化的数据管道，它屏蔽了底层的数据管理和服务等细节，以标准化的方式在Android 应用间共享数据

- URI: Uniform Resource Identifier，即统一资源标识符
- 类
  - ContentProvider
  - ContentResolver

- 使用
  - 在Androidmainfest.xml 中注册
  - 自定义ContentProvider 需要继承 ContentProvider ,并实现增删改查等方法。

# Looper/Handler

创建主线程时，会自动调用ActivityThread的1个静态的main()

## Looper
- src
  - frameworks/base/core/java/android/os/Looper.java

- 成员变量
  ```
  static final ThreadLocal<Looper> sThreadLocal = new ThreadLocal<Looper>();
  @UnsupportedAppUsage
  private static Looper sMainLooper;  // guarded by Looper.class
  private static Observer sObserver;

  @UnsupportedAppUsage
  final MessageQueue mQueue;
  final Thread mThread;

  @UnsupportedAppUsage
  private Printer mLogging;
  private long mTraceTag;
  ```

- 创建
  - Looper.prepareMainLooper();
    - prepare(false);// 通过prepare()方法创建Looper实例
      - sThreadLocal.set(new Looper(quitAllowed))
        - mQueue = new MessageQueue(quitAllowed);
        - mThread = Thread.currentThread()
    - sMainLooper = myLooper();

- 使用
  - Looper.loop(); //for循环里面从mQueue读取message
    - Message msg = queue.next();
    - msg.target.dispatchMessage(msg);


- 区分

  |创建looper|说明|
  |----------|----|
  |Looper.prepareMainLooper();|为主线程创建looper|
  |Looper.prepare()|为当前线程创建looper|

- 注意
  - 每个线程至多一个looper, 它是ThreadLocal<Looper>, 即与每个线程本身进行绑定, 可通过Looper.myLooper()获取

## Handler
- src: frameworks/base/core/java/android/os/Handler.java
- 成员变量
  ```
  @UnsupportedAppUsage
  final Looper mLooper;
  final MessageQueue mQueue;
  @UnsupportedAppUsage
  final Callback mCallback;
  final boolean mAsynchronous;
  @UnsupportedAppUsage
  IMessenger mMessenger;
  ```

- 功能: **发送和处理消息**

- 引入原因
  - 多个线程直接向UI线程发送消息并更新UI不安全

- 注意
  - 使用handler前, 确保相应线程的looper已经存在!
  - 一个线程可以有多个handler


- method
  - Handler() constructor: 创建handler实例
    - 通过mLooper = Looper.myLooper();获取looper
    - 通过mQueue = mLooper.mQueue;获取mQueue

  - sendMessage(): 发送消息
    - sendMessageDelayed(msg, 0)
    - sendMessageAtTime(msg, SystemClock.uptimeMillis() + delayMillis)
    - enqueueMessage(queue, msg, uptimeMillis)
      - msg.target = this;
      - queue.enqueueMessage(msg, uptimeMillis)

  - dispatchMessage(): 处理消息
    - handleCallback(msg)
    - mCallback.handleMessage(msg)
    - handleMessage()
      - 空方法, 子类必须override用来处理消息

