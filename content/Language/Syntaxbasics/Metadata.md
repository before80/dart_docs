+++
title = "Metadata"
date = 2024-01-05T20:29:36+08:00
weight = 30
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文: [https://dart.dev/language/metadata](https://dart.dev/language/metadata)

# Metadata 元数据

Use metadata to give additional information about your code. A metadata annotation begins with the character `@`, followed by either a reference to a compile-time constant (such as `deprecated`) or a call to a constant constructor.
使用元数据提供有关代码的其他信息。元数据注释以字符 `@` 开头，后跟对编译时常量的引用（例如 `deprecated` ）或对常量构造函数的调用。

Four annotations are available to all Dart code: [`@Deprecated`](https://api.dart.dev/stable/dart-core/deprecated-constant.html), [`@deprecated`](https://api.dart.dev/stable/dart-core/deprecated-constant.html), [`@override`](https://api.dart.dev/stable/dart-core/override-constant.html), and [`@pragma`](https://api.dart.dev/stable/dart-core/pragma-class.html). For examples of using `@override`, see [Extending a class](https://dart.dev/language/extend). Here’s an example of using the `@Deprecated` annotation:
所有 Dart 代码都可以使用四种注释： `@Deprecated` 、 `@deprecated` 、 `@override` 和 `@pragma` 。有关使用 `@override` 的示例，请参阅扩展类。以下是如何使用 `@Deprecated` 注释的示例：

```dart
class Television {
  /// Use [turnOn] to turn the power on instead.
  @Deprecated('Use turnOn instead')
  void activate() {
    turnOn();
  }

  /// Turns the TV's power on.
  void turnOn() {...}
  // ···
}
```

You can use `@deprecated` if you don’t want to specify a message. However, we [recommend](https://dart.dev/tools/linter-rules/provide_deprecation_message) always specifying a message with `@Deprecated`.
如果您不想指定消息，可以使用 `@deprecated` 。但是，我们建议始终使用 `@Deprecated` 指定消息。

You can define your own metadata annotations. Here’s an example of defining a `@Todo` annotation that takes two arguments:
您可以定义自己的元数据注释。以下是如何定义一个 `@Todo` 注释（它采用两个参数）的示例：

```dart
class Todo {
  final String who;
  final String what;

  const Todo(this.who, this.what);
}
```

And here’s an example of using that `@Todo` annotation:
以下是如何使用该 `@Todo` 注释的示例：

```dart
@Todo('Dash', 'Implement this function')
void doSomething() {
  print('Do something');
}
```

Metadata can appear before a library, class, typedef, type parameter, constructor, factory, function, field, parameter, or variable declaration and before an import or export directive.
元数据可以出现在库、类、typedef、类型参数、构造函数、工厂、函数、字段、参数或变量声明之前，以及 import 或 export 指令之前。
