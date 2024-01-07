+++
title = "扩展方法"
date = 2024-01-05T20:29:36+08:00
weight = 60
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文: [https://dart.dev/language/extension-methods](https://dart.dev/language/extension-methods)

## Extension methods 扩展方法

Extension methods add functionality to existing libraries. You might use extension methods without even knowing it. For example, when you use code completion in an IDE, it suggests extension methods alongside regular methods.

​	扩展方法为现有库添加功能。您可能在不知情的情况下使用了扩展方法。例如，当您在 IDE 中使用代码补全时，它会建议扩展方法和常规方法。

If watching videos helps you learn, check out this overview of extension methods.

​	如果观看视频有助于您学习，请查看此扩展方法概述。

<iframe width="560" height="315" title="Learn about extension methods in Dart" src="https://www.youtube.com/embed/D3j0OSfT9ZI" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen="" loading="lazy" style="box-sizing: border-box; color: rgb(33, 33, 33); font-family: &quot;Google Sans Text&quot;, Roboto, sans-serif; font-size: 16px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: left; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; background-color: rgb(255, 255, 255); text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"></iframe>



## 概述 Overview 

When you’re using someone else’s API or when you implement a library that’s widely used, it’s often impractical or impossible to change the API. But you might still want to add some functionality.

​	当您使用其他人的 API 或实现广泛使用的库时，更改 API 往往不切实际或不可能。但您可能仍希望添加一些功能。

For example, consider the following code that parses a string into an integer:

​	例如，考虑将字符串解析为整数的以下代码：

```
int.parse('42')
```

It might be nice—shorter and easier to use with tools—to have that functionality be on `String` instead:

​	最好是让该功能在 `String` 上，以便更短且更易于使用工具：

```
'42'.parseInt()
```

To enable that code, you can import a library that contains an extension of the `String` class:

​	要启用该代码，您可以导入包含 `String` 类的扩展的库：

```dart
import 'string_apis.dart';
// ···
print('42'.parseInt()); // Use an extension method.
```

Extensions can define not just methods, but also other members such as getter, setters, and operators. Also, extensions can have names, which can be helpful if an API conflict arises. Here’s how you might implement the extension method `parseInt()`, using an extension (named `NumberParsing`) that operates on strings:

​	扩展不仅可以定义方法，还可以定义其他成员，例如 getter、setter 和运算符。此外，扩展可以有名称，如果出现 API 冲突，这会很有用。以下是如何使用扩展（名为 `NumberParsing` ）在字符串上运行来实现扩展方法 `parseInt()` ：

```dart
extension NumberParsing on String {
  int parseInt() {
    return int.parse(this);
  }
  // ···
}
```

lib/string_apis.dart

The next section describes how to *use* extension methods. After that are sections about *implementing* extension methods.

​	下一部分介绍如何使用扩展方法。之后的部分介绍如何实现扩展方法。

## 使用扩展方法 Using extension methods 

Like all Dart code, extension methods are in libraries. You’ve already seen how to use an extension method—just import the library it’s in, and use it like an ordinary method:

​	与所有 Dart 代码一样，扩展方法位于库中。您已经了解如何使用扩展方法——只需导入它所在的库，然后像普通方法一样使用它：

```dart
// Import a library that contains an extension on String.
import 'string_apis.dart';
// ···
print('42'.padLeft(5)); // Use a String method.
print('42'.parseInt()); // Use an extension method.
```

That’s all you usually need to know to use extension methods. As you write your code, you might also need to know how extension methods depend on static types (as opposed to `dynamic`) and how to resolve [API conflicts](https://dart.dev/language/extension-methods#api-conflicts).

​	通常，您只需要知道这些内容即可使用扩展方法。在编写代码时，您可能还需要了解扩展方法如何依赖静态类型（与 `dynamic` 相反）以及如何解决 API 冲突。

### 静态类型和动态 Static types and dynamic 

You can’t invoke extension methods on variables of type `dynamic`. For example, the following code results in a runtime exception:
您无法对类型为 `dynamic` 的变量调用扩展方法。例如，以下代码会导致运行时异常：

```dart
dynamic d = '2';
print(d.parseInt()); // Runtime exception: NoSuchMethodError
```

Extension methods *do* work with Dart’s type inference. The following code is fine because the variable `v` is inferred to have type `String`:

​	扩展方法确实适用于 Dart 的类型推断。以下代码是正确的，因为变量 `v` 被推断为具有类型 `String` ：

```dart
var v = '2';
print(v.parseInt()); // Output: 2
```

The reason that `dynamic` doesn’t work is that extension methods are resolved against the static type of the receiver. Because extension methods are resolved statically, they’re as fast as calling a static function.

​	之所以 `dynamic` 不起作用，是因为扩展方法是针对接收者的静态类型解析的。由于扩展方法是静态解析的，因此它们与调用静态函数一样快。

For more information about static types and `dynamic`, see [The Dart type system](https://dart.dev/language/type-system).

​	有关静态类型和 `dynamic` 的更多信息，请参阅 Dart 类型系统。

### API 冲突 API conflicts 

If an extension member conflicts with an interface or with another extension member, then you have a few options.

​	如果扩展成员与接口或其他扩展成员冲突，那么您有几个选择。

One option is changing how you import the conflicting extension, using `show` or `hide` to limit the exposed API:

​	一种选择是更改导入冲突扩展的方式，使用 `show` 或 `hide` 来限制公开的 API：

```dart
// Defines the String extension method parseInt().
import 'string_apis.dart';

// Also defines parseInt(), but hiding NumberParsing2
// hides that extension method.
import 'string_apis_2.dart' hide NumberParsing2;

// ···
// Uses the parseInt() defined in 'string_apis.dart'.
print('42'.parseInt());
```

Another option is applying the extension explicitly, which results in code that looks as if the extension is a wrapper class:

​	另一种选择是显式应用扩展，这会导致代码看起来像是扩展是一个包装类：

```dart
// Both libraries define extensions on String that contain parseInt(),
// and the extensions have different names.
import 'string_apis.dart'; // Contains NumberParsing extension.
import 'string_apis_2.dart'; // Contains NumberParsing2 extension.

// ···
// print('42'.parseInt()); // Doesn't work.
print(NumberParsing('42').parseInt());
print(NumberParsing2('42').parseInt());
```

If both extensions have the same name, then you might need to import using a prefix:

​	如果两个扩展具有相同的名称，则可能需要使用前缀导入：

```dart
// Both libraries define extensions named NumberParsing
// that contain the extension method parseInt(). One NumberParsing
// extension (in 'string_apis_3.dart') also defines parseNum().
import 'string_apis.dart';
import 'string_apis_3.dart' as rad;

// ···
// print('42'.parseInt()); // Doesn't work.

// Use the ParseNumbers extension from string_apis.dart.
print(NumberParsing('42').parseInt());

// Use the ParseNumbers extension from string_apis_3.dart.
print(rad.NumberParsing('42').parseInt());

// Only string_apis_3.dart has parseNum().
print('42'.parseNum());
```

As the example shows, you can invoke extension methods implicitly even if you import using a prefix. The only time you need to use the prefix is to avoid a name conflict when invoking an extension explicitly.

​	如示例所示，即使使用前缀导入，也可以隐式调用扩展方法。唯一需要使用前缀的情况是，在显式调用扩展时避免名称冲突。

## 实现扩展方法 Implementing extension methods 

Use the following syntax to create an extension:

​	使用以下语法创建扩展：

```
extension <extension name>? on <type> {
  (<member definition>)*
}
```

For example, here’s how you might implement an extension on the `String` class:

​	例如，以下是如何在 `String` 类上实现扩展：

```dart
extension NumberParsing on String {
  int parseInt() {
    return int.parse(this);
  }

  double parseDouble() {
    return double.parse(this);
  }
}
```

lib/string_apis.dart

The members of an extension can be methods, getters, setters, or operators. Extensions can also have static fields and static helper methods. To access static members outside the extension declaration, invoke them through the declaration name like [class variables and methods](https://dart.dev/language/classes#class-variables-and-methods).

​	扩展的成员可以是方法、getter、setter 或运算符。扩展还可以具有静态字段和静态帮助程序方法。要在扩展声明之外访问静态成员，请通过声明名称调用它们，就像类变量和方法一样。

### 未命名扩展 Unnamed extensions 

When declaring an extension, you can omit the name. Unnamed extensions are visible only in the library where they’re declared. Since they don’t have a name, they can’t be explicitly applied to resolve [API conflicts](https://dart.dev/language/extension-methods#api-conflicts).

​	声明扩展时，您可以省略名称。未命名的扩展仅在声明它们的库中可见。由于它们没有名称，因此无法明确应用它们来解决 API 冲突。

```dart
extension on String {
  bool get isBlank => trim().isEmpty;
}
```

*info* **Note:** You can invoke an unnamed extension’s static members only within the extension declaration.

​	注意：您只能在扩展声明中调用未命名扩展的静态成员。

## 实现泛型扩展 Implementing generic extensions 

Extensions can have generic type parameters. For example, here’s some code that extends the built-in `List<T>` type with a getter, an operator, and a method:

​	扩展可以具有泛型类型参数。例如，以下是一些使用 getter、运算符和方法扩展内置 `List<T>` 类型的代码：

```dart
extension MyFancyList<T> on List<T> {
  int get doubleLength => length * 2;
  List<T> operator -() => reversed.toList();
  List<List<T>> split(int at) => [sublist(0, at), sublist(at)];
}
```

The type `T` is bound based on the static type of the list that the methods are called on.

​	类型 `T` 基于方法调用的列表的静态类型进行绑定。

## 资源 Resources 

For more information about extension methods, see the following:

​	有关扩展方法的更多信息，请参阅以下内容：

- [Article: Dart Extension Methods Fundamentals
  文章：Dart 扩展方法基础](https://medium.com/dartlang/extension-methods-2d466cd8b308)
- [Feature specification
  功能规范](https://github.com/dart-lang/language/blob/main/accepted/2.7/static-extension-methods/feature-specification.md#dart-static-extension-methods-design)
- [Extension methods sample
  扩展方法示例](https://github.com/dart-lang/samples/tree/main/extension_methods)
