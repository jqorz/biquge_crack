# 笔趣阁去广告破解版 Android
笔趣阁本身功能很好用，但是启动时的广告加载太慢，时间太长，
因此破解了启动广告及内部广告，此版本可用于安卓9.0

## 使用工具
Android Killer V1.3.1.0 - 用于修改smali文件，解包和打包
jadx-gui_0.8.0和jd-gui - 用于查看smali对应的java代码
先在jadx-gui里查看混淆后的java代码，再在Android Killer里根据方法名称，定位修改源码，修改好之后，再重新

` 由于正版从5.0开始加固了，因此本破解版基于4.0.20171226（版本号91）`

## 版本说明
- 1.0 去除启动页广告-成功去除启动广告
- 1.0.1 去除应用中广告-失败
- 1.0.2 去除应用中广告-失败
- 1.0.3 修改版本号
- 1.0.4 去除启动页广告-失败
- 1.0.6 去除看书界面从后台切回来时显示的广告-失败
- 1.0.7 去除看书界面从后台切回来时显示的广告-失败
- 1.0.8 去除看书界面从后台切回来时显示的广告-失败
- 1.0.9 去除看书界面从后台切回来时显示的广告-成功

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
### 0.0.7 去除看书界面从后台切回来时显示的广告
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

## 使用说明
由于本版本使用了新签名，因此无法覆盖安装。所以请先注册个账户，然后在主界面选择上传阅读进度到云端，然后卸载，换为此版本，再从云端下载阅读进度
