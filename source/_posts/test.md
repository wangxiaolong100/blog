---
title: Cordova镭射二维码扫描插件开发遇到的坑
date: 2020-09-26 22:44:48
tags: test
---

最近在做一个Android系统的盘点机，使用了Cordova，主要是为了学习一门新技术，做常规的Cordova开发并不难，之前也做过一个Hello World项目。和普通的web项目没有太大的区别。这次由于涉及到硬件驱动，所以比较细致的看了Cordova的Plugin这块。但是在实际操作中还是遇到了不少坑，这里记录一下。

首先，就是驱动这块给的只有.so的共享库，而网上的问题多集中在怎么加载.jar库，这部分的资源比较少。在手册中加载库使用的是\<lib-file\>，并且手册也只说加载.jar库，试了一次，不行，再查手册也就这个标签用于加载库，再看选项，还有个**arch**选项，加上
```
arch="device"
```
成功加载。

第二，镭射扫码在安卓端的实现是通过回调实现的，即扫描结果并不是直接返回的，而到了JS端就需要处理这个异步过程了，而手册也只介绍了同步的情况。Google了一圈以后大致就是先返回没有结果，并保持连接：
```java
PluginResult pluginResult = new PluginResult(PluginResult.Status.NO_RESULT);
pluginResult.setKeepCallback(true);
callbackContext.sendPluginResult(pluginResult);
```
然后在扫描完成的回调中返回扫描结果：
```java
PluginResult result = new PluginResult(PluginResult.Status.OK, code.trim());
result.setKeepCallback(false);
scanResultCallbackContext.sendPluginResult(result);
```
这里的关键就是返回NO_RESULT并且保持连接，搞定。

第三，就是在JS部分，老提示我定义的组件对象未定义，原因在打包vue时候，重写了Cordova的index.html。而里面有个重要的cordova.js被删除，在编辑完后加上
```javascript
<script src="cordova.js"></script>
```
搞定。