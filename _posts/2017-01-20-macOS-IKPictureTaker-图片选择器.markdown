---
title:  "macOS IKPictureTaker 图片选择器"
date:   2017-01-20 15:25:25
categories: [iOS]
tags: [macOS,IKPictureTaker]
---

OS X 有一个特有的控件叫做 [IKPictureTaker](https://developer.apple.com/reference/quartz/ikpicturetaker)，允许用户从计算机上选择一张图片，或者从摄像头捕捉一张图片。当用户选择了图片之后，这个控件会调用指定的代理方法。

<div align="center">
    <img src="http://upload-images.jianshu.io/upload_images/336727-62f743a2f5278b7d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240" style="zoom:45%">
</div>

<div align="center">
    <img src="http://upload-images.jianshu.io/upload_images/336727-293a26b295c2e96d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240" style="zoom:45%">
</div>

<div align="center">
    <img src="http://upload-images.jianshu.io/upload_images/336727-db962d2ac6346e6f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240" style="zoom:45%">
</div>


1.API
``` ruby
-(void) beginPictureTakerSheetForWindow:(NSWindow *)aWindow withDelegate:(id) delegate didEndSelector:(SEL) didEndSelector contextInfo:(void *) contextInfo; 
```

2.实现
``` ruby
#import <Quartz/Quartz.h>

[[IKPictureTaker pictureTaker] beginPictureTakerSheetForWindow:[NSApplication sharedApplication].keyWindow withDelegate:self didEndSelector:@selector(pictureTakerDidEnd:returnCode:contextInfo:) contextInfo:nil];
```
3.代理方法
``` ruby
- (void)pictureTakerDidEnd:(IKPictureTaker *)picker returnCode:(NSInteger)code contextInfo:(void*)contextInfo{
    NSImage * image = [picker outputImage];
    if (image) {
        self.imageView.image = image;
    }
    NSLog(@"NSImage:%@",image);
    NSLog(@"code:%ld",code);
}
```
