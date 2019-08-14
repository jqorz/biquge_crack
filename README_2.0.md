2.0版本反编译详细步骤记录

***
1. 先按照README.md里的步骤，使用xposed和fedx2提取好dex拷贝到电脑上（就是dex文件夹下的3个dex）

2. Android Killer里，选择apktools版本为shakaapktool3.0.0，选择apk进行反编译。应该会提示`APK 相关工作目录或文件不存在，无法继续!`

3. 使用baksmali_2.2.5.jar，将生成的dex文件分别转成smail的文件夹（就是smali文件夹下的smali.zip）

   `D:\Program\apktool和baksmali>java -jar baksmali_2.2.5.jar d dex1.dex`

   `D:\Program\apktool和baksmali>java -jar baksmali_2.2.5.jar d dex2.dex`

   `D:\Program\apktool和baksmali>java -jar baksmali_2.2.5.jar d dex3.dex`

4. 将生成的out文件夹拷贝到Android Killer的smail目录下面

5. 使用apktool_2.3.4.jar重新反编译原apk，作为备用

   `D:\Program\apktool和baksmali>java -jar apktool_2.3.4.jar d biquge.apk -o outdir`

6. 尝试在Android Killer编译打包，提示@layout/fz.xml找不到，从备用的文件夹发现有这个文件，拷贝到Android Killer的工作目录对应目录下面

7. 使用Android Killer成功打包成apk，运行崩溃，提示

   `java.lang.VerifyError: Rejecting class com.stub.StubApp because it failed compile-time verification (declaration of 'com.stub.StubApp'`

8. 发现onCreate()方法都被虚拟化了，想不到好方法。暂时放弃反编译此版本了，可以在新版本搜书，加到书架以后，上传进度到云端，再换用之前的破解版本进行看书。

   ***

   ### 如果使用其他版本的apktool，可能会遇到以下错误

   1. ` Invalid literal value: 256. Low 16 bits must be zeroed out. `，解决方法是定位到错误提示位置，把const/high16改为const
   2. ` Error for input '.parameter': Invalid directive` 百度说是apktool版本过久导致的，解决方法是使用单独文件夹里的那个baksmali_2.2.5.jar，重新将dex转为smali文件夹，然后合并替换到Android Killer项目的smail目录下面
   3. ` \Project\res\values-v22\styles.xml:9: error: Error: No resource found that matches the given name: attr 'android:keyboardNavigationCluster'.`说明资源解包出现问题。
      1. 方法一：通过Android Killer里，选择apktools版本为apktools2.3.4，附加参数-r（-r表示不解压资源文件，如果解压资源文件可能出现打包时提示资源找不到），然后删除项目重新打开解决，但是这样就没法修改res资源和AndroidManifest了，所以不推荐。
      2. 方法二：找到电脑中C盘下的1.apk的路径，例如：C:\Users\XX\AppData\Local\apktool\framework\1.apk，然后将其删掉，再重试就可以解决以上问题。
   4. 回编译的时候，出现dex溢出的情况，该情况出现的情况为，如果手动去融合smali代码的话，可能会导致dex溢出的情况。具体的溢出原因为dex中最多的方法数为65536，如果dex中的方法数超过该数的话，会报以下错误`Exception in thread "main" org.jf.util.ExceptionWithContext: Unsigned short value out of range: 65765` 解决方法是
      1. 确认smali文件夹同级目录下是否有smali_classes2文件夹，如果没有则建一个smali_classes2文件夹
      2. 将反编译得到的smali文件夹中的部分smali代码移到smali_classes2文件夹中
      3. 注意，要保持包名一致，否则运行的时候会找不到类。同时，Application类和在Application类中使用到的类不能移动，同时，该项目必须引入multiDex的jar，并在Application初始化中初始化multiJar，否则低版本的手机会找不到dex2的方法。