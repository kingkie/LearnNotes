# Flutter 介绍

* 什么是Flutter
* Flutter 历史
* Flutter 特点
* Flutter 优势
* 基础语言
* 打印 Hello World
* 框架结构
* 目录结构
* 学习资料

## 1. 什么是Flutter
Flutter是谷歌的高性能、跨端UI框架，可以通过一套代码，支持iOS、Android、Windows、MAC、Linux、Fuchsia（Google新的自研操作系统）等众多平台，且能达到原生性能。 Flutter也可以与平台原生代码进行混合开发。在全世界，Flutter正在被越来越多的开发者和组织使用，并且Flutter是完全免费、开源的。

## 2. Flutter 历史
* Beta1版本于2018年2月27日在2018 世界移动大会公布
* Beta2版本2018年3月6日发布
* 1.0版本于2018年12月5日(北京时间)发布
* 2.0版本于2021年3月4日(北京时间)发布
* 3.0版本于2022年5月12日(北京时间)发布
* 目前已发布更新到3.7版本

## 3. Flutter 特点
### 3.1 跨平台自绘引擎
&emsp;&emsp;Flutter 与用于构建移动应用程序的其他大多数框架不同，因为 Flutter 既不使用 WebView，也不使用操作系统的原生控件。 相反，Flutter 使用自己的高性能渲染引擎来绘制 Widget（组件）。这样不仅可以保证在 Android 和iOS 上 UI 的一致性，也可以避免对原生控件依赖而带来的限制及高昂的维护成本。  

&emsp;&emsp;Flutter 底层使用 Skia 作为其 2D 渲染引擎，Skia 是 Google的一个 2D 图形处理函数库，包含字型、坐标转换，以及点阵图，它们都有高效能且简洁的表现。Skia 是跨平台的，并提供了非常友好的 API，目前 Google Chrome浏览器和 Android 均采用 Skia 作为其 2D 绘图引擎。

### 3.2 高性能
Flutter 高性能主要靠两点来保证：
* 第一：Flutter APP 采用 Dart 语言开发。Dart 在 JIT（即时编译）模式下，执行速度与 JavaScript 基本持平。但是 Dart 支持 AOT，当以 AOT模式运行时，JavaScript 便远远追不上了。执行速度的提升对高帧率下的视图数据计算很有帮助。
* 第二：Flutter 使用自己的渲染引擎来绘制 UI ，布局数据等由 Dart 语言直接控制，所以在布局过程中不需要像 RN 那样要在 JavaScript 和 Native 之间通信，这在一些滑动和拖动的场景下具有明显优势，因为在滑动和拖动过程往往都会引起布局发生变化，所以 JavaScript 需要和 Native 之间不停的同步布局信息，这和在浏览器中JavaScript 频繁操作 DOM 所带来的问题是类似的，都会导致比较可观的性能开销。

### 3.3 采用Dart语言开发
&emsp;&emsp;在了解 Flutter 为什么选择了 Dart 而不是 JavaScript 之前我们先来介绍一下之前提到过的两个概念：JIT 和 AOT。

&emsp;&emsp;程序主要有两种运行方式：静态编译与动态解释。静态编译的程序在执行前程序会被提前编译为机器码（或中间字节码），通常将这种类型称为AOT （Ahead of time）即 “提前编译”。而解释执行则是在运行时将源码实时翻译为机器码来执行，通常将这种类型称为JIT（Just-in-time）即“即时编译”。

&emsp;&emsp;AOT 程序的典型代表是用 C/C++ 开发的应用，它们必须在执行前编译成机器码；而JIT的代表则非常多，如JavaScript、python等，事实上，所有脚本语言都支持 JIT 模式。但需要注意的是 JIT 和 AOT 指的是程序运行方式，和编程语言并非强关联的，有些语言既可以以 JIT 方式运行也可以以 AOT 方式运行，如Python，它可以在第一次执行时编译成中间字节码，然后在之后执行时再将字节码实施转为机器码执行。也许有人会说，中间字节码并非机器码，在程序执行时仍然需要动态将字节码转为机器码，这不应该是 JIT 吗 ? 是这样，但通常我们区分是否为AOT 的标准就是看代码在执行之前是否需要编译，只要需要编译，无论其编译产物是字节码还是机器码，都属于AOT。在此，读者不必纠结于概念，概念就是为了传达精神而发明的，只要读者能够理解其原理即可，得其神忘其形。

&emsp;&emsp;现在我们看看 Flutter 为什么选择 Dart 语言？笔者根据官方解释以及自己对 Flutter 的理解总结了以下几条（由于其他跨平台框架都将 JavaScript 作为其开发语言，所以主要将 Dart 和 JavaScript 做一个对比）：

- 开发效率高  
  Dart 运行时和编译器支持 Flutter 的两个关键特性的组合：
  - 基于 JIT 的快速开发周期：Flutter 在开发阶段采用，采用 JIT 模式，这样就避免了每次改动都要进行编译，极大的节省了开发时间；
  - 基于 AOT 的发布包: Flutter 在发布时可以通过 AOT 生成高效的机器码以保证应用性能。而 JavaScript 则不具有这个能力。

- 高性能  
  Flutter 旨在提供流畅、高保真的的 UI 体验。为了实现这一点，Flutter 中需要能够在每个动画帧中运行大量的代码。这意味着需要一种既能提供高性能的语言，而不会出现会丢帧的周期性暂停，而 Dart 支持 AOT，在这一点上可以做的比 JavaScript 更好。

- 快速内存分配  
  Flutter 框架使用函数式流，这使得它在很大程度上依赖于底层的内存分配器。因此，拥有一个能够有效地处理琐碎任务的内存分配器将显得十分重要，在缺乏此功能的语言中，Flutter 将无法有效地工作。当然 Chrome V8 的 JavaScript 引擎在内存分配上也已经做的很好，事实上 Dart 开发团队的很多成员都是来自Chrome 团队的，所以在内存分配上 Dart 并不能作为超越 JavaScript 的优势，而对于Flutter来说，它需要这样的特性，而 Dart 也正好满足而已。

- 类型安全和空安全  
  由于 Dart 是类型安全的语言，且 2.12 版本后也支持了空安全特性，所以 Dart 支持静态类型检测，可以在编译前发现一些类型的错误，并排除潜在问题，这一点对于前端开发者来说可能会更具有吸引力。与之不同的，JavaScript 是一个弱类型语言，也因此前端社区出现了很多给 JavaScript 代码添加静态类型检测的扩展语言和工具，如：微软的 TypeScript 以及Facebook 的 Flow。相比之下，Dart 本身就支持静态类型，这是它的一个重要优势。

## 4. 基础语言
如果你有过其他编程语言经验（尤其是 Java 或 JavaScript）的话会非常容易上手 Dart。
Dart 在设计时应该是同时借鉴了 Java 和 JavaScript，同时又引入了一些现代编程语言的特性，如空安全
[学习资料](https://dart.cn/guides/language/language-tour)
```dart
main(){
  print('hello dart');
}
```

1. 变量
   > dart是一种强大的脚本语言，可以不预先定义变量的类型，dart会自动类型推导
2. 常量
   > Dart的常量使用final 和const修饰

   - const修饰的常量在一开始的时候就需要赋值（编译的时候就已经赋好值了）
   - final修饰的常量可以在一开始的时候不赋值，但同样只能赋值一次（惰性赋值，运行时第一次使用时赋值）

3. 数据类型
   
   1）Numbers（数值）：int,double
   
   2）Strings(字符串) ： String

   3）Booleans（布尔） ： bool

   4）List（数组） ： 在Dart中数组是列表对象

   5）Maps（字典） ： Map为键值对相关对象

4. 运算符
   
   1）算数运算符   + - * / %(取余) ~/(取整)

   2）关系运算符   == != > < >= <=

   3）逻辑运算符   !(取反) &&(且运算) ||(或运算)

   4）赋值运算符

   5）条件表达式   if/else switch/case ?/: ??

   6）类型转换    int.parse(str)   myNum.toString()

5. ...

## 5. Flutter 框架结构
[Flutter框架介绍](https://book.flutterchina.club/chapter1/flutter_intro.html#_1-2-2-flutter%E6%A1%86%E6%9E%B6%E7%BB%93%E6%9E%84)

![官方提供的 Flutter 框架图](http://nas.xiaochao.site:8050/i/2023/02/08/qx0b8e.png)
Flutter 从上到下可以分为三层：框架层、引擎层和嵌入层
### 5.1 框架层
Flutter Framework，即框架层。这是一个纯 Dart实现的 SDK，它实现了一套基础库，自底向上，我们来简单介绍一下：

* 底下两层（Foundation 和 Animation、Painting、Gestures）在 Google 的一些视频中被合并为一个dart UI层，对应的是Flutter中的dart:ui包，它是 Flutter Engine 暴露的底层UI库，提供动画、手势及绘制能力。
* Rendering 层，即渲染层，这一层是一个抽象的布局层，它依赖于 Dart UI 层，渲染层会构建一棵由可渲染对象的组成的渲染树，当动态更新这些对象时，渲染树会找出变化的部分，然后更新渲染。渲染层可以说是Flutter 框架层中最核心的部分，它除了确定每个渲染对象的位置、大小之外还要进行坐标变换、绘制（调用底层 dart:ui ）。
* Widgets 层是 Flutter 提供的的一套基础组件库，在基础组件库之上，Flutter 还提供了 Material 和 Cupertino 两种视觉风格的组件库，它们分别实现了 Material 和 iOS 设计规范。

我们进行Flutter 开发时，大多数时候都是和 Flutter Framework 打交道。

### 5.2 引擎层
Engine，即引擎层。毫无疑问是 Flutter 的核心， 该层主要是 C++ 实现，其中包括了 Skia 引擎、Dart 运行时、文字排版引擎等。在代码调用 `dart:ui`库时，调用最终会走到引擎层，然后实现真正的绘制和显示。

### 5.3 嵌入层
Embedder，即嵌入层。Flutter 最终渲染、交互是要依赖其所在平台的操作系统 API，嵌入层主要是将 Flutter 引擎 ”安装“ 到特定平台上。嵌入层采用了当前平台的语言编写，例如 Android 使用的是 Java 和 C++， iOS 和 macOS 使用的是 Objective-C 和 Objective-C++，Windows 和 Linux 使用的是 C++。 Flutter 代码可以通过嵌入层，以模块方式集成到现有的应用中，也可以作为应用的主体。Flutter 本身包含了各个常见平台的嵌入层，假如以后 Flutter 要支持新的平台，则需要针对该新的平台编写一个嵌入层。


## 6. 目录结构
![Snipaste_2023-02-08_16-39-32](http://nas.xiaochao.site:8050/i/2023/02/08/r4mhkm.jpg)  
![Snipaste_2023-02-08_16-40-10](http://nas.xiaochao.site:8050/i/2023/02/08/r4mfuh.jpg)  
![Snipaste_2023-02-09_14-18-08](http://nas.xiaochao.site:8050/i/2023/02/09/ngez5h.jpg)  

## 7. 组件
### 7.1 基础组件
  Text、Image、ElevatedButton、Checkbox、Radio、TextField、Icon 等
### 7.2 布局类组件
  Center、Row、Column、Align、Stack 等
### 7.3 容器类组件
  Padding、Clip、Scaffold、Container 等
### 7.4 可滚动组件
  ListView、GridView、TabBarView、PageView 等
### 7.5 状态组件
  - StatelessWidget 是无状态组件，状态不可变的 widget
  - StatefulWidget 是有状态组件，持有的状态可能在 widget 生命周期改变。通俗的讲：如果我们想改变页面中的数据的话这个时候就需要用到 StatefulWidget

## 8. 状态管理
最常见的方法
* Widget 管理自己的状态。
* Widget 管理子 Widget 状态。
* 混合管理（父 Widget 和子 Widget 都管理状态）。

如何决定使用哪种管理方法？下面是官方给出的一些原则可以帮助你做决定：

  - 如果状态是用户数据，如复选框的选中状态、滑块的位置，则该状态最好由父 Widget 管理。
  - 如果状态是有关界面外观效果的，例如颜色、动画，那么状态最好由 Widget 本身来管理。
  - 如果某一个状态是不同 Widget 共享的则最好由它们共同的父 Widget 管理。

## 9. 路由管理
路由（Route）在移动开发中通常指页面（Page），所谓路由管理，就是管理页面之间如何跳转，通常也可被称为导航管理，都会维护一个路由栈，路由入栈（push）操作对应打开一个新页面，路由出栈（pop）操作对应页面关闭操作，而路由管理主要是指如何来管理路由栈


## 10. Demo 
```flutter
import 'package:flutter/material.dart';

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Flutter Demo',
      theme: ThemeData(
        primarySwatch: Colors.blue,
      ),
      home: const MyHomePage(title: 'Flutter Demo Home Page'),
    );
  }
}

class MyHomePage extends StatefulWidget {
  const MyHomePage({super.key, required this.title});
  final String title;

  @override
  State<MyHomePage> createState() => _MyHomePageState();
}

class _MyHomePageState extends State<MyHomePage> {
  int _counter = 0;

  void _incrementCounter() {
    setState(() {
      _counter++;
    });
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text(widget.title),
      ),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: <Widget>[
            const Text(
              'You have pushed the button this many times:',
            ),
            Text(
              '$_counter',
              style: Theme.of(context).textTheme.headline4,
            ),
          ],
        ),
      ),
      floatingActionButton: FloatingActionButton(
        onPressed: _incrementCounter,
        tooltip: 'Increment',
        child: const Icon(Icons.add),
      ), 
    );
  }
}

```
![APP 页面](http://nas.xiaochao.site:8050/i/2023/02/08/u6a503.jpg)

## 11. 开源项目
[flutter_mycommunity_app](https://github.com/chn-sunch/flutter_mycommunity_app)  
  > 整合了IM通讯，移动支付，地图导航，语音，智能验证码等功能的Flutter社交电商，2.4.2之后不开源了

[Flutter-Music-Player](https://github.com/ducafecat/Flutter-Music-Player)  
[健身追踪器 WorkoutTracker](https://github.com/ducafecat/WorkoutTracker)  
[口袋妖怪](https://github.com/ducafecat/FlutterDex)  
[flutterflip 游戏](https://github.com/ducafecat/flutterflip)  
[天气 flutterWeather](https://github.com/ArizArmeidi/FlutterWeather)  

## 12. 学习资料
* Flutter 官网 : https://flutter.dev/
* 官方 GitHub 地址 : https://github.com/flutter
* Dart 中文文档 : https://dart.cn/
* Flutter 中文网 : https://flutterchina.club/
* Dart 在线编辑调试 : https://dartpad.dev/
* Flutter 插件 : https://pub.flutter-io.cn/






