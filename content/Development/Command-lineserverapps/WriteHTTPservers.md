+++
title = "编写 HTTP 服务器"
date = 2024-01-05T20:29:36+08:00
weight = 40
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文: [https://dart.dev/tutorials/server/httpserver](https://dart.dev/tutorials/server/httpserver)

## Write HTTP servers 编写 HTTP 服务器

Here are some resources for writing servers using Dart:

​	以下是使用 Dart 编写服务器的一些资源：

- Documentation

- 
  文档
  
  - [Using Google Cloud](https://dart.dev/server/google-cloud) has information on Google Cloud products that Dart servers can use, such as Cloud Run.
  - 使用 Google Cloud 提供有关 Google Cloud 产品的信息，Dart 服务器可以使用这些产品，例如 Cloud Run。
  - [Using Google APIs](https://dart.dev/guides/google-apis) points to resources to help you use Firebase and Google client APIs from a Dart app.
  - 使用 Google API 指向资源，帮助您从 Dart 应用中使用 Firebase 和 Google 客户端 API。
  
- Samples

- 
  示例
  
  - A simple Dart HTTP server
  - 一个简单的 Dart HTTP 服务器
    - Uses the [`shelf`](https://pub.dev/packages/shelf) package.
    - 使用 `shelf` 包。
    - Also uses the [`shelf_router`](https://pub.dev/packages/shelf_router) and [`shelf_static`](https://pub.dev/packages/shelf_static) packages.
    - 还使用 `shelf_router` 和 `shelf_static` 包。
    - Is deployable on Cloud Run.
    - 可部署在 Cloud Run 上。
  - A Dart HTTP server that uses Cloud Firestore
  - 使用 Cloud Firestore 的 Dart HTTP 服务器
    - Uses the Cloud Firestore features in the [`googleapis`](https://pub.dev/packages/googleapis) package.
    - 在 `googleapis` 包中使用 Cloud Firestore 功能。
    - Also uses the [`googleapis_auth`](https://pub.dev/packages/googleapis_auth), [`shelf`](https://pub.dev/packages/shelf), and [`shelf_router`](https://pub.dev/packages/shelf_router) packages.
    - 还使用 `googleapis_auth` 、 `shelf` 和 `shelf_router` 包。
    - Is deployable on Cloud Run.
    - 可部署在 Cloud Run 上。
