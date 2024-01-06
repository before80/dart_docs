+++
title = "Generics"
date = 2024-01-05T20:29:36+08:00
weight = 30
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文: [https://dart.dev/language/generics](https://dart.dev/language/generics)

# Generics 为什么使用泛型？

If you look at the API documentation for the basic array type, [`List`](https://api.dart.dev/stable/dart-core/List-class.html), you’ll see that the type is actually `List<E>`. The <…> notation marks List as a *generic* (or *parameterized*) type—a type that has formal type parameters. [By convention](https://dart.dev/effective-dart/design#do-follow-existing-mnemonic-conventions-when-naming-type-parameters), most type variables have single-letter names, such as E, T, S, K, and V.
如果您查看基本数组类型 `List` 的 API 文档，您会看到该类型实际上是 `List<E>` 。<…> 符号将 List 标记为泛型（或参数化）类型，即具有形式类型参数的类型。根据惯例，大多数类型变量都具有单字母名称，例如 E、T、S、K 和 V。

## Why use generics? 为什么要使用泛型？

Generics are often required for type safety, but they have more benefits than just allowing your code to run:
泛型通常是类型安全所必需的，但它们的好处不仅仅在于允许您的代码运行：

- Properly specifying generic types results in better generated code.
  正确指定泛型类型可生成更好的代码。
- You can use generics to reduce code duplication.
  您可以使用泛型来减少代码重复。

If you intend for a list to contain only strings, you can declare it as `List<String>` (read that as “list of string”). That way you, your fellow programmers, and your tools can detect that assigning a non-string to the list is probably a mistake. Here’s an example:
如果您打算让列表仅包含字符串，则可以将其声明为 `List<String>` （将其读作“字符串列表”）。这样，您、您的程序员同事和您的工具可以检测到将非字符串分配给列表可能是一个错误。以下是一个示例：

```
var names = <String>[];
names.addAll(['Seth', 'Kathy', 'Lars']);
names.add(42); // Error
```

Another reason for using generics is to reduce code duplication. Generics let you share a single interface and implementation between many types, while still taking advantage of static analysis. For example, say you create an interface for caching an object:
使用泛型的另一个原因是减少代码重复。泛型允许您在许多类型之间共享单个接口和实现，同时仍然利用静态分析。例如，假设您创建了一个用于缓存对象的接口：

```dart
abstract class ObjectCache {
  Object getByKey(String key);
  void setByKey(String key, Object value);
}
```

You discover that you want a string-specific version of this interface, so you create another interface:
您发现您想要此接口的字符串特定版本，因此您创建了另一个接口：

```dart
abstract class StringCache {
  String getByKey(String key);
  void setByKey(String key, String value);
}
```

Later, you decide you want a number-specific version of this interface… You get the idea.
稍后，您决定想要此接口的数字特定版本……您明白了。

Generic types can save you the trouble of creating all these interfaces. Instead, you can create a single interface that takes a type parameter:
泛型类型可以省去您创建所有这些接口的麻烦。相反，您可以创建一个采用类型参数的单个接口：

```dart
abstract class Cache<T> {
  T getByKey(String key);
  void setByKey(String key, T value);
}
```

In this code, T is the stand-in type. It’s a placeholder that you can think of as a type that a developer will define later.
在此代码中，T 是替代类型。它是一个占位符，您可以将其视为开发人员稍后定义的类型。

## Using collection literals 使用集合文字

List, set, and map literals can be parameterized. Parameterized literals are just like the literals you’ve already seen, except that you add `<*type*>` (for lists and sets) or `<*keyType*, *valueType*>` (for maps) before the opening bracket. Here is an example of using typed literals:
列表、集合和映射文字可以参数化。参数化文字与您已经见过的文字一样，只是在开括号前添加 `<*type*>` （对于列表和集合）或 `<*keyType*, *valueType*>` （对于映射）。以下是用类型化文字的示例：

```dart
var names = <String>['Seth', 'Kathy', 'Lars'];
var uniqueNames = <String>{'Seth', 'Kathy', 'Lars'};
var pages = <String, String>{
  'index.html': 'Homepage',
  'robots.txt': 'Hints for web robots',
  'humans.txt': 'We are people, not machines'
};
```

## Using parameterized types with constructors 使用构造函数的参数化类型

To specify one or more types when using a constructor, put the types in angle brackets (`<...>`) just after the class name. For example:
要在使用构造函数时指定一个或多个类型，请在类名后紧跟尖括号 ( `<...>` ) 中的类型。例如：

```dart
var nameSet = Set<String>.from(names);
```

The following code creates a map that has integer keys and values of type View:
以下代码创建一个具有整数键和 View 类型值的映射：

```dart
var views = Map<int, View>();
```

## Generic collections and the types they contain 泛型集合及其包含的类型

Dart generic types are *reified*, which means that they carry their type information around at runtime. For example, you can test the type of a collection:
Dart 泛型类型是固化的，这意味着它们在运行时携带其类型信息。例如，您可以测试集合的类型：

```dart
var names = <String>[];
names.addAll(['Seth', 'Kathy', 'Lars']);
print(names is List<String>); // true
```

*info* **Note:** In contrast, generics in Java use *erasure*, which means that generic type parameters are removed at runtime. In Java, you can test whether an object is a List, but you can’t test whether it’s a `List<String>`.
注意：相比之下，Java 中的泛型使用擦除，这意味着泛型类型参数在运行时被删除。在 Java 中，您可以测试对象是否为 List，但不能测试它是否为 `List<String>` 。

## Restricting the parameterized type 限制参数化类型

When implementing a generic type, you might want to limit the types that can be provided as arguments, so that the argument must be a subtype of a particular type. You can do this using `extends`.
在实现泛型类型时，您可能希望限制可作为参数提供的类型，以便参数必须是特定类型的子类型。您可以使用 `extends` 来执行此操作。

A common use case is ensuring that a type is non-nullable by making it a subtype of `Object` (instead of the default, [`Object?`](https://dart.dev/null-safety/understanding-null-safety#top-and-bottom)).
一种常见用例是通过使类型成为 `Object` 的子类型（而不是默认值 `Object?` ）来确保类型不可为 null。

```dart
class Foo<T extends Object> {
  // Any type provided to Foo for T must be non-nullable.
}
```

You can use `extends` with other types besides `Object`. Here’s an example of extending `SomeBaseClass`, so that members of `SomeBaseClass` can be called on objects of type `T`:
除了 `Object` 之外，您还可以将 `extends` 与其他类型一起使用。以下是如何扩展 `SomeBaseClass` 的示例，以便可以在类型为 `T` 的对象上调用 `SomeBaseClass` 的成员：

```dart
class Foo<T extends SomeBaseClass> {
  // Implementation goes here...
  String toString() => "Instance of 'Foo<$T>'";
}

class Extender extends SomeBaseClass {...}
```

It’s OK to use `SomeBaseClass` or any of its subtypes as the generic argument:
可以使用 `SomeBaseClass` 或其任何子类型作为泛型参数：

```dart
var someBaseClassFoo = Foo<SomeBaseClass>();
var extenderFoo = Foo<Extender>();
```

It’s also OK to specify no generic argument:
也可以指定不带泛型参数：

```dart
var foo = Foo();
print(foo); // Instance of 'Foo<SomeBaseClass>'
```

Specifying any non-`SomeBaseClass` type results in an error:
指定任何非 `SomeBaseClass` 类型都会导致错误：

```dart
var foo = Foo<Object>();
```

## Using generic methods 使用泛型方法

Methods and functions also allow type arguments:
方法和函数也允许类型参数：

```dart
T first<T>(List<T> ts) {
  // Do some initial work or error checking, then...
  T tmp = ts[0];
  // Do some additional checking or processing...
  return tmp;
}
```

Here the generic type parameter on `first` (`<T>`) allows you to use the type argument `T` in several places:
此处 `first` （ `<T>` ）上的泛型类型参数允许您在多个位置使用类型参数 `T` ：

- In the function’s return type (`T`).
  在函数的返回类型（ `T` ）中。
- In the type of an argument (`List<T>`).
  在参数的类型（ `List<T>` ）中。
- In the type of a local variable (`T tmp`).
  在局部变量的类型（ `T tmp` ）中。
