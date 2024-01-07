+++
title = "使用 Google Cloud"
date = 2024-01-05T20:29:36+08:00
weight = 60
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文: [https://dart.dev/server/google-cloud](https://dart.dev/server/google-cloud)

## Using Google Cloud 使用 Google Cloud

Dart servers can use many [Google Cloud products](https://cloud.google.com/products), often with the help of the pre-packaged Docker [Official Images for Dart](https://hub.docker.com/_/dart). For information about creating HTTP servers with Dart, see the [Write HTTP servers page](https://dart.dev/tutorials/server/httpserver).

​	Dart 服务器可以使用许多 Google Cloud 产品，通常借助预先打包的 Docker 官方 Dart 镜像。有关使用 Dart 创建 HTTP 服务器的信息，请参阅编写 HTTP 服务器页面。

For information about other Google APIs (including Firebase) that you might want to use from Dart code, see the [Google APIs page](https://dart.dev/guides/google-apis).

​	有关您可能希望从 Dart 代码中使用的其他 Google API（包括 Firebase）的信息，请参阅 Google API 页面。

## 推荐的解决方案 Recommended solutions 

To run Dart in the Cloud, we recommend using serverless computing solutions.

​	要在云端运行 Dart，我们建议使用无服务器计算解决方案。

### Cloud Run

You can use Cloud Run’s flexible container support, combined with Dart’s Docker images, to run server-side Dart code. Creating scalable, high performance APIs and event-driven apps are good use cases for Cloud Run’s serverless platform, which frees developers from managing infrastructure.

​	您可以结合使用 Cloud Run 的灵活容器支持和 Dart 的 Docker 镜像，来运行服务器端 Dart 代码。创建可扩展的高性能 API 和事件驱动型应用是 Cloud Run 无服务器平台的良好用例，它使开发人员无需管理基础架构。

Examples of Dart servers implemented to run on Cloud Run are [in the dart-lang/samples/repo](https://github.com/dart-lang/samples/tree/main/server).

​	在 dart-lang/samples/repo 中提供了在 Cloud Run 上运行的 Dart 服务器示例。

For more information about using Cloud Run, see the documentation for [building and deploying a service in other languages](https://cloud.google.com/run/docs/quickstarts/build-and-deploy/other).

​	如需详细了解如何使用 Cloud Run，请参阅使用其他语言构建和部署服务的文档。

### Dart 的 Functions Framework - Functions Framework for Dart 

The Functions Framework is a FaaS (Function as a Service) framework that makes it easy to write Dart functions instead of server applications for handling web requests. Using the framework, you can create functions that handle HTTP requests and [CloudEvents](https://cloudevents.io/) and deploy them to Google Cloud.

​	Functions Framework 是一个 FaaS（Function as a Service）框架，它可以轻松编写 Dart 函数，而不是编写服务器应用程序来处理网络请求。使用该框架，您可以创建处理 HTTP 请求和 CloudEvent 的函数，并将它们部署到 Google Cloud。

The [Dart Functions Framework](https://pub.dev/packages/functions_framework) is a community-supported project.

​	Dart Functions Framework 是一个社区支持的项目。

For more information, see [the README](https://github.com/GoogleCloudPlatform/functions-framework-dart/blob/main/docs/README.md).

​	如需详细了解，请参阅自述文件。

## 其他解决方案 Other solutions 

Depending on your needs, you may also want to consider running Dart on the following Google Cloud compute platforms.

​	根据您的需求，您可能还需要考虑在以下 Google Cloud 计算平台上运行 Dart。

### Compute Engine

To run Dart code on Compute Engine, use Compute Engine’s support for running containers, combined with Dart’s Docker images.

​	要在 Compute Engine 上运行 Dart 代码，请使用 Compute Engine 对运行容器的支持，结合 Dart 的 Docker 镜像。

For more information, see the Compute Engine documentation for [using software containers](https://cloud.google.com/compute/docs/containers).

​	有关更多信息，请参阅 Compute Engine 文档，了解如何使用软件容器。

### Kubernetes

To run Dart on clusters of Compute Engine instances, use Google Kubernetes Engine (GKE).

​	要在 Compute Engine 实例集群上运行 Dart，请使用 Google Kubernetes Engine (GKE)。

For more information, see the [GKE overview](https://cloud.google.com/kubernetes-engine/docs/concepts/kubernetes-engine-overview).

​	有关更多信息，请参阅 GKE 概览。

### App Engine

[App Engine](https://cloud.google.com/appengine) support for Dart is incomplete and requires the [App Engine flexible environment](https://cloud.google.com/appengine/docs/flexible), which does not [autoscale to zero instances](https://cloud.google.com/run/docs/about-instance-autoscaling), so we recommend **Cloud Run** for new server-side Dart code. If you *want* to use App Engine, consider using the [`appengine` package](https://pub.dev/packages/appengine).

​	App Engine 对 Dart 的支持不完整，并且需要 App Engine 灵活环境，该环境不会自动缩减到零个实例，因此我们建议将 Cloud Run 用于新的服务器端 Dart 代码。如果您想使用 App Engine，请考虑使用 `appengine` 软件包。
