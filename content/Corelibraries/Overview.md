+++
title = "Dart 的核心库：概览"
date = 2024-01-05T20:29:36+08:00
weight = 1
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文: [https://dart.dev/libraries](https://dart.dev/libraries)

## Dart's core libraries Dart 的核心库

Dart has a rich set of core libraries that provide essentials for many everyday programming tasks such as working on collections of objects (`dart:collection`), making calculations (`dart:math`), and encoding/decoding data (`dart:convert`). Additional APIs are available in [commonly used packages](https://dart.dev/guides/libraries/useful-libraries).

​	Dart 拥有一套丰富的核心库，可为许多日常编程任务提供基本功能，例如处理对象集合 ( `dart:collection` )、进行计算 ( `dart:math` ) 和对数据进行编码/解码 ( `dart:convert` )。其他 API 可在常用软件包中找到。

## 库之旅 Library tour 

The following guides cover how to use major features of Dart’s core libraries. They provide just an overview, and are by no means comprehensive. Whenever you need more details about a library or its members, consult the [Dart API reference.](https://api.dart.dev/stable)

​	以下指南介绍如何使用 Dart 核心库的主要功能。它们仅提供概述，绝不是全面的。每当您需要有关库或其成员的更多详细信息时，请参阅 Dart API 参考。

- [dart:core](https://dart.dev/libraries/dart-core)

  Built-in types, collections, and other core functionality. This library is automatically imported into every Dart program. 

  内置类型、集合和其他核心功能。此库自动导入到每个 Dart 程序中。

- [dart:async](https://dart.dev/libraries/dart-async)

  Support for asynchronous programming, with classes such as Future and Stream. 

  对异步编程的支持，包括 Future 和 Stream 等类。

- [dart:math](https://dart.dev/libraries/dart-math)

  Mathematical constants and functions, plus a random number generator. 

  数学常量和函数，以及随机数生成器。

- [dart:convert](https://dart.dev/libraries/dart-convert)

  Encoders and decoders for converting between different data representations, including JSON and UTF-8. 

  用于在不同数据表示之间进行转换的编码器和解码器，包括 JSON 和 UTF-8。

- [dart:io](https://dart.dev/libraries/dart-io)

  I/O for programs that can use the Dart VM, including Flutter apps, servers, and command-line scripts. 

  适用于可以使用 Dart VM 的程序的 I/O，包括 Flutter 应用、服务器和命令行脚本。

- [dart:html](https://dart.dev/libraries/dart-html)

  DOM and other APIs for browser-based apps. 
  
  DOM 和其他适用于基于浏览器的应用的 API。

As mentioned, these pages are just an overview; they cover only a few dart:* libraries and no third-party libraries.

​	如上所述，这些页面只是概述；它们仅介绍了少数 dart:* 库，而没有介绍任何第三方库。

For an overview of all libraries that Dart supports on different platforms, check out the [Multi-platform libraries](https://dart.dev/libraries#multi-platform-libraries), [Native platform libraries](https://dart.dev/libraries#native-platform-libraries), and [Web platform libraries](https://dart.dev/libraries#web-platform-libraries) lists below.

​	如需了解 Dart 在不同平台上支持的所有库的概述，请查看下面的多平台库、原生平台库和 Web 平台库列表。

Other places to find library information are the [pub.dev site](https://pub.dev/) and the [Dart web developer library guide](https://dart.dev/web/libraries). You can find API documentation for all dart:* libraries in the [Dart API reference](https://api.dart.dev/stable) or, if you’re using Flutter, the [Flutter API reference](https://api.flutter.dev/).

​	查找库信息的另一个地方是 pub.dev 网站和 Dart Web 开发人员库指南。您可以在 Dart API 参考中找到所有 dart:* 库的 API 文档，或者如果您使用的是 Flutter，则可以在 Flutter API 参考中找到。

To learn more about the Dart language, check out the [language documentation and samples](https://dart.dev/language).

​	如需详细了解 Dart 语言，请查看语言文档和示例。

## 多平台库 Multi-platform libraries 

The following table lists the Dart core libraries that work on all [Dart platforms](https://dart.dev/overview#platform).

​	下表列出了适用于所有 Dart 平台的 Dart 核心库。

| Library 库                                                   | Notes 注释                                                   |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| [`dart:core`](https://api.dart.dev/stable/dart-core/dart-core-library.html) Built-in types, collections, and other core functionality for every Dart program. 每个 Dart 程序的内置类型、集合和其他核心功能。 |                                                              |
| [`dart:async`](https://api.dart.dev/stable/dart-async/dart-async-library.html), [`package:async`](https://pub.dev/packages/async) Support for asynchronous programming, with classes such as `Future` and `Stream`. 支持异步编程，包含 `Future` 和 `Stream` 等类。 `package:async` provides additional utilities around the `Future` and `Stream` types. `package:async` 提供了围绕 `Future` 和 `Stream` 类型的其他实用工具。 |                                                              |
| [`dart:collection`](https://api.dart.dev/stable/dart-collection/dart-collection-library.html), [`package:collection`](https://pub.dev/packages/collection) Classes and utilities that supplement the collection support in `dart:core`. 补充 `dart:core` 中集合支持的类和实用工具。 `package:collection` provides further collection implementations and functions for working on and with collections. `package:collection` 提供了进一步的集合实现和用于处理集合及其本身的功能。 |                                                              |
| [`dart:convert`](https://api.dart.dev/stable/dart-convert/dart-convert-library.html), [`package:convert`](https://pub.dev/packages/convert) Encoders and decoders for converting between different data representations, including JSON and UTF-8. 用于在不同数据表示之间进行转换的编码器和解码器，包括 JSON 和 UTF-8。 `package:convert` provides additional encoders and decoders. `package:convert` 提供了其他编码器和解码器。 |                                                              |
| [`dart:developer`](https://api.dart.dev/stable/dart-developer/dart-developer-library.html) Interaction with developer tools such as the debugger and inspector. 与调试器和检查器等开发者工具进行交互。 | [Native JIT](https://dart.dev/overview#native-platform) and the [development JavaScript compiler](https://dart.dev/tools/webdev#serve) only 原生 JIT 和仅开发 JavaScript 编译器 |
| [`dart:math`](https://api.dart.dev/stable/dart-math/dart-math-library.html) Mathematical constants and functions, plus a random number generator. 数学常量和函数，以及随机数生成器。 |                                                              |
| [`dart:typed_data`](https://api.dart.dev/stable/dart-typed_data/dart-typed_data-library.html), [`package:typed_data`](https://pub.dev/packages/typed_data) Lists that efficiently handle fixed sized data (for example, unsigned 8-byte integers) and SIMD numeric types. 有效处理固定大小数据（例如，无符号 8 字节整数）和 SIMD 数值类型的列表。 `package:typed_data` provides further classes and functions working on typed data. `package:typed_data` 提供了更多用于处理类型化数据的类和函数。 |                                                              |

## 原生平台库 Native platform libraries 

The following table lists the Dart core libraries that work on the [Dart native platform](https://dart.dev/overview#native-platform) (AOT- and JIT-compiled code).

​	下表列出了在 Dart 原生平台（AOT 和 JIT 编译代码）上运行的 Dart 核心库。

| Library 库                                                   | Notes 备注                                                   |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| [`dart:ffi`](https://api.dart.dev/stable/dart-ffi/dart-ffi-library.html), [`package:ffi`](https://pub.dev/packages/ffi) A foreign function interface that lets Dart code use native C APIs. 允许 Dart 代码使用原生 C API 的外部函数接口。 `package:ffi` contains utilities incl. support for converting Dart strings and C strings. `package:ffi` 包含实用程序，包括支持转换 Dart 字符串和 C 字符串。 |                                                              |
| [`dart:io`](https://api.dart.dev/stable/dart-io/dart-io-library.html), [`package:io`](https://pub.dev/packages/io) File, socket, HTTP, and other I/O support for non-web applications. 文件、套接字、HTTP 和其他非 Web 应用程序的 I/O 支持。 `package:io` provides functionality including support for ANSI colors, file copying, and standard exit codes. `package:io` 提供的功能包括对 ANSI 颜色、文件复制和标准退出代码的支持。 |                                                              |
| [`dart:isolate`](https://api.dart.dev/stable/dart-isolate/dart-isolate-library.html) Concurrent programming using isolates: independent workers similar to threads. 使用隔离进行并发编程：类似于线程的独立工作进程。 |                                                              |
| [`dart:mirrors`](https://api.dart.dev/stable/dart-mirrors/dart-mirrors-library.html) Basic reflection with support for introspection and dynamic invocation. 基本反射，支持内省和动态调用。 | Experimental 实验性 [Native JIT](https://dart.dev/overview#native-platform) only (*not* Flutter) 仅限原生 JIT（非 Flutter） |

## Web 平台库 Web platform libraries 

The following table lists the Dart core libraries that work on the [Dart web platform](https://dart.dev/overview#web-platform) (code compiled to JavaScript).

​	下表列出了可在 Dart Web 平台（已编译为 JavaScript 的代码）上运行的 Dart 核心库。

| Library 库                                                   | Notes 备注 |
| ------------------------------------------------------------ | ---------- |
| [`dart:html`](https://api.dart.dev/stable/dart-html/dart-html-library.html) HTML elements and other resources for web-based applications. HTML 元素和其他基于网络的应用程序资源。 |            |
| [`dart:indexed_db`](https://api.dart.dev/stable/dart-indexed_db/dart-indexed_db-library.html) Client-side key-value store with support for indexes. 支持索引的客户端键值存储。 |            |
| ~~[`dart:js`](https://api.dart.dev/stable/dart-js/dart-js-library.html)~~, [`dart:js_util`](https://api.dart.dev/stable/dart-js_util/dart-js_util-library.html), [`package:js`](https://pub.dev/packages/js) `dart:js_util` provides low-level primitives for interoperability; typically the higher-level annotations in `package:js` are recommended, as they help express interoperability more succinctly. For more details see [JavaScript interoperability](https://dart.dev/web/js-interop). `dart:js_util` 提供了用于互操作性的低级基元；通常建议使用 `package:js` 中更高级别的注释，因为它们有助于更简洁地表达互操作性。有关更多详细信息，请参阅 JavaScript 互操作性。 *Don’t use `dart:js` directly; direct use of those legacy APIs is deprecated*. 不要直接使用 `dart:js` ；不建议直接使用这些旧版 API。 |            |
| [`dart:svg`](https://api.dart.dev/stable/dart-svg/dart-svg-library.html) Scalable Vector Graphics. 可缩放矢量图形。 |            |
| [`dart:web_audio`](https://api.dart.dev/stable/dart-web_audio/dart-web_audio-library.html) High-fidelity audio programming in the browser. 浏览器中的高保真音频编程。 |            |
| [`dart:web_gl`](https://api.dart.dev/stable/dart-web_gl/dart-web_gl-library.html) 3D programming in the browser. 浏览器中的 3D 编程。 |            |
