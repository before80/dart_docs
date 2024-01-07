+++
title = "入门：命令行和服务器应用程序"
date = 2024-01-05T20:29:36+08:00
weight = 10
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文: [https://dart.dev/tutorials/server/get-started](https://dart.dev/tutorials/server/get-started)

## Get started: Command-line and server apps 入门：命令行和服务器应用程序

Follow these steps to start using the Dart SDK to develop command-line and server apps. First you’ll play with the Dart language in your browser, no download required. Then you’ll install the Dart SDK, write a small program, and run that program using the Dart VM. Finally, you’ll use an AOT (*ahead of time*) compiler to compile your finished program to native machine code, which you’ll execute using the Dart runtime.

​	按照以下步骤开始使用 Dart SDK 开发命令行和服务器应用程序。首先，您将在浏览器中使用 Dart 语言，无需下载。然后，您将安装 Dart SDK，编写一个小程序，并使用 Dart VM 运行该程序。最后，您将使用 AOT（提前）编译器将已完成的程序编译为本机机器代码，然后使用 Dart 运行时执行该代码。

## 1. 在 DartPad 中使用 Dart 代码 Play with Dart code in DartPad 

With [DartPad](https://dart.dev/tools/dartpad) you can experiment with the Dart language and APIs, no download necessary.

​	借助 DartPad，您可以试用 Dart 语言和 API，无需下载。

For example, here’s an embedded DartPad that lets you play with the code for a small Hello World program. Click **Run** to run the app; output appears in the console view. Try editing the source code—perhaps you’d like to change the greeting to use another language.

​	例如，这是一个嵌入式 DartPad，您可以用它来玩一个小型的 Hello World 程序的代码。单击运行以运行该应用；输出显示在控制台视图中。尝试编辑源代码——也许您想更改问候语以使用另一种语言。

*info* **Note:** If you see an empty box instead of code, go to the [DartPad troubleshooting page](https://dart.dev/tools/dartpad/troubleshoot).

​	注意：如果您看到一个空框而不是代码，请转到 DartPad 故障排除页面。

<iframe src="https://dartpad.dev/embed-dart.html?theme=light&amp;run=dartpad&amp;split=false&amp;ga_id=hello_world" data-gtm-vis-first-on-screen13029053_11="342" data-gtm-vis-total-visible-time13029053_11="100" data-gtm-vis-has-fired13029053_11="1" style="box-sizing: border-box; border: 1px solid rgb(204, 204, 204); margin-bottom: 1rem; min-height: 400px; resize: vertical; width: 896.007px;"></iframe>

More information:

​	更多信息：

- [DartPad documentation
  DartPad 文档](https://dart.dev/tools/dartpad)
- [Dart language tour
  Dart 语言之旅](https://dart.dev/language)
- [Dart core library documentation
  Dart 核心库文档](https://dart.dev/libraries)

##  2. 安装 Dart - Install Dart

To develop real apps, you need an SDK. You can either download the Dart SDK directly (as described below) or [download the Flutter SDK,](https://docs.flutter.dev/get-started/install) which includes the full Dart SDK.

​	要开发真正的应用，您需要一个 SDK。您可以直接下载 Dart SDK（如下所述），或下载包含完整 Dart SDK 的 Flutter SDK。

- Windows

  > Use [Chocolatey](https://chocolatey.org/) to install a stable release of the Dart SDK.
  >
  > ​	使用 Chocolatey 安装 Dart SDK 的稳定版本。
  >
  > *error* **Important:** These commands require administrator privileges. If you need help on starting an administrator-level command prompt, try a search like *[cmd admin](https://www.google.com/search?q=cmd+admin).*
  >
  > ​	重要提示：这些命令需要管理员权限。如果您需要有关启动管理员级别命令提示符的帮助，请尝试搜索类似 cmd admin 的内容。
  >
  > To install the Dart SDK:
  >
  > ​	要安装 Dart SDK：
  >
  > ```sh
  > C:\> choco install dart-sdk
  > ```

- Linux

  > You can use APT to install the Dart SDK on Linux.
  >
  > ​	您可以在 Linux 上使用 APT 安装 Dart SDK。
  >
  > 1. Perform the following one-time setup:   执行以下一次性设置：
  >
  >    ```sh
  >    $ sudo apt-get update
  >    $ sudo apt-get install apt-transport-https
  >    $ wget -qO- https://dl-ssl.google.com/linux/linux_signing_key.pub | sudo gpg --dearmor -o /usr/share/keyrings/dart.gpg
  >    $ echo 'deb [signed-by=/usr/share/keyrings/dart.gpg arch=amd64] https://storage.googleapis.com/download.dartlang.org/linux/debian stable main' | sudo tee /etc/apt/sources.list.d/dart_stable.list
  >    ```
  >
  > 2. Install the Dart SDK:   安装 Dart SDK：
  >
  >    ```sh
  >    $ sudo apt-get update
  >    $ sudo apt-get install dart
  >    ```

- Mac

  > With [Homebrew,](https://brew.sh/) installing Dart is easy.
  >
  > ​	使用 Homebrew，安装 Dart 非常简单。
  >
  > ```sh
  > $ brew tap dart-lang/dart
  > $ brew install dart
  > ```

​	*error* **Important:** For more information, including how to **adjust your `PATH`**, see [Get the Dart SDK](https://dart.dev/get-dart).

​	重要提示：有关更多信息，包括如何调整您的 `PATH` ，请参阅获取 Dart SDK。

## 3. 创建一个小应用程序 Create a small app 

Use the [`dart create`](https://dart.dev/tools/dart-create) command and the `console` template to create a command-line app:

​	使用 `dart create` 命令和 `console` 模板创建命令行应用程序：

```
$ dart create -t console cli
```

This command creates a small Dart app that has the following:

​	此命令创建一个具有以下内容的小型 Dart 应用程序：

- A main Dart source file, `bin/cli.dart`, that contains a top-level `main()` function. This is the entrypoint for your app.
- 一个包含顶级 `main()` 函数的主 Dart 源文件 `bin/cli.dart` 。这是您应用的入口点。
- An additional Dart file, `lib/cli.dart`, that contains the functionality of the app and is imported by the `cli.dart` file.
- 另一个包含应用功能的 Dart 文件 `lib/cli.dart` ，由 `cli.dart` 文件导入。
- A pubspec file, `pubspec.yaml`, that contains the app’s metadata, including information about which [packages](https://dart.dev/guides/packages) the app depends on and which versions of those packages are required.
- 一个包含应用元数据的 pubspec 文件 `pubspec.yaml` ，其中包括有关应用依赖的软件包以及所需软件包版本的信息。

*info* **Note:** Under the hood, `dart create` runs [`dart pub get`](https://dart.dev/tools/pub/cmd/pub-get), which scans the generated pubspec file and downloads dependencies. If you add other dependencies to your pubspec file, then run `dart pub get` to download them.

​	注意：在后台， `dart create` 运行 `dart pub get` ，后者扫描生成的 pubspec 文件并下载依赖项。如果您向 pubspec 文件添加其他依赖项，请运行 `dart pub get` 以下载它们。

## 4. 运行应用 Run the app 

To run the app from the command line, use the Dart VM by running the [`dart run`](https://dart.dev/tools/dart-run) command in the app’s top directory:

​	要从命令行运行应用，请在应用的顶级目录中运行 `dart run` 命令，使用 Dart VM：

```
$ cd cli
$ dart run
Hello world: 42!
```

If you want to run the app with debugging support, see [Dart DevTools](https://dart.dev/tools/dart-devtools).

​	如果您想在调试支持下运行应用，请参阅 Dart DevTools。

## 5. 修改应用 Modify the app 

Let’s customize the app you just created.

​	让我们自定义您刚刚创建的应用。

1. Edit `lib/cli.dart` to calculate a different result. For example, divide the previous value by two (for details about `~/`, see Arithmetic operators):

2. 
   编辑 `lib/cli.dart` 以计算不同的结果。例如，将前一个值除以二（有关 `~/` 的详细信息，请参阅算术运算符）：

   ```dart
   int calculate() {
     return 6 * 7 ~/ 2;
   }
   ```

3. Save your changes.

4. 保存您的更改。

5. Rerun the main entrypoint of your app:

6. 重新运行应用的主要入口点：

   ```
   $ dart run
   Hello world: 21!
   ```

More information: [Write command-line apps](https://dart.dev/tutorials/server/cmdline)

​	更多信息：编写命令行应用

## 6. 编译以进行生产 Compile for production 

The steps above used the Dart VM (`dart`) to run the app. The Dart VM is optimized for fast, incremental compilation to provide instant feedback during development. Now that your small app is done, it’s time to AOT compile your Dart code to optimized native machine code.

​	上述步骤使用 Dart VM ( `dart` ) 来运行应用。Dart VM 经过优化，可进行快速增量编译，以便在开发期间提供即时反馈。现在您的小型应用已完成，是时候将您的 Dart 代码 AOT 编译为经过优化的本机机器代码了。

Use the `dart compile` tool to AOT compile the program to machine code:

​	使用 `dart compile` 工具将程序 AOT 编译为机器代码：

```
$ dart compile exe bin/cli.dart
```

Notice how the compiled program starts instantly, completing quickly:

​	请注意已编译的程序如何立即启动，并快速完成：

```
$ time bin/cli.exe
Hello world: 21!

real	0m0.016s
user	0m0.008s
sys	0m0.006s
```

## What next?

Check out these resources:

​	查看以下资源：

- Additional tutorials and codelabs for Dart

- 
  Dart 的其他教程和代码实验室
  
  - [Tutorials
    教程](https://dart.dev/tutorials)
  - [Codelabs
    代码实验室](https://dart.dev/codelabs)
  
- Dart language, libraries, and conventions

- 
  Dart 语言、库和约定
  
  - [Language tour
    语言之旅](https://dart.dev/language)
  - [Dart core library documentation
    Dart 核心库文档](https://dart.dev/libraries)
  - [Effective Dart
    高效的 Dart](https://dart.dev/effective-dart)
  
- Tools and libraries

- 
  工具和库
  
  - [Dart SDK](https://dart.dev/tools/sdk)
  - [Dart tools
    Dart 工具](https://dart.dev/tools)
  - [IDEs
    IDE](https://dart.dev/tools#ides-and-editors)
  
- Other examples of natively compiled apps

- 
  其他原生编译应用示例
  
  - [native_app](https://github.com/dart-lang/samples/tree/main/native_app)

If you get stuck, find help at [Community and support.](https://dart.dev/community)

​	如果您遇到困难，请在社区和支持中查找帮助。
