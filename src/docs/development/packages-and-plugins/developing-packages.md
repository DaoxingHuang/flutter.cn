---
title: Developing packages & plugins
title: Flutter Packages 的开发和提交
short-title: Developing
short-title: 开发和提交
description: How to write packages and plugins for Flutter.
description: 如何编写和提交你的 Packages。
---

{{site.note.alert}}
  If you write plugins for Flutter, you should know that
  the plugin API was upgraded to 2.0 in Flutter 1.12
  to support [federated plugins][] and to make it easier to
  [test your plugin][].
  In Flutter 1.10, Flutter's pubspec format was updated to
  allow you to specify
  [which platforms a plugin][supported-platforms] supports,
  such as web and macos.

  Eventually, the old style of plugin will be deprecated and,
  in the short term, you will see a warning when the framework
  detects that you are using an old-style plugin.
  For information on how to upgrade your plugin, see
  [Supporting the new Android plugins APIs][].
{{site.note.end}}

## Package introduction

## Package 介绍

Packages enable the creation of modular code that can be
shared easily. A minimal package consists of the following:

通过使用 packages （的模式）可以创建易于共享的模块化代码。
一个最基本的 package 由以下内容构成：

**`pubspec.yaml`**
<br> A metadata file that declares the package name,
  version, author, and so on.

**pubspec.yaml` 文件**
<br> 用于定义 package 名称、版本号、作者等其他信息的元数据文件。

**`lib`**
<br> The `lib` directory contains the public code in
  the package, minimally a single `<package-name>.dart` file.
  
**`lib` 目录**
<br> 包含共享代码的 `lib` 目录，
其中至少包含一个 `<package-name>.dart` 文件。

{{site.alert.note}}

  For a list of dos and don'ts when writing an effective plugin,
  see the Medium article by Mehmet Fidanboylu,
  [Writing a good plugin][].
  
  有关编写高效插件的注意事项列表，请参考 Medium 上的文章：
  [Writing a good plugin][]。
{{site.alert.end}}

### Package types {#types}

### Package 类别 {#types}

Packages can contain more than one kind of content:

Package 包含以下两种类别：

**Dart packages**
<br> General packages written in Dart,
  for example the [`path`][] package.
  Some of these might contain Flutter specific
  functionality and thus have a dependency on the
  Flutter framework, restricting their use to Flutter only,
  for example the [`fluro`][] package.
  
**纯 Dart 库 (Dart packages)**
<br> 用 Dart 编写的传统 package，比如
  [`path`]。其中一些可能包含 Flutter
  的特定功能，因此依赖于 Flutter 框架，其使用范围仅限于 Flutter，比如
  [`fluro`]。

**Plugin packages**
<br> A specialized Dart package that contains an API written in
  Dart code combined with one or more platform-specific
  implementations.
  Plugin packages can be written for Android
  (using Kotlin or Java), iOS (using Swift or Objective-C),
  web (using Dart), macos (using Dart), or any combination
  thereof.
  A concrete example is the [`url_launcher`][] plugin package.
  To see how to use the `url_launcher` package, and how it
  was extended to implement support for web,
  see the Medium article by Harry Terkelsen,
  [How to Write a Flutter Web Plugin, Part 1][].
  
**原生插件 (Plugin packages)**
<br> 使用 Dart 编写的，按需使用
  Java 或 Kotlin、ObjC 或 Swift 分别在 Android 和/或 iOS 平台实现的 package。
  一个具体的例子是 [`battery`]。
  Plugin packages can be written for Android
  (using Kotlin or Java), iOS (using Swift or Objective-C),
  web (using Dart), macos (using Dart), or any combination
  thereof.
  A concrete example is the [`url_launcher`][] plugin package.
  To see how to use the `url_launcher` package, and how it
  was extended to implement support for web,
  see the Medium article by Harry Terkelsen,
  [How to Write a Flutter Web Plugin, Part 1][].

## Developing Dart packages {#dart}

## 开发纯 Dart 库的 packages {#dart}

The following instructions explain how to write a Flutter
package.

下面会为你介绍如何写 Flutter package。

### Step 1: Create the package

### 第一步：创建 package

To create a Flutter package, use the `--template=package` flag
with `flutter create`:

想要创建纯 Dart 库的 package，
请使用带有 `--template=package` 标志的 `flutter create` 命令：

```terminal
$ flutter create --template=package hello
```

This creates a package project in the `hello`
folder with the following content:

这将在 `hello` 目录下创建一个 package 项目，其中包含以下内容：

**LICENSE**
<br> A (mostly) empty license text file.

**LICENSE 文件**
<br> 大概率会是空的一个许可证文件。

**test/hello_test.dart**
<br> The [unit tests][] for the package.

**test/hello_test.dart 文件**
<br> Package 的 [单元测试][unit tests] 文件。

**hello.iml**
<br> A configuration file used by the IntelliJ IDEs.

**hello.iml 文件**
<br> 由 IntelliJ 生成的配置文件。

**.gitignore**
<br> A hidden file that tells Git which files or
  folders to ignore in a project.

**.gitignore 文件**
<br> 告诉 Git 系统应该隐藏哪些文件或文件夹的一个隐藏文件。

**.metadata**
<br> A hidden file used by IDEs to track the properties
  of the Flutter project.

**.metadata 文件**
<br> IDE 用来记录某个 Flutter 项目属性的的隐藏文件。

**pubspec.yaml**
<br> A yaml file containing metadata that specifies
  the package's dependencies. Used by the pub tool.

**pubspec.yaml 文件**
<br> pub 工具需要使用的，包含 package 依赖的 yaml 格式的文件。

**README.md**
<br> A starter markdown file that briefly describes
  the package's purpose.

**README.md 文件**
<br> 起步文档，用于描述 package。

**lib/hello.dart**
<br> A starter app containing Dart code for the package.

**lib/hello.dart 文件**
<br> package 的 Dart 实现代码。

**.idea/modules.xml**, **.idea/modules.xml**, **.idea/workspace.xml**
<br> A hidden folder containing configuration files
  for the IntelliJ IDEs.

**.idea/modules.xml**、**.idea/modules.xml**、**.idea/workspace.xml 文件**
<br> IntelliJ 的各自配置文件（包含在 .idea 隐藏文件夹下）。

**CHANGELOG.md**
<br> A (mostly) empty markdown file for tracking
  version changes to the package.

**CHANGELOG.md 文件**
<br> 又一个大概率为空的文档，用于记录 package 的版本变更。

### Step 2: Implement the package

For pure Dart packages, simply add the functionality
inside the main `lib/<package name>.dart` file,
or in several files in the `lib` directory.

对于纯 Dart 库的 package，只要在 `lib/<package name>.dart` 文件中添加功能实现，
或在 `lib` 目录中的多个文件中添加功能实现。

To test the package, add [unit tests][]
in a `test` directory.

如果要对 package 进行测试，在 `test` 目录下添加 [单元测试][unit tests]。

For additional details on how to organize the
package contents,
see the [Dart library package][] documentation.

关于如何组织 package 内容的更多详细信息，请参考 [Dart library package][] 文档。

## Developing plugin packages {#plugin}

## 开发原生插件类型的 packages {#plugin}

If you want to develop a package that calls into
platform-specific APIs, you need to develop a plugin package.
A plugin package is a specialized version of a
Dart package that, in addition to the content described above,
also contains platform-specific implementations written for
Android (Kotlin or Java code), iOS (Swift or Objective-C),
web (Dart), macos (Dart), or any subset thereof.
The API is connected to the platform-specific
implementation(s) using [platform channels][].

如果想要开发一个调用特定平台 API 的 package，你需要开发一个原生插件 packgae。
原生插件 packgae 是 Dart package 的特别版本，
除了要实现 Dart package 要实现的内容，还需要按需使用 Java 或 Kotlin、ObjC 
或 Swift 分别在 Android 和/或 iOS 平台实现，
你可以使用 [platform channels][] 中的 API 来实现特定平台的调用。

### Federated plugins

Federated plugins were introduced in Flutter 1.12
as a way of splitting support for different platforms
into separate packages. So, a federated plugin can use
one package for iOS, another for Android,
another for web, and yet another for your car
(as an example of an IoT device).
Among other benefits, this approach allows a
domain expert to extend an existing plugin to work for
the platform they know best.

A federated plugin requires the following packages:

**app-facing package**
: The package that plugin users depend on to use the plugin.
  This package specifies the API used by the Flutter app.

**platform package(s)**
: One or more packages that contain the platform-specific
  implementation code. The app-facing package calls into
  these packages&mdash;they aren't included into an app,
  unless they contain platform-specific functionality
  accessible to the end user.

**platform interface package**
: The package that glues the app-facing packing
  to the platform package(s). This package declares an
  interface that any platform package must implement to
  support the app-facing package. Having a single package
  that defines this interface ensures that all platform
  packages implement the same functionality in a uniform way.
  
For more information on federated plugins,
why they are useful, and how they are implemented,
see the Medium article by Harry Terkelsen,
[How To Write a Flutter Web Plugin, Part 2][].

### Specifying a plugin's supported platforms {#plugin-platforms}

### 指定一个插件支持的平台 {#plugin-platforms}

In Flutter 1.12 and later, plugins can specify
the platforms they support by adding keys to the
`platforms` map in the `pubspec.yaml` file.
For example, the following pubspec file shows the
`flutter:` map for the `hello` plugin, which supports
only iOS and Android:

从 Flutter 1.12 版开始，插件可以通过向 `pubspec.yaml` 中的 `platforms` map 
添加 keys 来指定其支持的平台。
例如，以下是 `hello` 插件的 `flutter:` map：

```yaml
flutter:
  plugin:
    platforms:
      android:
        package: com.example.hello
        pluginClass: HelloPlugin
      ios:
        pluginClass: HelloPlugin

environment:
  sdk: ">=2.1.0 <3.0.0"
  # Flutter versions prior to 1.12 did not support the
  # flutter.plugin.platforms map.
  flutter: ">=1.12.0 <2.0.0"
```

When adding plugin implementations for more platforms,
the `platforms` map should be updated accordingly. 
For example, here's the map in the pubspec file
for the `hello` plugin, when updated to add support
for macOS and web:

当为更多平台添加插件实现时，应相应地更新 `platforms` map，
例如这是支持 Android、iOS、macOS 和 web 的 `hello` 插件的 map：

```yaml
flutter:
  plugin:
    platforms:
      android:
        package: com.example.hello
        pluginClass: HelloPlugin
      ios:
        pluginClass: HelloPlugin
      macos:
        pluginClass: HelloPlugin
      web:
        pluginClass: HelloPlugin
        fileName: hello_web.dart

environment:
  sdk: ">=2.1.0 <3.0.0"
  # Flutter versions prior to 1.12 did not support the
  # flutter.plugin.platforms map.
  flutter: ">=1.12.0 <2.0.0"
```

### Step 1: Create the package

### 第一步：创建 package

To create a plugin package, use the `--template=plugin`
flag with `flutter create`.

想要创建原生插件 package，
请使用带有 `--template=plugin` 标志的 `flutter create` 命令。

Use the `--org` option to specify your organization,
using reverse domain name notation. This value is used
in various package and bundle identifiers in the
generated plugin code.

使用 `--org` 选项，以反向域名表示法来指定你的组织。
该值用于生成的 Android 及 iOS 代码。

```terminal
$ flutter create --org com.example --template=plugin hello
```

This creates a plugin project in the `hello` folder
with the following specialized content:

这将在 `hello` 目录下创建一个插件项目，其中包含以下内容：

**`lib/hello.dart`**
<br> The Dart API for the plugin.

**`lib/hello.dart` 文件**
<br> Dart 插件 API 实现。

**`android/src/main/java/com/example/hello/HelloPlugin.kt`**
<br> The Android platform-specific implementation of the plugin API
  in Kotlin.

**`android/src/main/java/com/example/hello/HelloPlugin.kt` 文件**
<br> Android 平台原生插件 API 实现（使用 Kotlin 编程语言）。

**`ios/Classes/HelloPlugin.m`**
<br> The iOS-platform specific implementation of the plugin API
  in Objective-C.

**`ios/Classes/HelloPlugin.m` 文件**
<br> iOS 平台原生插件 API 实现（使用 Objective-C 编程语言）。

**`example/`**
<br> A Flutter app that depends on the plugin,
  and illustrates how to use it.

**`example/` 文件**
<br> 一个依赖于该插件并说明了如何使用它的 Flutter 应用。

By default, the plugin project uses Swift for iOS code and
Kotlin for Android code. If you prefer Objective-C or Java,
you can specify the iOS language using `-i` and the
Android language using `-a`. For example:

默认情况下，插件项目中 iOS 代码使用 Swift 编写，
Android 代码使用 Kotlin 编写。
如果你更喜欢 Objective-C 或 Java，
你可以通过 `-i` 指定 iOS 所使用的语言和/或
使用`-a` 指定 Android 所使用的语言。比如：

```terminal
$ flutter create --template=plugin -i objc -a java hello
```

### Step 2: Implement the package {#edit-plugin-package}

### 第二步：实现 package {#edit-plugin-package}

As a plugin package contains code for several platforms
written in several programming languages,
some specific steps are needed to ensure a smooth experience.

由于原生插件类型的 package 包含了使用多种编程语言编写的多个平台代码，
因此需要一些特定步骤来保证体验的流畅性。

#### Step 2a: Define the package API (.dart)

#### 步骤 2a：定义 package API（.dart）

The API of the plugin package is defined in Dart code.
Open the main `hello/` folder in your favorite [Flutter editor][].
Locate the file `lib/hello.dart`.

原生插件类型 package 的 API 在 Dart 代码中要首先定义好，
使用你钟爱的 [Flutter 编辑器][Flutter editor]，
打开 `hello` 主目录，并找到 `lib/hello.dart` 文件。

#### Step 2b: Add Android platform code (.kt/.java)

#### 步骤 2b：添加 Android 平台代码（.kt/.java）

We recommend you edit the Android code using Android Studio.

我们建议你使用 Android Studio 来编辑 Android 代码。

Before editing the Android platform code in Android Studio,
first make sure that the code has been built at least once
(in other words, run the example app from your IDE/editor,
or in a terminal execute `cd hello/example; flutter build apk`).

使用 Android Studio 编辑 Android 平台代码之前，首先确保代码至少被构建过一次
（换句话说，即从 IDE/编辑器执行示例程序，
或在终端中执行以下命令：`cd hello/example; flutter build apk`）。

Then use the following steps:

接下来进行如下步骤：

1. Launch Android Studio.

   启动 Android Studio
   
1. Select **Import project** in the
   **Welcome to Android Studio** dialog,
   or select **File > New > Import Project...** from the menu,
   and select the `hello/example/android/build.gradle` file.

   在 “Welcome to Android Studio” 对话框中选择 “Import project”，
   或在菜单中选择“File > New > Import Project...”，
   然后选择 `hello/example/android/build.gradle` 文件；

1. In the **Gradle Sync** dialog, select **OK**.

   在“Gradle Sync”对话框中，选择“OK”；
   
1. In the **Android Gradle Plugin Update** dialog,
   select **Don't remind me again for this project**.

   在“Android Gradle Plugin Update”对话框中，
   选择“Don't remind me again for this project”。

The Android platform code of your plugin is located in
`hello/java/com.example.hello/HelloPlugin`.

The Android platform code of your plugin is located in
`hello/java/com.example.hello/HelloPlugin`.

You can run the example app from Android Studio by
pressing the run (&#9654;) button.

你可以在 Android Studio 中点击运行 &#9654; 按钮来运行示例程序。

#### Step 2c: Add iOS platform code (.swift/.h+.m)

#### 步骤 2c：添加 iOS 平台代码（.swift/.h+.m）

We recommend you edit the iOS code using Xcode.

我们建议你使用 Xcode 来编辑 iOS 代码。

Before editing the iOS platform code in Xcode,
first make sure that the code has been built at least once
(in other words, run the example app from your IDE/editor,
or in a terminal execute
`cd hello/example; flutter build ios --no-codesign`).

使用 Xcode 编辑 iOS 平台代码之前，首先确保代码至少被构建过一次
（即从 IDE/编辑器执行示例程序，或在终端中执行以下命令：
`cd hello/example; flutter build ios --no-codesign`）。

Then use the following steps:

接下来执行下面步骤：

1. Launch Xcode.

   启动 Xcode
   
1. Select **File > Open**, and select the
   `hello/example/ios/Runner.xcworkspace` file.

   选择“File > Open”，
   然后选择 `hello/example/ios/Runner.xcworkspace` 文件。

The iOS platform code for your plugin is located in
`Pods/Development Pods/hello/../../example/ios/.symlinks/plugins/hello/ios/Classes`
in the Project Navigator.

插件的 iOS 平台代码位于项目导航中的这个位置：
`Pods/Development Pods/hello/../../example/ios/.symlinks/plugins/hello/ios/Classes`。

You can run the example app by pressing the run (&#9654;) button.

你可以点击运行 &#9654; 按钮来运行示例程序。

#### Step 2d: Connect the API and the platform code

#### 步骤 2d：关联 API 和平台代码

Finally, you need to connect the API written in Dart code with
the platform-specific implementations.
This is done using a [platform channel][],
or through the interfaces defined in a platform
interface package.

最后，你需要将 Dart 编写的 API 代码与特定平台的实现相互关联。
这是通过 [platform channels][] 完成的。

### Testing your plugin

As of Flutter 1.12, it is now easier to write code to
test your plugin. For more information, see
[Testing your plugin][], a section in
[Supporting the new Android plugins APIs][].

## Adding documentation

## 添加文档

It is recommended practice to add the following documentation
to all packages:

建议将下列文档添加到所有 package 中：

1. A `README.md` file that introduces the package

   `README.md` 文件用来对 package 进行介绍
   
1. A `CHANGELOG.md` file that documents changes in each version
 
   `CHANGELOG.md` 文件用来记录每个版本的更改

1. A [`LICENSE`] file containing the terms under which the package
   is licensed

   [`LICENSE`][] 文件用来阐述 package 的许可条款
   
1. API documentation for all public APIs (see below for details)

   API 文档包含所有的公共 API（详情参见下文）

### API documentation

### API 文档

When you publish a package,
API documentation is automatically generated and
published to pub.dev/documentation.
For example, see the docs for [`device_info`][].

当你提交一个 package 时，会自动生成 API 文档并将其提交到 
pub.flutter-io.cn/documentation，示例请参见 [`device_info`][] 文档。


If you wish to generate API documentation locally on
your development machine, use the following commands:

如果你希望在本地开发环境中生成 API 文档，可以使用以下命令：

<ol markdown="1">
<li markdown="1"><t>Change directory to the location of your package:</t><t>将当前工作目录切换到 package 所在目录：</t>

```terminal
cd ~/dev/mypackage
```
</li>

<li markdown="1"><t>Tell the documentation tool where the
    Flutter SDK is located (change the following commands to reflect
    where you placed it):</t><t>告知文档工具 Flutter SDK 所在位置（请自行更改 Flutter SDK 该在的位置）</t>

```terminal
   export FLUTTER_ROOT=~/dev/flutter  # on macOS or Linux (适用于 macOS 或 Linux 操作系统)

   set FLUTTER_ROOT=~/dev/flutter     # on Windows (适用于 Windows 操作系统)
```
</li>

<li markdown="1"><t>Run the `dartdoc` tool
    (included as part of the Flutter SDK), as follows:</t><t>运行 `dartdoc` 工具（已经包含到 Flutter SDK 了）：</t>

```terminal
   $FLUTTER_ROOT/bin/cache/dart-sdk/bin/dartdoc   # on macOS or Linux (适用于 macOS 或 Linux 操作系统)

   %FLUTTER_ROOT%\bin\cache\dart-sdk\bin\dartdoc  # on Windows (适用于 Windows 操作系统)
```
</li>
</ol>

For tips on how to write API documentation, see
[Effective Dart Documentation][].

关于如何编写 API 文档的建议，请参阅 
[高效 Dart 指南][Effective Dart: Documentation]。

### Adding licenses to the LICENSE file

### 将许可证添加到 LICENSE 文件中

Individual licenses inside each LICENSE file should
be separated by 80 hyphens on their own on a line.

每个 LICENSE 文件中的各个许可证应由 80 个短线字符组成的线段进行分割。

If a LICENSE file contains more than one component license,
then each component license must start with the names of the
packages to which the component license applies,
with each package name on its own line, and the
list of package names separated from the actual
license text by a blank line.
(The packages need not match the names of the pub package.
For example, a package might itself contain code from
multiple third-party sources, and might need to include
a license for each one.)

如果 LICENSE 文件中包含多个组件许可证，那么每个组件许可证必须以其所在 package
的名称开始，每个 package 名称单独一行显示，并且 package
名称列表与实际许可证内容由空行隔开。（package 名称无需与 pub package 相匹配。
比如，一个 package 可能包含多个第三方代码，并且可能需要为每个 package 添加许可证。）

The following example shows a well-organized license file:

如下是一些优秀的许可证文件：

```none
package_1

<some license text>

--------------------------------------------------------------------------------
package_2

<some license text>
```

Here is another example of a well-organized license file:

这些也是可以的：

```none
package_1

<some license text>

--------------------------------------------------------------------------------
package_1
package_2

<some license text>
```

Here is an example of a poorly-organized license file:

这些是一些不太好的示例：

```none
<some license text>

--------------------------------------------------------------------------------
<some license text>
```

Another example of a poorly-organized license file:

这也是一些不太好的示例：

```
package_1

<some license text>
--------------------------------------------------------------------------------
<some license text>
```

## Publishing your package {#publish}

## 提交 package {#publish}

{{site.alert.tip}}

  Have you noticed that some of the packages and plugins
  on pub.dev are designated as [Flutter Favorites][]?
  These are the packages published by verified developers
  and are identified as the packages and plugins you
  should first consider using when writing your app.
  To learn more,
  see the [Flutter Favorites program][].
  
  你是否注意到一些 package 和插件旁边的 [Flutter Favorites][] 标识？
  这是官方挑选出的、由认证的开发者发布的 packages，
  并建议 Flutter 开发者们需要使用时首要考虑的 package。
  了解更多 [Flutter Favorites 项目][Flutter Favorites program]。
{{site.alert.end}}

Once you have implemented a package, you can publish it on
[pub.dev][], so that other developers can easily use it.

一旦完成了 package 的实现，你便可以将其提交到 [pub.dev][]
上，以便其他开发者可以轻松地使用它。

Prior to publishing, make sure to review the `pubspec.yaml`,
`README.md`, and `CHANGELOG.md` files to make sure their
content is complete and correct. Also, to improve the
quality and usability of your package (and to make it
more likely to achieve the status of a Flutter Favorite),
consider including the following items:

发布你的 package 之前，确保检查了这几个文件：`pubspec.yaml`、`README.md` 和
`CHANGELOG.md`，确保它们完整且争取，另外，为了提高 package 的可用性，
可以考虑加入如下的内容：

* Diverse code usage 

  代码的示例用法
  
* Screenshots, animated gifs, or videos

  屏幕截图，GIF 动画或者视频
 
* A link to the corresponding code repository

  代码库的正确指向链接

Next, run the publish command in `dry-run` mode
to see if everything passes analysis:

接下来，运行 dry-run 命令以检验是否所有内容都通过了分析：

```terminal
$ flutter pub publish --dry-run
```

The next step is publishing to pub.dev,
but be sure that you are ready because
[publishing is forever][]:

最后一步是发布，请注意：[发布是永久性][publishing is forever] 的，
运行以下提交命令：

```terminal
$ flutter pub publish
```
{{site.note.alert}}
设置了中国镜像的开发者们请注意：
目前所存在的镜像都不能（也不应该）进行 package 的上传。
如果你设置了镜像，执行上述发布代码可能会造成发布失败。
网络设定好后，无需取消中文镜像，执行下述代码可直接上传：

```terminal
$ flutter pub publish --server=https://pub.dartlang.org
```
{{site.note.end}}

For more details on publishing, see the
[publishing docs][] on dart.dev.

有关提交的详细信息，请查阅关于 Pub 站点的 [提交文档][publishing docs]。

## Handling package interdependencies {#dependencies}

## Package 依赖处理 {#dependencies}

If you are developing a package `hello` that depends on
the Dart API exposed by another package, you need to add
that package to the `dependencies` section of your
`pubspec.yaml` file. The code below makes the Dart API
of the `url_launcher` plugin available to `hello`:

如果你正在开发的 `hello` 依赖于另外一个 package 所公开的 Dart API，
你需要将该 package 添加到文件 `pubspec.yaml` 的 `dependencies` 段中。
以下代码使得插件 `url_launcher` 的 Dart API 在 `hello` 中可用：

```yaml
dependencies:
  url_launcher: ^5.0.0
```

You can now `import 'package:url_launcher/url_launcher.dart'`
and `launch(someUrl)` in the Dart code of `hello`.

现在你可以在 `hello` 的 Dart 代码中使用
`import 'package:url_launcher/url_launcher.dart'` 和 `launch(someUrl)`。

This is no different from how you include packages in
Flutter apps or any other Dart project.

这与你在 Flutter 应用或其他任何 Dart 项目中引入 package 的方式没什么区别。

But if `hello` happens to be a _plugin_ package
whose platform-specific code needs access
to the platform-specific APIs exposed by `url_launcher`,
you also need to add suitable dependency declarations
to your platform-specific build files, as shown below.

但碰巧 `hello` 是一个 **原生插件** package，其特定的平台代码如果需要访问 `url_launcher`
所公开的平台特定 API，那么还需要为特定平台的构建文件添加适当的依赖说明，如下所示：

### Android

The following example sets a dependency for
`url_launcher` in `hello/android/build.gradle`:

在 `hello/android/build.gradle` 文件中为 `url_launcher` 插件设定依赖关系。

```groovy
android {
    // lines skipped
    dependencies {
        provided rootProject.findProject(":url_launcher")
    }
}
```

You can now `import io.flutter.plugins.urllauncher.UrlLauncherPlugin`
and access the `UrlLauncherPlugin`
class in the source code at `hello/android/src`.

现在你可以在 `hello/android/src` 目录下的源代码文件中使用
`import io.flutter.plugins.urllauncher.UrlLauncherPlugin` 并访问类 `UrlLauncherPlugin`。

### iOS

The following example sets a dependency for
`url_launcher` in `hello/ios/hello.podspec`:

在 `hello/ios/hello.podspec` 文件中为 `url_launcher` 插件设定依赖关系。

```ruby
Pod::Spec.new do |s|
  # lines skipped
  s.dependency 'url_launcher'
```
You can now `#import "UrlLauncherPlugin.h"` and
access the `UrlLauncherPlugin` class in the source code
at `hello/ios/Classes`.

现在你可以在 `hello/ios/Classes` 目录下的源代码文件中使用 `#import "UrlLauncherPlugin.h"` 
并访问 `UrlLauncherPlugin` 这个类了。

### Web

All web dependencies are handled by the `pubspec.yaml`
file like any other Dart package.

与其他的 Dart package 一样，所有的 Web 依赖都由文件 `pubspec.yaml` 来处理。

{% comment %}
<!-- Remove until we have better text. -->
### MacOS

PENDING
{% endcomment %}


[Dart library package]: {{site.dart-site}}/guides/libraries/create-library-packages
[`device_info`]: {{site.pub-api}}/device_info/latest
[Effective Dart Documentation]: {{site.dart-site}}/guides/language/effective-dart/documentation
[federated plugins]: #federated-plugins
[`fluro`]: {{site.pub}}/packages/fluro
[Flutter editor]: /docs/get-started/editor
[Flutter Favorites]: {{site.pub}}/flutter/favorites
[Flutter Favorites program]: /docs/development/packages-and-plugins/favorites
[How to Write a Flutter Web Plugin, Part 1]: {{site.medium}}/flutter/how-to-write-a-flutter-web-plugin-5e26c689ea1
[How To Write a Flutter Web Plugin, Part 2]: {{site.medium}}/flutter/how-to-write-a-flutter-web-plugin-part-2-afdddb69ece6
[issue #33302]: https://github.com/flutter/flutter/issues/33302
[`LICENSE`]: #adding-licenses-to-the-license-file
[`path`]: {{site.pub}}/packages/path
[platform channel]: /docs/development/platform-integration/platform-channels
[pub.dev]: {{site.pub}}
[publishing docs]: {{site.dart-site}}/tools/pub/publishing
[publishing is forever]: {{site.dart-site}}/tools/pub/publishing#publishing-is-forever
[Supporting the new Android plugins APIs]: /docs/development/packages-and-plugins/plugin-api-migration
[supported-platforms]: #plugin-platforms
[test your plugin]: #testing-your-plugin
[Testing your plugin]: /docs/development/packages-and-plugins/plugin-api-migration#testing-your-plugin
[unit tests]: /docs/testing#unit-tests
[`url_launcher`]: {{site.pub}}/packages/url_launcher
[Writing a good plugin]: {{site.flutter-medium}}/writing-a-good-flutter-plugin-1a561b986c9c
