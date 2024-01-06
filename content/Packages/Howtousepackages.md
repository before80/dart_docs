+++
title = "如何使用软件包"
date = 2024-01-05T20:29:36+08:00
weight = 1
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文: [https://dart.dev/guides/packages](https://dart.dev/guides/packages)

## How to use packages 如何使用软件包

The Dart ecosystem uses *packages* to manage shared software such as libraries and tools. To get Dart packages, you use the **pub package manager**. You can find publicly available packages on the [**pub.dev site**,](https://pub.dev/) or you can load packages from the local file system or elsewhere, such as Git repositories. Wherever your packages come from, pub manages version dependencies, helping you get package versions that work with each other and with your SDK version.
Dart 生态系统使用软件包来管理共享软件，例如库和工具。要获取 Dart 软件包，请使用 pub 软件包管理器。您可以在 pub.dev 网站上找到可公开访问的软件包，也可以从本地文件系统或其他位置（例如 Git 存储库）加载软件包。无论您的软件包来自何处，pub 都会管理版本依赖关系，帮助您获取彼此兼容且与您的 SDK 版本兼容的软件包版本。

Most [Dart-savvy IDEs](https://dart.dev/tools#ides-and-editors) offer support for using pub that includes creating, downloading, updating, and publishing packages. Or you can use [`dart pub` on the command line](https://dart.dev/tools/pub/cmd).
大多数精通 Dart 的 IDE 都提供对 pub 的支持，包括创建、下载、更新和发布软件包。或者，您可以在命令行上使用 `dart pub` 。

At a minimum, a Dart package is a directory containing a [pubspec file](https://dart.dev/tools/pub/pubspec). The pubspec contains some metadata about the package. Additionally, a package can contain dependencies (listed in the pubspec), Dart libraries, apps, resources, tests, images, and examples.
Dart 软件包至少是一个包含 pubspec 文件的目录。pubspec 包含有关软件包的一些元数据。此外，软件包可以包含依赖关系（在 pubspec 中列出）、Dart 库、应用、资源、测试、图像和示例。

To use a package, do the following:
要使用软件包，请执行以下操作：

- Create a pubspec (a file named `pubspec.yaml` that lists package dependencies and includes other metadata, such as a version number).
  创建 pubspec（一个名为 `pubspec.yaml` 的文件，其中列出了软件包依赖关系并包含其他元数据，例如版本号）。
- Use [`dart pub get`](https://dart.dev/tools/pub/cmd/pub-get) to retrieve your package’s dependencies.
  使用 `dart pub get` 检索软件包的依赖项。
- If your Dart code depends on a library in the package, import the library.
  如果您的 Dart 代码依赖于软件包中的库，请导入该库。

## Creating a pubspec 创建 pubspec

The pubspec is a file named `pubspec.yaml` that’s in the top directory of your application. The simplest possible pubspec lists only the package name:
pubspec 是一个名为 `pubspec.yaml` 的文件，位于应用程序的顶级目录中。最简单的 pubspec 只列出软件包名称：

```
name: my_app
```

Here is an example of a pubspec that declares dependencies on two packages (`js` and `intl`) that are hosted on the pub.dev site:
以下是一个 pubspec 的示例，它声明对托管在 pub.dev 网站上的两个软件包 ( `js` 和 `intl` ) 的依赖关系：

```
name: my_app

dependencies:
  js: ^0.6.0
  intl: ^0.17.0
```

To update the `pubspec.yaml` file, without manual editing, you can run `dart pub add` command. The following example adds a dependency on `vector_math`.
要更新 `pubspec.yaml` 文件，无需手动编辑，您可以运行 `dart pub add` 命令。以下示例添加了对 `vector_math` 的依赖项。

```
$ dart pub add vector_math
Resolving dependencies... 
+ vector_math 2.1.3
Downloading vector_math 2.1.3...
Changed 1 dependency!
```

For details on creating a pubspec, see the [pubspec documentation](https://dart.dev/tools/pub/pubspec) and the documentation for the packages that you want to use.
有关创建 pubspec 的详细信息，请参阅 pubspec 文档和您想要使用的软件包的文档。

## Getting packages 获取软件包

Once you have a pubspec, you can run [`dart pub get`](https://dart.dev/tools/pub/cmd/pub-get) from the top directory of your application:
拥有 pubspec 后，您可以从应用程序的顶级目录运行 `dart pub get` ：

```
$ cd <path-to-my_app>
$ dart pub get
```

This process is called *getting the dependencies*.
此过程称为获取依赖项。

The `dart pub get` command determines which packages your app depends on, and puts them in a central [system cache](https://dart.dev/tools/pub/glossary#system-cache). If your app depends on a published package, pub downloads that package from the [pub.dev site.](https://pub.dev/) For a [Git dependency](https://dart.dev/tools/pub/dependencies#git-packages), pub clones the Git repository. Transitive dependencies are included, too. For example, if the `js` package depends on the `test` package, `pub` grabs both the `js` package and the `test` package.
命令确定您的应用依赖哪些软件包，并将它们放入中央系统缓存中。如果您的应用依赖已发布的软件包，pub 会从 pub.dev 网站下载该软件包。对于 Git 依赖项，pub 会克隆 Git 存储库。传递依赖项也会包含在内。例如，如果 `js` 软件包依赖于 `test` 软件包， `pub` 会同时获取 `js` 软件包和 `test` 软件包。

Pub creates a `package_config.json` file (under the `.dart_tool/` directory) that maps each package name that your app depends on to the corresponding package in the system cache.
Pub 会创建一个 `package_config.json` 文件（位于 `.dart_tool/` 目录下），将您的应用依赖的每个软件包名称映射到系统缓存中的相应软件包。

## Importing libraries from packages 从软件包导入库

To import libraries found in packages, use the `package:` prefix:
要导入在软件包中找到的库，请使用 `package:` 前缀：

```
import 'package:js/js.dart' as js;
import 'package:intl/intl.dart';
```

The Dart runtime takes everything after `package:` and looks it up within the `package_config.json` file for your app.
Dart 运行时会获取 `package:` 之后的所有内容，并在您应用的 `package_config.json` 文件中查找它。

You can also use this style to import libraries from within your own package. Let’s say that the `transmogrify` package is laid out as follows:
您还可以使用此样式从您自己的软件包中导入库。假设 `transmogrify` 软件包的布局如下：

```nocode
transmogrify/
  lib/
    transmogrify.dart
    parser.dart
  test/
    parser/
      parser_test.dart
```

The `parser_test.dart` file can import `parser.dart` like this:
`parser_test.dart` 文件可以像这样导入 `parser.dart` ：

```
import 'package:transmogrify/parser.dart';
```

## Upgrading a dependency 升级依赖项

The first time you get a new dependency for your package, pub downloads the latest version of it that’s compatible with your other dependencies. It then locks your package to *always* use that version by creating a **lockfile**. This is a file named `pubspec.lock` that pub creates and stores next to your pubspec. It lists the specific versions of each dependency (immediate and transitive) that your package uses.
当您第一次为软件包获取新依赖项时，pub 会下载与其他依赖项兼容的最新版本。然后，它通过创建锁定文件将软件包锁定为始终使用该版本。这是一个名为 `pubspec.lock` 的文件，由 pub 创建并存储在 pubspec 旁边。它列出了软件包使用的每个依赖项（直接和传递）的特定版本。

If your package is an [application package](https://dart.dev/tools/pub/glossary#application-package) you should check this file into [source control](https://dart.dev/guides/libraries/private-files). That way, everyone working on your app uses the same versions of all of its dependencies. Checking in the lockfile also ensures that your deployed app uses the same versions of code.
如果您的软件包是应用程序软件包，则应将此文件检入源代码管理。这样，所有处理您的应用的人员都使用其所有依赖项的相同版本。检入锁定文件还可以确保您的已部署应用使用相同版本的代码。

When you’re ready to upgrade your dependencies to the latest versions, use the [`dart pub upgrade`](https://dart.dev/tools/pub/cmd/pub-upgrade) command:
当您准备好将依赖项升级到最新版本时，请使用 `dart pub upgrade` 命令：

```
$ dart pub upgrade
```

The `dart pub upgrade` command tells pub to regenerate the lockfile, using the newest available versions of your package’s dependencies. If you want to upgrade only one dependency, you can specify the package to upgrade:
`dart pub upgrade` 命令告诉 pub 重新生成锁定文件，使用软件包依赖项的最新可用版本。如果您只想升级一个依赖项，则可以指定要升级的软件包：

```
$ dart pub upgrade transmogrify
```

That command upgrades `transmogrify` to the latest version but leaves everything else the same.
该命令将 `transmogrify` 升级到最新版本，但其他一切保持不变。

The `dart pub upgrade` command can’t always upgrade every package to its latest version, due to conflicting version constraints in the pubspec. To identify out-of-date packages that require editing the pubspec, use [`dart pub outdated`](https://dart.dev/tools/pub/cmd/pub-outdated).
由于 pubspec 中存在冲突的版本约束， `dart pub upgrade` 命令无法始终将每个软件包升级到其最新版本。要识别需要编辑 pubspec 的过时软件包，请使用 `dart pub outdated` 。

## More information 更多信息

The following pages have more information about packages and the pub package manager.
以下页面包含有关软件包和 pub 软件包管理器的更多信息。

### How to 操作方法

- [Creating packages
  创建软件包](https://dart.dev/guides/libraries/create-packages)
- [Publishing packages
  发布软件包](https://dart.dev/tools/pub/publishing)

### Reference 参考

- [Pub dependencies
  Pub 依赖项](https://dart.dev/tools/pub/dependencies)
- [Pub environment variables
  Pub 环境变量](https://dart.dev/tools/pub/environment-variables)
- [Pub glossary
  Pub 词汇表](https://dart.dev/tools/pub/glossary)
- [Pub package layout conventions
  Pub 软件包布局约定](https://dart.dev/tools/pub/package-layout)
- [Pub versioning philosophy
  Pub 版本控制理念](https://dart.dev/tools/pub/versioning)
- [Pubspec format
  Pubspec 格式](https://dart.dev/tools/pub/pubspec)

### Pub subcommands Pub 子命令

The `dart pub` tool provides the following subcommands:
`dart pub` 工具提供以下子命令：

- [`add`](https://dart.dev/tools/pub/cmd/pub-add)
- [`cache`](https://dart.dev/tools/pub/cmd/pub-cache)
- [`deps`](https://dart.dev/tools/pub/cmd/pub-deps)
- [`downgrade`](https://dart.dev/tools/pub/cmd/pub-downgrade)
- [`get`](https://dart.dev/tools/pub/cmd/pub-get)
- [`global`](https://dart.dev/tools/pub/cmd/pub-global)
- [`outdated`](https://dart.dev/tools/pub/cmd/pub-outdated)
- [`publish`](https://dart.dev/tools/pub/cmd/pub-lish)
- [`remove`](https://dart.dev/tools/pub/cmd/pub-remove)
- [`token`](https://dart.dev/tools/pub/cmd/pub-token)
- [`upgrade`](https://dart.dev/tools/pub/cmd/pub-upgrade)

For an overview of all the `dart pub` subcommands, see the [pub tool documentation](https://dart.dev/tools/pub/cmd).
有关所有 `dart pub` 子命令的概述，请参阅 pub 工具文档。

### Troubleshooting 故障排除

[Troubleshooting pub](https://dart.dev/tools/pub/troubleshoot) gives solutions to problems that you might encounter when using pub.
故障排除 pub 提供了解决您在使用 pub 时可能遇到的问题的方案。
