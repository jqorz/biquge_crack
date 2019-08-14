# 笔趣阁去广告破解版 Android 
笔趣阁本身功能很好用，但是启动时的广告加载太慢，时间太长，
因此借此机会，尝试破解启动广告及内部广告，进行反编译技术探索学习

> 1.0+版本已经无法获取最新的书籍，2.0版本被加固无法重新打包，因此可以在原版的最新版上先下载书籍到书架，然后上传进度到云端，再使用1.0+版本下载进度进行无广告阅读

`此项目主要用于反编译技术探索学习`

## 使用工具（在/tools文件夹下面）
1. Android Killer V1.3.1.0
> 用于修改smali文件，解包和打包

2. dex2jar2.1
> 用于将dex转成可以查看的jar，主要用于查看源码

    1. 进入cmd ,输入D:  打开到D盘
    2. 定位到dex2jar文件夹位置，比如cd D:/Program/dex2jar-2.0
    3. 执行bat文件，如d2j-dex2jar classes.dex（classes.dex是拷贝到该目录下的文件）
    4. dex2jar文件夹中便会生成对应的jar文件

3. jd-gui(Android Killer解压，bin文件夹里)
> 用于查看dex2jar生成的jar包中的代码

4. xposed框架+fdex2
> 加固脱壳

    1. 手机root后安装xposed，或者不root安装VirtualXposed
    2. 安装fdex2.apk
    3. xposed或者VirtualXposed中启用fdex2，重启手机
    4. 在fdex2中选择hook的app，打开该app
    5. 打开data/user/0/包名 拷贝出里面的dex到电脑

5. apktools.jar和baksmali.jar
> apktools可以把apk反编译，或者把反编译的代码打包成apk
>
> baksmali可以把dex转为smail

    1. 首先配置好java的环境变量
    2. 进入cmd ,输入D:  打开到D盘
    3. 定位到jar所在文件夹位置，比如cd D:/Program/apktool和baksmali
    4. 执行baksmali的jar，如D:\Program\apktool和baksmali>java -jar baksmali_2.2.5.jar d dex1.dex
    5. 执行apktools的jar，如D:\Program\apktool和baksmali>java -jar apktool_2.3.4.jar d biquge.apk -o outdir

` 先在jadx-gui里查看混淆后的java代码，再在Android Killer里根据方法名称，定位修改smail源码，修改好之后，再使用Android Killer重新打包`

## 版本说明（原版安装包在/origin，破解版在/crack）
### 1.0-1.0.9基于原版4.0.20171226（版本号91），此版本未加固
` 已经不能搜索到新的书籍了，尝试修改代码获取新书籍`
- 1.0 去除启动页广告-成功去除启动广告
- 1.0.1 去除应用中广告-失败
- 1.0.2 去除应用中广告-失败
- 1.0.3 修改版本号
- 1.0.4 去除启动页广告-失败
- 1.0.6 去除看书界面从后台切回来时显示的广告-失败
- 1.0.7 去除看书界面从后台切回来时显示的广告-失败
- 1.0.8 去除看书界面从后台切回来时显示的广告-失败
- 1.0.9 去除看书界面从后台切回来时显示的广告-成功
- 1.1.0 修改版本号到6.5_6520190730(129)，以规避升级弹窗
- 1.1.1 修改版本号到7.0.201908(135)，以规避升级弹窗
### 2.0.0 基于原版6.5.20190730(版本号129)，此版本已加固
` 此版本都被加固了，虽然能获取到dex，但是无法正常打包使用`
- 2.0.0 基于 6.5_6520190730版本(129)脱壳并尝试重新打包，失败（详细步骤可以查看README_2.0）
## 详细说明
### 1.0.0去除启动页广告
  - WelcomeActivity里修改onResume
  - WelcomeActivity去除广告监听
### 1.0.1去除应用中广告
  - MainActivity修改appkey字段为全为0
### 1.0.2 重新去除应用中广告
  - 再次在MainActivity修改appkey字段，改为空字符串
### 1.0.3 修改版本号
  - 由于正版已经升级至5.0，导致使用4.7的版本会提示该版本即将不可用并升级，所以将版本号由4.7改为5.0
### 1.0.4 去除看书界面从后台切回来时显示的广告
  - AndroidManifest中去除注册的推送的接收(未能成功去掉，去掉的只是推送服务而不是广告服务)
### 1.0.6 去除看书界面从后台切回来时显示的广告
  - tinker里的初始化去掉广告的注册id(未能成功去掉，去掉id只是让后台无法统计，但是还是会显示广告)
### 1.0.7 去除看书界面从后台切回来时显示的广告
  - 修改BookReadActivity里的广告view的显示为隐藏，具体修改内容为BookReadActivity$14第51行由const/8 v1, 0x0改为const/16 v1, 0x8，含义是setVisibility由VISIBLE改为GONE(未能去掉，隐藏的不是广告，而是进入书籍时的转圈进度条)
### 1.0.8 去除看书界面从后台切回来时显示的广告
  - 先全局检索"跳过"，定位到这个字符串的R文件的int值(0x7f0701b9)，再全局检索此int值，发现了XuliAdSpalshView，ShowOpenAdActivity，WelcomeActivity这三个Activity包含。其中WelcomeActivity已经处理了，所以修改ShowOpenAdActivity。
  - 查看ShowOpenAdActivity，可以在onCreate时直接finish，具体修改内容为拷贝ShowOpenAdActivity$1的53行（finish自己的代码）到ShowOpenAdActivity的169行(.line34位置)，在getWindows前就finish自己。
### 1.0.9 去除看书界面从后台切回来时显示的广告
  - 上面的finish有问题，具体的finish代码应该为
```
invoke-virtual {p0}, Lcom/biquge/ebook/app/ui/activity/ShowOpenAdActivity;->finish()V
```
将这段代码替换到ShowOpenAdActivity的169行(.line34位置)
### 1.1.0 升级版本，规避弹窗
### 2.0.0 新的6.5新版本
  - 之前的宜搜源已经失效，导致很多书搜索不到，这次6.5新版本采用了新的源，并且被加固。
  - 多次尝试后，无法解决onCreate()虚拟化的问题，因此尝试在1.0+版本进行修改
### 3.0.0 基于1.0+修改
  - 修改

## 使用说明
由于本版本使用了新签名，因此无法覆盖安装。所以请先注册个账户，然后在主界面选择上传阅读进度到云端，然后卸载，换为此版本，再从云端下载阅读进度
