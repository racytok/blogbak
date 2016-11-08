---
title: Android 6.0 动态权限管理
date: 2016-09-08 18:28:57
tags: Android
categories: Android技术文章
---

## 一.Android6.0权限新特性
1.	在Android 6.0版本之前
权限都是一条龙服务的，只要用户安装完，AndroidManifest清单上申请的权限都会被系统默认授权，并且授权后也撤销不了。这样的弊端在哪里呢？有些权限可能用户觉得不需要，比如他不想有通知的权限，不想受到通知的干扰，那么他就不能屏蔽通知，就是不需要的权限，他去不掉，自主权不在他那边。还有一些情况是，一些恶意程序，会利用这个权限默认授权，进行恶意获取用户数据和攻击。所以Android 6.0版本，一方面让用户更加容易的控制自己的隐私，一方面需要重新适配应用权限。

2.	Android 6.0
采用新的权限模型，只有在需要权限的时候，才告知用户是否授权，是在runtime时候授权，而不是在原来安装的时候 ，同时默认情况下每次在运行时打开页面时候，需要先检查是否有所需要的权限申请。这样的用户的自主性提高很多，比如用户可以给APP赋予摄像的权限，但是可以拒绝记录设备位置的权限，就是怕位置信息上传等等。

3.	在API 23中，权限满足的标准流程：
![流程图](http://p1.bpimg.com/1949/aedf4c681e933b22.jpg)
但这里有个问题，那就是在系统授权弹窗环节，提醒框会有个不再提示的复选框，如果用户点击不太提示，并拒绝授权，那么再下次授权的时候，系统授权弹窗的提示框就不会在提示，所以我们很有必要需要自定义权限弹窗提示框，那么流程图就变成如下了。

![流程图](http://p1.bpimg.com/1949/60b6ad0b8f369cce.jpg)
## 二.权限类型

![流程图](http://p1.bpimg.com/1949/e5f5bc94aa50d9b4.jpg)
我们可以看到整个权限里，可以分为系统权限和特殊权限授权。系统权限中，又分为normal和dangerous类型。
 normal：这个权限类型并不直接威胁到用户的隐私，可以直接在manifest清单里注册，系统会帮我们默认授权的。
 dangerous：这个可以直接给app访问用户一些敏感的数据，不仅需要在manifest清单里注册，同时在使用的时候，需要向系统请求授权。
 值得注意一点，这里有特殊权限授权的区别，分别是SYSTEM_ALERT_WINDOW 和 WRITE_SETTINGS，虽然这两个权限也是属于dangerous权限类型，但是这两个授权请求方式和其他dangerous权限是不一样的，需要特殊处理 。
（1）	Normal权限（正常权限）
```java
		ACCESS_LOCATION_EXTRA_COMMANDS
ACCESS_NETWORK_STATE
ACCESS_NOTIFICATION_POLICY
ACCESS_WIFI_STATE
BLUETOOTH
BLUETOOTH_ADMIN
BROADCAST_STICKY
CHANGE_NETWORK_STATE
CHANGE_WIFI_MULTICAST_STATE
CHANGE_WIFI_STATE
DISABLE_KEYGUARD
EXPAND_STATUS_BAR
GET_PACKAGE_SIZE
INSTALL_SHORTCUT
INTERNET
KILL_BACKGROUND_PROCESSES
MODIFY_AUDIO_SETTINGS
NFC
READ_SYNC_SETTINGS
READ_SYNC_STATS
RECEIVE_BOOT_COMPLETED
REORDER_TASKS
REQUEST_INSTALL_PACKAGES
SET_ALARM
SET_TIME_ZONE
SET_WALLPAPER
SET_WALLPAPER_HINTS
TRANSMIT_IR
UNINSTALL_SHORTCUT
USE_FINGERPRINT
VIBRATE
WAKE_LOCK
WRITE_SYNC_SETTINGS
```
（2）dangerous权限（危险权限）
```java
group:android.permission-group.CONTACTS
  permission:android.permission.WRITE_CONTACTS
  permission:android.permission.GET_ACCOUNTS
  permission:android.permission.READ_CONTACTS

group:android.permission-group.PHONE
  permission:android.permission.READ_CALL_LOG
  permission:android.permission.READ_PHONE_STATE
  permission:android.permission.CALL_PHONE
  permission:android.permission.WRITE_CALL_LOG
  permission:android.permission.USE_SIP
  permission:android.permission.PROCESS_OUTGOING_CALLS
  permission:com.android.voicemail.permission.ADD_VOICEMAIL

group:android.permission-group.CALENDAR
  permission:android.permission.READ_CALENDAR
  permission:android.permission.WRITE_CALENDAR

group:android.permission-group.CAMERA
  permission:android.permission.CAMERA

group:android.permission-group.SENSORS
  permission:android.permission.BODY_SENSORS

group:android.permission-group.LOCATION
  permission:android.permission.ACCESS_FINE_LOCATION
  permission:android.permission.ACCESS_COARSE_LOCATION

group:android.permission-group.STORAGE
  permission:android.permission.READ_EXTERNAL_STORAGE
  permission:android.permission.WRITE_EXTERNAL_STORAGE

group:android.permission-group.MICROPHONE
  permission:android.permission.RECORD_AUDIO

group:android.permission-group.SMS
  permission:android.permission.READ_SMS
  permission:android.permission.RECEIVE_WAP_PUSH
  permission:android.permission.RECEIVE_MMS
  permission:android.permission.RECEIVE_SMS
  permission:android.permission.SEND_SMS
  permission:android.permission.READ_CELL_BROADCASTS
```
## 三.动态权限管理
Step 1: 第一步和以前没什么两样，依然是在AndroidManifest添加权限声明。
Step 2: 检查权限：
```java
if (ContextCompat.checkSelfPermission(thisActivity,
                Manifest.permission.READ_CONTACTS)
        != PackageManager.PERMISSION_GRANTED) {
}else{
    //
}
```
> 
ContextCompat.checkSelfPermission，主要用于检测某个权限是否已经被授予，方法返回值为PackageManager.PERMISSION_DENIED或者PackageManager.PERMISSION_GRANTED。当返回DENIED就需要进行申请授权了。

Step 3:申请授权
```java
ActivityCompat.requestPermissions(thisActivity,
         new String[]{Manifest.permission.READ_CONTACTS},
                MY_PERMISSIONS_REQUEST_READ_CONTACTS);
```
> 
该方法是异步的，第一个参数是Context；第二个参数是需要申请的权限的字符串数组；第三个参数为requestCode，主要用于回调的时候检测。可以从方法名requestPermissions以及第二个参数看出，是支持一次性申请多个权限的，系统会通过对话框逐一询问用户是否授权。

Step 4：处理权限申请回调
```java
@Override
public void onRequestPermissionsResult(int requestCode,
        String permissions[], int[] grantResults) {
    switch (requestCode) {
        case MY_PERMISSIONS_REQUEST_READ_CONTACTS: {
            //如果请求被取消，则会返回空
            if(grantResults.length>0&&grantResults[0]== PackageManager.PERMISSION_GRANTED)
 {

                // 权限已同意，在这里可以进行联系人查看操作

            } else {

                // 权限被用户拒绝
            }
            return;
        }
    }
}
```
> 
对于权限的申请结果，首先验证requestCode定位到你的申请，然后验证grantResults对应于申请的结果，这里的数组对应于申请时的第二个权限字符串数组。如果你同时申请两个权限，那么grantResults的length就为2，分别记录你两个权限的申请结果。如果申请成功，就可以做你的事情了~

Ok，到此权限申请的基本步骤就完成了。然而大家发现这些都是系统的提示，如何让用户能接受权限申请呢？
```java
// Should we show an explanation?
if (ActivityCompat.shouldShowRequestPermissionRationale(thisActivity,
        Manifest.permission.READ_CONTACTS)) 
    // Show an expanation to the user *asynchronously* -- don't block
    // this thread waiting for the user's response! After the user
    // sees the explanation, try again to request the permission.

}
```
> 
这个API主要用于给用户一个申请权限的解释，该方法只有在用户在上一次已经拒绝过你的这个权限申请。也就是说，用户已经拒绝一次了，你又弹个授权框，你需要给用户一个解释，为什么要授权，则使用该方法。

综上，一个完整的使用过程就有了。
```java
// Here, thisActivity is the current activity
if (ContextCompat.checkSelfPermission(thisActivity,
                Manifest.permission.READ_CONTACTS)
        != PackageManager.PERMISSION_GRANTED) {

    // Should we show an explanation?
    if (ActivityCompat.shouldShowRequestPermissionRationale(thisActivity,
            Manifest.permission.READ_CONTACTS)) {

        // Show an expanation to the user *asynchronously* -- don't block
        // this thread waiting for the user's response! After the user
        // sees the explanation, try again to request the permission.

    } else {

        // No explanation needed, we can request the permission.

        ActivityCompat.requestPermissions(thisActivity,
                new String[]{Manifest.permission.READ_CONTACTS},
                MY_PERMISSIONS_REQUEST_READ_CONTACTS);

        // MY_PERMISSIONS_REQUEST_READ_CONTACTS is an
        // app-defined int constant. The callback method gets the
        // result of the request.
    }
}
```
## 附：一些流行的权限管理库
> *  	[https://github.com/hongyangAndroid/MPermissions](https://github.com/hongyangAndroid/MPermissions)
> *  	[https://github.com/mylhyl/AndroidAcp](https://github.com/mylhyl/AndroidAcp) 【推荐】
> *  	[https://github.com/hotchemi/PermissionsDispatcher](https://github.com/hotchemi/PermissionsDispatcher)
> *  	[https://github.com/ParkSangGwon/TedPermission](https://github.com/ParkSangGwon/TedPermission)
