---
title: Flutter学习笔记
date: 2019-05-23 16:38:09
tags:
---

其实按照[官方开发文档](https://flutter.io/get-started/install/)及[flutter中文网](https://flutterchina.club/get-started/install/)即可，本文仅记载我在学习过程中的一些笔记和要点。


[TOC]



## 搭建开发环境
[在国内使用官方镜像](https://github.com/flutter/flutter/wiki/Using-Flutter-in-China)，然后从github下载Flutter，执行环境检查。

```bash
export PUB_HOSTED_URL=https://pub.flutter-io.cn
export FLUTTER_STORAGE_BASE_URL=https://storage.flutter-io.cn
git clone -b dev https://github.com/flutter/flutter.git
export PATH="$PWD/flutter/bin:$PATH"
cd ./flutter
flutter doctor
```
为能一直运行flutter命令，最好将flutter放在系统环境变量`～/.bash_profile`中：
```bash
echo 'export PUB_HOSTED_URL=https://pub.flutter-io.cn' >> ~/.bash_profile
echo 'export FLUTTER_STORAGE_BASE_URL=https://storage.flutter-io.cn' >> ~/.bash_profile
echo 'export PATH="$PWD/flutter/bin:$PATH"' >> ~/.bash_profile
source ~/.bash_profile
```

![检查结果](https://note.youdao.com/yws/api/personal/file/WEB41870fb9a0d564e812bc755ca625fe34?method=download&shareKey=4ad1fafe6e56f6163eb0280c74f40df4)

按照检查结果安装缺失的东西（此过程对于我们web端不熟悉安卓和ios开发的同学来说，可能遇到很多问题，但基本上搜索下相关问题即可解决，或者咨询下APP端同学）。

遇到的坑：
要在mac上连接真机，需要安装libimobiledevice，这个包依赖python2.7,由于我的mac上安装了多个版本的python，导致使用brew安装时编译不通过，这时候就需要手动编译，并且指定python版本，参考[https://github.com/flutter/flutter/wiki/Using-Flutter-in-China](https://github.com/flutter/flutter/wiki/Using-Flutter-in-China)。
基本上安装时遇到问题，google也可以解决。

编辑器可用平时已习惯的vscode以及android studio。搜索安装flutter的插件即可。

## 初次体验flutter

使用编辑器的flutter插件即可创建新项目，或者命令行输入：
```bansh
flutter create myapp
```

在ios真机上跑需要真机签名

```bansh
 open .ios/Runner.xcworkspace
```

启动项目
```bansh
flutter run
```

flutter 版本升级
flutter SDK是通过git 托管的，所以只需要切换到对应的tag版本就行
```bash
git checkout -b <tagName> <origin tagName>
例如切换到0.8.3
git checkout -b v0.8.3 v0.8.3

flutter doctor
flutter --version 检查当前版本
```

flutter 依赖包管理都在`pubspec.yaml`里，类似npm的package.json，添加之后保存，在vscode中会自动执行依赖包安装，或者也可以使用命令手动安装
```bash
flutter packages get
```


## UI及布局
Flutter布局机制的核心就是widget。在Flutter中，几乎所有东西都是一个widget - 甚至布局模型都是widget。您在Flutter应用中看到的图像、图标和文本都是widget。 甚至你看不到的东西也是widget，例如行（row）、列（column）以及用来排列、约束和对齐这些可见widget的网格（grid）。

布局组件参考[https://flutter.io/docs/development/ui/widgets/layout](https://flutter.io/docs/development/ui/widgets/layout)


## widget

flutter的界面是通过一个个widget构建的，有两类，
1是StatelessWidget，无状态组件，这意味着它们的属性不能改变 - 所有的值都是最终的.

2是Stateful widgets ， StatefulWidget类本身是不变的，但是 State类在widget生命周期中始终存在。


### MaterialApp

由于flutter使用的Material Design风格样式，MaterialApp 理解就是整个APP的入口widget，包含APP的应用名称，主题色，路由等控件信息。

[MaterialApp](https://docs.flutter.io/flutter/material/MaterialApp-class.html)的主要属性如下：


- title ： 在任务管理窗口中所显示的应用名字
- theme ： 应用各种 UI 所使用的主题颜色
- color ： 应用的主要颜色值（primary color），也就是安卓任务管理窗口中所显示的应用颜色
- home ： 应用默认所显示的界面 Widget
- routes ： 应用的顶级导航表格，这个是多页面应用用来控制页面跳转的，类似于网页的网址
- initialRoute ：第一个显示的路由名字，默认值为 Window.defaultRouteName
- onGenerateRoute ： 生成路由的回调函数，当导航的命名路由的时候，会使用这个来生成界面
- onLocaleChanged ： 当系统修改语言的时候，会触发å这个回调
- navigatorObservers ： 应用 Navigator 的监听器
- debugShowMaterialGrid ： 是否显示 纸墨设计 基础布局网格，用来调试 UI 的工具
- showPerformanceOverlay ： 显示性能标签，https://flutter.io/debugging/#performanceoverlay
- checkerboardRasterCacheImages 、showSemanticsDebugger、debugShowCheckedModeBanner 各种调试开关

### Scaffold

Scaffold是Material中主要的布局组件,实现了基本的整体页面布局结构，只要是在 Material 中定义了的单个界面显示的布局控件元素，都可以使用 Scaffold 来绘制。

提供展示抽屉（drawers，比如：左边栏）、通知（snack bars） 以及 底部按钮（bottom sheets）。

我们可以将 Scaffold 理解为一个布局的容器。可以在这个容器中绘制我们的用户界面。

Scaffold的主要属性如下：

- appBar：显示在界面顶部的一个 AppBar，也就是 Android 中的 ActionBar 、Toolbar
- body：当前界面所显示的主要内容 Widget
- floatingActionButton：Material design中所定义的 FAB，界面的主要功能按钮
- persistentFooterButtons：固定在下方显示的按钮，比如对话框下方的确定、取消按钮
- drawer：侧边栏控件
- backgroundColor： 内容的背景颜色，默认使用的是 ThemeData.scaffoldBackgroundColor 的值
- bottomNavigationBar： 显示在页面底部的导航栏  示例：[Flutter学习之制作底部菜单导航](https://blog.csdn.net/qq_18948359/article/details/81409861)
- resizeToAvoidBottomPadding：类似于 Android 中的 android:windowSoftInputMode=”adjustResize”，控制界面内容 body 是否重新布局来避免底部被覆盖了，比如当键盘显示的时候，重新布局避免被键盘盖住内容。默认值为 true。

示例代码：
```dart
import 'package:flutter/material.dart';

void main() {
  runApp(new MaterialApp(
    title: 'Flutter Tutorial',
    home: new TutorialHome(),
  ));
}

class TutorialHome extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    //Scaffold是Material中主要的布局组件.
    return new Scaffold(
      appBar: new AppBar(
        leading: new IconButton(
          icon: new Icon(Icons.menu),
          tooltip: 'Navigation menu',
          onPressed: null,
        ),
        title: new Text('Example title'),
        actions: <Widget>[
          new IconButton(
            icon: new Icon(Icons.search),
            tooltip: 'Search',
            onPressed: null,
          ),
        ],
      ),
      //body占屏幕的大部分
      body: new Center(
        child: new Text('Hello, world!'),
      ),
      floatingActionButton: new FloatingActionButton(
        tooltip: 'Add', // used by assistive technologies
        child: new Icon(Icons.add),
        onPressed: null,
      ),
    );
  }
}
```

### Appbar

AppBar 和 SliverAppBar 是Material Design中的 App Bar，也就是 Android 中的 Toolbar，关于 Toolbar 的设计指南请参考Material Design中 Toolbar 的内容。
AppBar 和 SliverAppBar 都是继承StatefulWidget 类，都代表 Toobar，二者的区别在于 AppBar 位置的固定的应用最上面的；而 SliverAppBar 是可以跟随内容滚动的。

他们的主要属性如下：

- leading：在标题前面显示的一个控件，在首页通常显示应用的 logo；在其他界面通常显示为返回按钮

- title： Toolbar 中主要内容，通常显示为当前界面的标题文字

- actions：一个 Widget 列表，代表 Toolbar 中所显示的菜单，对于常用的菜单，通常使用 IconButton 来表示；对于不常用的菜单通常使用 PopupMenuButton 来显示为三个点，点击后弹出二级菜单

- bottom：一个 AppBarBottomWidget 对象，通常是 TabBar。用来在 Toolbar 标题下面显示一个 Tab 导航栏

- elevation：纸墨设计中控件的 z 坐标顺序，默认值为 4，对于可滚动的 SliverAppBar，当 SliverAppBar 和内容同级的时候，该值为 0， 当内容滚动 SliverAppBar 变为 Toolbar 的时候，修改 elevation 的值
flexibleSpace：一个显示在 AppBar 下方的控件，高度和 AppBar 高度一样，可以实现一些特殊的效果，该属性通常在 SliverAppBar 中使用

- backgroundColor：APP bar 的颜色，默认值为 ThemeData.primaryColor。改值通常和下面的三个属性一起使用

- brightness：App bar 的亮度，有白色和黑色两种主题，默认值为 ThemeData.primaryColorBrightness

- iconTheme：App bar 上图标的颜色、透明度、和尺寸信息。默认值为 ThemeData.primaryIconTheme

- textTheme： App bar 上的文字样式。默认值为 ThemeData.primaryTextTheme

- centerTitle： 标题是否居中显示，默认值根据不同的操作系统，显示方式不一样

### Container
Container(容器)，一个常用的控件，由基本的绘制、位置和大小控件组成。负责创建矩形的可视元素，可以用BoxDecoration来设计样式，比如背景、边框和阴影，Container也有边距、填充和大小限制，另外，还可以在三维空间利用矩阵进行变换。

Container的组成如下：

- 最里层的是child元素；
- child元素首先会被padding包着；
- 然后添加额外的constraints限制；
- 最后添加margin。

是不是很像CSS盒模型？

Container自身尺寸的调节分两种情况：

- Container在没有子节点（children）的时候，会试图去变得足够大。除非constraints是unbounded限制，在这种情况下，Container会试图去变得足够小。
- 带子节点的Container，会根据子节点尺寸调节自身尺寸，但是Container构造器中如果包含了width、height以及constraints，则会按照构造器中的参数来进行尺寸的调节。

Container的主要属性如下:

- key：Container唯一标识符，用于查找更新。
- alignment：控制child的对齐方式，如果container或者container父节点尺寸大于child的尺寸，这个属性设置会起作用，有很多种对齐方式。
- padding：decoration内部的空白区域，如果有child的话，child位于padding内部。padding与margin的不同之处在于，padding是包含在content内，而margin则是外部边界，设置点击事件的话，padding区域会响应，而margin区域不会响应。
- color：用来设置container背景色，如果foregroundDecoration设置的话，可能会遮盖color效果。
- decoration：绘制在child后面的装饰，设置了decoration的话，就不能设置color属性，否则会报错，此时应该在decoration中进行颜色的设置。
- foregroundDecoration：绘制在child前面的装饰。
- width：container的宽度，设置为double.infinity可以强制在宽度上撑满，不设置，则根据child和父节点两者一起布局。
- height：container的高度，设置为double.infinity可以强制在高度上撑满。
- constraints：添加到child上额外的约束条件。
- margin：围绕在decoration和child之外的空白区域，不属于内容区域。
- transform：设置container的变换矩阵，类型为Matrix4。
- child：container中的内容widget。

示例：
```dart
new Container(
  constraints: new BoxConstraints.expand(
    height:Theme.of(context).textTheme.display1.fontSize * 1.1 + 200.0,
  ),
  decoration: new BoxDecoration(
    border: new Border.all(width: 2.0, color: Colors.red),
    color: Colors.grey,
    borderRadius: new BorderRadius.all(new Radius.circular(20.0)),
    image: new DecorationImage(
      image: new NetworkImage('http://h.hiphotos.baidu.com/zhidao/wh%3D450%2C600/sign=0d023672312ac65c67506e77cec29e27/9f2f070828381f30dea167bbad014c086e06f06c.jpg'),
      centerSlice: new Rect.fromLTRB(270.0, 180.0, 1360.0, 730.0),
    ),
  ),
  padding: const EdgeInsets.all(8.0),
  alignment: Alignment.center,
  child: new Text('Hello World',
    style: Theme.of(context).textTheme.display1.copyWith(color: Colors.black)),
  transform: new Matrix4.rotationZ(0.3),
)
```

[Flutter 布局（一）- Container详解](https://www.jianshu.com/p/366b2446eaab)


### Slivers

Slivers有一个大家族控件，主要来实现滑动布局嵌套效果的各种列表。参考[Flutter：Slivers大家族，让滑动视图的组合变得很简单！](https://blog.csdn.net/yumi0629/article/details/83305627)

#### SliverAppBar
SliverAppBar上面已经讲过，相对于AppBar,SliverAppBar可以实现AppBar展开/收起的功能。
```dart
    CustomScrollView(
        slivers: <Widget>[
          SliverAppBar(
            actions: <Widget>[
              _buildAction(),
            ],
            title: Text('SliverAppBar'),
            backgroundColor: Theme.of(context).accentColor,
            expandedHeight: 200.0,
            flexibleSpace: FlexibleSpaceBar(
              background: Image.asset('images/food01.jpeg', fit: BoxFit.cover),
            ),
            // floating: floating,
            // snap: snap,
            // pinned: pinned,
          ),
          SliverFixedExtentList(
            itemExtent: 120.0,
            delegate: SliverChildListDelegate(
              products.map((product) {
                return _buildItem(product);
              }).toList(),
            ),
          ),
        ],
      );
```
主要属性：
- floating  如果为true，那么AppBar会在你做出下拉手势时就立即展开（即使ListView并没有到达顶部），该展开状态不显示flexibleSpace
- snap   如果同时设置floating和snap属性为true，那么AppBar会在你做出下拉手势时就立即全部展开（即使ListView并没有到达顶部），该展开状态显示flexibleSpace
- pinned  如果不想AppBar消失，则设置pinned属性为true即可

#### SliverList

SliverList的使用非常简单，只需设置delegate属性即可，我们一般使用SliverChildBuilderDelegate，注意记得设置childCount，否则Flutter没法知道怎么绘制：
```dart
    CustomScrollView(
        slivers: <Widget>[
          SliverList(
            delegate: SliverChildBuilderDelegate(
                  (BuildContext context, int index) {
                return _buildItem(context, products[index]);
              },
              childCount: 3,
            ),
          )
        ],
      );

```
#### SliverGrid

SliverGrid有三个构造函数：SliverGrid.count()、SliverGrid.extent和SliverGrid()。

```dart
SliverGrid(
  gridDelegate: SliverGridDelegateWithFixedCrossAxisCount(
    crossAxisCount: products.length,
  ),
  delegate: SliverChildBuilderDelegate(
    (BuildContext context, int index) {
      return _buildItem(products[index]);;
    }
);

```
#### SliverPersistentHeader

SliverPersistentHeader顾名思义，就是给一个可滑动的视图添加一个头（实际上，在CustomScrollView的slivers列表中，header可以出现在视图的任意位置，不一定要是在顶部）。这个Header会随着滑动而展开/收起，使用pinned和floating属性来控制收起时Header是否展示（pinned和floating属性不可以同时为true）,pinned和floating属性的具体意义和SliverAppBar中相同，这里就不再次解释了。

```dart
SliverPersistentHeader(
      pinned: pinned,
      floating: floating,
      delegate: _SliverAppBarDelegate(
        minHeight: 60.0,
        maxHeight: 180.0,
        child: Container(),
      ),
    );
```
构建一个SliverPersistentHeader需要传入一个delegate，这个delegate是SliverPersistentHeaderDelegate类型的，而SliverPersistentHeaderDelegate是一个abstract类，我们不能直接new一个SliverPersistentHeaderDelegate出来，因此，我们需要自定义一个delegate来实现SliverPersistentHeaderDelegate类。
写一个自定义SliverPersistentHeaderDelegate很简单，只需重写build()、get maxExtent、get minExtent和shouldRebuild()这四个方法，上面就是一个最简单的SliverPersistentHeaderDelegate的实现。其中，maxExtent表示header完全展开时的高度，minExtent表示header在收起时的最小高度。因此，对于我们上面的那个自定义Delegate，如果将minHeight和maxHeight的值设置为相同时，header就不会收缩了，这样的Header跟我们平常理解的Header更像。
```dart
class _SliverAppBarDelegate extends SliverPersistentHeaderDelegate {
  _SliverAppBarDelegate({
    @required this.minHeight,
    @required this.maxHeight,
    @required this.child,
  });

  final double minHeight;
  final double maxHeight;
  final Widget child;

  @override
  double get minExtent => minHeight;

  @override
  double get maxExtent => math.max(maxHeight, minHeight);

  @override
  Widget build(
      BuildContext context, double shrinkOffset, bool overlapsContent) {
    return new SizedBox.expand(child: child);
  }

  @override
  bool shouldRebuild(_SliverAppBarDelegate oldDelegate) {
    return maxHeight != oldDelegate.maxHeight ||
        minHeight != oldDelegate.minHeight ||
        child != oldDelegate.child;
  }
}

```

#### SliverToBoxAdapter

SliverPersistentHeader一般来说都是会展开/收起的（除非minExtent和maxExtent值相同），那么如果想要在滚动视图中添加一个普通的控件，那么就可以使用SliverToBoxAdapter来将各种视图组合在一起，放在CustomListView中。
```dart
    CustomScrollView(
        physics: ScrollPhysics(),
        slivers: <Widget>[
          SliverToBoxAdapter(
            child: _buildHeader(),
          ),
          SliverGrid.count(
            crossAxisCount: 3,
            children: products.map((product) {
              return _buildItemGrid(product);
            }).toList(),
          ),
          SliverToBoxAdapter(
            child: _buildSearch(),
          ),
          SliverFixedExtentList(
            itemExtent: 100.0,
            delegate: SliverChildListDelegate(
              products.map((product) {
                return _buildItemList(product);
              }).toList(),
            ),
          ),
          SliverToBoxAdapter(
            child: _buildFooter(),
          ),
        ],
      );

```

### ListView
ListView是最常用的滑动组件。

[flutter widget： ListView](https://www.jianshu.com/p/9830b1a6ae1f)

[图解 ListView 下拉刷新与上拉加载 ](https://yq.aliyun.com/articles/647000)

## 路由和导航

```dart
///不带参数的路由表跳转
Navigator.pushNamed(context, routeName);

///跳转新页面并且替换，比如登录页跳转主页
Navigator.pushReplacementNamed(context, routeName);

///跳转到新的路由，并且关闭给定路由的之前的所有页面
Navigator.pushNamedAndRemoveUntil(context, '/calendar', ModalRoute.withName('/'));

///带参数的路由跳转，并且监听返回
Navigator.push(context, new MaterialPageRoute(builder: (context) => new NotifyPage())).then((res) {
      ///获取返回处理
    });
```

## http

json转换model
[https://pub.dartlang.org/packages/json_serializable](https://pub.dartlang.org/packages/json_serializable)
```bash
flutter packages pub run build_runner build --delete-conflicting-outputs
```

如果需要把一个正常的返回包装成异步，例如从原始底层的invokeMethod回来的结果，可是使用Completer封装

## 插件

## 文件读写

## 调试和热重载

flutter命令行启动项目,启动后按`r`或者`R`即可实现热重载。
```bash
flutter run
```
如果是在已有APP上混合使用flutter的工程，可是使用:
```bash
flutter attach
```
此时会等待Android Studio或者Xcode中的原生项目运行启动，即可实现热重载。
![image](https://note.youdao.com/yws/api/personal/file/WEB8e8d62c29efe65b74183db135e24711b?method=download&shareKey=d949934c0e0f3a6a08f8fafa5379c566)

官方文档提供了Android Studio和vsCode两种开发环境配置。本文强烈建议使用vscode作为flutter主力开发开发调试工具(vsCode简直是神器)。

只需安装好[编辑器插件](https://flutter.io/docs/get-started/editor)，即可开启开发调试之路。普通调试方法参照[官方文档调试一节](https://flutter.io/docs/testing/oem-debuggers)就行，对于vsCode,有以下几点需要注意下：

1. vsCode插如果要实现类似Android Studio中的Inspector(类似于web检查元素)功能，需要flutter版本是v0.10.2以上。
2. 如果你打开的文件夹根目录直接就是flutter工程，直接运行vsCode调试(F5)基本上不会有问题，如果根目录不是flutter工程(例如配置了工作区，内部有多个项目，或者使用`project Manager`插件管理项目)，可能需要自己配置`launch.json`，指定工作目录，示例如下：
```json
//launch.json
// 欲了解更多信息，请访问: https://go.microsoft.com/fwlink/?linkid=830387
{
  "version": "0.2.0",
  "configurations": [

    {
      "name": "Ysj_iOS",
      "type": "dart",
      "request": "launch",
      "program": "${workspaceFolder}/YsjFlutter/lib/main.dart" //指定主文件
    },
    {
      "name": "Ysj_android",
      "type": "dart",
      "request": "launch",
      "program": "${workspaceFolder}/YsjFlutter/lib/main.dart",
      "args": [
          "--target-platform",
          "android-arm",
          "--flavor",
          "qa"
      ]
    },
    {
      "name": "newDriver4iOS",
      "type":"dart",
      "request": "attach",  //请求是连接,其实是执行flutter attach
      "cwd": "${workspaceFolder}/NewDriver4IOS/DriverCommunityWithFlutter" //指定工作目录
    }
  ]
}
```

对于混合架构的flutter工程想要在编辑器中调试和热重载，可以参考[Flutter新锐专家之路：工程研发体系篇](https://zhuanlan.zhihu.com/p/41376773)一文中的Native启动下的Flutter的调试与热重载逻辑。

但是，在vsCode中就稍微简单一些：
1. 在xcode或Android Studio运行原生项目，并打包安装到手机上(理论上不用运行手机上安装好开发版app也是可以的)。
2. vsCode使用命令行工具，运行`attach to Flutter Process`即可。
3. 或者如上文所述，`launch.json`添加自定义调试命令
```json
{
    "name": "newDriver4iOS",
    "type":"dart",
    "request": "attach",  //请求是连接,其实是执行flutter attach
    "cwd": "${workspaceFolder}/NewDriver4IOS/DriverCommunityWithFlutter" //指定工作目录
}
```


## 遇到的问题
Waiting for another flutter command to release the startup lock

  1. 打开flutter的安装目录`/bin/cache/ `
  2. 删除`lockfile`文件
  3. 重启AndroidStudio或者vscode

Error connecting to the service protocol: HttpException: , uri = http://127.0.0.1:1027/ws
  1. 打开 XCODE - Window - Devices and Simulators，找到用来调试的设备
  2. 取消勾选 Connect via network


安卓创建key
```bash
keytool -genkey -v -keystore ~/key.jks -keyalg RSA -keysize 2048 -validity 10000 -alias key
```



## 相关文档和参考

> 在Dart语言中使用下划线前缀标识符，会强制其变成私有的.

- [Flutter快速上车之Widget](https://zhuanlan.zhihu.com/p/43631849)
- [深入了解Flutter界面开发](https://zhuanlan.zhihu.com/p/36577285)
- [下一代移动端跨平台框架-Flutter大解密](https://mp.weixin.qq.com/s/ZMp2fSOTlYkZ_aNIOrUZdw)
- [关于Flutter的渠道（channels）：master、dev和beta](https://www.jianshu.com/p/2a1997c9a21f)
- [流言终结者- Flutter和RN谁才是更好的跨端开发方案？](https://zhuanlan.zhihu.com/p/44169959)
- [dart](https://www.dartlang.org/guides/language)
- [dart中文](http://dart.goodev.org/guides/language/language-tour)
- [Flutter新锐专家之路：工程研发体系篇](https://zhuanlan.zhihu.com/p/41376773)
- [Flutter 布局（一）- Container详解](https://www.jianshu.com/p/366b2446eaab)
- [Flutter：Slivers大家族，让滑动视图的组合变得很简单！](https://blog.csdn.net/yumi0629/article/details/83305627)