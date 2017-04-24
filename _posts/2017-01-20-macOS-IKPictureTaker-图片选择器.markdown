OS X 有一个特有的控件叫做 [IKPictureTaker](https://developer.apple.com/reference/quartz/ikpicturetaker)，允许用户从计算机上选择一张图片，或者从摄像头捕捉一张图片。当用户选择了图片之后，这个控件会调用指定的代理方法。
![系统图片选取](http://upload-images.jianshu.io/upload_images/336727-62f743a2f5278b7d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![摄像头选取](http://upload-images.jianshu.io/upload_images/336727-293a26b295c2e96d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![实现效果图](http://upload-images.jianshu.io/upload_images/336727-db962d2ac6346e6f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

1.API
```
-(void) beginPictureTakerSheetForWindow:(NSWindow *)aWindow withDelegate:(id) delegate didEndSelector:(SEL) didEndSelector contextInfo:(void *) contextInfo; 
```

2.import
```
#import <Quartz/Quartz.h>
```
3.实现代码
```
[[IKPictureTaker pictureTaker] beginPictureTakerSheetForWindow:[NSApplication sharedApplication].keyWindow withDelegate:self didEndSelector:@selector(pictureTakerDidEnd:returnCode:contextInfo:) contextInfo:nil];
```
4.代理方法
```
- (void)pictureTakerDidEnd:(IKPictureTaker *)picker returnCode:(NSInteger)code contextInfo:(void*)contextInfo{
    NSImage * image = [picker outputImage];
    if (image) {
        self.imageView.image = image;
    }
    NSLog(@"NSImage:%@",image);
    NSLog(@"code:%ld",code);
}
```
