+++
title = "注释"
date = 2024-01-05T20:29:36+08:00
weight = 20
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文: [https://dart.dev/language/comments](https://dart.dev/language/comments)

## Comments 注释

Dart supports single-line comments, multi-line comments, and documentation comments.

​	Dart 支持单行注释、多行注释和文档注释。

## 单行注释 Single-line comments 

A single-line comment begins with `//`. Everything between `//` and the end of line is ignored by the Dart compiler.

​	单行注释以 `//` 开头。Dart 编译器会忽略 `//` 和行尾之间的所有内容。

```dart
void main() {
  // TODO: refactor into an AbstractLlamaGreetingFactory?
  print('Welcome to my Llama farm!');
}
```

## 多行注释 Multi-line comments 

A multi-line comment begins with `/*` and ends with `*/`. Everything between `/*` and `*/` is ignored by the Dart compiler (unless the comment is a documentation comment; see the next section). Multi-line comments can nest.

​	多行注释以 `/*` 开头，以 `*/` 结尾。Dart 编译器会忽略 `/*` 和 `*/` 之间的所有内容（除非注释是文档注释；请参阅下一部分）。多行注释可以嵌套。

```dart
void main() {
  /*
   * This is a lot of work. Consider raising chickens.

  Llama larry = Llama();
  larry.feed();
  larry.exercise();
  larry.clean();
   */
}
```

## 文档注释 Documentation comments 

Documentation comments are multi-line or single-line comments that begin with `///` or `/**`. Using `///` on consecutive lines has the same effect as a multi-line doc comment.

​	文档注释是多行或单行注释，以 `///` 或 `/**` 开头。在连续行上使用 `///` 的效果与多行文档注释相同。

Inside a documentation comment, the analyzer ignores all text unless it is enclosed in brackets. Using brackets, you can refer to classes, methods, fields, top-level variables, functions, and parameters. The names in brackets are resolved in the lexical scope of the documented program element.

​	在文档注释中，分析器会忽略所有文本，除非它用方括号括起来。使用方括号，您可以引用类、方法、字段、顶级变量、函数和参数。方括号中的名称在已记录程序元素的词法范围内解析。

Here is an example of documentation comments with references to other classes and arguments:

​	下面是包含对其他类和参数的引用的文档注释示例：

```dart
/// A domesticated South American camelid (Lama glama).
///
/// Andean cultures have used llamas as meat and pack
/// animals since pre-Hispanic times.
///
/// Just like any other animal, llamas need to eat,
/// so don't forget to [feed] them some [Food].
class Llama {
  String? name;

  /// Feeds your llama [food].
  ///
  /// The typical llama eats one bale of hay per week.
  void feed(Food food) {
    // ...
  }

  /// Exercises your llama with an [activity] for
  /// [timeLimit] minutes.
  void exercise(Activity activity, int timeLimit) {
    // ...
  }
}
```

In the class’s generated documentation, `[feed]` becomes a link to the docs for the `feed` method, and `[Food]` becomes a link to the docs for the `Food` class.

​	在类的生成文档中， `[feed]` 变为指向 `feed` 方法的文档的链接， `[Food]` 变为指向 `Food` 类的文档的链接。

To parse Dart code and generate HTML documentation, you can use Dart’s documentation generation tool, [`dart doc`](https://dart.dev/tools/dart-doc). For an example of generated documentation, see the [Dart API documentation.](https://api.dart.dev/stable) For advice on how to structure your comments, see [Effective Dart: Documentation.](https://dart.dev/effective-dart/documentation)

​	要解析 Dart 代码并生成 HTML 文档，您可以使用 Dart 的文档生成工具 `dart doc` 。有关生成文档的示例，请参阅 Dart API 文档。有关如何构建注释的建议，请参阅有效的 Dart：文档。
