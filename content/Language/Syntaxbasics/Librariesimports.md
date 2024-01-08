+++
title = "库和导入"
date = 2024-01-05T20:29:36+08:00
weight = 40
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文: [https://dart.dev/language/libraries](https://dart.dev/language/libraries)

## Libraries & imports 库和导入

The `import` and `library` directives can help you create a modular and shareable code base. Libraries not only provide APIs, but are a unit of privacy: identifiers that start with an underscore (`_`) are visible only inside the library. *Every Dart file (plus its parts) is a [library](https://dart.dev/tools/pub/glossary#library)*, even if it doesn’t use a [`library`](https://dart.dev/language/libraries#library-directive) directive.

​	`import` 和 `library` 指令可以帮助您创建模块化且可共享的代码库。库不仅提供 API，而且是一个隐私单元：以下划线开头的标识符 ( `_` ) 仅在库内可见。每个 Dart 文件（及其部分）都是一个[库]({{< ref "/Packages/Packagereference/Glossary#库-library">}})，即使它不使用 `library` 指令。

Libraries can be distributed using [packages](https://dart.dev/guides/packages).

​	库可以使用[包]({{< ref "/Packages/Howtousepackages">}})进行分发。

*info* If you’re curious why Dart uses underscores instead of access modifier keywords like `public` or `private`, see [SDK issue 33383](https://github.com/dart-lang/sdk/issues/33383).

​	如果您好奇为什么 Dart 使用下划线而不是像 `public` 或 `private` 这样的访问修饰符关键字，请参阅 SDK 问题 33383。

## 使用库 Using libraries 

Use `import` to specify how a namespace from one library is used in the scope of another library.

​	使用 `import` 来指定如何在一个库的范围内使用另一个库的命名空间。

For example, Dart web apps generally use the [dart:html](https://api.dart.dev/stable/dart-html) library, which they can import like this:

​	例如，Dart Web 应用通常使用 [dart:html]({{< ref "/Corelibraries/darthtml">}}) 库，它们可以像这样导入该库：

```dart
import 'dart:html';
```

The only required argument to `import` is a URI specifying the library. For built-in libraries, the URI has the special `dart:` scheme. For other libraries, you can use a file system path or the `package:` scheme. The `package:` scheme specifies libraries provided by a package manager such as the pub tool. For example:

​	`import` 的唯一必需参数是指定库的 URI。对于内置库，URI 具有特殊的 `dart:` 方案。对于其他库，您可以使用文件系统路径或 `package:` 方案。 `package:` 方案指定由包管理器（例如 pub 工具）提供的库。例如：

```dart
import 'package:test/test.dart';
```

*info* **Note:** *URI* stands for uniform resource identifier. *URLs* (uniform resource locators) are a common kind of URI.

​	注意：URI 代表统一资源标识符。URL（统一资源定位符）是一种常见的 URI。

### 指定库前缀 Specifying a library prefix 

If you import two libraries that have conflicting identifiers, then you can specify a prefix for one or both libraries. For example, if library1 and library2 both have an Element class, then you might have code like this:

​	如果您导入的两个库具有冲突的标识符，则可以为一个或两个库指定前缀。例如，如果 library1 和 library2 都具有 Element 类，则您的代码可能如下所示：

```dart
import 'package:lib1/lib1.dart';
import 'package:lib2/lib2.dart' as lib2;

// Uses Element from lib1.
Element element1 = Element();

// Uses Element from lib2.
lib2.Element element2 = lib2.Element();
```

### 仅导入库的一部分 Importing only part of a library 

If you want to use only part of a library, you can selectively import the library. For example:

​	如果您只想使用库的一部分，则可以选择性地导入该库。例如：

```dart
// Import only foo.
import 'package:lib1/lib1.dart' show foo;

// Import all names EXCEPT foo.
import 'package:lib2/lib2.dart' hide foo;
```



#### 延迟加载库 Lazily loading a library 

*Deferred loading* (also called *lazy loading*) allows a web app to load a library on demand, if and when the library is needed. Here are some cases when you might use deferred loading:

​	延迟加载（也称为惰性加载）允许网络应用在需要时按需加载库。以下是一些您可能使用延迟加载的情况：

- To reduce a web app’s initial startup time.
- 减少网络应用的初始启动时间。
- To perform A/B testing—trying out alternative implementations of an algorithm, for example.
- 执行 A/B 测试 - 例如，尝试算法的备用实现。
- To load rarely used functionality, such as optional screens and dialogs.
- 加载很少使用的功能，例如可选屏幕和对话框。

*report_problem* **Only `dart compile js` supports deferred loading.** Flutter and the Dart VM don’t support deferred loading. To learn more, see [issue #33118](https://github.com/dart-lang/sdk/issues/33118) and [issue #27776.](https://github.com/dart-lang/sdk/issues/27776)

​	只有 `dart compile js` 支持延迟加载。Flutter 和 Dart VM 不支持延迟加载。要了解更多信息，请参阅 [issue #33118](https://github.com/dart-lang/sdk/issues/33118) 和[issue #27776.](https://github.com/dart-lang/sdk/issues/27776)。

To lazily load a library, you must first import it using `deferred as`.

​	要延迟加载库，您必须首先使用 `deferred as` 导入它。

```dart
import 'package:greetings/hello.dart' deferred as hello;
```

When you need the library, invoke `loadLibrary()` using the library’s identifier.

​	当您需要该库时，使用该库的标识符调用 `loadLibrary()` 。

```dart
Future<void> greet() async {
  await hello.loadLibrary();
  hello.printGreeting();
}
```

In the preceding code, the `await` keyword pauses execution until the library is loaded. For more information about `async` and `await`, see [asynchrony support](https://dart.dev/language/async).

​	在前面的代码中， `await` 关键字会暂停执行，直到加载该库。有关 `async` 和 `await` 的更多信息，请参阅[异步支持]({{< ref "/Language/Concurrency/Asynchronoussupport">}})。

You can invoke `loadLibrary()` multiple times on a library without problems. The library is loaded only once.

​	您可以在库上多次调用 `loadLibrary()` ，而不会出现问题。该库仅加载一次。

Keep in mind the following when you use deferred loading:

​	使用延迟加载时，请记住以下几点：

- A deferred library’s constants aren’t constants in the importing file. Remember, these constants don’t exist until the deferred library is loaded.
- 延迟加载库的常量在导入文件中不是常量。请记住，这些常量在延迟加载库加载之前不存在。
- You can’t use types from a deferred library in the importing file. Instead, consider moving interface types to a library imported by both the deferred library and the importing file.
- 您不能在导入文件中使用延迟加载库中的类型。相反，请考虑将接口类型移至延迟加载库和导入文件都导入的库。
- Dart implicitly inserts `loadLibrary()` into the namespace that you define using `deferred as namespace`. The `loadLibrary()` function returns a [`Future`](https://dart.dev/libraries/dart-async#future).
- Dart 会将 `loadLibrary()` 隐式插入您使用 `deferred as namespace` 定义的命名空间中。 `loadLibrary()` 函数返回 `Future` 。

### `library` 指令 - The `library` directive 

To specify library-level [doc comments](https://dart.dev/effective-dart/documentation#consider-writing-a-library-level-doc-comment) or [metadata annotations](https://dart.dev/language/metadata), attach them to a `library` declaration at the start of the file.

​	要指定库级[文档注释]({{< ref "/EffectiveDart/Documentation#consider-writing-a-library-level-doc-comment-考虑编写库级文档注释">}})或[元数据注释]({{< ref "/Language/Syntaxbasics/Metadata">}})，请将它们附加到文件开头处的 `library` 声明。

```dart
/// A really great test library.
@TestOn('browser')
library;
```

## 实现库 Implementing libraries 

See [Create Packages](https://dart.dev/guides/libraries/create-packages) for advice on how to implement a package, including:

​	有关如何[实现包]({{< ref "/Packages/Creatingpackages">}})的建议，请参阅创建包，包括：

- How to organize library source code.
- 如何组织库源代码。
- How to use the `export` directive.
- 如何使用 `export` 指令。
- When to use the `part` directive.
- 何时使用 `part` 指令。
- How to use conditional imports and exports to implement a library that supports multiple platforms.
- 如何使用条件导入和导出实现支持多个平台的库。
