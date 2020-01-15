# 四大组件

- Activity
- Service
- Broadcast
- ContentProvider

## Activity
- Activity提供一个界面, 让用户点击和各种滑动操作
- 生命周期
  - 启动Activity： onCreate()--->onStart()--->onResume()，Activity进入运行状态。
  - Activity退居后台： 当前Activity转到新的Activity界面或按Home键回到主屏： onPause()--->onStop()，进入停滞状态。
  - Activity返回前台： onRestart()--->onStart()--->onResume()，再次回到运行状态。
  - Activity退居后台，且系统内存不足， 系统会杀死这个后台状态的Activity，若再次回到这个Activity,则会走onCreate()-->onStart()--->onResume()
  - 锁定屏与解锁屏幕 只会调用onPause()，而不会调用onStop方法，开屏后则调用onResume()

![Activity](http://ww1.sinaimg.cn/large/005Kyrj9ly1gaxn8z37a3j30f20iudg0.jpg)


## Service
- Service是一种能在后台执行耗时任务的没有界面显示的组件
- Service运行在主线程(UI线程)的
- Service本身不能做耗时操作，而是通过子线程去完成

## Broadcast
## ContentProvider
