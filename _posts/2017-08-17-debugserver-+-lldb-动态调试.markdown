---
title:  "debugserver-+-lldb-动态调试"
date:   2017-08-17 17:19:25
categories: [iOS]
tags: [越狱,debugserver,lldb]
---


##### 以调试设备界面为例,改变背景颜色、获取VPN界面UISwitch控件响应事件.
Mac通过`ssh`连接越狱设备,默认密码`alpine`
``` ruby
Nelson:~ Nelson$ ssh root@192.168.xx.xxx
```

启动`Preferences`进程,开启`1234`端口,等待任意IP地址的lldb接入
``` ruby
# debugserver -x backboard *:1234 /Applications/Preferences.app/Preferences
```
``` ruby
Nelson-iPad:~ root# debugserver -x backboard *:1234 /Applications/Preferences.app/Preferences
debugserver-@(#)PROGRAM:debugserver  PROJECT:debugserver-340.3.51.1
 for arm64.
Listening to port 1234 for a connection from *...
```
Mac启动新窗口终端,进入Xcode的lldb调试模式
``` ruby
# /Applications/Xcode.app/Contents/Developer/usr/bin/lldb
```
``` ruby
Nelson:~ Nelson$ /Applications/Xcode.app/Contents/Developer/usr/bin/lldb
(lldb) 
```
连接正在等待的debugserver
``` ruby
# process connect connect://192.168.xx.xxx:1234
```
``` ruby
(lldb) process connect connect://192.168.xx.xxx:1234
Process 6529 stopped
* thread #1, queue = 'com.apple.main-thread', stop reason = signal SIGSTOP
    frame #0: 0x00000001819f54bc libsystem_kernel.dylib`mach_msg_trap + 8
libsystem_kernel.dylib`mach_msg_trap:
->  0x1819f54bc <+8>: ret    

libsystem_kernel.dylib`mach_msg_overwrite_trap:
    0x1819f54c0 <+0>: mov    x16, #-0x20
    0x1819f54c4 <+4>: svc    #0x80
    0x1819f54c8 <+8>: ret    
(lldb)  
```
打印所有界面层次
``` ruby
(lldb) po [[[UIApplication sharedApplication] keyWindow] recursiveDescription]
```

<div align="center">
    <img src="http://upload-images.jianshu.io/upload_images/336727-50bbeb4f622abefa.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240" style="zoom:45%">
</div>

搜索`UITableView:`获取内存地址为`0x13e051800`

<div align="center">
    <img src="http://upload-images.jianshu.io/upload_images/336727-1fa3813e74637645.jpeg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240" style="zoom:45%">
</div>

修改`UITableView(0x13e051800)`的背景颜色为`yellowColor`
``` ruby
(lldb) po [(UITableView*)0x13e051800 setBackgroundColor:[UIColor yellowColor]]
```
现在界面处理调试状态,需要手动刷新下界面
``` ruby
(lldb) e (void)[CATransaction flush]
```

<div align="center">
    <img src="http://upload-images.jianshu.io/upload_images/336727-e0bc3cae51adee44.PNG?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240" style="zoom:45%">
</div>

修改另外一个`UITableView(0x13e8ac400)`的背景颜色
``` ruby
(lldb) po [(UITableView*)0x13e8ac400 setBackgroundColor:[UIColor greenColor]]
(lldb) e (void)[CATransaction flush]
```

<div align="center">
    <img src="http://upload-images.jianshu.io/upload_images/336727-93d875c44f187c4a.PNG?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240" style="zoom:45%">
</div>

获取`VPN`界面的`UISwitch`的`allTargets `
<div align="center">
    <img src="http://upload-images.jianshu.io/upload_images/336727-bb1a1b98657c72d3.PNG?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240" style="zoom:45%">
</div>

``` ruby
(lldb) po [(UISwitch *)0x13f263980 allTargets]
```
``` ruby
(lldb) po [(UISwitch *)0x13f263980 allTargets]
{(
    <VPNToggleCell: 0x13e0c3400; baseClass = UITableViewCell; frame = (0 55.5; 594.5 45); text = '状态'; autoresize = W; tag = 6; layer = <CALayer: 0x13f004a80>>
)}

(lldb) 
```
此处的`Target`为上一步获取到的`VPNToggleCell(0x13e0c3400)`
``` ruby
(lldb) po [(UISwitch *)0x13f263980 actionsForTarget:(id)0x13e0c3400 forControlEvent:0]
```
``` ruby
(lldb) po [(UISwitch *)0x13f263980 actionsForTarget:(id)0x13e0c3400 forControlEvent:0]
<__NSArrayM 0x13ddd9ca0>(
controlChanged:
)
(lldb)
``` 
获取到了`UISwitch`的响应方法为`controlChanged:`,接下来为`UISwitch`的点击添加断点
``` ruby
(lldb) br set -n "-[VPNToggleCell controlChanged:]"
Breakpoint 1: no locations (pending).
WARNING:  Unable to resolve breakpoint to any actual locations.
```
添加断点失败了,也就是说明`controlChanged:`这个方法不属于`VPNToggleCell `这个类,于是查找`Runtime Header`,找到了`PSControlTableCell`这个类

[PSControlTableCell.h](https://github.com/NSExceptional/iOS-9.0.2-Headers/blob/54364bb0dbcca5d316609e7a17af88ff97566631/System/Library/PrivateFrameworks/Preferences.framework/PSControlTableCell.h)
<div align="center">
    <img src="http://upload-images.jianshu.io/upload_images/336727-29ed85ead16366d6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240" style="zoom:45%">
</div>


``` ruby
(lldb) br set -n "-[PSControlTableCell controlChanged:]"
```
``` ruby
(lldb) br set -n "-[PSControlTableCell controlChanged:]"
Breakpoint 3: where = Preferences`-[PSControlTableCell controlChanged:], address = 0x0000000189488618
(lldb) 
```
断点添加成功了,查看下所有的断点列表
``` ruby
(lldb) br list
```
``` ruby
(lldb) br list
Current breakpoints:
3: name = '-[PSControlTableCell controlChanged:]', locations = 1, resolved = 1, hit count = 0
  3.1: where = Preferences`-[PSControlTableCell controlChanged:], address = 0x0000000189488618, resolved, hit count = 0 

(lldb)
```
按需求可以对断点进行以下操作:
`3针对以上的断点序号`
禁用断点:`(lldb) br dis 3`
启用断点:`(lldb) br en 3`
删除断点:`(lldb) br del 3`

退出调试状态
``` ruby
(lldb) c
```
此时界面可以进行操作了,点击`VPN`界面的`UISwitch`执行了断点操作,再次进入了调试模式
``` ruby
(lldb) c
Process 6529 resuming
Process 6529 stopped
* thread #1, queue = 'com.apple.main-thread', stop reason = breakpoint 3.1
    frame #0: 0x0000000189488618 Preferences`-[PSControlTableCell controlChanged:]
Preferences`-[PSControlTableCell controlChanged:]:
->  0x189488618 <+0>:  stp    x24, x23, [sp, #-0x40]!
    0x18948861c <+4>:  stp    x22, x21, [sp, #0x10]
    0x189488620 <+8>:  stp    x20, x19, [sp, #0x20]
    0x189488624 <+12>: stp    x29, x30, [sp, #0x30]
(lldb)  
```
执行`c`、`s`进行下一步操作
``` ruby
(lldb) c
```
``` ruby
(lldb) n
```
进入调试模式
``` ruby
(lldb) process interrupt
```

`lldb`其他指令

| 指令 | 指令说明 | 
| :-------------: |:-------------:|
|thread list|线程列表|
|image list -o -f|进程列表|
|frame info|查看当前代码|


