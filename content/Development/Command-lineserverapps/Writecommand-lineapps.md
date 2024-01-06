+++
title = "编写命令行应用"
date = 2024-01-05T20:29:36+08:00
weight = 20
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文: [https://dart.dev/tutorials/server/cmdline](https://dart.dev/tutorials/server/cmdline)

## Write command-line apps 编写命令行应用

#### What's the point? 有什么意义？

- Command-line applications need to do input and output.
  命令行应用程序需要进行输入和输出。
- The `dart:io` library provides I/O functionality.
  `dart:io` 库提供 I/O 功能。
- The args package helps define and parse command-line arguments.
  args 包有助于定义和解析命令行参数。
- A `Future` object represents a value that will be available at some time in the future.
  `Future` 对象表示将来某个时间可用的值。
- Streams provide a series of asynchronous data events.
  流提供一系列异步数据事件。
- Most input and output requires the use of streams.
  大多数输入和输出都需要使用流。

*info* **Note:** This tutorial uses the `async` and `await` language features, which rely on the [`Future`](https://api.dart.dev/stable/dart-async/Future-class.html) and [`Stream`](https://api.dart.dev/stable/dart-async/Stream-class.html) classes for asynchronous support. To learn more about these features, see the [asynchronous programming codelab](https://dart.dev/codelabs/async-await) and the [streams tutorial](https://dart.dev/tutorials/language/streams).
注意：本教程使用 `async` 和 `await` 语言功能，这些功能依赖于 `Future` 和 `Stream` 类来提供异步支持。要详细了解这些功能，请参阅异步编程代码实验室和流教程。

This tutorial teaches you how to build command-line apps and shows you a few small command-line applications. These programs use resources that most command-line applications need, including the standard output, error, and input streams, command-line arguments, files and directories, and more.
本教程将教您如何构建命令行应用，并向您展示一些小型命令行应用程序。这些程序使用大多数命令行应用程序所需的资源，包括标准输出、错误和输入流、命令行参数、文件和目录等。

## Running an app with the standalone Dart VM 使用独立的 Dart VM 运行应用

To run a command-line app in the Dart VM, use `dart run`. The `dart` commands are included with the [Dart SDK](https://dart.dev/tools/sdk).
要在 Dart VM 中运行命令行应用，请使用 `dart run` 。 `dart` 命令包含在 Dart SDK 中。

*error* **Important:** The location of the SDK installation directory (we’ll call it `<sdk-install-dir>`) depends on your platform and how you installed the SDK. You can find `dart` in `<sdk-install-dir>/bin`. By putting this directory in your PATH you can refer to the `dart` command by name.
重要提示：SDK 安装目录的位置（我们称之为 `<sdk-install-dir>` ）取决于您的平台以及您安装 SDK 的方式。您可以在 `<sdk-install-dir>/bin` 中找到 `dart` 。通过将此目录放在您的 PATH 中，您可以按名称引用 `dart` 命令。

Let’s run a small program.
我们来运行一个小程序。

1. Create a file called `hello_world.dart` that contains this code:
   创建一个名为 `hello_world.dart` 的文件，其中包含此代码：

   ```dart
   void main() {
     print('Hello, World!');
   }
   ```

2. In the directory that contains the file you just created, run the program:
   在包含您刚刚创建的文件的目录中，运行该程序：

   ```
   $ dart run hello_world.dart
   Hello, World!
   ```

The Dart tool supports many commands and options. Use `dart --help` to see commonly used commands and options. Use `dart --verbose` to see all options.
Dart 工具支持许多命令和选项。使用 `dart --help` 查看常用的命令和选项。使用 `dart --verbose` 查看所有选项。

## Overview of the dcat app code dcat 应用代码概述

This tutorial covers the details of a small sample app called `dcat`, which displays the contents of any files listed on the command line. This app uses various classes, functions, and properties available to command-line apps. Continue on in the tutorial to learn about each part of the app and the various APIs used.
本教程介绍了一个名为 `dcat` 的小型示例应用的详细信息，该应用显示命令行中列出的任何文件的内容。此应用使用各种类、函数和属性，这些类、函数和属性可供命令行应用使用。继续学习本教程，了解应用的每个部分和使用的各种 API。

```dart
import 'dart:convert';
import 'dart:io';

import 'package:args/args.dart';

const lineNumber = 'line-number';

void main(List<String> arguments) {
  exitCode = 0; // Presume success
  final parser = ArgParser()..addFlag(lineNumber, negatable: false, abbr: 'n');

  ArgResults argResults = parser.parse(arguments);
  final paths = argResults.rest;

  dcat(paths, showLineNumbers: argResults[lineNumber] as bool);
}

Future<void> dcat(List<String> paths, {bool showLineNumbers = false}) async {
  if (paths.isEmpty) {
    // No files provided as arguments. Read from stdin and print each line.
    await stdin.pipe(stdout);
  } else {
    for (final path in paths) {
      var lineNumber = 1;
      final lines = utf8.decoder
          .bind(File(path).openRead())
          .transform(const LineSplitter());
      try {
        await for (final line in lines) {
          if (showLineNumbers) {
            stdout.write('${lineNumber++} ');
          }
          stdout.writeln(line);
        }
      } catch (_) {
        await _handleError(path);
      }
    }
  }
}

Future<void> _handleError(String path) async {
  if (await FileSystemEntity.isDirectory(path)) {
    stderr.writeln('error: $path is a directory');
  } else {
    exitCode = 2;
  }
}
```

### Getting dependencies 获取依赖项

You might notice that dcat depends on a package named **args**. To get the args package, use the [pub package manager](https://dart.dev/guides/packages).
您可能会注意到 dcat 依赖于一个名为 args 的软件包。要获取 args 软件包，请使用 pub 软件包管理器。

A real app has tests, license files, dependency files, examples, and so on. For the first app though, we can easily create only what is necessary with the [`dart create`](https://dart.dev/tools/dart-create) command.
真正的应用程序具有测试、许可证文件、依赖文件、示例等。不过，对于第一个应用程序，我们可以轻松地仅使用 `dart create` 命令创建必要的应用程序。

1. Inside a directory, create the dcat app with the dart tool.
   在目录中，使用 dart 工具创建 dcat 应用程序。

   ```
   $ dart create dcat
   ```

2. Change to the created directory.
   切换到已创建的目录。

   ```
   $ cd dcat
   ```

3. Inside the `dcat` directory, use [`dart pub add`](https://dart.dev/tools/pub/cmd/pub-add) to add the `args` package as a dependency. This adds `args` to the list of your dependencies found in the `pubspec.yaml` file.
   在 `dcat` 目录中，使用 `dart pub add` 将 `args` 包添加为依赖项。这会将 `args` 添加到在 `pubspec.yaml` 文件中找到的依赖项列表中。

   ```
   $ dart pub add args
   ```

4. Open the `bin/dcat.dart` file and copy the preceding code into it.
   打开 `bin/dcat.dart` 文件，并将前面的代码复制到其中。

*info* **Note:** To learn more about using packages and organizing your code, check out [How to use packages](https://dart.dev/guides/packages) and [Package layout conventions](https://dart.dev/tools/pub/package-layout).
注意：要详细了解如何使用包和组织代码，请查看如何使用包和包布局约定。

### Running dcat 运行 dcat

Once you have your app’s dependencies, you can run the app from the command line over any text file, like `pubspec.yaml`:
获得应用程序的依赖项后，您可以通过命令行对任何文本文件（如 `pubspec.yaml` ）运行该应用程序：

```
$ dart run bin/dcat.dart -n pubspec.yaml
1 name: dcat
2 description: A sample command-line application.
3 version: 1.0.0
4 # repository: https://github.com/my_org/my_repo
5 
6 environment:
7   sdk: ^3.2.0
8 
9 # Add regular dependencies here.
10 dependencies:
11   args: ^2.4.2
12   # path: ^1.8.0
13 
14 dev_dependencies:
15   lints: ^2.1.0
16   test: ^1.24.0
```

This command displays each line of the specified file. Because you specified the `-n` option, a line number is displayed before each line.
此命令会显示指定文件的每一行。由于您指定了 `-n` 选项，因此每行前都会显示一个行号。

## Parsing command-line arguments 解析命令行参数

The [args package](https://pub.dev/packages/args) provides parser support for transforming command-line arguments into a set of options, flags, and additional values. Import the package’s [args library](https://pub.dev/documentation/args/latest/args/args-library.html) as follows:
args 包提供解析器支持，可将命令行参数转换为一组选项、标志和附加值。按如下方式导入包的 args 库：

```dart
import 'package:args/args.dart';
```

The `args` library contains these classes, among others:
库 `args` 包含这些类，包括：

| Class 类                                                     | Description 说明                                             |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| [ArgParser](https://pub.dev/documentation/args/latest/args/ArgParser-class.html) | A command-line argument parser. 命令行参数解析器。           |
| [ArgResults](https://pub.dev/documentation/args/latest/args/ArgResults-class.html) | The result of parsing command-line arguments using `ArgParser`. 使用 `ArgParser` 解析命令行参数的结果。 |

The following code in the `dcat` app uses these classes to parse and store the specified command-line arguments:
应用程序 `dcat` 中的以下代码使用这些类来解析和存储指定的命令行参数：

```dart
void main(List<String> arguments) {
  exitCode = 0; // Presume success
  final parser = ArgParser()..addFlag(lineNumber, negatable: false, abbr: 'n');

  ArgResults argResults = parser.parse(arguments);
  final paths = argResults.rest;

  dcat(paths, showLineNumbers: argResults[lineNumber] as bool);
}
```

The Dart runtime passes command-line arguments to the app’s `main` function as a list of strings. The `ArgParser` is configured to parse the `-n` option. Then, the result of parsing command-line arguments is stored in `argResults`.
Dart 运行时将命令行参数作为字符串列表传递给应用的 `main` 函数。 `ArgParser` 配置为解析 `-n` 选项。然后，解析命令行参数的结果存储在 `argResults` 中。

The following diagram shows how the `dcat` command line used above is parsed into an `ArgResults` object.
下图显示了如何将上面使用的 `dcat` 命令行解析为 `ArgResults` 对象。

![Run dcat from the command-line](./Writecommand-lineapps_img/dcat-dart-run.svg+xml)

You can access flags and options by name, treating an `ArgResults` like a `Map`. You can access other values using the `rest` property.
您可以按名称访问标志和选项，将 `ArgResults` 视为 `Map` 。您可以使用 `rest` 属性访问其他值。

The [API reference](https://pub.dev/documentation/args/latest/args/args-library.html) for the `args` library provides detailed information to help you use the `ArgParser` and `ArgResults` classes.
`args` 库的 API 参考提供了详细信息，可帮助您使用 `ArgParser` 和 `ArgResults` 类。

## Reading and writing with stdin, stdout, and stderr 使用 stdin、stdout 和 stderr 进行读写

Like other languages, Dart has standard output, standard error, and standard input streams. The standard I/O streams are defined at the top level of the `dart:io` library:
与其他语言一样，Dart 具有标准输出、标准错误和标准输入流。标准 I/O 流在 `dart:io` 库的顶层定义：

| Stream                                                    | Description 说明             |
| --------------------------------------------------------- | ---------------------------- |
| [stdout](https://api.dart.dev/stable/dart-io/stdout.html) | The standard output 标准输出 |
| [stderr](https://api.dart.dev/stable/dart-io/stderr.html) | The standard error 标准错误  |
| [stdin](https://api.dart.dev/stable/dart-io/stdin.html)   | The standard input 标准输入  |

Import the `dart:io` library as follows:
按如下方式导入 `dart:io` 库：

```dart
import 'dart:io';
```

*info* **Note:** Web apps (apps that depend on `dart:html`) can’t use the `dart:io` library.
注意：Web 应用（依赖于 `dart:html` 的应用）无法使用 `dart:io` 库。

### stdout

The following code from the `dcat` app writes the line numbers to `stdout` (if the `-n` option is specified) followed by the contents of the line from the file.
以下代码来自 `dcat` 应用，它将行号写入 `stdout` （如果指定了 `-n` 选项），然后写入文件中的行内容。

```dart
if (showLineNumbers) {
  stdout.write('${lineNumber++} ');
}
stdout.writeln(line);
```

The `write()` and `writeln()` methods take an object of any type, convert it to a string, and print it. The `writeln()` method also prints a newline character. The `dcat` app uses the `write()` method to print the line number so the line number and text appear on the same line.
`write()` 和 `writeln()` 方法获取任何类型的对象，将其转换为字符串，然后打印出来。 `writeln()` 方法还会打印一个换行符。 `dcat` 应用使用 `write()` 方法打印行号，以便行号和文本出现在同一行上。

You can also use the `writeAll()` method to print a list of objects, or use `addStream()` to asynchronously print all the elements from a stream.
您还可以使用 `writeAll()` 方法打印对象列表，或使用 `addStream()` 异步打印流中的所有元素。

`stdout` provides more functionality than the `print()` function. For example, you can display the contents of a stream with `stdout`. However, you must use `print()` instead of `stdout` for apps that run on the web.
`stdout` 提供的功能比 `print()` 函数更多。例如，您可以使用 `stdout` 显示流的内容。但是，对于在网络上运行的应用，您必须使用 `print()` 而不是 `stdout` 。

### stderr

Use `stderr` to write error messages to the console. The standard error stream has the same methods as `stdout`, and you use it in the same way. Although both `stdout` and `stderr` print to the console, their output is separate and can be redirected or piped in the command line or programmatically to different destinations.
使用 `stderr` 将错误消息写入控制台。标准错误流具有与 `stdout` 相同的方法，并且以相同的方式使用它。尽管 `stdout` 和 `stderr` 都打印到控制台，但它们的输出是分开的，并且可以在命令行或以编程方式重定向或管道传输到不同的目标。

This code from `dcat` prints an error message if the user tries to list a directory.
如果用户尝试列出目录，则来自 `dcat` 的此代码会打印一条错误消息。

The following code from the `dcat` app prints an error message if the user tries to output the lines of a directory instead of a file.
如果用户尝试输出目录的行而不是文件，则来自 `dcat` 应用程序的以下代码会打印一条错误消息。

```dart
if (await FileSystemEntity.isDirectory(path)) {
  stderr.writeln('error: $path is a directory');
} else {
  exitCode = 2;
}
```

### stdin

The standard input stream typically reads data synchronously from the keyboard, although it can read asynchronously and get input piped in from the standard output of another program.
标准输入流通常从键盘同步读取数据，尽管它可以异步读取并从另一个程序的标准输出获取输入管道。

Here’s a small program that reads a single line from `stdin`:
这是一个从 `stdin` 读取单行的简单程序：

```dart
import 'dart:io';

void main() {
  stdout.writeln('Type something');
  final input = stdin.readLineSync();
  stdout.writeln('You typed: $input');
}
```

The `readLineSync()` method reads text from the standard input stream, blocking until the user types in text and presses return. This little program prints out the typed text.
`readLineSync()` 方法从标准输入流读取文本，阻塞直到用户键入文本并按回车键。这个小程序会打印出键入的文本。

In the `dcat` app, if the user does not provide a filename on the command line, the program instead reads from stdin using the `pipe()` method. Because `pipe()` is asynchronous (returning a `Future`, even though this code doesn’t use that return value), the code that calls it uses `await`.
在 `dcat` 应用程序中，如果用户未在命令行中提供文件名，则程序会使用 `pipe()` 方法从标准输入中读取。由于 `pipe()` 是异步的（返回 `Future` ，即使此代码未使用该返回值），因此调用它的代码使用 `await` 。

```dart
await stdin.pipe(stdout);
```

In this case, the user types in lines of text, and the app copies them to stdout. The user signals the end of input by pressing Control+D (or Control+Z on Windows).
在这种情况下，用户键入文本行，应用程序将它们复制到标准输出。用户通过按 Control + D （或在 Windows 上按 Control + Z ）来发出输入结束信号。

```
$ dart run bin/dcat.dart
The quick brown fox jumps over the lazy dog.
The quick brown fox jumps over the lazy dog.
```

## Getting info about a file 获取有关文件的信息

The [`FileSystemEntity`](https://api.dart.dev/stable/dart-io/FileSystemEntity-class.html) class in the `dart:io` library provides properties and static methods that help you inspect and manipulate the file system.
`FileSystemEntity` 库中的 `dart:io` 类提供了可帮助您检查和操作文件系统的属性和静态方法。

For example, if you have a path, you can determine whether the path is a file, a directory, a link, or not found by using the `type()` method from the `FileSystemEntity` class. Because the `type()` method accesses the file system, it performs the check asynchronously.
例如，如果您有路径，则可以使用 `FileSystemEntity` 类中的 `type()` 方法来确定该路径是文件、目录、链接还是未找到。由于 `type()` 方法会访问文件系统，因此它会异步执行检查。

The following code from the `dcat` app uses `FileSystemEntity` to determine if the path provided on the command line is a directory. The returned `Future` completes with a boolean that indicates if the path is a directory or not. Because the check is asynchronous, the code calls `isDirectory()` using `await`.
来自 `dcat` 应用程序的以下代码使用 `FileSystemEntity` 来确定命令行上提供的路径是否为目录。返回的 `Future` 使用布尔值完成，该布尔值指示路径是否为目录。由于检查是异步的，因此代码使用 `await` 调用 `isDirectory()` 。`dcat` 类中的其他有趣方法包括 `FileSystemEntity` 、 `Future` 、 `isDirectory()` 、 `await` 和 ，所有这些方法也使用 返回值。

```dart
if (await FileSystemEntity.isDirectory(path)) {
  stderr.writeln('error: $path is a directory');
} else {
  exitCode = 2;
}
```

Other interesting methods in the `FileSystemEntity` class include `isFile()`, `exists()`, `stat()`, `delete()`, and `rename()`, all of which also use a `Future` to return a value.

`FileSystemEntity` is the superclass for the `File`, `Directory`, and `Link` classes.
`FileSystemEntity` 是 `File` 、 `Directory` 和 `Link` 类的超类。

## Reading a file 读取文件

The `dcat` apps opens each file listed on the command line with the `openRead()` method, which returns a `Stream`. The `await for` block waits for the file to be read and decoded asynchronously. When the data becomes available on the stream, the app prints it to stdout.
该 `dcat` 应用使用 `openRead()` 方法打开命令行中列出的每个文件，该方法会返回一个 `Stream` 。 `await for` 块等待文件以异步方式读取和解码。当数据在流中可用时，该应用会将其打印到 stdout。

```dart
for (final path in paths) {
  var lineNumber = 1;
  final lines = utf8.decoder
      .bind(File(path).openRead())
      .transform(const LineSplitter());
  try {
    await for (final line in lines) {
      if (showLineNumbers) {
        stdout.write('${lineNumber++} ');
      }
      stdout.writeln(line);
    }
  } catch (_) {
    await _handleError(path);
  }
}
```

The following highlights the rest of the code, which uses two decoders that transform the data before making it available in the `await for` block. The UTF8 decoder converts the data into Dart strings. `LineSplitter` splits the data at newlines.
以下突出显示了其余代码，该代码使用两个解码器在 `await for` 块中提供数据之前对数据进行转换。UTF8 解码器将数据转换为 Dart 字符串。 `LineSplitter` 在换行符处拆分数据。

```dart
for (final path in paths) {
  var lineNumber = 1;
  final lines = utf8.decoder
      .bind(File(path).openRead())
      .transform(const LineSplitter());
  try {
    await for (final line in lines) {
      if (showLineNumbers) {
        stdout.write('${lineNumber++} ');
      }
      stdout.writeln(line);
    }
  } catch (_) {
    await _handleError(path);
  }
}
```

The `dart:convert` library provides these and other data converters, including one for JSON. To use these converters you need to import the `dart:convert` library:
库提供这些和其他数据转换器，包括一个用于 JSON 的转换器。要使用这些转换器，您需要导入库：

```dart
import 'dart:convert';
```

## Writing to a file 写入文件

The easiest way to write text to a file is to create a [`File`](https://api.dart.dev/stable/dart-io/File-class.html) object and use the `writeAsString()` method:
将文本写入文件的最简单方法是创建一个 `File` 对象并使用 `writeAsString()` 方法：

```dart
final quotes = File('quotes.txt');
const stronger = 'That which does not kill us makes us stronger. -Nietzsche';

await quotes.writeAsString(stronger, mode: FileMode.append);
```

The `writeAsString()` method writes the data asynchronously. It opens the file before writing and closes the file when done. To append data to an existing file, you can use the optional named parameter `mode` and set its value to `FileMode.append`. Otherwise, the mode defaults to `FileMode.write` and the previous contents of the file, if any, are overwritten.
`writeAsString()` 方法异步写入数据。它在写入之前打开文件，并在完成后关闭文件。要将数据追加到现有文件，可以使用可选的命名参数 `mode` 并将其值设置为 `FileMode.append` 。否则，模式默认为 `FileMode.write` ，并且文件的先前内容（如果有）将被覆盖。

If you want to write more data, you can open the file for writing. The `openWrite()` method returns an `IOSink`, which has the same type as stdin and stderr. When using the `IOSink` returned from `openWrite()`, you can continue to write to the file until done, at which time, you must manually close the file. The `close()` method is asynchronous and returns a `Future`.
如果要写入更多数据，可以打开文件进行写入。 `openWrite()` 方法返回一个 `IOSink` ，其类型与 stdin 和 stderr 相同。在使用从 `openWrite()` 返回的 `IOSink` 时，可以继续将数据写入文件，直到完成，此时必须手动关闭文件。 `close()` 方法是异步的，并返回一个 `Future` 。

```dart
final quotes = File('quotes.txt').openWrite(mode: FileMode.append);

quotes.write("Don't cry because it's over, ");
quotes.writeln('smile because it happened. -Dr. Seuss');
await quotes.close();
```

## Getting environment information 获取环境信息

Use the [`Platform`](https://api.dart.dev/stable/dart-io/Platform-class.html) class to get information about the machine and operating system that your app is running on.
使用 `Platform` 类获取有关应用程序正在运行的计算机和操作系统的的信息。

The static [`Platform.environment`](https://api.dart.dev/stable/dart-io/Platform/environment.html) property provides a copy of the environment variables in an immutable map. If you need a mutable map (modifiable copy) you can use `Map.of(Platform.environment)`.
静态 `Platform.environment` 属性以不可变映射的形式提供环境变量的副本。如果您需要一个可变映射（可修改的副本），可以使用 `Map.of(Platform.environment)` 。

```dart
final envVarMap = Platform.environment;

print('PWD = ${envVarMap['PWD']}');
print('LOGNAME = ${envVarMap['LOGNAME']}');
print('PATH = ${envVarMap['PATH']}');
```

`Platform` provides other useful properties that give information about the machine, OS, and currently running app. For example:
`Platform` 提供其他有用的属性，这些属性提供有关机器、操作系统和当前正在运行的应用程序的信息。例如：

- [`Platform.isMacOS()`](https://api.dart.dev/stable/dart-io/Platform/isMacOS.html)
- [`Platform.numberOfProcessors`](https://api.dart.dev/stable/dart-io/Platform/numberOfProcessors.html)
- [`Platform.script`](https://api.dart.dev/stable/dart-io/Platform/script.html)

## Setting exit codes 设置退出代码

The `dart:io` library defines a top-level property, `exitCode`, that you can change to set the exit code for the current invocation of the Dart VM. An exit code is a number passed from a Dart app to the parent process to indicate the success, failure, or other state of the execution of the app.
`dart:io` 库定义了一个顶级属性 `exitCode` ，您可以更改该属性以设置 Dart VM 当前调用的退出代码。退出代码是从 Dart 应用程序传递给父进程的数字，用于指示应用程序执行的成功、失败或其他状态。

The `dcat` app sets the exit code in the `_handleError()` function to indicate that an error occurred during execution.
`dcat` 应用程序在 `_handleError()` 函数中设置退出代码，以指示执行期间发生错误。

```dart
Future<void> _handleError(String path) async {
  if (await FileSystemEntity.isDirectory(path)) {
    stderr.writeln('error: $path is a directory');
  } else {
    exitCode = 2;
  }
}
```

An exit code of `2` indicates that the app encountered an error.
退出代码 `2` 表示应用程序遇到错误。

An alternative to using `exitCode` is to use the top-level `exit()` function, which sets the exit code and exits the app immediately. For example, the `_handleError()` function could call `exit(2)` instead of setting `exitCode` to 2, but `exit()` would quit the program and it might not process all of the files specified by the running command.
使用 `exitCode` 的替代方法是使用顶级 `exit()` 函数，该函数设置退出代码并立即退出应用程序。例如， `_handleError()` 函数可以调用 `exit(2)` 而不是将 `exitCode` 设置为 2，但 `exit()` 会退出程序，并且它可能不会处理正在运行的命令指定的所有文件。

*info* Generally speaking, you’re better off using the `exitCode` property, which sets the exit code but allows the program to continue through to its natural completion.
一般来说，最好使用 `exitCode` 属性，该属性设置退出代码，但允许程序继续执行到其自然完成。

Although you can use any number for an exit code, by convention, the codes in the table below have the following meanings:
虽然您可以对退出代码使用任意数字，但根据惯例，下表中的代码具有以下含义：

| Code 代码 | Meaning 含义  |
| --------- | ------------- |
| 0         | Success 成功  |
| 1         | Warnings 警告 |
| 2         | Errors 错误   |

## Summary 摘要

This tutorial described some basic APIs found in the following classes from the `dart:io` library:
本教程介绍了在 `dart:io` 库的以下类中找到的一些基本 API：

| API                                                          | Description 说明                                             |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| [`IOSink`](https://api.dart.dev/stable/dart-io/IOSink-class.html) | Helper class for objects that consume data from streams 用于从流中使用数据的对象的帮助程序类 |
| [`File`](https://api.dart.dev/stable/dart-io/File-class.html) | Represents a file on the native file system 表示本机文件系统上的文件 |
| [`Directory`](https://api.dart.dev/stable/dart-io/Directory-class.html) | Represents a directory on the native file system 表示本机文件系统上的目录 |
| [`FileSystemEntity`](https://api.dart.dev/stable/dart-io/FileSystemEntity-class.html) | Superclass for File and Directory 文件和目录的超类           |
| [`Platform`](https://api.dart.dev/stable/dart-io/Platform-class.html) | Provides information about the machine and operating system 提供有关机器和操作系统的的信息 |
| [`stdout`](https://api.dart.dev/stable/dart-io/stdout.html)  | The standard output stream 标准输出流                        |
| [`stderr`](https://api.dart.dev/stable/dart-io/stderr.html)  | The standard error stream 标准错误流                         |
| [`stdin`](https://api.dart.dev/stable/dart-io/stdin.html)    | The standard input stream 标准输入流                         |
| [`exitCode`](https://api.dart.dev/stable/dart-io/exitCode.html) | Access and set the exit code 访问和设置退出代码              |
| [`exit()`](https://api.dart.dev/stable/dart-io/exit.html)    | Sets the exit code and quits 设置退出代码并退出              |

In addition, this tutorial covered two classes from `package:args` that help with parsing and using command-line arguments: [`ArgParser`](https://pub.dev/documentation/args/latest/args/ArgParser-class.html) and [`ArgResults`](https://pub.dev/documentation/args/latest/args/ArgResults-class.html).
此外，本教程还介绍了 `package:args` 中的两个类，它们有助于解析和使用命令行参数： `ArgParser` 和 `ArgResults` 。

For more classes, functions, and properties, consult the API docs for [`dart:io`](https://api.dart.dev/stable/dart-io/dart-io-library.html), [`dart:convert`](https://api.dart.dev/stable/dart-convert/dart-convert-library.html), and [`package:args`](https://pub.dev/documentation/args/latest/args/args-library.html).
有关更多类、函数和属性，请参阅 `dart:io` 、 `dart:convert` 和 `package:args` 的 API 文档。

For another example of a command line app, check out the [`command_line`](https://github.com/dart-lang/samples/tree/main/command_line) sample.
有关命令行应用程序的另一个示例，请查看 `command_line` 示例。

## What next? 接下来做什么？

If you’re interested in server-side programming, check out the [next tutorial](https://dart.dev/tutorials/server/httpserver).
如果您对服务器端编程感兴趣，请查看下一个教程。
