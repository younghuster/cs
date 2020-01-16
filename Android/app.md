# 四大组件

- Activity
- Service
- Broadcast
- ContentProvider

## Activity
- **Activity提供一个界面, 让用户点击和各种滑动操作**

   以电子邮件应用为例
   - 一个显示新电子邮件列表的 Activity
   - 一个用于撰写电子邮件的 Activity
   - 一个用于阅读电子邮件的 Activity

- 生命周期
![Activity](http://ww1.sinaimg.cn/large/005Kyrj9ly1gaxn8z37a3j30f20iudg0.jpg)

- 4种状态

|状态|解释|
|----|----|
|running|表明Activity处于活动（完全可见）状态，用户可以与屏幕进行交互，此时，Activity处于栈顶|
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
|----------|----|
|standard|默认的启动模式，每次启动Activity的时候都会创建一个新的实例。不会复用Activity，对内存消耗较大|
|singleTop|栈顶复用模式，如果在任务的栈顶正好存在该Activity的实例， 就重用该实例，否者就会创建新的实例并放入栈顶(即使栈中已经存在该Activity实例，只要不在栈顶，都会创建实例)|
|singletask|栈内复用模式, 如果在栈中已经有该Activity的实例，就重用该实例(会调用实例的onNewIntent())。重用时，会让该实例回到栈顶，因此在它上面的实例将会被移除栈。如果栈中不存在该实例，将会创建新的实例放入栈中|
|singleInstance|单一实例，独享一个任务栈，整个手机操作系统里面只有一个实例存在。用的较少|

## Service
- Service是一种能在后台执行耗时任务的没有界面显示的组件
- Service运行在主线程(UI线程)的
- Service本身不能做耗时操作，而是通过子线程去完成

## Broadcast
## ContentProvider
