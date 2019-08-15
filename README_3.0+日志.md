## 3.0版本反编译详细步骤记录

> 由于2.0版本无法继续进行，因此决定基于未加固的版本进行修改，使用1.0+版本里的最新版1.1.0，此版本有可以直接打包的dex，因此可以对比参考1.1.0版本和2.0.0版本的jar。主要修改目标是让站内搜书功能可以正常使用。
>
> 使用和2.0版本同样的方法，提取出3个dex，对应的3个jar和smali。我们查看原版7.0版本的dex3-dex2jar和未加固版本的classes-dex2jar来进行代码对比
***
- 3.0.0 修改站内搜索的地址为新地址，这样就可以搜到新书了

***
## 详细说明
### V3.0.0

1. 全局搜索"宜搜"，没有找到，因此搜索源应该是在线获取的，使用Flidder进行抓包
2. 发现7.0版本使用的搜索引擎配置的地址是`https://appbdsc.cdn.bcebos.com/v6/base/SearchEngine.html`
    旧版本使用的是`https://quapp.1122dh.com/v4/gudianbiquge/SearchEngine.html`
    两者返回值信息一致
3. 搜索/SearchEngine.html字符串，发现新版的网址配置文件位置是com.biquge.ebook.app.app.h.class，旧版的网址配置文件地址是com.biquge.ebook.app.app.f.class
4. 修改f.class 1094 行 ，由`const-string/jumbo v1, "v4/gudianbiquge"`修改为` const-string/jumbo v1, "v6/base"`
5. 打包编译运行，发现以前搜索不到的书可以正常搜索到了。

