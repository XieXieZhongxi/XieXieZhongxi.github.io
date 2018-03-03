---
title:  "Charles 4.2.1 HTTPS抓包"
date:   2017-12-12 18:21:23
categories: [iOS]
tags: [Charles,HTTPS]
---



<div align="center">
	<img src="http://upload-images.jianshu.io/upload_images/4048192-12cf0cd7b90f5ca1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/658" style="zoom:50%">
</div>
[Charles](https://www.charlesproxy.com)


#### iPhone抓包
`Mac`必须与`iPhone`连接同一`WiFi`

`Proxy` -> `SSL Proxying Settings` -> `SSL Proxying` -> `Add`


<div align="center">
<img src="http://upload-images.jianshu.io/upload_images/336727-654fed64b49cec91.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240" style="zoom:45%" />
</div>
	
- Host:为需要过滤的域名地址,`*` 表示不过滤
 - Port:固定为`443`,`*`表示任意端口


查看Mac `IP`地址,iPhone添加代理

<div align="center">
<img src="http://upload-images.jianshu.io/upload_images/336727-f9123bd3d8b76fdb.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240" style="zoom:45%" />
<!-- <img src="http://upload-images.jianshu.io/upload_images/336727-2e161f7384fda365.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240" style="zoom:25%" /> -->
</div>

`Safari`访问[chls.pro/ssl](chls.pro/ssl),安装描述文件
<div align="center">
<img src="http://upload-images.jianshu.io/upload_images/336727-965cab435bd0e984.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240" style="zoom:25%" />
</div>

`设置` -> `通用` -> `关于本机` -> `证书信任设置`,开启完全信任
<div align="center">
<img src="http://upload-images.jianshu.io/upload_images/336727-230c6db30c00b8c5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240" style="zoom:25%" />
</div>

此时可以进行抓包了
<div align="center">
<img src="http://upload-images.jianshu.io/upload_images/336727-2b28ca25e5e67f48.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240" style="zoom:45%" />
</div>

#### Mac端抓包

启动`Charles`客户端
`Proxy` -> `macOS Proxy`
`Proxy` -> `SSL Proxying Settings` -> `SSL Proxying` -> `Add`

 - Host:为需要过滤的域名地址,`*`表示不过滤
 - Port:固定为`443`,`*`表示任意端口

`Help` -> `SSL Proxying` -> `Install Charles Root Certificate`
    此时会打开`钥匙串访问`安装`Charles Proxy CA`证书,双击证书,展开`信任`选项,选择`始终信任`,如果证书安装不了请搜索`Charles Proxy CA`,删除就已失效证书再进行安装操作.
 <div align="center">
<img src="http://upload-images.jianshu.io/upload_images/336727-49203130682509a5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240" style="zoom:45%" />
</div>

此时Mac端可以进行抓包了
<div align="center">
<img src="http://upload-images.jianshu.io/upload_images/336727-00b947453f0dad85.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240" style="zoom:45%" />
<img src="http://upload-images.jianshu.io/upload_images/336727-fa169ba9dd23df92.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240" style="zoom:45%" />
</div>






