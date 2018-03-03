---
title:  "OpenSimulator-快捷打开模拟器、真机沙盒文件"
date:   2017-12-05 15:49:25
categories: [iOS]
tags: [macOS,sandbox,simulator]
---


实现原理比较简单,模拟器列表只显示当前启动的模拟器

[OpenSimulator](https://github.com/XieXieZhongxi/OpenSimulator)

``` ruby
//获取当前活动的window集合
CG_EXTERN CFArrayRef __nullable CGWindowListCopyWindowInfo(CGWindowListOption option,CGWindowID relativeToWindow)
```

``` ruby
//这个就是模拟器对应的window信息
{
    kCGWindowAlpha = 1;
    kCGWindowBounds =     {
        Height = 818;
        Width = 421;
        X = 299;
        Y = 22;
    };
    kCGWindowLayer = 0;
    kCGWindowMemoryUsage = 1128;
    kCGWindowName = "iPhone 7 - iOS 11.1";
    kCGWindowNumber = 13808;
    kCGWindowOwnerName = Simulator;
    kCGWindowOwnerPID = 44458;
    kCGWindowSharingState = 1;
    kCGWindowStoreType = 1;
}
```
根据window信息前往`/Library/Developer/CoreSimulator/Devices/device_set.plist`查找对应的`UDID`,然后找到所安装的所有应用.
<div align="center">
    <img src="http://upload-images.jianshu.io/upload_images/336727-c2ad01fcedf21481.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240" style="zoom:45%">
</div>

关于真机的沙盒路径,使用了`webServer`
当连接了设备,则会显示设备的用户名称
<div align="center">
    <img src="http://upload-images.jianshu.io/upload_images/336727-4b39c566d14dfb55.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240" style="zoom:45%">
</div>

选择`Usage`则会打开一个文件夹,查看`README.md`使用说明
`GCDWebUploader.bundle`、`WebServer.framework`添加到工程项目
调用`[WebServer connect];`就可以开启`webServer `服务.
<div align="center">
    <img src="http://upload-images.jianshu.io/upload_images/336727-a9e0b49a50f4a709.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240" style="zoom:45%">
</div>


`Open Web Server`则打开设备的沙盒服务器
<div align="center">
    <img src="http://upload-images.jianshu.io/upload_images/336727-7d8c5be0d6280097.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240" style="zoom:45%">
</div>







