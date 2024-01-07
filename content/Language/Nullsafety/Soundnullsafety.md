+++
title = "健全的空安全"
date = 2024-01-05T20:29:36+08:00
weight = 1
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文: [https://dart.dev/null-safety](https://dart.dev/null-safety)

## Sound null safety 健全的空安全

The Dart language enforces sound null safety.

​	Dart 语言强制执行健全的空安全性。

Null safety prevents errors that result from unintentional access of variables set to `null`.

​	空安全可防止因意外访问设置为 `null` 的变量而导致的错误。

For example, if a method expects an integer but receives `null`, your app causes a runtime error. This type of error, a null dereference error, can be difficult to debug.

​	例如，如果某个方法期望一个整数但收到 `null` ，则您的应用会引发运行时错误。这种类型的错误（空引用错误）可能很难调试。

With sound null safety, all variables require a value. This means Dart considers all variables *non-nullable*. You can assign values of the declared type only, like `int i=42`. You can never assign a value of `null` to default variable types. To specify that a variable type can have a `null` value, add a `?` after the type annotation: `int? i`. These specific types can contain either a `null` *or* a value of the defined type.

​	通过可靠的空安全，所有变量都需要一个值。这意味着 Dart 将所有变量视为非空。您只能分配已声明类型的变量，例如 `int i=42` 。您永远无法将 `null` 的值分配给默认变量类型。要指定变量类型可以具有 `null` 值，请在类型注释后添加 `?` ： `int? i` 。这些特定类型可以包含 `null` 或已定义类型的变量。

Sound null safety changes potential **runtime errors** into **edit-time** analysis errors. With null safety, the Dart analyzer and compilers flag if a non-nullable variable has either:

​	可靠的空安全将潜在的运行时错误更改为编辑时分析错误。通过空安全，如果非空变量具有以下情况，Dart 分析器和编译器会标记出来：

- Not been initialized with a non-null value
- 尚未使用非空值初始化
- Been assigned a `null` value. These checks allows you to fix these errors *before* deploying your app.
- 已分配 `null` 值。这些检查允许您在部署应用之前修复这些错误。

## 通过示例进行介绍 Introduction through examples 

With null safety, none of the variables in the following code can be `null`:

​	有了空安全，以下代码中的任何变量都不能为 `null` ：

```
// With null safety, none of these can ever be null.
var i = 42; // Inferred to be an int.
String name = getFileName();
final b = Foo();
```

To indicate that a variable might have the value `null`, just add `?` to its type declaration:

​	要指示变量可能具有值 `null` ，只需在其类型声明中添加 `?` ：

```
int? aNullableInt = null;
```

- To try an interactive example, see the [null safety codelab](https://dart.dev/codelabs/null-safety).
- 要尝试一个交互式示例，请参阅空安全代码实验室。
- To learn more about this topic, see [Understanding null safety](https://dart.dev/null-safety/understanding-null-safety).
- 要详细了解此主题，请参阅了解空安全。

## 空安全原则 Null safety principles 

Dart supports null safety using the following two core design principles:

​	Dart 使用以下两个核心设计原则支持空安全：

- **Non-nullable by default**. Unless you explicitly tell Dart that a variable can be null, it’s considered non-nullable. This default was chosen after research found that non-null was by far the most common choice in APIs.
- 默认情况下不可为 null。除非您明确告诉 Dart 变量可以为 null，否则它将被视为不可为 null。在研究发现非空是 API 中最常见的选项后，选择了此默认值。
- **Fully sound**. Dart’s null safety is sound, which enables compiler optimizations. If the type system determines that something isn’t null, then that thing can *never* be null. Once you migrate your whole project and its dependencies to null safety, you reap the full benefits of soundness—not only fewer bugs, but smaller binaries and faster execution.
- 完全健全。Dart 的空安全是健全的，这使得编译器优化成为可能。如果类型系统确定某物不为 null，那么该事物永远不会为 null。一旦您将整个项目及其依赖项迁移到空安全，您将获得健全性的全部好处——不仅是更少的错误，还有更小的二进制文件和更快的执行速度。

## Dart 3 和空安全 Dart 3 and null safety 

Dart 3 has built-in sound null safety. Dart 3 prevents code without it from running.

​	Dart 3 内置健全的空安全。Dart 3 阻止不包含空安全的代码运行。

To learn how to migrate to Dart 3, check out the [Dart 3 migration guide](https://dart.dev/resources/dart-3-migration). Packages developed without null safety support cause issues when resolving dependencies:

​	要了解如何迁移到 Dart 3，请查看 [Dart 3 迁移指南](https://dart.dev/resources/dart-3-migration)。在解析依赖项时，未经空安全支持开发的软件包会导致问题：

```
$ dart pub get

Because pkg1 doesn't support null safety, version solving failed.
The lower bound of "sdk: '>=2.9.0 <3.0.0'" must be 2.12.0 or higher to enable null safety.
```

Libraries incompatible with Dart 3 cause analysis or compilation errors.

​	与 Dart 3 不兼容的库会导致分析或编译错误。

```
$ dart analyze .
Analyzing ....                         0.6s

  error • lib/pkg1.dart:1:1 • The language version must be >=2.12.0. 
  Try removing the language version override and migrating the code.
  • illegal_language_version_override
$ dart run bin/my_app.dart
../pkg1/lib/pkg1.dart:1:1: Error: Library doesn't support null safety.
// @dart=2.9
^^^^^^^^^^^^
```

To resolve these issues:

​	要解决这些问题：

1. Check for [null safe versions](https://dart.dev/null-safety/migration-guide#check-dependency-status) of any packages you installed from pub.dev
2. 检查您从 pub.dev 安装的任何软件包的空安全版本
3. [migrate](https://dart.dev/null-safety#migrate) all of your source code to use sound null safety.
4. 将所有源代码迁移为使用健全的空安全。

Dart 3 can be found in the stable channels for Dart and Flutter. To learn more, check out [the download page](https://dart.dev/get-dart/archive) for details. To test your code for Dart 3 compatibility, use Dart 3 or later.

​	可以在 Dart 和 Flutter 的稳定频道中找到 Dart 3。要了解更多信息，请查看下载页面了解详情。要测试您的代码是否与 Dart 3 兼容，请使用 Dart 3 或更高版本。

```
$ dart --version                     # make sure this reports 3.0.0-417.1.beta or higher
$ dart pub get / flutter pub get     # this should resolve without issues
$ dart analyze / flutter analyze     # this should pass without errors
```

If the `pub get` step fails, check the [status of the dependencies](https://dart.dev/null-safety/migration-guide#check-dependency-status).

​	如果 `pub get` 步骤失败，请检查依赖项的状态。

If the `analyze` step fails, update your code to resolve the issues listed by the analyzer.

​	如果 `analyze` 步骤失败，请更新您的代码以解决分析器列出的问题。

## Dart 2.x 和空安全 Dart 2.x and null safety 

From Dart 2.12 to 2.19, you need to enable null safety. You cannot use null safety in SDK versions earlier than Dart 2.12.

​	从 Dart 2.12 到 2.19，您需要启用空安全。您无法在早于 Dart 2.12 的 SDK 版本中使用空安全。

To enable sound null safety, set the [SDK constraint lower-bound](https://dart.dev/tools/pub/pubspec#sdk-constraints) to a [language version](https://dart.dev/guides/language/evolution#language-versioning) of 2.12 or later. For example, your `pubspec.yaml` file might have the following constraints:

​	若要启用声音空安全，请将 SDK 约束下限设置为 2.12 或更高版本的语言版本。例如，您的 `pubspec.yaml` 文件可能具有以下约束：

```
environment:
  sdk: '>=2.12.0 <3.0.0'
```

## 迁移现有代码 Migrating existing code 

*report_problem* **Warning:** Dart 3 removes the `dart migrate` tool. If you need help migrating your code, run the tool with the 2.19 SDK, then upgrade to Dart 3.

​	警告：Dart 3 移除了 `dart migrate` 工具。如果您需要帮助迁移代码，请使用 2.19 SDK 运行该工具，然后升级到 Dart 3。

You can migrate without the tool, but it involves hand editing code.

​	您可以在没有该工具的情况下进行迁移，但这涉及手动编辑代码。

Dart code written without null safety support can be migrated to use null safety. We recommend using the `dart migrate` tool, included in the Dart SDK versions 2.12 to 2.19.

​	编写的 Dart 代码不具有空安全支持，可以迁移为使用空安全。我们建议使用 `dart migrate` 工具，该工具包含在 Dart SDK 版本 2.12 到 2.19 中。

```
$ cd my_app
$ dart migrate
```

To learn how to migrate your code to null safety, check out the [migration guide](https://dart.dev/null-safety/migration-guide).

​	要了解如何将代码迁移到空安全，请查看迁移指南。

## 了解更多信息 Where to learn more 

To learn more about null safety, check out the following resources:

​	要了解有关空安全的更多信息，请查看以下资源：

- [Null safety codelab
  空安全代码实验室](https://dart.dev/codelabs/null-safety)
- [Understanding null safety
  了解空安全](https://dart.dev/null-safety/understanding-null-safety)
- [Migration guide for existing code
  现有代码的迁移指南](https://dart.dev/null-safety/migration-guide)
- [Null safety FAQ
  Null 安全常见问题解答](https://dart.dev/null-safety/faq)
- [Null safety sample code
  Null 安全示例代码](https://github.com/dart-lang/samples/tree/main/null_safety/calculate_lix)
