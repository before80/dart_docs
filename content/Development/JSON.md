+++
title = "JSON"
date = 2024-01-05T20:29:36+08:00
weight = 10
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文: [https://dart.dev/guides/json](https://dart.dev/guides/json)

## Using JSON 使用 JSON

Most mobile and web apps use JSON for tasks such as exchanging data with a web server. This page discusses Dart support for JSON *serialization* and *deserialization*: converting Dart objects to and from JSON.
大多数移动应用和网页应用使用 JSON 来执行任务，例如与 Web 服务器交换数据。此页面讨论了 Dart 对 JSON 序列化和反序列化的支持：将 Dart 对象转换为 JSON 和从 JSON 转换回 Dart 对象。

## Libraries 库

The following libraries and packages are useful across Dart platforms:
以下库和软件包在所有 Dart 平台上都很有用：

- [dart:convert](https://dart.dev/libraries/dart-convert)
  Converters for both JSON and UTF-8 (the character encoding that JSON requires).
  JSON 和 UTF-8（JSON 所需的字符编码）的转换器。
- [package:json_serializable](https://pub.dev/packages/json_serializable)
  An easy-to-use code generation package. When you add some metadata annotations and use the builder provided by this package, the Dart build system generates serialization and deserialization code for you.
  一个易于使用的代码生成软件包。当您添加一些元数据注释并使用此软件包提供的构建器时，Dart 构建系统会为您生成序列化和反序列化代码。
- [package:built_value](https://pub.dev/packages/built_value)
  A powerful, opinionated alternative to json_serializable.
  json_serializable 的一个强大、固执的替代品。

## Flutter resources Flutter 资源

- [JSON and serialization JSON 和序列化](https://docs.flutter.dev/development/data-and-backend/json)

  Shows how Flutter apps can serialize and deserialize both with dart:convert and with json_serializable. 展示了 Flutter 应用如何使用 dart:convert 和 json_serializable 序列化和反序列化。

## Web app resources 网络应用资源

- [Fetch data from the internet 从互联网获取数据](https://dart.dev/tutorials/server/fetch-data)

  Demonstrates how to use `package:http` to retrieve data with a web server. 演示如何使用 `package:http` 从网络服务器检索数据。
