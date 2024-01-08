+++
title = "包布局约定"
date = 2024-01-05T20:29:36+08:00
weight = 20
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文: [https://dart.dev/tools/pub/package-layout](https://dart.dev/tools/pub/package-layout)

## Package layout conventions 包布局约定

When you build a [pub package](https://dart.dev/guides/packages), we encourage you to follow the conventions that this page describes. They describe how you organize the files and directories within your package, and how to name things.

​	在构建 pub 包时，我们建议您遵循此页面所述的约定。它们描述了如何在包中组织文件和目录，以及如何命名事物。

![Flutter logo](./Packagelayoutconventions_img/64.png) **Flutter note:** Flutter apps can use custom directories for their assets. For details, see [Adding assets and images](https://docs.flutter.dev/development/ui/assets-and-images) on the [Flutter website.](https://docs.flutter.dev/)

​	Flutter 注释：Flutter 应用可以使用自定义目录作为其资源。有关详细信息，请参阅 Flutter 网站上的添加资源和图像。

Here’s what a complete package (named `enchilada`) that uses every corner of these guidelines might look like:

​	以下是使用这些准则的每个部分的完整包（名为 `enchilada` ）可能的样子：

```nocode
enchilada/
  .dart_tool/ *
  pubspec.yaml
  pubspec.lock **
  LICENSE
  README.md
  CHANGELOG.md
  benchmark/
    make_lunch.dart
  bin/
    enchilada
  doc/
    api/ ***
    getting_started.md
  example/
    main.dart
  integration_test/
    app_test.dart
  lib/
    enchilada.dart
    tortilla.dart
    guacamole.css
    src/
      beans.dart
      queso.dart
  test/
    enchilada_test.dart
    tortilla_test.dart
  tool/
    generate_docs.dart
  web/
    index.html
    main.dart
    style.css
```

 The `.dart_tool/` directory exists after you’ve run `dart pub get`. Don’t check it into source control. To learn more, see [Project specific caching for tools](https://dart.dev/tools/pub/package-layout#project-specific-caching-for-tools).

​	 在运行 `dart pub get` 后， `.dart_tool/` 目录存在。不要将其检入源代码管理。要了解更多信息，请参阅工具的项目特定缓存。

The `pubspec.lock` file exists after you’ve run `dart pub get`. Leave it out of source control unless your package is an [application package](https://dart.dev/tools/pub/glossary#application-package).

​	在运行 `dart pub get` 后， `pubspec.lock` 文件存在。除非您的包是应用程序包，否则将其排除在源代码管理之外。

The `doc/api` directory exists locally after you’ve run [`dart doc`](https://dart.dev/tools/dart-doc). Don’t check the `api` directory into source control.

​	在运行 `dart doc` 后， `doc/api` 目录在本地存在。不要将 `api` 目录检入源代码管理。

## The pubspec pubspec

```nocode
enchilada/
  pubspec.yaml
  pubspec.lock
```

Every package has a [*pubspec*](https://dart.dev/tools/pub/pubspec), a file named `pubspec.yaml`, in the root directory of the package. That’s what *makes* it a package.

​	每个软件包都有一个 pubspec，即一个名为 `pubspec.yaml` 的文件，位于软件包的根目录中。这正是它成为软件包的原因。

Running [`dart pub get`](https://dart.dev/tools/pub/cmd/pub-get), [`dart pub upgrade`](https://dart.dev/tools/pub/cmd/pub-upgrade), or [`dart pub downgrade`](https://dart.dev/tools/pub/cmd/pub-downgrade) on the package creates a **lockfile**, named `pubspec.lock`. If your package is an [application package](https://dart.dev/tools/pub/glossary#application-package), check the lockfile into source control. Otherwise, don’t.

​	在软件包上运行 `dart pub get` 、 `dart pub upgrade` 或 `dart pub downgrade` 会创建一个名为 `pubspec.lock` 的锁定文件。如果您的软件包是应用程序软件包，请将锁定文件检入源代码管理。否则，请不要检入。

For more information, see the [pubspec page](https://dart.dev/tools/pub/pubspec).

​	有关更多信息，请参阅 pubspec 页面。

## 许可证 LICENSE 

```nocode
enchilada/
  LICENSE
```

If you’re publishing your package, include a license file named `LICENSE`. We recommend using an [OSI-approved license](https://opensource.org/licenses) such as [BSD-3-Clause,](https://opensource.org/licenses/BSD-3-Clause) so that others can reuse your work.

​	如果您要发布软件包，请包含一个名为 `LICENSE` 的许可证文件。我们建议使用经过 OSI 批准的许可证，例如 BSD-3-Clause，以便其他人可以重复使用您的作品。

## README.md

```nocode
enchilada/
  README.md
```

One file that’s very common in open source is a *README* file that describes the project. This is especially important in pub. When you upload to the [pub.dev site,](https://pub.dev/) your `README.md` file is shown—rendered as [Markdown](https://pub.dev/packages/markdown)—on the page for your package. This is the perfect place to introduce people to your code.

​	开源中非常常见的一个文件是描述项目的 README 文件。这在 pub 中尤为重要。当您上传到 pub.dev 网站时，您的 `README.md` 文件会显示在软件包的页面上，并以 Markdown 形式呈现。这是向人们介绍您的代码的理想场所。

For guidance on how to write a great README, see [Writing package pages](https://dart.dev/guides/libraries/writing-package-pages).

​	有关如何编写优秀 README 的指导，请参阅编写软件包页面。

## CHANGELOG.md

```nocode
enchilada/
  CHANGELOG.md
```

Include a `CHANGELOG.md` file that has a section for each release of your package, with notes to help users of your package upgrade. Users of your package often review the changelog to discover bug fixes and new features, or to determine how much effort it will take to upgrade to the latest version of your package.

​	包含一个 `CHANGELOG.md` 文件，其中包含软件包的每个版本的章节，并附有帮助软件包用户升级的说明。软件包用户通常会查看变更日志以发现错误修复和新功能，或确定升级到软件包最新版本需要花费多少精力。

To support tools that parse `CHANGELOG.md`, use the following format:

​	要支持解析 `CHANGELOG.md` 的工具，请使用以下格式：

- Each version has its own section with a heading.
- 每个版本都有自己的章节，并带有标题。
- The version headings are either all level 1 or all level 2.
- 版本标题均为 1 级或 2 级。
- The version heading text contains a package version number, optionally prefixed with “v”.
- 版本标题文本包含软件包版本号，前面可以选加“v”。

When you upload your package to the [pub.dev site,](https://pub.dev/) your package’s `CHANGELOG.md` file (if any) appears in the **Changelog** tab, rendered as [Markdown.](https://pub.dev/packages/markdown)

​	将软件包上传到 pub.dev 网站时，软件包的 `CHANGELOG.md` 文件（如果有）会显示在“变更日志”标签中，并以 Markdown 呈现。

Here’s an example of a `CHANGELOG.md` file. As the example shows, you can add subsections.

​	以下是一个 `CHANGELOG.md` 文件的示例。如示例所示，您可以添加子部分。

```
# 1.0.1

* Fixed missing exclamation mark in `sayHi()` method.

# 1.0.0

* **Breaking change:** Removed deprecated `sayHello()` method.
* Initial stable release.

## Upgrading from 0.1.x

Change all calls to `sayHello()` to instead be to `sayHi()`.

# 0.1.1

* Deprecated the `sayHello()` method; use `sayHi()` instead.

# 0.1.0

* Initial development release.
```

## 公共目录 Public directories 

Two directories in your package are public to other packages: `lib` and `bin`. You place [public libraries](https://dart.dev/tools/pub/package-layout#public-libraries) in `lib` and [public tools](https://dart.dev/tools/pub/package-layout#public-tools) in `bin`.

​	软件包中的两个目录对其他软件包是公开的： `lib` 和 `bin` 。您将公共库置于 `lib` 中，将公共工具置于 `bin` 中。

### Public libraries 公共库

The following directory structure shows the `lib` portion of enchilada:

​	以下目录结构显示了 enchilada 的 `lib` 部分：

```nocode
enchilada/
  lib/
    enchilada.dart
    tortilla.dart
```

Many [packages](https://dart.dev/tools/pub/glossary#package) define Dart libraries that other packages can import and use. These public Dart library files go inside a directory called `lib`.

​	许多软件包定义了其他软件包可以导入和使用的 Dart 库。这些公共 Dart 库文件位于名为 `lib` 的目录中。

Most packages define a single library that users can import. In that case, its name should usually be the same as the name of the package, like `enchilada.dart` in the example here. But you can also define other libraries with whatever names make sense for your package.

​	大多数软件包定义了一个用户可以导入的单个库。在这种情况下，其名称通常应与软件包的名称相同，如本例中的 `enchilada.dart` 。但您也可以使用对您的软件包有意义的任何名称来定义其他库。

When you do, users can import these libraries using the name of the package and the library file, like so:

​	执行此操作时，用户可以使用软件包的名称和库文件来导入这些库，如下所示：

```
import 'package:enchilada/enchilada.dart';
import 'package:enchilada/tortilla.dart';
```

If you want to organize your public libraries, you can also create subdirectories inside `lib`. If you do that, users will specify that path when they import it. Say you have the following file hierarchy:

​	如果要组织您的公共库，您还可以在 `lib` 内创建子目录。如果您这样做，用户将在导入时指定该路径。假设您具有以下文件层次结构：

```nocode
enchilada/
  lib/
    some/
      path/
        olives.dart
```

Users import `olives.dart` as follows:

​	用户按如下方式导入 `olives.dart` ：

```
import 'package:enchilada/some/path/olives.dart';
```

Note that only *libraries* should be in `lib`. *Entrypoints*—Dart scripts with a `main()` function—cannot go in `lib`. If you place a Dart script inside `lib`, you will discover that any `package:` imports it contains don’t resolve. Instead, your entrypoints should go in the appropriate [entrypoint directory](https://dart.dev/tools/pub/glossary#entrypoint-directory).

​	请注意，只有库应该在 `lib` 中。入口点（带有 `main()` 函数的 Dart 脚本）不能放在 `lib` 中。如果您将 Dart 脚本放在 `lib` 中，您会发现它包含的任何 `package:` 导入都无法解析。相反，您的入口点应该放在适当的入口点目录中。

*info* **Tip for web apps:** For the best performance when developing web apps, put [implementation files](https://dart.dev/tools/pub/package-layout#implementation-files) under `/lib/src`, instead of elsewhere under `/lib`. Also, avoid imports of `package:*package_name*/src/...`.

​	网络应用提示：为了在开发网络应用时获得最佳性能，请将实现文件放在 `/lib/src` 下，而不是放在 `/lib` 下的其他位置。此外，避免导入 `package:*package_name*/src/...` 。

For more information on packages, see [Creating packages](https://dart.dev/guides/libraries/create-packages).

​	有关包的更多信息，请参阅创建包。

### 公共工具 Public tools 

Dart scripts placed inside of the `bin` directory are public. If you’re inside the directory of a package, you can use [`dart run`](https://dart.dev/tools/dart-run) to run scripts from the `bin` directories of any other package the package depends on. From *any* directory, you can [run scripts](https://dart.dev/tools/pub/cmd/pub-global#running-a-script) from packages that you have activated using [`dart pub global activate`](https://dart.dev/tools/pub/cmd/pub-global#activating-a-package).

​	放在 `bin` 目录中的 Dart 脚本是公共的。如果您位于某个包的目录中，则可以使用 `dart run` 从包所依赖的任何其他包的 `bin` 目录运行脚本。您可以从任何目录运行使用 `dart pub global activate` 激活的包中的脚本。

If you intend for your package to be depended on, and you want your scripts to be private to your package, place them in the top-level `tool` directory. If you don’t intend for your package to be depended on, you can leave your scripts in `bin`.

​	如果您打算让您的包被依赖，并且您希望您的脚本对您的包是私有的，请将它们放在顶级 `tool` 目录中。如果您不打算让您的包被依赖，则可以将您的脚本留在 `bin` 中。

## 公共资产 Public assets 

```nocode
enchilada/
  lib/
    guacamole.css
```

While most packages exist to let you reuse Dart code, you can also reuse other kinds of content. For example, a package for [Bootstrap](https://getbootstrap.com/) might include a number of CSS files for consumers of the package to use.

​	虽然大多数包的存在是为了让您重用 Dart 代码，但您也可以重用其他类型的内容。例如，Bootstrap 的包可能包含许多 CSS 文件，供包的使用者使用。

These go in the top-level `lib` directory. You can put any kind of file in there and organize it with subdirectories however you like.

​	这些文件放在顶级 `lib` 目录中。您可以在其中放置任何类型文件，并根据需要使用子目录对其进行整理。

## 实现文件 Implementation files 

```nocode
enchilada/
  lib/
    src/
      beans.dart
      queso.dart
```

The libraries inside `lib` are publicly visible: other packages are free to import them. But much of a package’s code is internal implementation libraries that should only be imported and used by the package itself. Those go inside a subdirectory of `lib` called `src`. You can create subdirectories in there if it helps you organize things.

​	库内部 `lib` 是公开可见的：其他包可以自由导入它们。但包的大部分代码是内部实现库，只能由包本身导入和使用。这些代码位于 `lib` 的子目录中，称为 `src` 。如果这有助于您整理内容，您可以在其中创建子目录。

You are free to import libraries that live in `lib/src` from within other Dart code in the *same* package (like other libraries in `lib`, scripts in `bin`, and tests) but you should never import from another package’s `lib/src` directory. Those files are not part of the package’s public API, and they might change in ways that could break your code.

​	您可以从同一包中的其他 Dart 代码（例如 `lib` 中的其他库、 `bin` 中的脚本和测试）中导入位于 `lib/src` 中的库，但您绝不应从另一个包的 `lib/src` 目录中导入。这些文件不属于包的公共 API，它们可能会以破坏您代码的方式发生更改。

How you import libraries from within your own package depends on the locations of the libraries:
您从自己的包中导入库的方式取决于库的位置：

- When [reaching inside or outside `lib/`](https://dart.dev/effective-dart/usage#dont-allow-an-import-path-to-reach-into-or-out-of-lib) (lint: [*avoid_relative_lib_imports*](https://dart.dev/tools/linter-rules/avoid_relative_lib_imports)), use `package:`.
- 在 `lib/` 内部或外部访问时（lint：avoid_relative_lib_imports），请使用 `package:` 。
- Otherwise, [prefer relative imports](https://dart.dev/effective-dart/usage#prefer-relative-import-paths).
- 否则，最好使用相对导入。

For example:

​	例如：

```
// When importing from lib/beans.dart
import 'src/beans.dart';

// When importing from test/beans_test.dart
import 'package:enchilada/src/beans.dart';
```

The name you use here (in this case `enchilada`) is the name you specify for your package in its [pubspec](https://dart.dev/tools/pub/pubspec).

​	您在此处使用的名称（在本例中为 `enchilada` ）是您在 pubspec 中为包指定的名称。

##  Web 文件 Web files

```nocode
enchilada/
  web/
    index.html
    main.dart
    style.css
```

For web packages, place entrypoint code—Dart scripts that include `main()` and supporting files, such as CSS or HTML—under `web`. You can organize the `web` directory into subdirectories if you like.

​	对于网络软件包，将入口点代码（包含 `main()` 的 Dart 脚本和支持文件，例如 CSS 或 HTML）放在 `web` 下。如果愿意，可以将 `web` 目录整理成子目录。

Put [library code](https://dart.dev/tools/pub/package-layout#public-libraries) under `lib`. If the library isn’t imported directly by code under `web`, or by another package, put it under `lib/src`. Put [web-based examples](https://dart.dev/tools/pub/package-layout#examples) under `example`. See [Public assets](https://dart.dev/tools/pub/package-layout#public-assets) for tips on where to put assets, such as images.

​	将库代码放在 `lib` 下。如果库未被 `web` 下的代码或其他软件包直接导入，请将其放在 `lib/src` 下。将基于网络的示例放在 `example` 下。有关放置资产（例如图像）的位置的提示，请参阅公共资产。

## 命令行应用 Command-line apps 

```nocode
enchilada/
  bin/
    enchilada
```

Some packages define programs that can be run directly from the command line. These can be shell scripts or any other scripting language, including Dart.

​	某些软件包定义可以直接从命令行运行的程序。这些可以是 shell 脚本或任何其他脚本语言，包括 Dart。

If your package defines code like this, put it in a directory named `bin`. You can run that script from anywhere on the command line, if you set it up using [`dart pub global`](https://dart.dev/tools/pub/cmd/pub-global#running-a-script-from-your-path).

​	如果您的软件包定义了这样的代码，请将其放在名为 `bin` 的目录中。如果您使用 `dart pub global` 设置了该脚本，则可以从命令行的任何位置运行该脚本。

## 测试和基准 Tests and benchmarks 

```nocode
enchilada/
  test/
    enchilada_test.dart
    tortilla_test.dart
```

Every package should have tests. With pub, the convention is that most of these go in a `test` directory (or some directory inside it if you like) and have `_test` at the end of their file names.

​	每个软件包都应该有测试。在 pub 中，惯例是将其中大部分放在 `test` 目录中（或者您喜欢的该目录中的某个目录），并在其文件名末尾加上 `_test` 。

Typically, these use the [test](https://pub.dev/packages/test) package.

​	通常，这些使用测试软件包。

```nocode
enchilada/
  integration_test/
    app_test.dart
```

Flutter app packages may also have special integration tests, which use the [integration_test](https://docs.flutter.dev/cookbook/testing/integration/introduction) package. These tests live in their own `integration_test` directory.

​	Flutter 应用软件包可能还具有特殊的集成测试，这些测试使用 integration_test 软件包。这些测试位于它们自己的 `integration_test` 目录中。

Other packages may choose to follow a similar pattern, to separate their slower integration tests from their unit tests, but note that by default `dart test` will not run these tests. You will have to explicitly run them with `dart test integration_test`.

​	其他软件包可以选择遵循类似的模式，以将较慢的集成测试与单元测试分开，但请注意，默认情况下 `dart test` 不会运行这些测试。您必须使用 `dart test integration_test` 显式运行它们。

```nocode
enchilada/
  benchmark/
    make_lunch.dart
```

Packages that have performance critical code may also include *benchmarks*. These test the API not for correctness but for speed (or memory use, or maybe other empirical metrics).

​	对于性能关键代码的软件包，也可以包含基准测试。这些测试不是为了正确性而测试 API，而是为了速度（或内存使用，或其他经验指标）。

## 文档 Documentation 

```nocode
enchilada/
  doc/
    api/
    getting_started.md
```

If you have code and tests, the next piece you might want is good documentation. That goes inside a directory named `doc`.

​	如果您有代码和测试，您可能想要做的下一件事就是编写良好的文档。这些文档位于名为 `doc` 的目录中。

When you run the [`dart doc`](https://dart.dev/tools/dart-doc) tool, it places the API documentation, by default, under `doc/api`. Since the API documentation is generated from the source code, you should not place it under source control.

​	当您运行 `dart doc` 工具时，它默认情况下会将 API 文档放在 `doc/api` 下。由于 API 文档是从源代码生成的，因此您不应将其置于源代码管理之下。

Other than the generated `api`, we don’t have any guidelines about format or organization of the documentation that you author. Use whatever markup format that you prefer.

​	除了生成的 `api` 之外，我们没有任何关于您编写的文档的格式或组织的准则。使用您喜欢的任何标记格式。

## 示例 Examples 

```nocode
enchilada/
  example/
    main.dart
```

Code, tests, docs, what else could your users want? Standalone example programs that use your package, of course! Those go inside the `example` directory. If the examples are complex and use multiple files, consider making a directory for each example. Otherwise, you can place each one right inside `example`.

​	代码、测试、文档，您的用户还能想要什么？当然，是使用您软件包的独立示例程序！这些程序位于 `example` 目录中。如果示例很复杂并使用多个文件，请考虑为每个示例创建一个目录。否则，您可以将每个示例直接放在 `example` 中。

In your examples, use `package:` to import files from your own package. That ensures that the example code in your package looks exactly like code outside of your package would look.

​	在您的示例中，使用 `package:` 从您自己的软件包中导入文件。这可确保您软件包中的示例代码看起来与软件包外部的代码完全一样。

If you might publish your package, consider creating an example file with one of the following names:

​	如果您可能发布您的软件包，请考虑使用以下名称之一创建一个示例文件：

- `example/example[.md]`
- `example[/lib]/main.dart`
- `example[/lib]/*package_name*.dart`
- `example[/lib]/*package_name*_example.dart`
- `example[/lib]/example.dart`
- `example/README[.md]`

When you publish a package that contains one or more of the above files, the pub.dev site creates an **Example** tab to display the first file it finds (searching in the order shown in the list above). For example, if your package has many files under its `example` directory, including a file named `README.md`, then your package’s Example tab displays the contents of `example/README.md` (parsed as [Markdown.)](https://pub.dev/packages/markdown)

​	当您发布包含上述文件之一或多个文件的软件包时，pub.dev 网站会创建一个示例选项卡来显示它找到的第一个文件（按上述列表中显示的顺序搜索）。例如，如果您的软件包在其 `example` 目录下具有许多文件，包括名为 `README.md` 的文件，那么您的软件包的示例选项卡将显示 `example/README.md` 的内容（解析为 Markdown。）

## 内部工具和脚本 Internal tools and scripts 

```nocode
enchilada/
  tool/
    generate_docs.dart
```

Mature packages often have little helper scripts and programs that people run while developing the package itself. Think things like test runners, documentation generators, or other bits of automation.

​	成熟的软件包通常会有一些小型帮助脚本和程序，人们在开发软件包本身时会运行这些脚本和程序。想想诸如测试运行器、文档生成器或其他自动化程序之类的东西。

Unlike the scripts in `bin`, these are *not* for external users of the package. If you have any of these, place them in a directory called `tool`.

​	与 `bin` 中的脚本不同，这些脚本不适用于软件包的外部用户。如果您有其中任何一个，请将它们放在名为 `tool` 的目录中。

## 工具的项目特定缓存 Project-specific caching for tools 

*info* Do not check the `.dart_tool/` directory into source control. Instead, keep `.dart_tool/` in `.gitignore`.

​	不要将 `.dart_tool/` 目录检入源代码管理。相反，将 `.dart_tool/` 保留在 `.gitignore` 中。

The `.dart_tool/` directory is created when you run `dart pub get` and might be deleted at any time. Various tools use this directory for caching files specific to your project and/or local machine. The `.dart_tool/` directory should never be checked into source control, or copied between machines.

​	当您运行 `dart pub get` 时，将创建 `.dart_tool/` 目录，并且该目录可能会随时被删除。各种工具使用此目录来缓存特定于您的项目和/或本地计算机的文件。切勿将 `.dart_tool/` 目录检入源代码管理，或在计算机之间复制该目录。

It is also generally safe to delete the `.dart_tool/` directory, though some tools might need recompute the cached information.

​	通常情况下，删除 `.dart_tool/` 目录也是安全的，尽管某些工具可能需要重新计算缓存的信息。

**Example:** The [`dart pub get`](https://dart.dev/tools/pub/cmd/pub-get) tool will download and extract dependencies to a global `$PUB_CACHE` directory, and then write a `.dart_tool/package_config.json` file mapping *package names* to directories in the global `$PUB_CACHE` directory. The `.dart_tool/package_config.json` file is used by other tools, such as the analyzer and compilers when they need to resolve statements such as `import 'package:foo/foo.dart'`.

​	示例： `dart pub get` 工具会将依赖项下载并解压缩到全局 `$PUB_CACHE` 目录，然后编写一个 `.dart_tool/package_config.json` 文件，将包名称映射到全局 `$PUB_CACHE` 目录中的目录。当分析器和编译器等其他工具需要解析诸如 `import 'package:foo/foo.dart'` 之类的语句时，它们会使用 `.dart_tool/package_config.json` 文件。

When developing a tool that needs project-specific caching, you might consider using the `.dart_tool/` directory because most users already ignore it with `.gitignore`. When caching files for your tool in a user’s `.dart_tool/` directory, you should use a unique subdirectory. To avoid collisions, subdirectories of the form `.dart_tool/<tool_package_name>/` are reserved for the package named `<tool_package_name>`. If your tool isn’t distributed through the [pub.dev site,](https://pub.dev/) you might consider publishing a placeholder package in order to reserve the unique name.

​	在开发需要项目特定缓存的工具时，您可以考虑使用 `.dart_tool/` 目录，因为大多数用户已经使用 `.gitignore` 忽略了它。在用户的 `.dart_tool/` 目录中缓存工具的文件时，您应该使用唯一的子目录。为了避免冲突，形式为 `.dart_tool/<tool_package_name>/` 的子目录专用于名为 `<tool_package_name>` 的软件包。如果您的工具不是通过 pub.dev 网站分发，您可以考虑发布占位符软件包以保留唯一名称。

**Example:** [`package:build`](https://pub.dev/packages/build) provides a framework for writing code generation steps. When running these build steps, files are cached in `.dart_tool/build/`. This helps speed-up future re-runs of the build steps.

​	示例： `package:build` 提供了一个用于编写代码生成步骤的框架。运行这些构建步骤时，文件会缓存在 `.dart_tool/build/` 中。这有助于加快构建步骤的未来重新运行速度。

*report_problem* **Warning:** When developing a tool that wants to cache files in `.dart_tool/`, ensure the following:

​	警告：在开发想要在 `.dart_tool/` 中缓存文件的工具时，请确保以下内容：

- You are using a subdirectory named after a package you own (`.dart_tool/<my_tool_package_name>/`)
- 您正在使用以您拥有的软件包命名的子目录 ( `.dart_tool/<my_tool_package_name>/` )
- Your files don’t belong under source control, as `.dart_tool/` is generally listed in `.gitignore`
- 您的文件不属于源代码管理，因为 `.dart_tool/` 通常在 `.gitignore` 中列出
