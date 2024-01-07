+++
title = "命令行和服务器应用：概览"
date = 2024-01-05T20:29:36+08:00
weight = 1
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文: [https://dart.dev/server](https://dart.dev/server)

## Command-line and server apps 命令行和服务器应用

This page points to tools and documentation that can help you develop command-line and server apps.

​	此页面指向可帮助您开发命令行和服务器应用的工具和文档。

[Get started](https://dart.dev/tutorials/server/get-started)

## Tools 工具

- [DartPad](https://dart.dev/tools/dartpad)

  Handy for both beginners and experts, DartPad lets you try out language features and dart:* APIs. 

  DartPad 既适合初学者，也适合专家，它允许您试用语言特性和 dart:* API。

  *info* **Note:** DartPad does **not** support using VM libraries, such as `dart:io`, or importing libraries from packages besides the [currently supported packages](https://github.com/dart-lang/dart-pad/wiki/Package-and-plugin-support#currently-supported-packages). 

  注意：DartPad 不支持使用 VM 库，例如 `dart:io` ，或从除当前支持的包之外的包中导入库。

- [Dart SDK](https://dart.dev/tools/sdk)

  [Install the Dart SDK](https://dart.dev/get-dart) to get the core Dart libraries and [tools](https://dart.dev/tools). 
  
  安装 Dart SDK 以获取核心 Dart 库和工具。

## 框架 Frameworks 

Server-side frameworks written in Dart include:

​	用 Dart 编写的服务器端框架包括：

- [Serverpod](https://serverpod.dev/)

  A scalable app server that supports code generation, authentication, real-time communication, databases, and caching. 

  一个可扩展的应用程序服务器，支持代码生成、身份验证、实时通信、数据库和缓存。

- [Dart Frog](https://dartfrog.vgv.dev/)

  A fast, minimalistic backend framework for Dart. 

  一个快速、极简的 Dart 后端框架。

- More tools 更多工具

  The [Tools](https://dart.dev/tools) page links to generally useful tools, such as Dart plugins for your favorite IDE or editor. 
  
  工具页面链接到通常有用的工具，例如您最喜欢的 IDE 或编辑器的 Dart 插件。

For additional options, see [#server packages on pub.dev](https://pub.dev/packages?q=topic%3Aserver).

​	有关其他选项，请参阅 pub.dev 上的 #server 包。

## 教程 Tutorials 

You might find the following tutorials helpful.

​	您可能会发现以下教程很有用。

- [Get started 开始](https://dart.dev/tutorials/server/get-started)

  Learn how to use the Dart SDK to develop command-line and server apps. 

  了解如何使用 Dart SDK 开发命令行和服务器应用。

- [gRPC Quickstart gRPC 快速入门](https://grpc.io/docs/languages/dart/quickstart/)

  Walks you through running and modifying a client-server example that uses the gRPC framework. 

  引导您运行和修改一个使用 gRPC 框架的客户端-服务器示例。

- [Write command-line apps 编写命令行应用程序](https://dart.dev/tutorials/server/cmdline)

  Introduces dart:io and the args package. 

  介绍 dart:io 和 args 包。

- [Write HTTP servers 编写 HTTP 服务器](https://dart.dev/tutorials/server/httpserver)

  Features the shelf package. 
  
  介绍 shelf 包。

## 更多资源 More resources 

- [Dart API](https://api.dart.dev/stable)

  API reference for `dart:*` libraries. 

  `dart:*` 库的 API 参考。

- [dart:io documentation](https://dart.dev/libraries/dart-io)

  Shows how to use the major features of the dart:io library. You can use the dart:io library in command-line scripts, servers, and non-web [Flutter apps.](https://flutter.dev/) 
  
  展示如何使用 dart:io 库的主要功能。您可以在命令行脚本、服务器和非 Web Flutter 应用中使用 dart:io 库。
