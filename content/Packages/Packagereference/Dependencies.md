+++
title = "依赖项"
date = 2024-01-05T20:29:36+08:00
weight = 1
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文: [https://dart.dev/tools/pub/dependencies](https://dart.dev/tools/pub/dependencies)

## Package dependencies 软件包依赖项

Dependencies are one of the core concepts of the [pub package manager](https://dart.dev/guides/packages). A *dependency* is another package that your package needs to work. Dependencies are specified in your [pubspec](https://dart.dev/tools/pub/pubspec). You list only *immediate dependencies*: the software that your package uses directly. Pub handles [transitive dependencies](https://dart.dev/tools/pub/glossary#transitive-dependency) for you.

​	依赖项是 pub 软件包管理器的核心概念之一。依赖项是您的软件包工作所需的另一个软件包。依赖项在您的 pubspec 中指定。您只列出直接依赖项：您的软件包直接使用的软件。Pub 会为您处理传递依赖项。

This page has detailed information on how to specify dependencies. At the end is a list of [best practices for package dependencies](https://dart.dev/tools/pub/dependencies#best-practices).

​	此页面详细介绍了如何指定依赖项。最后列出了有关软件包依赖项的最佳做法。

## 概述 Overview 

For each dependency, you specify the *name* of the package you depend on and the *range of versions* of that package that you allow. You can also specify the [*source*](https://dart.dev/tools/pub/glossary#source). The source tells pub how to locate the package.

​	对于每个依赖项，您需要指定所依赖的软件包的名称以及允许的该软件包的版本范围。您还可以指定源。源告诉 pub 如何找到该软件包。

As an example, you specify a dependency in the following format:

​	例如，您可以使用以下格式指定依赖项：

```
dependencies:
  transmogrify: ^1.0.0
```

This YAML code creates a dependency on the `transmogrify` package using the default package repository ([pub.dev](https://pub.dev/)) and allowing any version from `1.0.0` to `2.0.0` (but not including `2.0.0`). To learn about this syntax, check out [version constraints](https://dart.dev/tools/pub/dependencies#version-constraints).

​	此 YAML 代码使用默认软件包存储库 ( pub.dev) 创建对 `transmogrify` 软件包的依赖项，并允许从 `1.0.0` 到 `2.0.0` 的任何版本（但不包括 `2.0.0` ）。要了解此语法，请查看版本约束。

To specify a source other than pub.dev, use `sdk`, `hosted`, `git`, or `path`. For example, the following YAML code uses `path` to tell pub to get `transmogrify` from a local directory:

​	要指定除 pub.dev 之外的其他源，请使用 `sdk` 、 `hosted` 、 `git` 或 `path` 。例如，以下 YAML 代码使用 `path` 告诉 pub 从本地目录获取 `transmogrify` ：

```
dependencies:
  transmogrify:
    path: /Users/me/transmogrify
```

The next section describes the format for each dependency source.

​	下一部分介绍每个依赖项源的格式。

## 依赖项源 Dependency sources 

Pub can use the following sources to locate packages:

​	Pub 可以使用以下源来查找软件包：

- [SDK](https://dart.dev/tools/pub/dependencies#sdk)
- [Hosted packages
  托管软件包](https://dart.dev/tools/pub/dependencies#hosted-packages)
- [Git packages
  Git 软件包](https://dart.dev/tools/pub/dependencies#git-packages)
- [Path packages
  路径包](https://dart.dev/tools/pub/dependencies#path-packages)

### 托管包 Hosted packages 

A *hosted* package is one that can be downloaded from the pub.dev site (or another HTTP server that speaks the same API). Here’s an example of declaring a dependency on a hosted package:

​	托管包是可以从 pub.dev 网站（或使用相同 API 的其他 HTTP 服务器）下载的包。以下是如何声明对托管包的依赖关系的示例：

```
dependencies:
  transmogrify: ^1.4.0
```

This example specifies that your package depends on a hosted package named `transmogrify` and works with any version from 1.4.0 to 2.0.0 (but not 2.0.0 itself).

​	此示例指定您的包依赖于名为 `transmogrify` 的托管包，并且适用于从 1.4.0 到 2.0.0（但不包括 2.0.0 本身）的任何版本。

If you want to use your [own package repository](https://dart.dev/tools/pub/custom-package-repositories), you can use `hosted` to specify its URL. The following YAML code creates a dependency on the `transmogrify` package using the `hosted` source:

​	如果您想使用自己的包存储库，可以使用 `hosted` 来指定其 URL。以下 YAML 代码使用 `hosted` 源创建对 `transmogrify` 包的依赖关系：

```yaml
environment:
  sdk: '>=2.15.0 < 3.0.0'

dependencies:
  transmogrify:
    hosted: https://some-package-server.com
    version: ^1.4.0
```

The version constraint is optional but recommended. If no version constraint is given, `any` is assumed.

​	版本约束是可选的，但建议使用。如果未给出版本约束，则假定为 `any` 。

*merge_type* **Version note:** If your package has a [language version](https://dart.dev/guides/language/evolution#language-versioning) before 2.15, you must use a more verbose `hosted` format:

​	版本说明：如果您的包的语言版本低于 2.15，则必须使用更详细的 `hosted` 格式：

```yaml
environment:
  sdk: '>=2.14.0 < 3.0.0'

dependencies:
  transmogrify:
    hosted:
      name: transmogrify
      url: https://some-package-server.com
    version: ^1.4.0
```

### Git 包 Git packages 

Sometimes you live on the bleeding edge and need to use packages that haven’t been formally released yet. Maybe your package itself is still in development and is using other packages that are being developed at the same time. To make that easier, you can depend directly on a package stored in a [Git](https://git-scm.com/) repository.

​	有时，您生活在技术前沿，需要使用尚未正式发布的包。也许您的包本身仍在开发中，并且正在使用同时开发的其他包。为了简化操作，您可以直接依赖存储在 Git 存储库中的包。

```
dependencies:
  kittens:
    git: https://github.com/munificent/kittens.git
```

The `git` here says this package is found using Git, and the URL after that is the Git URL that can be used to clone the package.

​	这里 `git` 表示此软件包是使用 Git 找到的，其后的 URL 是可用于克隆软件包的 Git URL。

Even if the package repo is private, if you can [connect to the repo using SSH,](https://help.github.com/articles/connecting-to-github-with-ssh/) then you can depend on the package by using the repo’s SSH URL:

​	即使软件包存储库是私有的，如果您能够使用 SSH 连接到存储库，那么您可以使用存储库的 SSH URL 来依赖该软件包：

```
dependencies:
  kittens:
    git: git@github.com:munificent/kittens.git
```

If you want to depend on a specific commit, branch, or tag, add a `ref` key to the description:

​	如果您想依赖特定的提交、分支或标记，请向说明中添加 `ref` 键：

```
dependencies:
  kittens:
    git:
      url: git@github.com:munificent/kittens.git
      ref: some-branch
```

The ref can be anything that Git allows to [identify a commit.](https://www.kernel.org/pub/software/scm/git/docs/user-manual.html#naming-commits)

​	引用可以是 Git 允许用来标识提交的任何内容。

Pub assumes that the package is in the root of the Git repository. To specify a different location in the repo, specify a `path` relative to the repository root:

​	Pub 假设软件包位于 Git 存储库的根目录中。要指定存储库中的其他位置，请指定相对于存储库根目录的 `path` ：

```
dependencies:
  kittens:
    git:
      url: git@github.com:munificent/cats.git
      path: path/to/kittens
```

The path is relative to the Git repo’s root.

​	路径相对于 Git 存储库的根目录。

Git dependencies are not allowed as dependencies for packages uploaded to [pub.dev](https://pub.dev/).

​	不允许将 Git 依赖项作为上传到 pub.dev 的软件包的依赖项。

### Path packages 路径软件包

Sometimes you find yourself working on multiple related packages at the same time. Maybe you are creating a framework while building an app that uses it. In those cases, during development you really want to depend on the *live* version of that package on your local file system. That way changes in one package are instantly picked up by the one that depends on it.
有时您会发现自己同时处理多个相关软件包。也许您在构建使用它的应用时正在创建一个框架。在这些情况下，在开发过程中，您真的希望依赖本地文件系统上该软件包的实时版本。这样，一个软件包中的更改会立即被依赖它的软件包所选取。

To handle that, pub supports *path dependencies*.
为了处理这种情况，pub 支持路径依赖项。

```
dependencies:
  transmogrify:
    path: /Users/me/transmogrify
```

This says the root directory for `transmogrify` is `/Users/me/transmogrify`. For this dependency, pub generates a symlink directly to the `lib` directory of the referenced package directory. Any changes you make to the dependent package are seen immediately. You don’t need to run pub every time you change the dependent package.
这表示 `transmogrify` 的根目录为 `/Users/me/transmogrify` 。对于此依赖项，pub 会直接生成一个符号链接到所引用包目录的 `lib` 目录。您对依赖包所做的任何更改都会立即显示。您无需每次更改依赖包时都运行 pub。

Relative paths are allowed and are considered relative to the directory containing your pubspec.
允许使用相对路径，并且这些路径被视为相对于包含您的 pubspec 的目录。路径依赖关系对于本地开发很有用，但在与外界共享代码时不起作用——并非每个人都能访问您的文件系统。因此，如果您的 pubspec 中有任何路径依赖关系，您就无法将其上传到 pub.dev 网站。

Path dependencies are useful for local development, but do not work when sharing code with the outside world—not everyone can get to your file system. Because of this, you cannot upload a package to the [pub.dev site](https://pub.dev/) if it has any path dependencies in its pubspec.
相反，典型的工作流程是：

Instead, the typical workflow is:

​	在本地编辑您的 pubspec 以使用路径依赖关系。

1. Edit your pubspec locally to use a path dependency.
2. 处理主包及其依赖的包。
3. Work on the main package and the package it depends on.
4. 两者都正常工作后，发布依赖包。
5. Once they’re both working, publish the dependent package.
6. 将您的 pubspec 更改为指向其依赖项的现已托管版本。
7. Change your pubspec to point to the now hosted version of its dependent.
8. 如果您愿意，也可以发布您的主包。
9. Publish your main package too, if you want.
10. SDK 源代码用于随包一起发布的任何 SDK，这些 SDK 本身可能是依赖项。目前，Flutter 是唯一受支持的 SDK。

### SDK

The SDK source is used for any SDKs that are shipped along with packages, which may themselves be dependencies. Currently, Flutter is the only SDK that is supported.

The syntax looks like this:

​	语法如下所示：

```
dependencies:
  flutter_driver:
    sdk: flutter
```

The identifier after `sdk:` indicates which SDK the package comes from. If it’s `flutter`, the dependency is satisfiable as long as:

​	`sdk:` 后面的标识符指示软件包来自哪个 SDK。如果它是 `flutter` ，只要满足以下条件，依赖关系就可以满足：

- Pub is running in the context of the `flutter` executable
- Pub 在 `flutter` 可执行文件的上下文中运行
- The Flutter SDK contains a package with the given name
- Flutter SDK 包含具有给定名称的软件包

If it’s an unknown identifier, the dependency is always considered unsatisfied.

​	如果它是未知标识符，则始终认为依赖关系不满足。

## 版本约束 Version constraints 

Let’s say that your Package A depends upon Package B. How can you communicate to other developers which version of Package B remains compatible with a given version of Package A?

​	假设您的软件包 A 依赖于软件包 B。您如何向其他开发人员传达哪个版本的软件包 B 与给定版本的软件包 A 保持兼容？

To let developers know version compatibility, specify version constraints. You want to allow the widest range of versions possible to give your package users flexibility. The range should exclude versions that don’t work or haven’t been tested.

​	要让开发人员了解版本兼容性，请指定版本约束。您希望允许最广泛的版本范围，以便为您的软件包用户提供灵活性。该范围应排除无法正常工作或尚未经过测试的版本。

The Dart community uses semantic versioning[1](https://dart.dev/tools/pub/dependencies#fn:semver).

​	Dart 社区使用语义版本控制 [1](https://dart.dev/tools/pub/dependencies#fn:semver) 。

You can express version constraints using either *traditional syntax* or *caret syntax*. Both syntaxes specify a range of compatible versions.

​	您可以使用传统语法或插入符号语法来表达版本约束。两种语法都指定了兼容版本范围。

The traditional syntax provides an explicit range like `'>=1.2.3 <2.0.0'`. The caret syntax provides an explicit starting version`^1.2.3`

​	传统语法提供一个显式范围，如 `'>=1.2.3 <2.0.0'` 。插入符号语法提供一个显式起始版本 `^1.2.3`

```
environment:
  # This package must use a 2.x version of the Dart SDK starting with 2.14.
  sdk: '>=2.14.0 < 3.0.0'

dependencies:
  transmogrify:
    hosted:
      name: transmogrify
      url: https://some-package-server.com
    # This package must use a 1.x version of transmogrify starting with 1.4.
    version: ^1.4.0
```

To learn more about pub’s version system, see the [package versioning page](https://dart.dev/tools/pub/versioning#semantic-versions).

​	要详细了解 pub 的版本系统，请参阅软件包版本控制页面。

### 传统语法 Traditional syntax 

A version constraint that uses the traditional syntax can use any of the following values:

​	使用传统语法的版本约束可以使用以下任何值：

| **Value 值** | **Allows 允许**                                            | **Use? 使用？** | **Notes 备注**                                               |
| :----------: | :--------------------------------------------------------- | :-------------: | :----------------------------------------------------------- |
|    `any`     | All versions 所有版本                                      |       No        | Serves as a explicit declaration of empty version constraint. 用作显式声明的空版本约束。 |
|   `1.2.3`    | Only the given version 仅给定版本                          |       No        | Limits adoption of your package due the additional limits it places on apps that use your package. 由于对使用您的软件包的应用施加了其他限制，因此限制了软件包的采用。 |
|  `>=1.2.3`   | Given version or later 给定版本或更高版本                  |       Yes       |                                                              |
|   `>1.2.3`   | Versions later than the given version 高于给定版本的版本   |       No        |                                                              |
|  `<=1.2.3`   | Given version or earlier 给定版本或更低版本                |       No        |                                                              |
|   `<1.2.3`   | Versions earlier than the given version 低于给定版本的版本 |       No        | Use this when you know an upper bound version that *doesn’t* work with your package. This version might be the first to introduce some breaking change. 当您知道某个上限版本无法与您的软件包配合使用时，请使用此选项。此版本可能是第一个引入一些重大更改的版本。 |

You can specify any combination of version values as their ranges intersect. For example, if you set the version value as `'>=1.2.3 <2.0.0'`, this combines the both limitations so the dependency can be any version from `1.2.3` to `2.0.0` excluding `2.0.0` itself.

​	您可以指定任何版本值组合，因为它们的范围相交。例如，如果您将版本值设置为 `'>=1.2.3 <2.0.0'` ，则会合并这两个限制，因此依赖项可以是 `1.2.3` 到 `2.0.0` 之间的任何版本，但不包括 `2.0.0` 本身。

*report_problem* **Warning:** If you include the greater than (**>**) character in the version constraint, **quote the entire constraint string**. This prevents YAML from interpreting the character as YAML syntax. For example: never use `>=1.2.3 <2.0.0`. Use `'>=1.2.3 <2.0.0'` or `^1.2.3`.

​	警告：如果您在版本约束中包含大于号 (>) 字符，请引用整个约束字符串。这可以防止 YAML 将该字符解释为 YAML 语法。例如：切勿使用 `>=1.2.3 <2.0.0` 。请使用 `'>=1.2.3 <2.0.0'` 或 `^1.2.3` 。

### 插入符号语法 Caret syntax 

Caret syntax expresses the version constraint in a compact way. `^version` means *the range of all versions guaranteed to be backwards compatible with the given version*. This range would include all versions up to the next one to introduce a breaking change. As Dart uses semantic versioning, this would be the next major version for any package version 1.0 or later or the next minor version for any package version earlier than 1.0.

​	插入符号语法以紧凑的方式表示版本约束。 `^version` 表示与给定版本向后兼容的所有版本的范围。此范围将包括所有版本，直到下一个引入重大更改的版本。由于 Dart 使用语义版本控制，对于任何 1.0 或更高版本的软件包版本，这将是下一个主要版本；对于任何低于 1.0 的软件包版本，这将是下一个次要版本。

| Version value 版本值 | Range covers to 范围涵盖  | Caret Syntax 插入符号语法 | Traditional Syntax 传统语法 |
| :------------------: | :-----------------------: | :-----------------------: | :-------------------------: |
|        >=1.0         | Next major 下一个主要版本 |         `^1.3.0`          |     `'>=1.3.0 <2.0.0'`      |
|         <1.0         | Next minor 下一个次要版本 |         `^0.1.2`          |     `'>=0.1.2 <0.2.0'`      |

The following example shows caret syntax:

​	以下示例显示插入符号语法：

```
dependencies:
  # Covers all versions from 1.3.0 to 1.y.z, not including 2.0.0
  path: ^1.3.0
  # Covers all versions from 1.1.0 to 1.y.z, not including 2.0.0
  collection: ^1.1.0
  # Covers all versions from 0.1.2 to 0.1.z, not including 0.2.0
  string_scanner: ^0.1.2
```

## 开发依赖项 Dev dependencies 

Pub supports two flavors of dependencies: regular dependencies and *dev dependencies.* Dev dependencies differ from regular dependencies in that *dev dependencies of packages you depend on are ignored*. Here’s an example:

​	Pub 支持两种依赖项：常规依赖项和开发依赖项。开发依赖项与常规依赖项的不同之处在于，您所依赖的软件包的开发依赖项会被忽略。以下是一个示例：

Say the `transmogrify` package uses the `test` package in its tests and only in its tests. If someone just wants to use `transmogrify`—import its libraries—it doesn’t actually need `test`. In this case, it specifies `test` as a dev dependency. Its pubspec will have something like:

​	假设 `transmogrify` 软件包在其测试中仅使用 `test` 软件包。如果某人只想使用 `transmogrify` （导入其库），则实际上不需要 `test` 。在这种情况下，它将 `test` 指定为开发依赖项。其 pubspec 将包含类似以下内容：

```
dev_dependencies:
  test: '>=0.5.0 <0.12.0'
```

Pub gets every package that your package depends on, and everything *those* packages depend on, transitively. It also gets your package’s dev dependencies, but it *ignores* the dev dependencies of any dependent packages. Pub only gets *your* package’s dev dependencies. So when your package depends on `transmogrify` it will get `transmogrify` but not `test`.

​	Pub 会获取您的软件包所依赖的每个软件包，以及这些软件包所依赖的所有内容（传递依赖项）。它还会获取您的软件包的开发依赖项，但会忽略任何依赖软件包的开发依赖项。Pub 仅获取您的软件包的开发依赖项。因此，当您的软件包依赖于 `transmogrify` 时，它将获取 `transmogrify` ，但不会获取 `test` 。

The rule for deciding between a regular or dev dependency is simple: If the dependency is imported from something in your `lib` or `bin` directories, it needs to be a regular dependency. If it’s only imported from `test`, `example`, etc. it can and should be a dev dependency.

​	在常规依赖项和开发依赖项之间进行决策的规则很简单：如果依赖项是从 `lib` 或 `bin` 目录中的某个内容导入的，则它需要成为常规依赖项。如果它仅从 `test` 、 `example` 等导入，则它可以且应该成为开发依赖项。

Using dev dependencies makes dependency graphs smaller. That makes `pub` run faster, and makes it easier to find a set of package versions that satisfies all constraints.

​	使用开发依赖项可减小依赖项图。这会使 `pub` 运行得更快，并且更容易找到一组满足所有约束的软件包版本。

## 依赖项覆盖 Dependency overrides 

You can use `dependency_overrides` to temporarily override all references to a dependency.

​	您可以使用 `dependency_overrides` 暂时覆盖对依赖项的所有引用。

For example, perhaps you are updating a local copy of transmogrify, a published package. Transmogrify is used by other packages in your dependency graph, but you don’t want to clone each package locally and change each pubspec to test your local copy of transmogrify.

​	例如，您可能正在更新已发布软件包 transmogrify 的本地副本。Transmogrify 由依赖项图中的其他软件包使用，但您不想克隆每个软件包到本地并更改每个 pubspec 以测试 transmogrify 的本地副本。

In this situation, you can override the dependency using `dependency_overrides` to specify the directory holding the local copy of the package.

​	在这种情况下，您可以使用 `dependency_overrides` 覆盖依赖项，以指定包含软件包本地副本的目录。

The pubspec would look something like the following:

​	pubspec 看起来应类似于以下内容：

```
name: my_app
dependencies:
  transmogrify: ^1.2.0
dependency_overrides:
  transmogrify:
    path: ../transmogrify_patch/
```

When you run [`dart pub get`](https://dart.dev/tools/pub/cmd/pub-get) or [`dart pub upgrade`](https://dart.dev/tools/pub/cmd/pub-upgrade), the pubspec’s lockfile is updated to reflect the new path to your dependency and, wherever transmogrify is used, pub uses the local version instead.

​	当您运行 `dart pub get` 或 `dart pub upgrade` 时，pubspec 的 lockfile 会更新以反映依赖项的新路径，并且无论在何处使用 transmogrify，pub 都会使用本地版本。

You can also use `dependency_overrides` to specify a particular version of a package:

​	您还可以使用 `dependency_overrides` 指定软件包的特定版本：

```
name: my_app
dependencies:
  transmogrify: ^1.2.0
dependency_overrides:
  transmogrify: '3.2.1'
```

*report_problem* **Warning:** Using a dependency override involves some risk. For example, using an override to specify a version outside the range that the package claims to support, or using an override to specify a local copy of a package that has unexpected behaviors, may break your application.

​	警告：使用依赖项覆盖会带来一些风险。例如，使用覆盖来指定超出软件包声称支持的范围的版本，或使用覆盖来指定具有意外行为的软件包的本地副本，可能会破坏您的应用程序。

Only the dependency overrides in a **package’s own pubspec** are considered during package resolution. Dependency overrides inside any depended-on packages are ignored.

​	在软件包解析期间，只会考虑软件包自己的 pubspec 中的依赖项覆盖。将忽略任何依赖软件包中的依赖项覆盖。

As a result, if you publish a package to pub.dev, keep in mind that your package’s dependency overrides are ignored by all users of your package.

​	因此，如果您将软件包发布到 pub.dev，请记住您的软件包的依赖项覆盖将被您的软件包的所有用户忽略。

## 最佳做法 Best practices 

It’s important to actively manage your dependencies and ensure that your packages use the freshest versions possible. If any dependency is stale, then you might have not only a stale version of that package, but also stale versions of other packages in your dependency graph that depend on that package. These stale versions can have a negative impact on the stability, performance, and quality of apps.

​	积极管理您的依赖项并确保您的软件包使用尽可能最新的版本非常重要。如果任何依赖项已过时，那么您可能不仅拥有该软件包的过时版本，而且还拥有依赖该软件包的依赖关系图中其他软件包的过时版本。这些过时版本可能会对应用的稳定性、性能和质量产生负面影响。

We recommend the following best practices for package dependencies.

​	我们建议您对软件包依赖项采用以下最佳做法。

### 使用插入符号语法 Use caret syntax 

Specify dependencies using the [caret syntax](https://dart.dev/tools/pub/dependencies#caret-syntax). This allows the pub tool to select newer versions of the package when they become available. Further, it places an upper bound on the allowed version.

​	使用插入符号语法指定依赖项。这允许 pub 工具在有新版本可用时选择较新的软件包版本。此外，它对允许的版本设置了上限。

### 依赖于最新的稳定软件包版本 Depend on the latest stable package versions 

Use [`dart pub upgrade`](https://dart.dev/tools/pub/cmd/pub-upgrade) to update to the latest package versions that your pubspec allows. To identify dependencies in your app or package that aren’t on the latest stable versions, use [`dart pub outdated`](https://dart.dev/tools/pub/cmd/pub-outdated).
使用 `dart pub upgrade` 更新到 pubspec 允许的最新软件包版本。要识别应用程序或软件包中不在最新稳定版本上的依赖项，请使用 `dart pub outdated` 。

### 每当更新软件包依赖项时进行测试 Test whenever you update package dependencies 

If you run [`dart pub upgrade`](https://dart.dev/tools/pub/cmd/pub-upgrade) without updating your pubspec, the API should stay the same and your code should run as before—but test to make sure. If you modify the pubspec and update to a new major version, then you might encounter breaking changes, so you need to test even more thoroughly.

​	如果您在不更新 pubspec 的情况下运行 `dart pub upgrade` ，则 API 应保持不变，并且您的代码应像以前一样运行，但请进行测试以确保如此。如果您修改 pubspec 并更新到新的主要版本，则可能会遇到重大更改，因此您需要进行更彻底的测试。

### 验证下载的软件包的完整性 Verify the integrity of downloaded packages 

When retrieving new dependencies, use the [`--enforce-lockfile`](https://dart.dev/tools/pub/cmd/pub-get#--enforce-lockfile) option to ensure the extracted package content matches the contents of the original archive. Without modifying the [lockfile](https://dart.dev/tools/pub/glossary#lockfile), this flag only resolves new dependencies if:

​	检索新依赖项时，请使用 `--enforce-lockfile` 选项以确保提取的软件包内容与原始存档的内容匹配。在不修改锁定文件的情况下，此标志仅在满足以下条件时才解析新依赖项：

- `pubspec.yaml` is satisfied
- `pubspec.yaml` 得到满足
- `pubspec.lock` is not missing
- `pubspec.lock` 不缺失
- The packages’ [content hashes](https://dart.dev/tools/pub/glossary#content-hashes) match
- 软件包的内容哈希值匹配

------

[1] Pub follows version `2.0.0-rc.1` of the [semantic versioning specification](https://semver.org/spec/v2.0.0-rc.1.html) because that version allows packages to use build identifiers (`+12345`) to differentiate versions. [↩](https://dart.dev/tools/pub/dependencies#fnref:semver)

[1] Pub 遵循语义版本控制规范的 `2.0.0-rc.1` 版本，因为该版本允许软件包使用构建标识符 ( `+12345` ) 来区分版本。↩
