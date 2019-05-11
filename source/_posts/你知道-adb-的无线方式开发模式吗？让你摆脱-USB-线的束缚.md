---
title: 你知道 adb 的无线方式开发模式吗？让你摆脱 USB 线的束缚
date: 2019-05-11 17:37:06
tags: #Android
---

### 开发场景

做Android开发的基本都知道，平常一般都是直接通过 USB 线或者 Type C线的方式连接，来完成日常的开发和调试，这种开发模式存在几个问题点，是一个极简主义者所不能忍受的

- 电脑的 USB 口比较少，特别是 MAC 电脑，新版的就更是少得可怜；
- 有时候有些功能模块比较耗电的时候，手机耗电的速度会比电脑充电的速度慢，比如开发直播间模块，要长时间开摄像头的情况下；
- 开发好功能点，有时候要拿给同事看效果的时候也不太方便，隔着几个工位的时候，经常要拔掉线；

### 解决方案：开始使用 adb WiFi 调试模式

使用 adb WiFi 调试模式，通过以下几个步骤即可完成（开始这些配置之前先确保你的 adb 环境是配置好的）：
1. 确保 Android 手机和电脑连接的是同一局域网的 WiFi；
2. 通过 USB 线连接 Android 手机；
3. 设置手机侦听端口 5555 上的 TCP/IP 连接：
    ``` 
    $ adb tcpip 5555
    ```
    ※ 注意: 如果有多个手机连接在电脑上，需要用 -s ‘serial_number’ 参数指定目标手机，比如：
    ```
    $ adb -s '04157df4d349bf21' tcpip 5555
    ```
    在命令行中看到 TCP mode port: 5555 就表示监听成功：
    ```bash
    ~/Downloads » adb tcpip 5555 
    restarting in TCP mode port: 5555
    ------------------------------------------------------------
    ~/Downloads » adb -s '04157df4d349bf21' tcpip 5555
    ------------------------------------------------------------
    ~/Downloads » adb -s '04157df4d349bf21' tcpip 5555
    ------------------------------------------------------------
    ~/Downloads » adb -s '04157df4d349bf21' tcpip 5555
    restarting in TCP mode port: 5555
    ------------------------------------------------------------
    ```
4. 通过 connect 命令和 IP 地址以及端口号连接到目标手机，比如：
    ```
    $ adb connect 192.168.1.146:5555
    ```
    当看到 connected to xxx:5555 的提示语就表示连接成功：
    ```
    ~/Downloads » adb connect 192.168.1.146:5555
    connected to 192.168.1.146:5555
    ------------------------------------------------------------
    ~/Downloads »
    ```
5. 拔掉 USB 线，验证一下，看到如下提示语，那么恭喜你，已成功打开 adb WiFi 的大门，可以开始畅游你的无线调试之旅啦。
    ```
    ~/Downloads » adb devices
    List of devices attached
    192.168.1.146:5555	device
    ------------------------------------------------------------
    ~/Downloads »
    ```


### 敲命令行的你很酷很帅，不过 IDE Plugins 的方式能让你更舒畅

上面介绍的是 adb 无线连接的基本实现，不过每次都得经历那些步骤，体验不是很好，能否有一种方式，直接在 Android Studio 中直接鼠标点点的快速方式来完成呢？答案：木有错，有得。

##### AS 中插件市场的搜索结果，关键词：adb wifi
![image.png](https://upload-images.jianshu.io/upload_images/215971-4486c3554a5cd8c7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

##### 全部都安装体验之后，最终只有两款能够顺利操作完成连接的：ADB WiFi Connect、WIFI ADB ULITIMATE，对比各种优缺点之后，最终只留下：WIFI ADB ULITIMATE

- WIFI ADB ULITIMATE（可用，推荐使用这个）
    ![WIFI ADB ULITIMATE](https://upload-images.jianshu.io/upload_images/215971-0413705166559a63.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
- ADB WiFi Connect（可用）
    ![ADB WiFi Connect](https://upload-images.jianshu.io/upload_images/215971-91402aeb764f7e01.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


##### 两款插件的使用对比结论，供大家快速选择适合自己的那一款【毕竟青菜萝卜，各有所好】
对比点 | WIFI ADB ULITIMATE | ADB WiFi Connect
---|---|---
入口 | run 旁边，每次使用打开一个新的对话框 | 操作窗口依附在 IDE 的右边工具窗口中，操作比较方便
功能点 | 连接、断开连击、记录连接设备、设备断开自动刷新 | 连接【以及主动输入目标 IP 地址的连接方式】、断开连接、USB 线和 WIFI 两种连接方式的设备分组展示、自动记录最近的 10 次连接记录
优点 | 设备断开和连接，自动刷新、操作直接 Log 提示 | 操作窗口固定、自动记录、可以手动输入 IP 地址进行连接
缺点 | 操作窗口是独立 Java 窗口程序，不跟随 IDE、需要手动点 Save | 设备断开需要手动刷新才能识别出新的连接情况【后续期望作者能够加上自动监听】


### 最后，再提两点关于 adb 的事情
- 关于 adb 使用，谷歌官方的说明文档: [https://developer.android.com/studio/command-line/adb](https://developer.android.com/studio/command-line/adb)
- 在找 adb wifi 插件的时候，还发现一款跟 adb 相关的插件（提供一些跟当前开发项目app常用的操作），觉得挺好用：ADB Idea，操作选项如下：
    ```
    ADB Revoke Permissions
    ADB Revoke Permissions and Restart
    ADB Grant Permissions
    ADB Uninstall App
    ADB Kill App
    ADB Start App
    ADB ReStart App
    ADB Clear App Data
    ADB Start App With Debugger
    ADB Restart App With Debugger
    ```
