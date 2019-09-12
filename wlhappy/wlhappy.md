# WlHappy

> 工作开发常用库，收集汇总一些平时Android开发中常用到的Lib

- [RxHttp 网络请求](#rxhttp-网络请求)

## RxHttp 网络请求

[github](https://github.com/liujingxing/RxHttp)

在清单文件里面加上

1. android:name="com.wl1217.mybox.MyApp"

2. android:networkSecurityConfig="@xml/network_config"

3. 必要的权限

4. 不混淆bean包下的所有

   ```
       -keep class com.wl1217.wlhappy.bean.** { *; }
   ```

5. 在App.build添加，java 1.8 的支持

   ```Gradle
   android{
    compileOptions {
           sourceCompatibility JavaVersion.VERSION_1_8
           targetCompatibility JavaVersion.VERSION_1_8
       }
    }
   ```

