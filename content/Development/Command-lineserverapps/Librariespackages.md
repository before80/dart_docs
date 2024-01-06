+++
title = "库和包"
date = 2024-01-05T20:29:36+08:00
weight = 50
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文: [https://dart.dev/server/libraries](https://dart.dev/server/libraries)

## Command-line and server libraries and packages 命令行和服务器库和包

The [Dart SDK](https://dart.dev/tools/sdk) contains [dart:io](https://api.dart.dev/stable/dart-io/dart-io-library.html) and other libraries that provide low-level command-line & server APIs.
Dart SDK 包含 dart:io 和其他提供低级命令行和服务器 API 的库。

## SDK libraries SDK 库

The Dart SDK contains dart:io and other libraries that provide low-level web APIs.
Dart SDK 包含 dart:io 和其他提供低级 Web API 的库。

- [The dart:io documentation dart:io 文档](https://dart.dev/libraries/dart-io)

  An example-driven tour of using the dart:io library. Topics include working with files & directories, and making & handling HTTP requests. 使用 dart:io 库的示例驱动之旅。主题包括使用文件和目录，以及发出和处理 HTTP 请求。

- [dart:io API reference dart:io API 参考](https://api.dart.dev/stable/dart-io/dart-io-library.html)

  Complete reference documentation for the dart:io library. dart:io 库的完整参考文档。

## Community packages 社区包

The [pub.dev site](https://pub.dev/) allows you to search for packages that support command-line and server apps by specifying the platforms your app needs to support. You can also search for words that describe the functionality you need.
pub.dev 网站允许您通过指定您的应用需要支持的平台来搜索支持命令行和服务器应用的包。您还可以搜索描述您所需功能的字词。

### Command-line packages 命令行包

Command-line apps often use the following packages, in addition to [general-purpose packages](https://dart.dev/guides/libraries/useful-libraries#general-purpose-packages) such as `archive`, `intl`, and `yaml`:
除了 `archive` 、 `intl` 和 `yaml` 等通用包之外，命令行应用通常使用以下包：

| **Package 包**                                    | **Description 说明**                                         |
| ------------------------------------------------- | ------------------------------------------------------------ |
| [args](https://pub.dev/packages/args)             | Parses raw command-line arguments into a set of options and values. 将原始命令行参数解析为一组选项和值。 |
| [cli_util](https://pub.dev/packages/cli_util)     | Provides utilities for building command-line apps. 提供用于构建命令行应用的实用工具。 |
| [completion](https://pub.dev/packages/completion) | Adds command-line completion to apps that use the `args` package. 为使用 `args` 包的应用添加命令行完成功能。 |
| [path](https://pub.dev/packages/path)             | Provides comprehensive, cross-platform operations for manipulating paths. 提供全面的跨平台操作来处理路径。 |
| [usage](https://pub.dev/packages/usage)           | Wraps Google Analytics. 封装 Google Analytics。              |

### Server packages 服务器包

Server apps can choose from many packages, in addition to the packages listed in the previous table and [general-purpose packages](https://dart.dev/guides/libraries/useful-libraries#general-purpose-packages) such as `logging`:
除了前表中列出的包和 `logging` 等通用包之外，服务器应用还可以选择许多包：

| **Package 包**                                  | **Description 说明**                                         |
| ----------------------------------------------- | ------------------------------------------------------------ |
| [crypto](https://pub.dev/packages/crypto)       | Implements cryptographic hashing functions for algorithms such as SHA-1, SHA-256, MD5, and HMAC. 实现 SHA-1、SHA-256、MD5 和 HMAC 等算法的加密哈希函数。 |
| [grpc](https://pub.dev/packages/grpc)           | Implements [gRPC](https://grpc.io/), a high performance, open source, general RPC framework that puts mobile and HTTP/2 first. 实现 gRPC，一个高性能、开源、通用的 RPC 框架，它优先考虑移动设备和 HTTP/2。 |
| [shelf](https://pub.dev/packages/shelf)         | Provides a model for web server middleware that encourages composition and easy reuse. 提供了一个 Web 服务器中间件模型，鼓励组合和轻松重用。 |
| [dart_frog](https://pub.dev/packages/dart_frog) | A fast, minimalistic backend framework for Dart built on top of Shelf. 一个基于 Shelf 构建的快速、极简的后端框架。 |
| [serverpod](https://pub.dev/packages/serverpod) | A scalable app server that supports code generation, authentication, real-time communication, databases, and caching. 一个可扩展的应用程序服务器，支持代码生成、身份验证、实时通信、数据库和缓存。 |
