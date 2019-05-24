---
title: adb启动模拟器
date: 2019-05-24 10:11:55
tags: flutter
---
adb 启动模拟器：
显示系统中全部Android平台： android list targets

显示系统中全部AVD（模拟器）：

方法一：android list avd

方法二：emulator -list-avds

创建AVD（模拟器）： android create avd -n 模拟器名称 -t target的id（android-8）

启动模拟器： emulator -avd 模拟器名称

删除AVD（模拟器）：android delete avd -n 模拟器名称

启动Activity ：adb shell am start -n 包名/包名＋类名（-n 类名,-a action,-d date,-m MIME-TYPE,-c category,-e 扩展数据,等）。

指定虚拟机安装app adb -s emulator-554 install ..apk
---------------------
作者：for_perfect
来源：CSDN
原文：https://blog.csdn.net/u010359739/article/details/54708960
版权声明：本文为博主原创文章，转载请附上博文链接！