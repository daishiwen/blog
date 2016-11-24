---
layout: post
category: 技术
tags: [Android]
title: APK Signature Scheme v2
header: no
show: true
---

### v2简介

摘自官方文档：  
<en>
<p>APK Signature Scheme v2 is a whole-file signature scheme that increases verification speed and strengthens integrity guarantees by detecting any changes to the protected parts of the APK.</p>

<p>Signing using APK Signature Scheme v2 inserts an APK Signing Block into the APK file immediately before the ZIP Central Directory section. Inside the APK Signing Block, v2 signatures and signer identity information are stored in an APK Signature Scheme v2 Block.</p>

<p>APK Signature Scheme v2 was introduced in Android 7.0 (Nougat). To make a APK installable on Android 6.0 (Marshmallow) and older devices, the APK should be signed using JAR signing before being signed with the v2 scheme.</p>
</en>

通过检测APK受保护的部分，可提高验证速度，同时也加强了完整性保护。  
在Android 7.0 (Nougat)中引入。

gradle插件升级到2.2+，构建APK时默认使用的签名方式就是APK Signature Scheme v2
目前多渠道打包脚本是在签名后的apk内的mate-info下注入${channel}.txt 文件，违背了v2 Scheme的完整性保护原则。所以在7.0及其以上的设备上安装失败。

解决方案一（不推荐）：

{% highlight java %}
signingConfigs {
       release {
           storeFile file("xxxx")
           storePassword "xxxx"
           keyAlias "xxxx"
           keyPassword "xxxx"
           v2SigningEnabled false
       }
       debug {
           storeFile file("xxxx")
           storePassword "xxxx"
           keyAlias "xxxx"
           keyPassword "xxxx"
           v2SigningEnabled false
       }
   }
{% endhighlight %}

通过在build.gradle中手动关闭APK Signature Scheme v2来避免此问题，但是这是一种妥协性的解决方案。所以不推荐。

解决方案二（推荐）：

* build.gradle中加入v2SigningEnabled false，或者当你gradle插件版本在2.2+时可以省略不写，因为它默认是开启哒；  
* 打一个没有签名的release包，把指定signingConfig的地方注释掉就可以。在build.gradle中的配置如下：  

{% highlight java %}
buildTypes {
    release {
	zipAlignEnabled true
	debuggable false
	// signingConfig signingConfigs.release
	minifyEnabled true
	proguardFiles getDefaultProguardFile('proguard-android-optimize.txt'), 'proguard-project.txt'
    }
}
{% endhighlight %}

* 配置基本信息，配置文件为channels.properties（在工程的根目录下）；

```
[defaultConfig]
APP_NAME=QyerApp
VERSION_CODE=7100
VERSION_NAME=7.1.0
ks_password=xxxx
apksigner_dir=25.0.0
[channels]
CHANNELS=qyer,google,hiapk,gfan,wandoujia,360m
         xiaomi
         qq
         samsung
         91m
         nduoa
         appchina
	 ...
```

defaultConfig中是打包时所需的基本信息，channels中是渠道信息。`需要注意的是你的环境变量中必须得有ANDROID_HOME`，你可以在Terminal中输入`cat $ANDROID_HOME`看是否有打印。apksigner_dir是android sdk中build-tools的版本号，我指定的是25.0.0，如果你没有这个版本也可以指定24.0.3。
apksigner是在24.0.3才开始提供的，也就是说你在24.0.3之前的版本中是找不到apksigner的。

CHANNELS可以使用逗号分割一直往后加，但如果你嫌太长也可以使用换行。你可以这样写（必须和"="平行，否则会识别不出来）：

```
CHANNELS=qyer
         xiaomi
         qq
         samsung
         91m
         nduoa
         appchina
	 ...
```
* 开始打包

Terminal中进入工程的根目录，执行`sh build.sh`

首次QyerApp/python/QyerApp-qyer-release-unsigned.apk不存在的情况下：

```
sh build.sh

目标文件：QyerApp/python/QyerApp-qyer-release-unsigned.apk不存在，开始编译目标文件...
Observed package id 'build-tools;23.0.0-preview' in inconsistent location '/Users/KEVIN/Library/Android/sdk/build-tools/23.0.0_rc1' (Expected '/Users/KEVIN/Library/Android/sdk/build-tools/23.0.0-preview')
Observed package id 'build-tools;23.0.0-preview' in inconsistent location '/Users/KEVIN/Library/Android/sdk/build-tools/23.0.0_rc1' (Expected '/Users/KEVIN/Library/Android/sdk/build-tools/23.0.0-preview')
Incremental java compilation is an incubating feature.
The TaskInputs.source(Object) method has been deprecated and is scheduled to be removed in Gradle 4.0. Please use TaskInputs.file(Object).skipWhenEmpty() instead.
:QyerApp:clean
:google-play-services_lib:clean
:AndroidExLibrary:AndroidExLibrary:clean
:AndroidExLibrary:QyerLibrary:clean
:QyerApp:preBuild UP-TO-DATE
:QyerApp:preQyerReleaseBuild UP-TO-DATE
:QyerApp:checkQyerReleaseManifest
:QyerApp:preFastlaneAdhocBuild UP-TO-DATE
:QyerApp:preFastlaneDebugBuild UP-TO-DATE
:QyerApp:preFastlaneReleaseBuild UP-TO-DATE
......

开始编译渠道包...
包信息: QyerApp 7.1.0 7100
渠道数量: 56
渠道名称: ['qyer', 'google', 'hiapk', 'gfan', 'wandoujia', '360m', 'xiaomi', 'qq', 'samsung', '91m', 'nduoa', 'appchina', 'goapk', 'baidu', 'liantong', 'mm', 'crossmo', 'eoe', '3g', 'mumayi', 'hicloud', '163m', 'meizu', 'liqucn', 'oppo', 'dangle', 'sohu', 'lenovo', 'aliyun', 'amazon', 'taobao', 'sogouzs', 'jinli', 'letv', 'lephone', 'alimama', 'jinritoutiao', 'sjht001', 'sjht002', 'sjht003', 'sem', 'gdtcs', 'ws1', 'ws2', 'dianru1', 'dianruwifi', 'wnys', 'dianqu', 'qumi', 'jiawoduoduo', 'jiawoxiaoyi', 'jiawodstj', 'iqiyi', 'zhtg', 'zhtt', 'vivo']
signing...
signed: QyerApp/python/outputs_apk/QyerApp-qyer-release_7.1.0.apk
signed: QyerApp/python/outputs_apk/QyerApp-google-release_7.1.0.apk
signed: QyerApp/python/outputs_apk/QyerApp-hiapk-release_7.1.0.apk
signed: QyerApp/python/outputs_apk/QyerApp-gfan-release_7.1.0.apk
signed: QyerApp/python/outputs_apk/QyerApp-wandoujia-release_7.1.0.apk
signed: QyerApp/python/outputs_apk/QyerApp-360m-release_7.1.0.apk
signed: QyerApp/python/outputs_apk/QyerApp-xiaomi-release_7.1.0.apk
......
```

QyerApp/python/QyerApp-qyer-release-unsigned.apk存在的情况下：

```
sh build.sh

目标文件已存在，是否使用此文件编译渠道包？
please input (y/n) :y
开始编译渠道包...
包信息: QyerApp 7.1.0 7100
渠道数量: 56
渠道名称: ['qyer', 'google', 'hiapk', 'gfan', 'wandoujia', '360m', 'xiaomi', 'qq', 'samsung', '91m', 'nduoa', 'appchina', 'goapk', 'baidu', 'liantong', 'mm', 'crossmo', 'eoe', '3g', 'mumayi', 'hicloud', '163m', 'meizu', 'liqucn', 'oppo', 'dangle', 'sohu', 'lenovo', 'aliyun', 'amazon', 'taobao', 'sogouzs', 'jinli', 'letv', 'lephone', 'alimama', 'jinritoutiao', 'sjht001', 'sjht002', 'sjht003', 'sem', 'gdtcs', 'ws1', 'ws2', 'dianru1', 'dianruwifi', 'wnys', 'dianqu', 'qumi', 'jiawoduoduo', 'jiawoxiaoyi', 'jiawodstj', 'iqiyi', 'zhtg', 'zhtt', 'vivo']
signing...
signed: QyerApp/python/outputs_apk/QyerApp-qyer-release_7.1.0.apk
signed: QyerApp/python/outputs_apk/QyerApp-google-release_7.1.0.apk
signed: QyerApp/python/outputs_apk/QyerApp-hiapk-release_7.1.0.apk
signed: QyerApp/python/outputs_apk/QyerApp-gfan-release_7.1.0.apk
signed: QyerApp/python/outputs_apk/QyerApp-wandoujia-release_7.1.0.apk
signed: QyerApp/python/outputs_apk/QyerApp-360m-release_7.1.0.apk
signed: QyerApp/python/outputs_apk/QyerApp-xiaomi-release_7.1.0.apk
......
```
