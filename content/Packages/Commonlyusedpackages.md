+++
title = "常用软件包"
date = 2024-01-05T20:29:36+08:00
weight = 10
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文: [https://dart.dev/guides/libraries/useful-libraries](https://dart.dev/guides/libraries/useful-libraries)

## Commonly used packages 常用软件包

This page lists some of the most popular and useful [packages](https://dart.dev/guides/packages) that Dart developers have published. To find more packages—and search [core libraries](https://dart.dev/libraries) as well—use the [pub.dev site.](https://pub.dev/)
此页面列出了 Dart 开发人员发布的一些最受欢迎且最有用的包。要查找更多包（以及搜索核心库），请使用 pub.dev 网站。

Commonly used packages fall into three groups:
常用包分为三组：

- [General-purpose packages
  通用包](https://dart.dev/guides/libraries/useful-libraries#general-purpose-packages)
- [Packages that expand on Dart core libraries
  扩展 Dart 核心库的包](https://dart.dev/guides/libraries/useful-libraries#packages-that-correspond-to-sdk-libraries)
- [Specialized packages
  专业包](https://dart.dev/guides/libraries/useful-libraries#specialized-packages)

## General-purpose packages 通用包

The following packages are useful for a wide range of projects.
以下包对各种项目都很有用。

| **Package 包**                                               | **Description 说明**                                         | **Commonly used APIs 常用 API**                              |
| ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| [archive](https://pub.dev/packages/archive)                  | Encodes and decodes various archive and compression formats. 对各种存档和压缩格式进行编码和解码。 | Archive, ArchiveFile, TarEncoder, TarDecoder, ZipEncoder, ZipDecoder Archive、ArchiveFile、TarEncoder、TarDecoder、ZipEncoder、ZipDecoder |
| [characters](https://pub.dev/packages/characters)            | String manipulation for user-perceived characters (Unicode grapheme clusters). 针对用户感知的字符（Unicode 字符集簇）进行字符串操作。 | String.characters, Characters, CharacterRange String.characters、Characters、CharacterRange |
| [http](https://pub.dev/packages/http)                        | A set of high-level functions and classes that make it easy to consume HTTP resources. 一组高级函数和类，便于使用 HTTP 资源。 | delete(), get(), post(), read() delete()、get()、post()、read() |
| [intl](https://pub.dev/packages/intl)                        | Internationalization and localization facilities, with support for plurals and genders, date and number formatting and parsing, and bidirectional text. 国际化和本地化工具，支持复数和性别、日期和数字格式化和解析以及双向文本。 | Bidi, DateFormat, MicroMoney, TextDirection Bidi、DateFormat、MicroMoney、TextDirection |
| [json_serializable](https://pub.dev/packages/json_serializable) | An easy-to-use code generation package. For more information, see [JSON Support](https://dart.dev/guides/json). 一个易于使用的代码生成包。有关更多信息，请参阅 JSON 支持。 | @JsonSerializable                                            |
| [logging](https://pub.dev/packages/logging)                  | A configurable mechanism for adding message logging to your application. 一种可配置的机制，用于向您的应用程序添加消息日志记录。 | LoggerHandler, Level, LogRecord LoggerHandler、Level、LogRecord |
| [mockito](https://pub.dev/packages/mockito)                  | A popular framework for mocking objects in tests. Especially useful if you are writing tests for dependency injection. Used with the [test](https://pub.dev/packages/test) package. 一个流行的用于在测试中模拟对象的框架。如果您正在编写依赖注入测试，它特别有用。与 test 包一起使用。 | Answering, Expectation, Verification 回答、期望、验证        |
| [path](https://pub.dev/packages/path)                        | Common operations for manipulating different types of paths. For more information, see [Unboxing Packages: path.](https://news.dartlang.org/2016/06/unboxing-packages-path.html) 用于操作不同类型路径的常见操作。有关更多信息，请参阅拆箱包：path。 | absolute(), basename(), extension(), join(), normalize(), relative(), split() absolute()、basename()、extension()、join()、normalize()、relative()、split() |
| [quiver](https://pub.dev/packages/quiver)                    | Utilities that make using core Dart libraries more convenient. Some of the libraries where Quiver provides additional support include async, cache, collection, core, iterables, patterns, and testing. 使使用核心 Dart 库更方便的实用程序。Quiver 提供额外支持的一些库包括 async、cache、collection、core、iterables、patterns 和 testing。 | CountdownTimer (quiver.async); MapCache (quiver.cache); MultiMap, TreeSet (quiver.collection); EnumerateIterable (quiver.iterables); center(), compareIgnoreCase(), isWhiteSpace() (quiver.strings) CountdownTimer (quiver.async)；MapCache (quiver.cache)；MultiMap、TreeSet (quiver.collection)；EnumerateIterable (quiver.iterables)；center()、compareIgnoreCase()、isWhiteSpace() (quiver.strings) |
| [shelf](https://pub.dev/packages/shelf)                      | Web server middleware for Dart. Shelf makes it easy to create and compose web servers, and parts of web servers. Dart 的 Web 服务器中间件。Shelf 使得创建和组合 Web 服务器及其部分变得容易。 | Cascade, Pipeline, Request, Response, Server Cascade、Pipeline、Request、Response、Server |
| [stack_trace](https://pub.dev/packages/stack_trace)          | Methods for parsing, inspecting, and manipulating stack traces produced by the underlying Dart implementation. Also provides functions to produce string representations of stack traces in a more readable format than the native StackTrace implementation. For more information, see [Unboxing Packages: stack_trace.](https://news.dartlang.org/2016/01/unboxing-packages-stacktrace.html) 用于解析、检查和操作由底层 Dart 实现生成的堆栈跟踪的方法。还提供函数以更具可读性的格式生成堆栈跟踪的字符串表示形式，而不是本机 StackTrace 实现。有关更多信息，请参阅 Unboxing Packages: stack_trace。 | Trace.current(), Trace.format(), Trace.from() Trace.current()、Trace.format()、Trace.from() |
| [test](https://pub.dev/packages/test)                        | A standard way of writing and running tests in Dart. 在 Dart 中编写和运行测试的标准方法。 | expect(), group(), test() expect()、group()、test()          |
| [yaml](https://pub.dev/packages/yaml)                        | A parser for YAML. YAML 的解析器。                           | loadYaml(), loadYamlStream() loadYaml()、loadYamlStream()    |

## Packages that expand on Dart core libraries 扩展 Dart 核心库的软件包

Each of the following packages builds upon a [core library](https://dart.dev/libraries), adding functionality and filling in missing features:
以下每个软件包都基于核心库构建，添加功能并填补缺失的功能：

| **Package 包**                                    | **Description 说明**                                         | **Commonly used APIs 常用 API**                              |
| ------------------------------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| [async](https://pub.dev/packages/async)           | Expands on dart:async, adding utility classes to work with asynchronous computations. For more information, see [Unboxing Packages: async part 1](https://news.dartlang.org/2016/03/unboxing-packages-async-part-1.html), [part 2](https://news.dartlang.org/2016/03/unboxing-packages-async-part-2.html), and [part 3.](https://news.dartlang.org/2016/04/unboxing-packages-async-part-3.html) 扩展 dart:async，添加实用程序类以处理异步计算。有关更多信息，请参阅 Unboxing Packages: async 第 1 部分、第 2 部分和第 3 部分。 | AsyncMemoizer, CancelableOperation, FutureGroup, LazyStream, Result, StreamCompleter, StreamGroup, StreamSplitter AsyncMemoizer、CancelableOperation、FutureGroup、LazyStream、Result、StreamCompleter、StreamGroup、StreamSplitter |
| [collection](https://pub.dev/packages/collection) | Expands on dart:collection, adding utility functions and classes to make working with collections easier. For more information, see [Unboxing Packages: collection.](https://news.dartlang.org/2016/01/unboxing-packages-collection.html) 扩展 dart:collection，添加实用程序函数和类，以便更轻松地处理集合。有关更多信息，请参阅 Unboxing Packages: collection。 | Equality, CanonicalizedMap, MapKeySet, MapValueSet, PriorityQueue, QueueList Equality、CanonicalizedMap、MapKeySet、MapValueSet、PriorityQueue、QueueList |
| [convert](https://pub.dev/packages/convert)       | Expands on dart:convert, adding encoders and decoders for converting between different data representations. One of the data representations is *percent encoding*, also known as *URL encoding*. 扩展 dart:convert，添加用于在不同数据表示之间进行转换的编码器和解码器。其中一种数据表示是百分号编码，也称为 URL 编码。 | HexDecoder, PercentDecoder HexDecoder、PercentDecoder        |
| [io](https://pub.dev/packages/io)                 | Contains two libraries, ansi and io, to simplify working with files, standard streams, and processes. Use the ansi library to customize terminal output. The io library has APIs for dealing with processes, stdin, and file duplication. 包含两个库，ansi 和 io，以简化对文件、标准流和进程的操作。使用 ansi 库自定义终端输出。io 库具有用于处理进程、stdin 和文件复制的 API。 | copyPath(), isExecutable(), ExitCode, ProcessManager, sharedStdIn copyPath()、isExecutable()、ExitCode、ProcessManager、sharedStdIn |

## Specialized packages 专业包

Here are some tips for finding packages that are more specialized, such as packages for mobile (Flutter) and web development.
以下是一些查找更专业包的提示，例如移动（Flutter）和 Web 开发包。

### Flutter packages Flutter 软件包

See [Using packages](https://docs.flutter.dev/development/packages-and-plugins/using-packages) on the Flutter site. Or use the pub.dev site to [search for Flutter packages.](https://pub.dev/flutter)
请参阅 Flutter 网站上的使用软件包。或使用 pub.dev 网站搜索 Flutter 软件包。

### Web packages Web 软件包

See [Web libraries and packages](https://dart.dev/web/libraries). Or use the pub.dev site to [search for web packages.](https://pub.dev/web)
请参阅 Web 库和软件包。或使用 pub.dev 网站搜索 Web 软件包。

### Command-line and server packages 命令行和服务器软件包

See [Command-line and server libraries and packages](https://dart.dev/server/libraries). Or use the pub.dev site to [search for other packages.](https://pub.dev/)
请参阅命令行和服务器库和软件包。或使用 pub.dev 网站搜索其他软件包。
