+++
title = "Dart 简介"
date = 2024-01-05T20:29:36+08:00
weight = 1
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文: [https://dart.dev/language](https://dart.dev/language)

## Introduction to Dart - Dart 简介

This page provides a brief introduction to the Dart language through samples of its main features.

​	此页面通过其主要功能的示例简要介绍了 Dart 语言。

To learn more about the Dart language, visit the in-depth, individual topic pages listed under **Language** in the left side menu.

​	要详细了解 Dart 语言，请访问左侧菜单中“语言”下列出的深入的各个主题页面。

For coverage of Dart’s core libraries, check out the [core library documentation](https://dart.dev/libraries). You can also try the [Dart cheatsheet codelab](https://dart.dev/codelabs/dart-cheatsheet), for a more hands-on introduction.

​	有关 Dart 核心库的介绍，请查看核心库文档。您还可以尝试 Dart 速查表代码实验室，以获得更直观的介绍。

## Hello World 

Every app requires the top-level `main()` function, where execution starts. Functions that don’t explicitly return a value have the `void` return type. To display text on the console, you can use the top-level `print()` function:

​	每个应用程序都需要顶级 `main()` 函数，从该函数开始执行。未明确返回值的函数具有 `void` 返回类型。若要显示控制台上的文本，可以使用顶级 `print()` 函数：

```dart
void main() {
  print('Hello, World!');
}
```

Read more about [the `main()` function](https://dart.dev/language/functions#the-main-function) in Dart, including optional parameters for command-line arguments.

​	阅读有关 Dart 中的 `main()` 函数的更多信息，包括用于命令行参数的可选参数。

## 变量 Variables 

Even in [type-safe](https://dart.dev/language/type-system) Dart code, you can declare most variables without explicitly specifying their type using `var`. Thanks to type inference, these variables’ types are determined by their initial values:

​	即使在类型安全的 Dart 代码中，您也可以使用 `var` 声明大多数变量，而无需明确指定其类型。借助类型推断，这些变量的类型由其初始值确定：

```dart
var name = 'Voyager I';
var year = 1977;
var antennaDiameter = 3.7;
var flybyObjects = ['Jupiter', 'Saturn', 'Uranus', 'Neptune'];
var image = {
  'tags': ['saturn'],
  'url': '//path/to/saturn.jpg'
};
```

[Read more](https://dart.dev/language/variables) about variables in Dart, including default values, the `final` and `const` keywords, and static types.
详细了解 Dart 中的变量，包括默认值、 `final` 和 `const` 关键字以及静态类型。

## 控制流语句 Control flow statements 

Dart supports the usual control flow statements:

​	Dart 支持通常的控制流语句：

```dart
if (year >= 2001) {
  print('21st century');
} else if (year >= 1901) {
  print('20th century');
}

for (final object in flybyObjects) {
  print(object);
}

for (int month = 1; month <= 12; month++) {
  print(month);
}

while (year < 2016) {
  year += 1;
}
```

Read more about control flow statements in Dart, including [`break` and `continue`](https://dart.dev/language/loops), [`switch` and `case`](https://dart.dev/language/branches), and [`assert`](https://dart.dev/language/error-handling#assert).

​	详细了解 Dart 中的控制流语句，包括 `break` 和 `continue` 、 `switch` 和 `case` 以及 `assert` 。

## 函数 Functions 

[We recommend](https://dart.dev/effective-dart/design#types) specifying the types of each function’s arguments and return value:

​	我们建议指定每个函数的参数和返回值的类型：

```dart
int fibonacci(int n) {
  if (n == 0 || n == 1) return n;
  return fibonacci(n - 1) + fibonacci(n - 2);
}

var result = fibonacci(20);
```

A shorthand `=>` (*arrow*) syntax is handy for functions that contain a single statement. This syntax is especially useful when passing anonymous functions as arguments:

​	对于包含单个语句的函数，简写 `=>` （箭头）语法非常方便。在将匿名函数作为参数传递时，此语法特别有用：

```dart
flybyObjects.where((name) => name.contains('turn')).forEach(print);
```

Besides showing an anonymous function (the argument to `where()`), this code shows that you can use a function as an argument: the top-level `print()` function is an argument to `forEach()`.

​	除了显示一个匿名函数（ `where()` 的参数），此代码还显示您可以将函数用作参数：顶级 `print()` 函数是 `forEach()` 的参数。

[Read more](https://dart.dev/language/functions) about functions in Dart, including optional parameters, default parameter values, and lexical scope.

​	阅读有关 Dart 中函数的更多信息，包括可选参数、默认参数值和词法作用域。

## 注释 Comments 

Dart comments usually start with `//`.

​	Dart 注释通常以 `//` 开头。

```
// This is a normal, one-line comment.

/// This is a documentation comment, used to document libraries,
/// classes, and their members. Tools like IDEs and dartdoc treat
/// doc comments specially.

/* Comments like these are also supported. */
```

[Read more](https://dart.dev/language/comments) about comments in Dart, including how the documentation tooling works.

​	阅读有关 Dart 中注释的更多信息，包括文档工具如何工作。

## 导入 Imports 

To access APIs defined in other libraries, use `import`.

​	要访问在其他库中定义的 API，请使用 `import` 。

```dart
// Importing core libraries
import 'dart:math';

// Importing libraries from external packages
import 'package:test/test.dart';

// Importing files
import 'path/to/my_other_file.dart';
```

[Read more](https://dart.dev/language/libraries) about libraries and visibility in Dart, including library prefixes, `show` and `hide`, and lazy loading through the `deferred` keyword.

​	阅读有关 Dart 中的库和可见性的更多信息，包括库前缀、 `show` 和 `hide` ，以及通过 `deferred` 关键字进行延迟加载。

## 类 Classes 

Here’s an example of a class with three properties, two constructors, and a method. One of the properties can’t be set directly, so it’s defined using a getter method (instead of a variable). The method uses string interpolation to print variables’ string equivalents inside of string literals.

​	这是一个具有三个属性、两个构造函数和一个方法的类的示例。其中一个属性无法直接设置，因此使用 getter 方法（而不是变量）对其进行定义。该方法使用字符串插值在字符串文字中打印变量的字符串等价物。

```dart
class Spacecraft {
  String name;
  DateTime? launchDate;

  // Read-only non-final property
  int? get launchYear => launchDate?.year;

  // Constructor, with syntactic sugar for assignment to members.
  Spacecraft(this.name, this.launchDate) {
    // Initialization code goes here.
  }

  // Named constructor that forwards to the default one.
  Spacecraft.unlaunched(String name) : this(name, null);

  // Method.
  void describe() {
    print('Spacecraft: $name');
    // Type promotion doesn't work on getters.
    var launchDate = this.launchDate;
    if (launchDate != null) {
      int years = DateTime.now().difference(launchDate).inDays ~/ 365;
      print('Launched: $launchYear ($years years ago)');
    } else {
      print('Unlaunched');
    }
  }
}
```

[Read more](https://dart.dev/language/built-in-types#strings) about strings, including string interpolation, literals, expressions, and the `toString()` method.

​	阅读有关字符串的更多信息，包括字符串插值、文字、表达式和 `toString()` 方法。

You might use the `Spacecraft` class like this:

​	您可以像这样使用 `Spacecraft` 类：

```dart
var voyager = Spacecraft('Voyager I', DateTime(1977, 9, 5));
voyager.describe();

var voyager3 = Spacecraft.unlaunched('Voyager III');
voyager3.describe();
```

[Read more](https://dart.dev/language/classes) about classes in Dart, including initializer lists, optional `new` and `const`, redirecting constructors, `factory` constructors, getters, setters, and much more.

​	详细了解 Dart 中的类，包括初始化器列表、可选 `new` 和 `const` 、重定向构造函数、 `factory` 构造函数、getter、setter 等。

## 枚举 Enums 

Enums are a way of enumerating a predefined set of values or instances in a way which ensures that there cannot be any other instances of that type.

​	枚举是一种枚举一组预定义值或实例的方式，可确保不存在该类型的任何其他实例。

Here is an example of a simple `enum` that defines a simple list of predefined planet types:

​	这是一个简单的 `enum` 的示例，它定义了一个简单的预定义行星类型列表：

```dart
enum PlanetType { terrestrial, gas, ice }
```

Here is an example of an enhanced enum declaration of a class describing planets, with a defined set of constant instances, namely the planets of our own solar system.

​	这是一个增强枚举声明的示例，该声明描述了行星的类，其中包含一组定义的常量实例，即我们自己太阳系的行星。

```dart
/// Enum that enumerates the different planets in our solar system
/// and some of their properties.
enum Planet {
  mercury(planetType: PlanetType.terrestrial, moons: 0, hasRings: false),
  venus(planetType: PlanetType.terrestrial, moons: 0, hasRings: false),
  // ···
  uranus(planetType: PlanetType.ice, moons: 27, hasRings: true),
  neptune(planetType: PlanetType.ice, moons: 14, hasRings: true);

  /// A constant generating constructor
  const Planet(
      {required this.planetType, required this.moons, required this.hasRings});

  /// All instance variables are final
  final PlanetType planetType;
  final int moons;
  final bool hasRings;

  /// Enhanced enums support getters and other methods
  bool get isGiant =>
      planetType == PlanetType.gas || planetType == PlanetType.ice;
}
```

You might use the `Planet` enum like this:

​	您可以像这样使用 `Planet` 枚举：

```dart
final yourPlanet = Planet.earth;

if (!yourPlanet.isGiant) {
  print('Your planet is not a "giant planet".');
}
```

[Read more](https://dart.dev/language/enums) about enums in Dart, including enhanced enum requirements, automatically introduced properties, accessing enumerated value names, switch statement support, and much more.

​	详细了解 Dart 中的枚举，包括增强的枚举要求、自动引入的属性、访问枚举值名称、switch 语句支持等。

## 继承 Inheritance 

Dart has single inheritance.

​	Dart 具有单一继承。

```dart
class Orbiter extends Spacecraft {
  double altitude;

  Orbiter(super.name, DateTime super.launchDate, this.altitude);
}
```

[Read more](https://dart.dev/language/extend) about extending classes, the optional `@override` annotation, and more.

​	详细了解扩展类、可选 `@override` 注释等。

## 混入 Mixins 

Mixins are a way of reusing code in multiple class hierarchies. The following is a mixin declaration:

​	混入是将代码在多个类层次结构中重用的方法。以下是一个混入声明：

```dart
mixin Piloted {
  int astronauts = 1;

  void describeCrew() {
    print('Number of astronauts: $astronauts');
  }
}
```

To add a mixin’s capabilities to a class, just extend the class with the mixin.

​	要将混入的功能添加到类，只需使用混入扩展类即可。

```dart
class PilotedCraft extends Spacecraft with Piloted {
  // ···
}
```

`PilotedCraft` now has the `astronauts` field as well as the `describeCrew()` method.

​	`PilotedCraft` 现在具有 `astronauts` 字段以及 `describeCrew()` 方法。

[Read more](https://dart.dev/language/mixins) about mixins.

​	详细了解 mixin。

## 接口和抽象类 Interfaces and abstract classes 

All classes implicitly define an interface. Therefore, you can implement any class.

​	所有类隐式定义一个接口。因此，您可以实现任何类。

```dart
class MockSpaceship implements Spacecraft {
  // ···
}
```

Read more about [implicit interfaces](https://dart.dev/language/classes#implicit-interfaces), or about the explicit [`interface` keyword](https://dart.dev/language/class-modifiers#interface).

​	详细了解隐式接口或显式 `interface` 关键字。

You can create an abstract class to be extended (or implemented) by a concrete class. Abstract classes can contain abstract methods (with empty bodies).

​	您可以创建一个抽象类，由具体类扩展（或实现）。抽象类可以包含抽象方法（主体为空）。

```dart
abstract class Describable {
  void describe();

  void describeWithEmphasis() {
    print('=========');
    describe();
    print('=========');
  }
}
```

Any class extending `Describable` has the `describeWithEmphasis()` method, which calls the extender’s implementation of `describe()`.

​	任何扩展 `Describable` 的类都有 `describeWithEmphasis()` 方法，该方法调用扩展程序的 `describe()` 实现。

[Read more](https://dart.dev/language/class-modifiers#abstract) about abstract classes and methods.

​	详细了解抽象类和方法。

## 异步 Async 

Avoid callback hell and make your code much more readable by using `async` and `await`.

​	避免回调地狱，使用 `async` 和 `await` 使您的代码更具可读性。

```dart
const oneSecond = Duration(seconds: 1);
// ···
Future<void> printWithDelay(String message) async {
  await Future.delayed(oneSecond);
  print(message);
}
```

The method above is equivalent to:

​	上面的方法等效于：

```dart
Future<void> printWithDelay(String message) {
  return Future.delayed(oneSecond).then((_) {
    print(message);
  });
}
```

As the next example shows, `async` and `await` help make asynchronous code easy to read.

​	如下例所示， `async` 和 `await` 有助于使异步代码易于阅读。

```dart
Future<void> createDescriptions(Iterable<String> objects) async {
  for (final object in objects) {
    try {
      var file = File('$object.txt');
      if (await file.exists()) {
        var modified = await file.lastModified();
        print(
            'File for $object already exists. It was modified on $modified.');
        continue;
      }
      await file.create();
      await file.writeAsString('Start describing $object in this file.');
    } on IOException catch (e) {
      print('Cannot create description for $object: $e');
    }
  }
}
```

You can also use `async`, which gives you a nice, readable way to build streams.

​	您还可以使用 `async` ，它为您提供了一种构建流的不错且可读的方式。

```dart
Stream<String> report(Spacecraft craft, Iterable<String> objects) async {
  for (final object in objects) {
    await Future.delayed(oneSecond);
    yield '${craft.name} flies by $object';
  }
}
```

[Read more](https://dart.dev/language/async) about asynchrony support, including `async` functions, `Future`, `Stream`, and the asynchronous loop (`await for`).

​	阅读有关异步支持的更多信息，包括 `async` 函数、 `Future` 、 `Stream` 和异步循环 ( `await for` )。

## 异常 Exceptions 

To raise an exception, use `throw`:

​	要引发异常，请使用 `throw` ：

```dart
if (astronauts == 0) {
  throw StateError('No astronauts.');
}
```

To catch an exception, use a `try` statement with `on` or `catch` (or both):

​	要捕获异常，请使用带有 `on` 或 `catch` （或两者）的 `try` 语句：

```dart
Future<void> describeFlybyObjects(List<String> flybyObjects) async {
  try {
    for (final object in flybyObjects) {
      var description = await File('$object.txt').readAsString();
      print(description);
    }
  } on IOException catch (e) {
    print('Could not describe object: $e');
  } finally {
    flybyObjects.clear();
  }
}
```

Note that the code above is asynchronous; `try` works for both synchronous code and code in an `async` function.

​	请注意，上面的代码是异步的； `try` 适用于同步代码和 `async` 函数中的代码。

[Read more](https://dart.dev/language/error-handling#exceptions) about exceptions, including stack traces, `rethrow`, and the difference between `Error` and `Exception`.

​	阅读有关异常的更多信息，包括堆栈跟踪、 `rethrow` 以及 `Error` 和 `Exception` 之间的差异。

## 重要概念 Important concepts 

As you continue to learn about the Dart language, keep these facts and concepts in mind:

​	在您继续学习 Dart 语言时，请牢记以下事实和概念：

- Everything you can place in a variable is an *object*, and every object is an instance of a *class*. Even numbers, functions, and `null` are objects. With the exception of `null` (if you enable [sound null safety](https://dart.dev/null-safety)), all objects inherit from the [`Object`](https://api.dart.dev/stable/dart-core/Object-class.html) class.
  
- 您可以放入变量中的所有内容都是对象，每个对象都是某个类的实例。即使是数字、函数和 `null` 也是对象。除了 `null` （如果您启用了健全的空安全）之外，所有对象都继承自 `Object` 类。
  
  *merge_type* **Version note:** [Null safety](https://dart.dev/null-safety) was introduced in Dart 2.12. Using null safety requires a [language version](https://dart.dev/guides/language/evolution#language-versioning) of at least 2.12.
  
  版本说明：Dart 2.12 中引入了空安全。使用空安全需要至少为 2.12 的语言版本。

- Although Dart is strongly typed, type annotations are optional because Dart can infer types. In `var number = 101`, `number` is inferred to be of type `int`.
  
- 尽管 Dart 是强类型语言，但类型注释是可选的，因为 Dart 可以推断类型。在 `var number = 101` 中，推断 `number` 的类型为 `int` 。
  
- If you enable [null safety](https://dart.dev/null-safety), variables can’t contain `null` unless you say they can. You can make a variable nullable by putting a question mark (`?`) at the end of its type. For example, a variable of type `int?` might be an integer, or it might be `null`. If you *know* that an expression never evaluates to `null` but Dart disagrees, you can add `!` to assert that it isn’t null (and to throw an exception if it is). An example: `int x = nullableButNotNullInt!`
  
- 如果您启用空安全，则变量不能包含 `null` ，除非您说它们可以。您可以在其类型的末尾放置一个问号 ( `?` ) 来使变量可为 null。例如，类型为 `int?` 的变量可以是整数，也可以是 `null` 。如果您知道某个表达式永远不会计算为 `null` ，但 Dart 不这么认为，则可以添加 `!` 来断言它不是 null（如果它是 null，则抛出异常）。示例： `int x = nullableButNotNullInt!`
  
- When you want to explicitly say that any type is allowed, use the type `Object?` (if you’ve enabled null safety), `Object`, or—if you must defer type checking until runtime—the [special type `dynamic`](https://dart.dev/effective-dart/design#avoid-using-dynamic-unless-you-want-to-disable-static-checking).
  
- 当您想要明确地说允许任何类型时，请使用类型 `Object?` （如果您已启用空安全）、 `Object` ，或者——如果您必须将类型检查推迟到运行时——特殊类型 `dynamic` 。
  
- Dart supports generic types, like `List<int>` (a list of integers) or `List<Object>` (a list of objects of any type).
  
- Dart 支持泛型类型，例如 `List<int>` （整数列表）或 `List<Object>` （任何类型的对象列表）。
  
- Dart supports top-level functions (such as `main()`), as well as functions tied to a class or object (*static* and *instance methods*, respectively). You can also create functions within functions (*nested* or *local functions*).
  Dart 支持顶级函数（例如 `main()` ），以及与类或对象绑定的函数（分别为静态方法和实例方法）。您还可以在函数内创建函数（嵌套或局部函数）。

- Similarly, Dart supports top-level *variables*, as well as variables tied to a class or object (static and instance variables). Instance variables are sometimes known as *fields* or *properties*.
  
- 同样，Dart 支持顶级变量，以及与类或对象绑定的变量（静态变量和实例变量）。实例变量有时称为字段或属性。
  
- Unlike Java, Dart doesn’t have the keywords `public`, `protected`, and `private`. If an identifier starts with an underscore (`_`), it’s private to its library. For details, see [Libraries and imports](https://dart.dev/language/libraries).
  
- 与 Java 不同，Dart 没有关键字 `public` 、 `protected` 和 `private` 。如果标识符以下划线开头 ( `_` )，则它对库是私有的。有关详细信息，请参阅库和导入。
  
- *Identifiers* can start with a letter or underscore (`_`), followed by any combination of those characters plus digits.
  
- 标识符可以以字母或下划线 ( `_` ) 开头，后跟这些字符与数字的任意组合。
  
- Dart has both *expressions* (which have runtime values) and *statements* (which don’t). For example, the [conditional expression](https://dart.dev/language/operators#conditional-expressions) `condition ? expr1 : expr2` has a value of `expr1` or `expr2`. Compare that to an [if-else statement](https://dart.dev/language/branches#if), which has no value. A statement often contains one or more expressions, but an expression can’t directly contain a statement.
  
- Dart 既有表达式（具有运行时值）又有语句（没有值）。例如，条件表达式 `condition ? expr1 : expr2` 的值为 `expr1` 或 `expr2` 。将其与没有值的 if-else 语句进行比较。语句通常包含一个或多个表达式，但表达式不能直接包含语句。
  
- Dart tools can report two kinds of problems: *warnings* and *errors*. Warnings are just indications that your code might not work, but they don’t prevent your program from executing. Errors can be either compile-time or run-time. A compile-time error prevents the code from executing at all; a run-time error results in an [exception](https://dart.dev/language/error-handling#exceptions) being raised while the code executes.
  
- Dart 工具可以报告两种问题：警告和错误。警告只是表明您的代码可能无法运行，但它们不会阻止您的程序执行。错误可以是编译时错误或运行时错误。编译时错误会阻止代码执行；运行时错误会导致在代码执行时引发异常。

## 其他资源 Additional resources 

You can find more documentation and code samples in the [core library documentation](https://dart.dev/libraries/dart-core) and the [Dart API reference](https://api.dart.dev/). This site’s code follows the conventions in the [Dart style guide](https://dart.dev/effective-dart/style).

​	您可以在核心库文档和 Dart API 参考中找到更多文档和代码示例。此网站的代码遵循 Dart 风格指南中的约定。
