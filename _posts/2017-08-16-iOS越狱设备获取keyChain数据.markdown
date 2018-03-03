---
title:  "iOS越狱设备获取keyChain数据"
date:   2017-08-16 17:32:25
categories: [iOS]
tags: [越狱,keyChain]
---


根据苹果的介绍，iOS设备中的`keychain`是一个安全的存储容器，可以用来为不同应用保存敏感信息比如用户名，密码，网络密码，认证令牌。苹果自己用`keychain`来保存Wi-Fi网络密码，VPN凭证等等。它是一个sqlite数据库，位于`/private/var/Keychains/keychain-2.db`，其保存的所有数据都是加密过的.

1.下载**[Keychain-Dumper](https://github.com/ptoomey3/Keychain-Dumper)**工具,将`keychain_dumper`可执行文件通过助手或者`iFunBox`拷贝至越狱设备的`/tmp`目录

2.`ssh`连接越狱设备
``` ruby
Nelson:~ Nelson$ ssh root@192.168.xx.xxx
```
3.添加权限
`keychain_dumper `
``` ruby
Nelson-iPad:~ root# chmod 777 /tmp/keychain_dumper
```
`keychain-2.db`
``` ruby
Nelson-iPad:~ root# chmod +r /private/var/Keychains/keychain-2.db
```
4.查看数据
``` ruby
Nelson-iPad:~ root# /tmp/keychain_dumper -h
```
``` ruby
Usage: keychain_dumper [-e]|[-h]|[-agnick]
<no flags>: Dump Password Keychain Items (Generic Password, Internet Passwords)
-a: Dump All Keychain Items (Generic Passwords, Internet Passwords, Identities, Certificates, and Keys)
-e: Dump Entitlements
-g: Dump Generic Passwords
-n: Dump Internet Passwords
-i: Dump Identities
-c: Dump Certificates
-k: Dump Keys
```
