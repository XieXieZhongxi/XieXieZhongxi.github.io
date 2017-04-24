####[NSStatusBar](https://developer.apple.com/reference/appkit/nsstatusbar) : macOS系统的顶部导航栏item
```
@interface AppDelegate (){
    NSStatusItem * statusItem;
}
```

```
- (void)applicationDidFinishLaunching:(NSNotification *)aNotification {
    // Insert code here to initialize your application
    /* 初始化 */
    statusItem = [[NSStatusBar systemStatusBar] statusItemWithLength:NSVariableStatusItemLength];
    /* 设置NSImage * /
    [statusItem.button setImage:[NSImage imageNamed:@"statusItem"]];
    /* 设置点击响应事件 */
    statusItem.action = @selector(touchStatusItem:);
}
```

####[NSPopover](https://developer.apple.com/reference/appkit/nspopover) : pop视图
```
@interface AppDelegate (){
    NSPopover * popover;
}
```

```
-(void)touchStatusItem:(NSStatusBarButton *)button{
    /* 初始化 */
    popover = [[NSPopover alloc]init];
    /* 设置动画 */
    popover.behavior = NSPopoverBehaviorTransient;
    /* 设置外观 */
    popover.appearance = [NSAppearance appearanceNamed:NSAppearanceNameVibrantLight];
    /* 设置展示视图 */
    popover.contentViewController = [[PopViewController alloc]initWithNibName:@"PopViewController" bundle:nil];
    /* 设置展示方位 */
    [popover showRelativeToRect:button.bounds ofView:button preferredEdge:NSRectEdgeMaxY];
}
```

####效果图
![NSStatusBar + NSPopover效果图](http://upload-images.jianshu.io/upload_images/336727-c9945e7b7576aa2d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
