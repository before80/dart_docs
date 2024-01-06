+++
title = "Glossary"
date = 2024-01-05T20:29:36+08:00
weight = 10
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文: [https://dart.dev/tools/pub/glossary](https://dart.dev/tools/pub/glossary)

# Glossary of package terms 软件包术语表

The following terms are used in the documentation for [package management](https://dart.dev/guides/packages) and the [pub tool](https://dart.dev/tools/pub/cmd).
以下术语用于软件包管理和 pub 工具的文档中。

## Application package 应用程序包

A package that contains a program or app, with a [main entrypoint](https://dart.dev/tools/pub/glossary#entrypoint). Meant to be run directly, either on the command line or in a browser.
包含程序或应用的包，具有一个主入口点。旨在直接运行，无论是在命令行还是在浏览器中。

Application packages may have [dependencies](https://dart.dev/tools/pub/glossary#dependency) on other packages, but are never depended on themselves. Unlike regular [packages](https://dart.dev/tools/pub/glossary#package), they are not intended to be shared.
应用程序包可能依赖于其他包，但绝不会依赖于自身。与常规包不同，它们并不旨在共享。

Application packages should check their [lockfiles](https://dart.dev/tools/pub/glossary#lockfile) into source control, so that everyone working on the application and every location the application is deployed has a consistent set of dependencies. Because their dependencies are constrained by the lockfile, application packages usually specify `any` for their dependencies’ [version constraints](https://dart.dev/tools/pub/glossary#version-constraint).
应用程序包应将其锁定文件检入源代码管理，以便所有处理应用程序的人员和应用程序部署的每个位置都具有一组一致的依赖项。由于其依赖项受锁定文件约束，因此应用程序包通常为其依赖项的版本约束指定 `any` 。

## Content hashes 内容哈希

The pub.dev repository maintains a sha256 hash of each package version it hosts. Pub clients can use this hash to validate the integrity of downloaded packages, and protect against changes on the repository.
pub.dev 存储库维护其托管的每个软件包版本的 sha256 哈希。Pub 客户端可以使用此哈希来验证下载的软件包的完整性，并防止存储库上的更改。

When `dart pub get` downloads a package, it computes the hash of the downloaded archive. The hash of each hosted dependency is stored with the [resolution](https://dart.dev/tools/pub/cmd/pub-get) in the [lockfile](https://dart.dev/tools/pub/glossary#lockfile).
当 `dart pub get` 下载软件包时，它会计算下载的存档的哈希值。每个托管依赖项的哈希值与锁定文件中的解析一起存储。

The pub client uses this content hash to verify that running `dart pub get` again using the same lockfile, potentially on a different computer, uses exactly the same packages.
pub 客户端使用此内容哈希值来验证再次使用相同锁定文件（可能在不同的计算机上）运行 `dart pub get` 时，使用完全相同的软件包。

If the locked hash doesn’t match what’s currently in the pub cache, pub redownloads the archive. If it still doesn’t match, the lockfile updates and a warning is printed. For example:
如果锁定的哈希值与 pub 缓存中的当前哈希值不匹配，pub 会重新下载存档。如果仍然不匹配，则锁定文件会更新并打印警告。例如：

```
$ dart pub get
Resolving dependencies...
Cached version of foo-1.0.0 has wrong hash - redownloading.
 ~ foo 1.0.0 (was 1.0.0)
The existing content-hash from pubspec.lock doesn't match contents for:
 * foo-1.0.0 from "pub.dev"
This indicates one of:
 * The content has changed on the server since you created the pubspec.lock.
 * The pubspec.lock has been corrupted.
 
The content-hashes in pubspec.lock has been updated.

For more information see:
https://dart.dev/go/content-hashes

Changed 1 dependency!
```

The updated content hash will show up in your version control diff, and should make you suspicious.
更新的内容哈希值会显示在您的版本控制差异中，并应引起您的怀疑。

To make a discrepancy become an error instead of a warning, use [`dart pub get --enforce-lockfile`](https://dart.dev/tools/pub/cmd/pub-get#--enforce-lockfile). It will cause the resolution to fail if it cannot find package archives with the same hashes, without updating the lockfile.
要使差异成为错误而不是警告，请使用 `dart pub get --enforce-lockfile` 。如果它找不到具有相同哈希值的软件包存档，它将导致解析失败，而不会更新锁定文件。

```
$ dart pub get --enforce-lockfile
Resolving dependencies...
Cached version of foo-1.0.0 has wrong hash - redownloading.
~ foo 1.0.0 (was 1.0.0)
The existing content-hash from pubspec.lock doesn't match contents for:
 * foo-1.0.0 from "pub.dev"

This indicates one of:
 * The content has changed on the server since you created the pubspec.lock.
 * The pubspec.lock has been corrupted.

For more information see:
https://dart.dev/go/content-hashes
Would change 1 dependency.
Unable to satisfy `pubspec.yaml` using `pubspec.lock`.

To update `pubspec.lock` run `dart pub get` without
`--enforce-lockfile`.
```

## Dependency 依赖项

Another package that your package relies on. If your package wants to import code from some other package, that package must be a dependency. Dependencies are specified in your package’s [pubspec](https://dart.dev/tools/pub/pubspec) and described in [Package dependencies](https://dart.dev/tools/pub/dependencies).
您的软件包依赖的另一个软件包。如果您的软件包想要从某个其他软件包导入代码，则该软件包必须是依赖项。依赖项在软件包的 pubspec 中指定，并在软件包依赖项中描述。

To see the dependencies used by a package, use [`pub deps`](https://dart.dev/tools/pub/cmd/pub-deps).
要查看软件包使用的依赖项，请使用 `pub deps` 。

## Entrypoint 入口点

In the general context of Dart, an *entrypoint* is a Dart library that is directly invoked by a Dart implementation. When you reference a Dart library in a `<script>` tag or pass it as a command-line argument to the standalone Dart VM, that library is the entrypoint. In other words, it’s usually the `.dart` file that contains `main()`.
在 Dart 的一般上下文中，入口点是直接由 Dart 实现调用的 Dart 库。当您在 `<script>` 标签中引用 Dart 库或将其作为命令行参数传递给独立的 Dart VM 时，该库就是入口点。换句话说，它通常是包含 `main()` 的 `.dart` 文件。

In the context of pub, an *entrypoint package* or *root package* is the root of a dependency graph. It will usually be an application. When you run your app, it’s the entrypoint package. Every other package it depends on will not be an entrypoint in that context.
在 pub 的上下文中，入口点包或根包是依赖关系图的根。它通常是一个应用程序。当您运行应用程序时，它是入口点包。它所依赖的其他每个包在该上下文中都不是入口点。

A package can be an entrypoint in some contexts and not in others. Say your app uses a package `A`. When you run your app, `A` is not the entrypoint package. However, if you go over to `A` and execute its tests, in that context, it *is* the entrypoint since your app isn’t involved.
一个包在某些上下文中可以是入口点，而在其他上下文中则不是。假设您的应用程序使用包 `A` 。当您运行应用程序时， `A` 不是入口点包。但是，如果您转到 `A` 并执行其测试，在这种情况下，它是入口点，因为您的应用程序没有参与。

## Entrypoint directory 入口点目录

A directory inside your package that is allowed to contain [Dart entrypoints](https://dart.dev/tools/pub/glossary#entrypoint).
包中允许包含 Dart 入口点的目录。

Pub has a list of these directories: `benchmark`, `bin`, `example`, `test`, `tool`, and `web` (and `lib`, for [Flutter apps](https://docs.flutter.dev/packages-and-plugins/developing-packages)). Any subdirectories of those (except `bin`) may also contain entrypoints.
Pub 拥有以下目录的列表： `benchmark` 、 `bin` 、 `example` 、 `test` 、 `tool` 和 `web` （以及 `lib` ，适用于 Flutter 应用）。这些目录的任何子目录（ `bin` 除外）也可能包含入口点。

## Immediate dependency 直接依赖项

A [dependency](https://dart.dev/tools/pub/glossary#dependency) that your package directly uses itself. The dependencies you list in your pubspec are your package’s immediate dependencies. All other dependencies are [transitive dependencies](https://dart.dev/tools/pub/glossary#transitive-dependency).
您的软件包直接使用的依赖项。您在 pubspec 中列出的依赖项是您的软件包的直接依赖项。所有其他依赖项都是传递依赖项。

## Library 库

A library is a single compilation unit, made up of a single primary file and any optional number of [parts](https://dart.dev/resources/glossary#part-file). Libraries have their own private scope.
库是一个单一编译单元，由一个主文件和任意数量的部件组成。库有自己的私有范围。

## Lockfile 锁定文件

A file named `pubspec.lock` that specifies the concrete versions and other identifying information for every immediate and transitive dependency a package relies on.
一个名为 `pubspec.lock` 的文件，其中指定了软件包所依赖的每个直接和传递依赖项的具体版本和其他标识信息。

Unlike the pubspec, which only lists immediate dependencies and allows version ranges, the lockfile comprehensively pins down the entire dependency graph to specific versions of packages. A lockfile ensures that you can recreate the exact configuration of packages used by an application.
与仅列出直接依赖项并允许版本范围的 pubspec 不同，锁定文件将整个依赖项图全面固定到特定版本的软件包。锁定文件可确保您可以重新创建应用程序使用的软件包的确切配置。

The lockfile is generated automatically for you by pub when you run [`pub get`](https://dart.dev/tools/pub/cmd/pub-get), [`pub upgrade`](https://dart.dev/tools/pub/cmd/pub-upgrade), or [`pub downgrade`](https://dart.dev/tools/pub/cmd/pub-downgrade). Pub includes a [content hash](https://dart.dev/tools/pub/glossary#content-hashes) for each package to check against during future resolutions.
当您运行 `pub get` 、 `pub upgrade` 或 `pub downgrade` 时，锁定文件会由 pub 自动为您生成。Pub 为每个软件包包含一个内容哈希，以便在将来的解析过程中进行检查。

If your package is an [application package](https://dart.dev/tools/pub/glossary#application-package), you will typically check this into source control. For regular packages, you usually won’t.
如果您的软件包是应用程序软件包，您通常会将其检入源代码管理。对于常规软件包，您通常不会这样做。



## Package 包

A collection of [libraries](https://dart.dev/tools/pub/glossary#library) under a directory, with a [pubspec.yaml](https://dart.dev/tools/pub/pubspec) in the root of that directory.
一个目录下的库集合，该目录的根目录中有一个 pubspec.yaml。

Packages can have [dependencies](https://dart.dev/tools/pub/glossary#dependency) on other packages *and* can be dependencies themselves. A package’s `/lib` directory contains the [public libraries](https://dart.dev/tools/pub/package-layout#public-libraries) that other packages can import and use. They can also include scripts to be run directly. A package that is not intended to be depended on by other packages is an [application package](https://dart.dev/tools/pub/glossary#application-package). Shared packages are [published](https://dart.dev/tools/pub/publishing) to pub.dev, but you can also have non-published packages.
软件包可以依赖其他软件包，也可以自己成为依赖项。软件包的 `/lib` 目录包含其他软件包可以导入和使用的公共库。它们还可以包括要直接运行的脚本。不打算被其他软件包依赖的软件包是应用程序软件包。共享软件包发布到 pub.dev，但您也可以拥有未发布的软件包。

Don’t check the [lockfile](https://dart.dev/tools/pub/glossary#lockfile) of a package into source control, since libraries should support a range of dependency versions. The [version constraints](https://dart.dev/tools/pub/glossary#version-constraint) of a package’s [immediate dependencies](https://dart.dev/tools/pub/glossary#immediate-dependency) should be as wide as possible while still ensuring that the dependencies will be compatible with the versions that were tested against.
不要将软件包的锁定文件检入源代码管理，因为库应该支持一系列依赖项版本。软件包的直接依赖项的版本约束应尽可能宽松，同时仍确保依赖项与经过测试的版本兼容。

Since [semantic versioning](https://semver.org/spec/v2.0.0-rc.1.html) requires that libraries increment their major version numbers for any backwards incompatible changes, packages will usually require their dependencies’ versions to be greater than or equal to the versions that were tested and less than the next major version. So if your library depended on the (fictional) `transmogrify` package and you tested it at version 1.2.1, your version constraint would be [`^1.2.1`](https://dart.dev/tools/pub/dependencies#caret-syntax).
由于语义版本控制要求库为任何向后不兼容的更改增加其主要版本号，因此软件包通常要求其依赖项的版本大于或等于已测试的版本，且小于下一个主要版本。因此，如果您的库依赖于（虚构的） `transmogrify` 软件包，并且您在版本 1.2.1 中对其进行了测试，那么您的版本约束将是 `^1.2.1` 。

## SDK constraint SDK 约束

The declared versions of the Dart SDK itself that a package declares that it supports. An SDK constraint is specified using normal [version constraint](https://dart.dev/tools/pub/glossary#version-constraint) syntax, but in a special *environment* section [in the pubspec](https://dart.dev/tools/pub/pubspec#sdk-constraints).
软件包声明其支持的 Dart SDK 本身的已声明版本。SDK 约束使用常规版本约束语法指定，但位于 pubspec 中的特殊环境部分中。

## Source 源

A kind of place that pub can get packages from. A source isn’t a specific place like the pub.dev site or some specific Git URL. Each source describes a general procedure for accessing a package in some way. For example, *git* is one source. The git source knows how to download packages given a Git URL. Several different [supported sources](https://dart.dev/tools/pub/dependencies#dependency-sources) are available.
pub 可以从中获取软件包的一种位置。源不是像 pub.dev 网站或某个特定的 Git URL 那样的特定位置。每个源都描述了一种以某种方式访问软件包的常规过程。例如，git 就是一个源。git 源知道如何根据 Git URL 下载软件包。有几个不同的受支持源可用。

## System cache 系统缓存

When pub gets a remote package, it downloads it into a single *system cache* directory maintained by pub. On Mac and Linux, this directory defaults to `~/.pub-cache`. On Windows, the directory defaults to `%LOCALAPPDATA%\Pub\Cache`, though its exact location may vary depending on the Windows version. You can specify a different location using the [PUB_CACHE](https://dart.dev/tools/pub/environment-variables) environment variable.
当 pub 获取远程软件包时，它会将其下载到由 pub 维护的单个系统缓存目录中。在 Mac 和 Linux 上，此目录默认为 `~/.pub-cache` 。在 Windows 上，此目录默认为 `%LOCALAPPDATA%\Pub\Cache` ，但其确切位置可能因 Windows 版本而异。您可以使用 PUB_CACHE 环境变量指定其他位置。

Once packages are in the system cache, pub creates a `package_config.json` file that maps each package used by your application to the corresponding package in the cache.
软件包进入系统缓存后，pub 会创建一个 `package_config.json` 文件，将应用程序使用的每个软件包映射到缓存中的相应软件包。

You only have to download a given version of a package once and can then reuse it in as many packages as you would like. If you specify the `--offline` flag to use cached packages, you can delete and regenerate your `package_config.json` files without having to access the network.
您只需下载某个软件包的某个版本一次，然后便可以在任意数量的软件包中重复使用它。如果您指定 `--offline` 标志以使用缓存的软件包，则可以删除并重新生成 `package_config.json` 文件，而无需访问网络。

## Transitive dependency 传递依赖项

A dependency that your package indirectly uses because one of its dependencies requires it. If your package depends on A, which in turn depends on B which depends on C, then A is an [immediate dependency](https://dart.dev/tools/pub/glossary#immediate-dependency) and B and C are transitive ones.
您的软件包间接使用的依赖项，因为它的某个依赖项需要它。如果您的软件包依赖于 A，而 A 又依赖于 B，B 又依赖于 C，那么 A 是直接依赖项，而 B 和 C 是传递依赖项。

## Uploader 上传者

Someone who has administrative permissions for a package. A package uploader can upload new versions of the package, and they can also [add and remove other uploaders](https://dart.dev/tools/pub/publishing#uploaders) for that package.
拥有软件包管理权限的人员。软件包上传者可以上传软件包的新版本，还可以为该软件包添加和移除其他上传者。

If a package has a verified publisher, then all members of the publisher can upload the package.
如果软件包有经过验证的发布者，那么该发布者所有成员都可以上传该软件包。

## Verified publisher 经过验证的发布者

One or more users who own a set of packages. Each verified publisher is identified by a verified domain name, such as **dart.dev**. For general information about verified publishers, see the [verified publishers page](https://dart.dev/tools/pub/verified-publishers). For details on creating a verified publisher and transferring packages to it, see the documentation for [publishing packages](https://dart.dev/tools/pub/publishing#verified-publisher).
拥有多组软件包的一个或多个用户。每个经过验证的发布者都由经过验证的域名标识，例如 dart.dev。有关经过验证的发布者的常规信息，请参阅经过验证的发布者页面。有关创建经过验证的发布者并向其中转移软件包的详细信息，请参阅发布软件包的文档。

## Version constraint 版本约束

A constraint placed on each [dependency](https://dart.dev/tools/pub/glossary#dependency) of a package that specifies which versions of that dependency the package is expected to work with. This can be a single version (`0.3.0`) or a range of versions (`^1.2.1`). While `any` is also allowed, for performance reasons we don’t recommend it.
对软件包的每个依赖项施加的约束，指定该软件包预期与该依赖项的哪些版本配合使用。这可以是单个版本 ( `0.3.0` ) 或一系列版本 ( `^1.2.1` )。虽然 `any` 也允许，但出于性能原因，我们不建议使用它。

For more information, see [Version constraints](https://dart.dev/tools/pub/dependencies#version-constraints).
有关更多信息，请参阅版本约束。

[Packages](https://dart.dev/tools/pub/glossary#package) should always specify version constraints for all of their dependencies. [Application packages](https://dart.dev/tools/pub/glossary#application-package), on the other hand, should usually allow any version of their dependencies, since they use the [lockfile](https://dart.dev/tools/pub/glossary#lockfile) to manage their dependency versions.
软件包应始终为其所有依赖项指定版本约束。另一方面，应用程序软件包通常应允许其依赖项的任何版本，因为它们使用锁定文件来管理其依赖项版本。

For more information, see [Pub Versioning Philosophy](https://dart.dev/tools/pub/versioning).
有关更多信息，请参阅 Pub 版本控制理念。
