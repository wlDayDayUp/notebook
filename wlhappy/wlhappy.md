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

6. VersionUpdate

   用于APP检查更新的实体类

7. IDialogBtCallback

   弹出框按钮的接口，确定，取消

8. GlobConfig

   全局配置类

9. Url

   接口配置

10. CheckUpdate

    检查更新

11. MapLocationUtils

    ​	高德地图定位工具类

12. MyApp

    Application

13. Build.gradle （Project）

    ```groovy
    apply from: 'config.gradle'
    ```

14. Config.gradle

