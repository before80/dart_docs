+++
title = "pubspec 文件"
date = 2024-01-05T20:29:36+08:00
weight = 40
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文: [https://dart.dev/tools/pub/pubspec](https://dart.dev/tools/pub/pubspec)

# The pubspec file  - pubspec 文件

Every [pub package](https://dart.dev/guides/packages) needs some metadata so it can specify its [dependencies](https://dart.dev/tools/pub/glossary#dependency). Pub packages that are shared with others also need to provide some other information so users can discover them. All of this metadata goes in the package’s *pubspec:* a file named `pubspec.yaml` that’s written in the [YAML](https://yaml.org/) language.
每个 pub 包都需要一些元数据，以便它可以指定其依赖项。与他人共享的 Pub 包还需要提供一些其他信息，以便用户可以发现它们。所有这些元数据都位于包的 pubspec 中：一个以 YAML 语言编写的名为 `pubspec.yaml` 的文件。

## Supported fields 支持的字段

A pubspec can have the following fields:
pubspec 可以具有以下字段：

- `name`

  Required for every package. [*Learn more.*](https://dart.dev/tools/pub/pubspec#name) 每个软件包都必需。了解更多信息。

- `version`

  Required for packages that are hosted on the pub.dev site. [*Learn more.*](https://dart.dev/tools/pub/pubspec#version) 托管在 pub.dev 网站上的软件包必需。了解更多信息。

- `description`

  Required for packages that are hosted on the pub.dev site. [*Learn more.*](https://dart.dev/tools/pub/pubspec#description) 托管在 pub.dev 网站上的软件包必需。了解更多信息。

- `homepage`

  Optional. URL pointing to the package’s homepage (or source code repository). [*Learn more.*](https://dart.dev/tools/pub/pubspec#homepage) 可选。指向软件包主页（或源代码存储库）的 URL。了解更多信息。

- `repository`

  Optional. URL pointing to the package’s source code repository. [*Learn more.*](https://dart.dev/tools/pub/pubspec#repository) 可选。指向软件包源代码存储库的 URL。了解更多信息。

- `issue_tracker`

  Optional. URL pointing to an issue tracker for the package. [*Learn more.*](https://dart.dev/tools/pub/pubspec#issue-tracker) 可选。指向软件包问题跟踪器的 URL。了解更多信息。

- `documentation`

  Optional. URL pointing to documentation for the package. [*Learn more.*](https://dart.dev/tools/pub/pubspec#documentation) 可选。指向软件包文档的 URL。了解更多信息。

- `dependencies`

  Can be omitted if your package has no dependencies. [*Learn more.*](https://dart.dev/tools/pub/pubspec#dependencies) 如果您的软件包没有依赖项，则可以省略。了解更多信息。

- `dev_dependencies`

  Can be omitted if your package has no dev dependencies. [*Learn more.*](https://dart.dev/tools/pub/pubspec#dependencies) 如果您的软件包没有开发依赖项，则可以省略。了解更多信息。

- `dependency_overrides`

  Can be omitted if you do not need to override any dependencies. [*Learn more.*](https://dart.dev/tools/pub/pubspec#dependencies) 如果您不需要覆盖任何依赖项，则可以省略。了解更多信息。

- `environment`

  Required as of Dart 2. [*Learn more.*](https://dart.dev/tools/pub/pubspec#sdk-constraints) Dart 2 中的必要内容。了解更多信息。

- `executables`

  Optional. Used to put a package’s executables on your PATH. [*Learn more.*](https://dart.dev/tools/pub/pubspec#executables) 可选。用于将软件包的可执行文件放在您的 PATH 中。了解更多信息。

- `platforms`

  Optional. Used to explicitly declare supported platforms on the pub.dev site. [*Learn more.*](https://dart.dev/tools/pub/pubspec#platforms) 可选。用于在 pub.dev 网站上明确声明受支持的平台。了解更多信息。

- `publish_to`

  Optional. Specify where to publish a package. [*Learn more.*](https://dart.dev/tools/pub/pubspec#publish_to) 可选。指定软件包的发布位置。了解更多信息。

- `funding`

  Optional. List of URLs where users can sponsor development of the package. [*Learn more.*](https://dart.dev/tools/pub/pubspec#funding) 可选。用户可以赞助软件包开发的 URL 列表。了解更多信息。

- `false_secrets`

  Optional. Specify files to ignore when conducting a pre-publishing search for potential leaks of secrets. [*Learn more.*](https://dart.dev/tools/pub/pubspec#false_secrets) 可选。指定在进行预发布搜索以查找潜在的秘密泄露时要忽略的文件。了解更多信息。

- `screenshots`

  Optional. Specify a list of screenshot files to display on the [pub.dev site](https://pub.dev/). [*Learn more.*](https://dart.dev/tools/pub/pubspec#screenshots) 可选。指定要在 pub.dev 网站上显示的屏幕截图文件列表。了解更多信息。

- `topics`

  Optional. List of topics for the package. [*Learn more.*](https://dart.dev/tools/pub/pubspec#topics) 可选。软件包的主题列表。了解更多信息。

Pub ignores all other fields.
Pub 忽略所有其他字段。

![Flutter logo](./Pubspecfile_img/64.png) **Flutter note:** Pubspecs for [Flutter apps](https://flutter.dev/) can have [additional fields](https://docs.flutter.dev/development/tools/pubspec) for configuring the environment and managing assets.
![Flutter logo](https://dart.dev/assets/img/shared/flutter/icon/64.png) Flutter 注释：Flutter 应用的 Pubspec 可以有其他字段，用于配置环境和管理资产。

If you add a custom field, give it a unique name that won’t clash with future pubspec fields. For example, instead of adding `bugs`, you might add a field named `my_pkg_bugs`.
如果您添加自定义字段，请为其指定一个不会与未来的 pubspec 字段冲突的唯一名称。例如，不要添加 `bugs` ，而可以添加名为 `my_pkg_bugs` 的字段。

## Example 示例

A simple but complete pubspec looks something like the following:
一个简单但完整的 pubspec 如下所示：

```
name: newtify
description: >-
  Have you been turned into a newt?  Would you like to be?
  This package can help. It has all of the
  newt-transmogrification functionality you have been looking
  for.
version: 1.2.3
homepage: https://example-pet-store.com/newtify
documentation: https://example-pet-store.com/newtify/docs

environment:
  sdk: '>=2.12.0 <3.0.0'
  
dependencies:
  efts: ^2.0.4
  transmogrify: ^0.4.0
  
dev_dependencies:
  test: '>=1.15.0 <2.0.0'
```

## Details 详细信息

This section has more information about each of the pubspec fields.
本部分提供了有关每个 pubspec 字段的更多信息。

### Name 名称

Every package needs a name. It’s how other packages refer to yours, and how it appears to the world, should you publish it.
每个软件包都需要一个名称。其他软件包会通过此名称引用您的软件包，如果您发布软件包，它也会以该名称显示给全世界。

The name should be all lowercase, with underscores to separate words, `just_like_this`. Use only basic Latin letters and Arabic digits: `[a-z0-9_]`. Also, make sure the name is a valid Dart identifier—that it doesn’t start with digits and isn’t a [reserved word](https://dart.dev/language/keywords).
名称应全部采用小写字母，并用下划线分隔单词， `just_like_this` 。仅使用基本拉丁字母和阿拉伯数字： `[a-z0-9_]` 。此外，请确保名称是有效的 Dart 标识符，即它不以数字开头，也不是保留字。

Try to pick a name that is clear, terse, and not already in use. A quick search of packages on the [pub.dev site](https://pub.dev/packages) to make sure that nothing else is using your name is recommended.
请尝试选择一个清晰、简洁且尚未使用的名称。建议您快速搜索 pub.dev 网站上的软件包，以确保没有其他软件包使用您的名称。

### Version 版本

Every package has a version. A version number is required to host your package on the pub.dev site, but can be omitted for local-only packages. If you omit it, your package is implicitly versioned `0.0.0`.
每个软件包都有一个版本。在 pub.dev 网站上托管软件包需要版本号，但仅供本地使用的软件包可以省略。如果您省略它，您的软件包将隐式版本化 `0.0.0` 。

Versioning is necessary for reusing code while letting it evolve quickly. A version number is three numbers separated by dots, like `0.2.43`. It can also optionally have a build ( `+1`, `+2`, `+hotfix.oopsie`) or prerelease (`-dev.4`, `-alpha.12`, `-beta.7`, `-rc.5`) suffix.
版本控制对于在让代码快速演进的同时重用代码是必要的。版本号是三个用点分隔的数字，如 `0.2.43` 。它还可以选择带有构建 ( `+1` , `+2` , `+hotfix.oopsie` ) 或预发布 ( `-dev.4` , `-alpha.12` , `-beta.7` , `-rc.5` ) 后缀。

Each time you publish your package, you publish it at a specific version. Once that’s been done, consider it hermetically sealed: you can’t touch it anymore. To make more changes, you’ll need a new version.
每次发布软件包时，您都会以特定版本发布它。一旦完成，请认为它是密封的：您无法再触碰它。要进行更多更改，您需要一个新版本。

When you select a version, follow [semantic versioning.](https://semver.org/spec/v2.0.0-rc.1.html)
选择版本时，请遵循语义版本控制。

### Description 说明

This is optional for your own personal packages, but if you intend to publish your package you must provide a description, which should be in English. The description should be relatively short—60 to 180 characters—and tell a casual reader what they might want to know about your package.
对于您自己的个人软件包，这是可选的，但如果您打算发布您的软件包，您必须提供说明，该说明应为英文。说明应相对简短——60 到 180 个字符——并告诉普通读者他们可能想要了解的有关您的软件包的信息。

Think of the description as the sales pitch for your package. Users see it when they [browse for packages.](https://pub.dev/packages) The description is plain text: no markdown or HTML.
将描述视为您软件包的销售宣传。用户在浏览软件包时会看到它。描述是纯文本：没有标记或 HTML。

### Homepage 主页

This should be a URL pointing to the website for your package. For [hosted packages](https://dart.dev/tools/pub/dependencies#hosted-packages), this URL is linked from the package’s page. While providing a `homepage` is optional, *please provide* it or `repository` (or both). It helps users understand where your package is coming from.
这应指向您软件包网站的 URL。对于托管软件包，此 URL 会从软件包页面链接。虽然提供 `homepage` 是可选的，但请提供它或 `repository` （或两者）。它有助于用户了解您的软件包来自何处。

### Repository 代码库

The optional `repository` field should contain the URL for your package’s source code repository—for example, `https://github.com/<user>/<repository>`. If you publish your package to the pub.dev site, then your package’s page displays the repository URL. While providing a `repository` is optional, *please provide* it or `homepage` (or both). It helps users understand where your package is coming from.
可选的 `repository` 字段应包含您软件包的源代码库的 URL，例如 `https://github.com/<user>/<repository>` 。如果您将软件包发布到 pub.dev 网站，那么您的软件包页面会显示代码库 URL。虽然提供 `repository` 是可选的，但请提供它或 `homepage` （或两者）。它有助于用户了解您的软件包来自何处。

### Issue tracker 问题跟踪器

The optional `issue_tracker` field should contain a URL for the package’s issue tracker, where existing bugs can be viewed and new bugs can be filed. The pub.dev site attempts to display a link to each package’s issue tracker, using the value of this field. If `issue_tracker` is missing but `repository` is present and points to GitHub, then the pub.dev site uses the default issue tracker (`https://github.com/<user>/<repository>/issues`).
可选的 `issue_tracker` 字段应包含软件包的问题跟踪器的 URL，可以在其中查看现有错误并提交新错误。pub.dev 网站尝试使用此字段的值显示指向每个软件包的问题跟踪器的链接。如果 `issue_tracker` 缺失但 `repository` 存在且指向 GitHub，那么 pub.dev 网站会使用默认问题跟踪器 ( `https://github.com/<user>/<repository>/issues` )。

### Documentation 文档

Some packages have a site that hosts documentation, separate from the main homepage and from the Pub-generated API reference. If your package has additional documentation, add a `documentation:` field with that URL; pub shows a link to this documentation on your package’s page.
有些软件包拥有一个托管文档的网站，该网站独立于主页和 Pub 生成的 API 参考。如果您的软件包有其他文档，请添加一个带有该 URL 的 `documentation:` 字段；pub 会在您的软件包页面上显示指向此文档的链接。

### Dependencies 依赖项

[Dependencies](https://dart.dev/tools/pub/glossary#dependency) are the pubspec’s *raison d’être*. In this section you list each package that your package needs in order to work.
依赖项是 pubspec 的存在理由。在此部分中，您列出软件包工作所需的每个软件包。

Dependencies fall into one of two types. *Regular dependencies* are listed under `dependencies:`—these are packages that anyone using your package will also need. Dependencies that are only needed in the development phase of the package itself are listed under `dev_dependencies`.
依赖项分为两种类型。常规依赖项列在 `dependencies:` 下——这些是使用您的软件包的任何人都需要的软件包。仅在软件包本身的开发阶段需要的依赖项列在 `dev_dependencies` 下。

During the development process, you might need to temporarily override a dependency. You can do so using `dependency_overrides`.
在开发过程中，您可能需要临时覆盖依赖项。您可以使用 `dependency_overrides` 来执行此操作。

For more information, see [Package dependencies](https://dart.dev/tools/pub/dependencies).
有关更多信息，请参阅软件包依赖项。

### Executables 可执行文件

A package may expose one or more of its scripts as executables that can be run directly from the command line. To make a script publicly available, list it under the `executables` field. Entries are listed as key/value pairs:
软件包可能会将其一个或多个脚本公开为可直接从命令行运行的可执行文件。要公开脚本，请将其列在 `executables` 字段下。条目列为键/值对：

```nocode
<name-of-executable>: <Dart-script-from-bin>
```

For example, the following pubspec entry lists two scripts:
例如，以下 pubspec 条目列出了两个脚本：

```
executables:
  slidy: main
  fvm:
```

Once the package is activated using `dart pub global activate`, typing `slidy` executes `bin/main.dart`. Typing `fvm` executes `bin/fvm.dart`. If you don’t specify the value, it is inferred from the key.
一旦使用 `dart pub global activate` 激活软件包，键入 `slidy` 会执行 `bin/main.dart` 。键入 `fvm` 会执行 `bin/fvm.dart` 。如果您未指定值，则会从键中推断值。

For more information, see [pub global](https://dart.dev/tools/pub/cmd/pub-global#running-a-script-from-your-path).
有关更多信息，请参阅 pub global。

### Platforms 平台

When you [publish a package](https://dart.dev/tools/pub/publishing), pub.dev automatically detects the platforms that the package supports. If this platform-support list is incorrect, use `platforms` to explicitly declare which platforms your package supports.
发布软件包时，pub.dev 会自动检测软件包支持的平台。如果此平台支持列表不正确，请使用 `platforms` 明确声明您的软件包支持哪些平台。

For example, the following `platforms` entry causes pub.dev to list the package as supporting Android, iOS, Linux, macOS, Web, and Windows:
例如，以下 `platforms` 条目会导致 pub.dev 将软件包列为支持 Android、iOS、Linux、macOS、Web 和 Windows：

```
# This package supports all platforms listed below.
platforms:
  android:
  ios:
  linux:
  macos:
  web:
  windows:
```

Here is an example of declaring that the package supports only Linux and macOS (and not, for example, Windows):
以下是一个示例，声明该软件包仅支持 Linux 和 macOS（例如，不支持 Windows）：

```
# This package supports only Linux and macOS.
platforms:
  linux:
  macos:
```

![Flutter logo](https://dart.dev/assets/img/shared/flutter/icon/64.png) **Flutter note:** Flutter plugins use [plugin declarations](https://docs.flutter.dev/development/packages-and-plugins/developing-packages#plugin-platforms) instead of this field.
![Flutter logo](https://dart.dev/assets/img/shared/flutter/icon/64.png) Flutter 注：Flutter 插件使用插件声明，而不是此字段。

*merge_type* **Version note:** Support for the `platforms` entry was added in Dart 2.16.
版本注：对 `platforms` 条目的支持已添加到 Dart 2.16 中。

### Publish_to 发布到

The default uses the [pub.dev site.](https://pub.dev/) Specify `none` to prevent a package from being published. This setting can be used to specify a [custom pub package server](https://dart.dev/tools/pub/custom-package-repositories) to publish.
默认情况下使用 pub.dev 网站。指定 `none` 以防止发布软件包。此设置可用于指定要发布到的自定义 pub 软件包服务器。

```
publish_to: none
```

### Funding 资金

Package authors can use the `funding` property to specify a list of URLs that provide information on how users can help fund the development of the package. For example:
软件包作者可以使用 `funding` 属性指定一个 URL 列表，其中提供有关用户如何帮助资助软件包开发的信息。例如：

```
funding:
 - https://www.buymeacoffee.com/example_user
 - https://www.patreon.com/some-account
```

If published to [pub.dev](https://pub.dev/) the links are displayed on the package page. This aims to help users fund the development of their dependencies.
如果发布到 pub.dev，则链接会显示在软件包页面上。这旨在帮助用户资助其依赖项的开发。

### False_secrets

When you try to [publish a package](https://dart.dev/tools/pub/publishing), pub conducts a search for potential leaks of secret credentials, API keys, or cryptographic keys. If pub detects a potential leak in a file that would be published, then pub warns you and refuses to publish the package.
当您尝试发布软件包时，pub 会搜索潜在的秘密凭据、API 密钥或加密密钥泄露。如果 pub 在将要发布的文件中检测到潜在泄露，则 pub 会警告您并拒绝发布软件包。

Leak detection isn’t perfect. To avoid false positives, you can tell pub not to search for leaks in certain files, by creating an allowlist using [`gitignore` patterns](https://git-scm.com/docs/gitignore#_pattern_format) under `false_secrets` in the pubspec.
泄露检测并不完美。为了避免误报，您可以通过在 pubspec 中的 `false_secrets` 下使用 `gitignore` 模式创建允许列表，告诉 pub 不要在某些文件中搜索泄露。

For example, the following entry causes pub not to look for leaks in the file `lib/src/hardcoded_api_key.dart` and in all `.pem` files in the `test/localhost_certificates/` directory:
例如，以下条目导致 pub 不在文件 `lib/src/hardcoded_api_key.dart` 中以及 `test/localhost_certificates/` 目录中的所有 `.pem` 文件中查找泄露：

```
false_secrets:
 - /lib/src/hardcoded_api_key.dart
 - /test/localhost_certificates/*.pem
```

Starting a `gitignore` pattern with slash (`/`) ensures that the pattern is considered relative to the package’s root directory.
以斜杠 ( `/` ) 开头的 `gitignore` 模式可确保该模式相对于软件包的根目录被视为相对的。

*report_problem* **Don’t rely on leak detection.** It uses a limited set of patterns to detect common mistakes. You’re responsible for managing your credentials, preventing accidental leaks, and revoking credentials that are accidentally leaked.
不要依赖泄漏检测。它使用一组有限的模式来检测常见错误。您负责管理您的凭据、防止意外泄漏以及撤销意外泄漏的凭据。

*merge_type* **Version note:** Dart 2.15 added support for the `false_secrets` field.
版本说明：Dart 2.15 添加了对 `false_secrets` 字段的支持。

### Screenshots 屏幕截图

Packages can showcase their widgets or other visual elements using screenshots displayed on their pub.dev page. To specify screenshots for the package to display, use the `screenshots` field.
软件包可以使用在其 pub.dev 页面上显示的屏幕截图来展示其小部件或其他可视元素。要为要显示的软件包指定屏幕截图，请使用 `screenshots` 字段。

A package can list up to 10 screenshots under the `screenshots` field. Don’t include logos or other branding imagery in this section. Each screenshot includes one `description` and one `path`. The `description` explains what the screenshot depicts in no more than 160 characters. For example:
一个软件包可以在 `screenshots` 字段下最多列出 10 个屏幕截图。请勿在此部分中包含徽标或其他品牌形象。每个屏幕截图都包含一个 `description` 和一个 `path` 。 `description` 以不超过 160 个字符解释屏幕截图所描述的内容。例如：

```
screenshots:
  - description: 'This screenshot shows the transformation of a number of bytes 
  to a human-readable expression.'
    path: path/to/image/in/package/500x500.webp
  - description: 'This screenshot shows a stack trace returning a human-readable
  representation.'
    path: path/to/image/in/package.png
```

Pub.dev limits screenshots to the following specifications:
Pub.dev 将屏幕截图限制为以下规格：

- File size: max 4 MB per image.
  文件大小：每个图像最大 4 MB。
- File types: `png`, `jpg`, `gif`, or `webp`.
  文件类型： `png` 、 `jpg` 、 `gif` 或 `webp` 。
- Static and animated images are both allowed.
  静态和动画图像均被允许。

Keep screenshot files small. Each download of the package includes all screenshot files.
保持屏幕截图文件较小。每次下载软件包都会包含所有屏幕截图文件。

Pub.dev generates the package’s thumbnail image from the first screenshot. If this screenshot uses animation, pub.dev uses its first frame.
Pub.dev 从第一个屏幕截图生成软件包的缩略图。如果此屏幕截图使用动画，pub.dev 将使用其第一帧。

### Topics 主题

Package authors can use the `topics` field to categorize their package. Topics can be used to assist discoverability during search with filters on pub.dev. Pub.dev displays the topics on the package page as well as in the search results.
软件包作者可以使用 `topics` 字段对他们的软件包进行分类。主题可用于在 pub.dev 上使用过滤器进行搜索时帮助发现。Pub.dev 在软件包页面以及搜索结果中显示主题。

The field consists of a list of names. For example:
该字段由名称列表组成。例如：

```
topics:
  - network
  - http
```

Pub.dev requires topics to follow these specifications:
Pub.dev 要求主题遵循以下规范：

- Tag each package with at most 5 topics.
  最多为每个软件包标记 5 个主题。

- Write the topic name following these requirements:

  
  按照以下要求编写主题名称：

  - Use between 2 and 32 characters.
    使用 2 到 32 个字符。
  - Use only lowercase alphanumeric characters or hyphens (`a-z`, `0-9`, `-`).
    仅使用小写字母数字字符或连字符 ( `a-z` 、 `0-9` 、 `-` )。
  - Don’t use two consecutive hyphens (`--`).
    不要使用两个连续的连字符 ( `--` )。
  - Start the name with lowercase alphabet characters (`a-z`).
    名称以小写字母字符开头 ( `a-z` )。
  - End with alphanumeric characters (`a-z` or `0-9`).
    以字母数字字符结尾 ( `a-z` 或 `0-9` )。

When choosing topics, consider if [existing topics](https://pub.dev/topics) are relevant. Tagging with existing topics helps users discover your package.
选择主题时，请考虑现有主题是否相关。使用现有主题进行标记有助于用户发现您的软件包。

### SDK constraints SDK 约束

A package can indicate which versions of its dependencies it supports, but packages have another implicit dependency: the Dart platform itself. The Dart platform evolves over time, and a package might only work with certain versions of the platform.
软件包可以指明其支持的依赖项版本，但软件包还有另一个隐式依赖项：Dart 平台本身。Dart 平台会随着时间推移而演变，软件包可能仅适用于某些版本的平台。

A package can specify those versions using an *SDK constraint*. This constraint goes inside a separate top-level `environment` field in the pubspec and uses the same [version constraint](https://dart.dev/tools/pub/dependencies#version-constraints) syntax as dependencies.
软件包可以使用 SDK 约束来指定这些版本。此约束位于 pubspec 中单独的顶级 `environment` 字段内，并使用与依赖项相同的版本约束语法。

*merge_type* **Version note:** For a package to use a feature introduced after 2.0, its pubspec must have a lower constraint that’s at least the version when the feature was introduced. For details, check out [Language versioning](https://dart.dev/guides/language/evolution#language-versioning).
版本说明：对于软件包来说，要使用 2.0 之后引入的功能，其 pubspec 必须具有至少为引入该功能的版本的下限约束。有关详细信息，请查看语言版本控制。

For example, the following constraint says that this package works with any Dart SDK that’s version 3.0.0 or higher:
例如，以下约束表示此软件包适用于任何版本为 3.0.0 或更高版本的 Dart SDK：

```
environment:
  sdk: ^3.0.0
```

Pub tries to find the latest version of a package whose SDK constraint works with the version of the Dart SDK that you have installed.
Pub 尝试查找与您已安装的 Dart SDK 版本兼容的 SDK 约束的软件包的最新版本。

Omitting the SDK constraint is an error. When the pubspec has no SDK constraint, `dart pub get` fails with a message like the following:
省略 SDK 约束是一个错误。当 pubspec 没有 SDK 约束时， `dart pub get` 会失败，并显示类似以下内容的消息：

```
pubspec.yaml has no lower-bound SDK constraint.
You should edit pubspec.yaml to contain an SDK constraint:

environment:
  sdk: '^3.0.0'
  
See https://dart.dev/go/sdk-constraint
```

*merge_type* **Version note:** Before Dart 2.19, pub disallowed caret syntax in SDK constraints. In earlier versions, provide a complete range, such as `'>=2.12.0 <3.0.0'`. For more information, check out the [Caret syntax](https://dart.dev/tools/pub/dependencies#caret-syntax) documentation.
版本说明：在 Dart 2.19 之前，pub 不允许在 SDK 约束中使用脱字符语法。在早期版本中，请提供一个完整范围，例如 `'>=2.12.0 <3.0.0'` 。有关更多信息，请查看脱字符语法文档。

#### Flutter SDK constraints Flutter SDK 约束

Pub supports specifying Flutter SDK constraints under the `environment:` field:
Pub 支持在 `environment:` 字段下指定 Flutter SDK 约束：

```
environment:
  sdk: '>=1.19.0 <3.0.0'
  flutter: ^0.1.2
```

A Flutter SDK constraint is satisfied only if pub is running in the context of the `flutter` executable, and the Flutter SDK’s `version` file matches the given version constraint. Otherwise, the package will not be selected.
仅当 pub 在 `flutter` 可执行文件的上下文中运行，并且 Flutter SDK 的 `version` 文件与给定的版本约束匹配时，才会满足 Flutter SDK 约束。否则，不会选择该软件包。

To publish a package with a Flutter SDK constraint, you must specify a Dart SDK constraint with a minimum version of at least 1.19.0, to ensure that older versions of pub won’t accidentally install packages that need Flutter.
要发布具有 Flutter SDK 约束的软件包，您必须指定最小版本至少为 1.19.0 的 Dart SDK 约束，以确保旧版本的 pub 不会意外安装需要 Flutter 的软件包。
