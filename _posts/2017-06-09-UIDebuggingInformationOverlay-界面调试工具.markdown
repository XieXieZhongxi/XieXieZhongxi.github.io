---
layout: post
title:  "UIDebuggingInformationOverlay"
date:   2017-06-08 11:38:45 +0200
categories: jekyll update
---

##### `UIKit`添加了私有类`UIDebuggingInformationOverlay`,字面理解为`界面调试信息层`

- ![UIDebuggingInformationOverlay界面.png](http://upload-images.jianshu.io/upload_images/336727-d9f7c7f3ce0dda1c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### usage
- 因为是私有类,所有审核的时候需要设置

{% highlight objective_c %}
#ifdef DEBUG
.........
#endif
{% endhighlight %}

- 代码直接启动调试界面
{% highlight objective_c %}
#ifdef DEBUG
    Class overlayClass = NSClassFromString(@"UIDebuggingInformationOverlay");
    [overlayClass performSelector:NSSelectorFromString(@"prepareDebuggingOverlay")];
    id overlayObject = [overlayClass performSelector:NSSelectorFromString(@"overlay")];
    [overlayObject performSelector:NSSelectorFromString(@"toggleVisibility")];
#endif
{% endhighlight %}

- 手动2个手指点击状态栏启动调试界面
{% highlight objective_c %}
#ifdef DEBUG
    Class overlayClass = NSClassFromString(@"UIDebuggingInformationOverlay");
    [overlayClass performSelector:NSSelectorFromString(@"prepareDebuggingOverlay")];
#endif
{% endhighlight %}

- ##### `UIDebuggingInformationOverlay`提供了6个功能
- View Hierarchy
```
您可以检查任何视图的细节，包括其框架和实例变量。如果您有多个窗口，还可以在窗口之间切换.
```
- VC Hierarchy
```
显示了主动视图控制器的层次结构。从这里，您可以检查任何视图控制器的细节，包括其视图.
```
- Ivar Explorer
```
可让您访问UIApplication实例的变量和任何对象变量
```
- Measure
```
它可以测量屏幕元素的尺寸（以点为单位）。首先，选择是否要在“水平”或“垂直”轴上查看测量。然后在屏幕上拖动手指，使用控制台内的放大查看器来协助您
```

- ![Vertical.gif](http://upload-images.jianshu.io/upload_images/336727-984586dafdefe69c.gif?imageMogr2/auto-orient/strip)

- ![Horizontal.gif](http://upload-images.jianshu.io/upload_images/336727-f565644c4647a148.gif?imageMogr2/auto-orient/strip)

- Spec Compare
```
将屏幕截图添加到设备，然后从“规格比较”屏幕中选择。所选屏幕截图将显示在实际应用程序的顶部。然后，您可以向下拖动以减少alpha，并将规范与实际实现进行比较。
```

- `这个功能就可以实现界面与设计图的对比!!!`

- ![Spec Compare.gif](http://upload-images.jianshu.io/upload_images/336727-e4270abdd7bbf6e5.gif?imageMogr2/auto-orient/strip)

- System Color Audit
```
暂时未能获取任何信息
```


