+++
title = "库和包"
date = 2024-01-05T20:29:36+08:00
weight = 30
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文: [https://dart.dev/web/libraries](https://dart.dev/web/libraries)

## Web libraries and packages 网络库和软件包

The [Dart SDK](https://dart.dev/tools/sdk) contains [dart:html](https://api.dart.dev/stable/dart-html/dart-html-library.html) and other libraries that provide low-level web APIs. You can supplement or replace these APIs using web packages.
Dart SDK 包含 dart:html 和其他提供低级网络 API 的库。您可以使用网络软件包补充或替换这些 API。

## SDK libraries SDK 库

The Dart SDK contains dart:html and other libraries that provide low-level web APIs.
Dart SDK 包含 dart:html 和其他提供低级网络 API 的库。

- [Build a web app with Dart 使用 Dart 构建网络应用](https://dart.dev/web/get-started)

  A quick overview of how to build, run, and debug a web app with Dart. 有关如何使用 Dart 构建、运行和调试网络应用的快速概述。

- [The dart:html documentation dart:html 文档](https://dart.dev/libraries/dart-html)

  An example-driven tour of using the dart:html library. Topics include manipulating the DOM programmatically, making HTTP requests, and using WebSockets. 使用 dart:html 库的示例驱动之旅。主题包括以编程方式操作 DOM、发出 HTTP 请求和使用 WebSocket。

- [dart:html API reference dart:html API 参考](https://api.dart.dev/stable/dart-html/dart-html-library.html)

  Complete reference documentation for the dart:html library. dart:html 库的完整参考文档。

## Web packages Web 软件包

Many [packages](https://dart.dev/guides/packages) support web development with Dart. In particular, the [Flutter framework](https://flutter.dev/) has [web support](https://flutter.dev/web), in addition to mobile, desktop, and embedded device support.
许多软件包支持使用 Dart 进行网络开发。特别是，除了移动、桌面和嵌入式设备支持外，Flutter 框架还具有网络支持。

To find more libraries that support writing web apps, search pub.dev for [web packages](https://pub.dev/web).
要查找支持编写网络应用的其他库，请在 pub.dev 中搜索网络软件包。

Your Dart code can also interact with existing JavaScript or TypeScript libraries using Dart’s [JavaScript interoperability](https://dart.dev/interop/js-interop) support.
您的 Dart 代码还可以使用 Dart 的 JavaScript 互操作性支持与现有的 JavaScript 或 TypeScript 库进行交互。
