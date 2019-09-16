#### 说明一下关于笔趣阁的几个版本
+ 1.未加固google play->java版 使用地址 shuapi.jiaston.com(104.16.130.241 美国)
+ 2.未加固(1.0)/加固(2.0)flutter版 dynic.1122dh.com(117.91.192.41 江苏省扬州市 电信)
+ 未加固(5.0)/加固(6.0/7.0), google app 中因为要上架google, 作者做了很多优化, 包括修改服务器, 但是证明用户的数据库是通用的(同一个/分布式 数据库)
#### 去除以下内容:

+ 1."打赏开发者免广告" 
 \res\layout\view_pay_ad_layout.xml 下插页内容
```
    <?xml version="1.0" encoding="utf-8"?>
    <LinearLayout android:gravity="center" android:orientation="vertical" android:layout_width="wrap_content" android:layout_height="wrap_content"
      xmlns:android="http://schemas.android.com/apk/res/android">
      <!--ImageView android:layout_width="wrap_content" android:layout_height="wrap_content" android:src="@drawable/icon_pay_dashang" />
      <TextView android:textSize="14.0sp" android:textColor="#ff12b7f5" android:layout_width="wrap_content" android:layout_height="wrap_content" android:layout_marginTop="2.0dip" android:text="@string/app_vip_donate_developer_confirm" /-->
    </LinearLayout>
``` 
+ 2. 去提示自动更新
搜索"v4/google/version"(抓包获得)->\smali\com\biquge\ebook\app\app\f.small->1170行
修改为:
```
const-string/jumbo v0, "https://update.zsdfm.com/v7/google/version.html"
```
+ 3. 根据 @kingkiller 的方案新增了一下几种去除
	+ 3.1 assets\gdt_plugin\gdtadv2.jar 文件删除
	+ 3.2 AndroidManifest 中所有 ruishaad
	+ 3.3 AndroidManifest 中所有 腾讯的tinker
	+ 3.4 AndroidManifest 中所有 google-ad




