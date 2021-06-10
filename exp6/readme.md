# 实验六 安卓系统访问控制策略与机制

## 实验环境

- Android Studio 4.1

- Windows 10


## 实验目的

- 熟悉Andriod Studio模拟器开发安卓的应用程序的环境和基本流程，并能实现简单的应用程序的开发。

- 了解使用ABD在命令行中对模拟设备进行基本的操作。

## 实验过程

### ADB实验

#### 命令行

```bash
# 查看开启的模拟器
adb devices

# 连接模拟器终端
adb -s emulator-5554 shell

# 输出环境变量
echo $PATH

# 查看系统版本，lsb_release -a 不可用
uname -a

# 查看当前目录下文件
ls

# 查看防火墙规则
iptables -nL
```

在命令行中执行以上命令成功

![img](img/5554shell.PNG)

```bash
# 将文件复制到设备

adb pull /etc/hosts D:\study\moblieinet\exp6\etc\hosts

```

![img](img/adbpull.PNG)

在命令行中执行adbpush成功

![img](img/pullok.PNG)

```bash
# 从设备复制文件

adb push C:\Users\lenovo\Desktop\windows.txt /sdcard/

```

![img](img/adbpush.PNG)

在命令行中执行adbpush成功

![img](img/pushok.PNG)

```bash
# 安装应用

adb install path_to_apk

```

#### Activity Manager (am)

```bash
# Camera（照相机）的启动方法为:
am start -n com.android.camera/com.android.camera.Camera

# Browser（浏览器）的启动方法为：
am start -n com.android.browser/com.android.browser.BrowserActivity

# 启动浏览器 :
am start -a android.intent.action.VIEW -d  http://sec.cuc.edu.cn/

# 拨打电话 :
am start -a android.intent.action.CALL -d tel:10086

# 发短信：
am start -a android.intent.action.SENDTO -d sms:10086 --es sms_body ye --ez exit_on_sent true
```

启动浏览器成功

![img](img/amcucok.PNG)

拨打电话成功

![img](img/am10086ok.PNG)

发短信成功

![img](img/ammessageok.PNG)

#### 软件包管理器 (pm)

```bash
# 查看第三方软件包
pm list packages -3

# 卸载指定软件包
pm uninstall PACKAGE_NAME
```

![img](img/pmlist.PNG)

#### 其他adb实验

```bash
# 常用的按键对应的KEY_CODE
adb shell input keyevent 22 //焦点去到发送键
adb shell input keyevent 66 //回车按下

adb shell input keyevent 4 // 物理返回键
adb shell input keyevent 3 // 物理HOME键
```

发送信息完成

![img](img/input.gif)

---

### Hello World v1

#### 构建第一个 Android 应用

- 根据 [Android - FirstApp](https://developer.android.google.cn/training/basics/firstapp) 给出的指示一步一步进行操作。

- 额外设置：

```
Application Name设置为：MISDemo
Company Domain设置为：cuc.edu.cn
```

![img](img/activityconfig.PNG)

- 呈现效果：

    ![img](img/apkok.gif)


#### 问题回答

**按照向导创建的工程在模拟器里运行成功的前提下，生成的APK文件在哪儿保存的？**

- APK文件应该在 `\MISDemo\app\buid\outputs\apk\debug` 目录下

    ![img](img/apkdebug.PNG)

**使用adb shell是否可以绕过MainActivity页面直接“唤起”第二个DisplayMessageActivity页面？是否可以在直接唤起的这个DisplayMessageActivity页面上显示自定义的一段文字，比如：你好移动互联网安全**

- 可以

    ```bash
    am start -n cuc.edu.cn/cuc.edu.cn.DisplayMessageActivity --es "cuc.edu.cn.MESSAGE" "Hello, mobile Internet Security"
    ```

    ![img](img/hellook.gif)

**如何实现在真机上运行你开发的这个Hello World程序？**

- 使用WiFi或者USB数据线让PC与安卓手机相连。此处选择USB数据线。

- 然后在Android Studio中选择对应的手机，并在手机上运行程序即可。

**如何修改代码实现通过 `adb shell am start -a android.intent.action.VIEW -d http://sec.cuc.edu.cn/` 可以让我们的cuc.edu.cn.misdemo程序出现在“用于打开浏览器的应用程序选择列表”？**

- 打开`app` > `manifests` > `AndroidManifest.xml`，在AndroidManifest,xml配置文件中的.MainActivity模块下添加如下代码

    ```bash
    // AndroidManifest.xml  <activity android:name=".MainActivity">
    <action android:name="android.intent.action.VIEW" />
    <category android:name="android.intent.category.DEFAULT" />
    <category android:name="android.intent.category.BROWSABLE" />
    <data android:scheme="http" />
    <data android:scheme="https" />
    ```

**如何修改应用程序默认图标？**

- 打开`Project` > `app` > `res` > `mipmap` > `New Image Assert`，选择图片进行设置

    ![img](img/appimg.PNG)

- 图片设置成功

    ![img](img/appimgok.PNG)


**如何修改代码使得应用程序图标在手机主屏幕上实现隐藏？**

- 未修改前主界面如下

    ![img](img/beforehide.PNG)

- 在`app` > `manifests` > `AndroidManifest.xml`中添加如下代码，Run App

    ```bash
    <data
                    android:host="MainActivity"
                    android:scheme="com.key.poc" />
    ```

    ![img](img/afterhide.PNG)



## 遇到的问题

- Android Studio安装完成后adb的环境变量未设置成功

解决方法：

添加系统变量指向SDK目录下platform-tools

![img](img/S1.PNG)


## 参考资料

- [第六章实验](https://c4pr1c3.github.io/cuc-mis/chap0x06/exp.html#hello-world-v1)

- [移动互联网安全（2021）](https://www.bilibili.com/video/BV1rr4y1A7nz?p=100)