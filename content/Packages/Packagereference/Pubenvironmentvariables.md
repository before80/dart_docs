+++
title = "配置 pub 环境变量"
date = 2024-01-05T20:29:36+08:00
weight = 30
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文: [https://dart.dev/tools/pub/environment-variables](https://dart.dev/tools/pub/environment-variables)

## Configuring pub environment variables 配置 pub 环境变量

Environment variables allow you to customize pub to suit your needs.
环境变量允许您自定义 pub 以满足您的需求。

- `PUB_CACHE`

  Some of pub’s dependencies are downloaded to the pub cache. By default, this directory is located under `$HOME/.pub-cache` (on macOS and Linux), or in `%LOCALAPPDATA%\Pub\Cache` (on Windows). (The precise location of the cache may vary depending on the Windows version.) You can use the `PUB_CACHE` environment variable to specify another location. For more information, see [The system package cache](https://dart.dev/tools/pub/cmd/pub-get#the-system-package-cache). pub 的某些依赖项会下载到 pub 缓存。默认情况下，此目录位于 `$HOME/.pub-cache` （在 macOS 和 Linux 上）或 `%LOCALAPPDATA%\Pub\Cache` （在 Windows 上）。（缓存的确切位置可能因 Windows 版本而异。）您可以使用 `PUB_CACHE` 环境变量指定其他位置。有关更多信息，请参阅系统软件包缓存。

- `PUB_HOSTED_URL`

  Pub downloads dependencies from the [pub.dev site.](https://pub.dev/) To specify the location of a particular mirror server, use the `PUB_HOSTED_URL` environment variable. For example: Pub 从 pub.dev 网站下载依赖项。要指定特定镜像服务器的位置，请使用 `PUB_HOSTED_URL` 环境变量。例如：

```
PUB_HOSTED_URL = https://pub.example.com
```

For more information about using a private package repository, see [Overriding the default package repository](https://dart.dev/tools/pub/custom-package-repositories#default-override).
有关使用私有软件包存储库的更多信息，请参阅覆盖默认软件包存储库。

*info* **Note:** If you are attempting to use `pub get` behind a corporate firewall and it fails, please see [`pub get` fails from behind a corporate firewall](https://dart.dev/tools/pub/troubleshoot#pub-get-fails-from-behind-a-corporate-firewall) for information on how to set up the proxy environment variables for your platform.
注意：如果您尝试在公司防火墙后面使用 `pub get` 并失败，请参阅 `pub get` 在公司防火墙后面失败，了解如何为您的平台设置代理环境变量的信息。
