+++
title = "类型定义"
date = 2024-01-05T20:29:36+08:00
weight = 40
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文: [https://dart.dev/language/typedefs](https://dart.dev/language/typedefs)

## Typedefs 类型定义

A type alias—often called a *typedef* because it’s declared with the keyword `typedef`—is a concise way to refer to a type. Here’s an example of declaring and using a type alias named `IntList`:

​	类型别名（通常称为 typedef，因为它使用关键字 `typedef` 声明）是引用类型的简洁方式。以下是如何声明和使用名为 `IntList` 的类型别名的示例：

```dart
typedef IntList = List<int>;
IntList il = [1, 2, 3];
```

A type alias can have type parameters:

​	类型别名可以具有类型参数：

```dart
typedef ListMapper<X> = Map<X, List<X>>;
Map<String, List<String>> m1 = {}; // Verbose.
ListMapper<String> m2 = {}; // Same thing but shorter and clearer.
```

*merge_type* **Version note:** Before 2.13, typedefs were restricted to function types. Using the new typedefs requires a [language version](https://dart.dev/guides/language/evolution#language-versioning) of at least 2.13.

​	版本说明：在 2.13 之前，typedef 仅限于函数类型。使用新的 typedef 需要至少为 2.13 的语言版本。

We recommend using [inline function types](https://dart.dev/effective-dart/design#prefer-inline-function-types-over-typedefs) instead of typedefs for functions, in most situations. However, function typedefs can still be useful:

​	我们建议在大多数情况下使用内联函数类型而不是函数的 typedef。但是，函数 typedef 仍然很有用：

```dart
typedef Compare<T> = int Function(T a, T b);

int sort(int a, int b) => a - b;

void main() {
  assert(sort is Compare<int>); // True!
}
```
