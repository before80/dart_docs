+++
title = "Classes"
date = 2024-01-05T20:29:36+08:00
weight = 1
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文: [https://dart.dev/language/classes](https://dart.dev/language/classes)

# Classes 类

Dart is an object-oriented language with classes and mixin-based inheritance. Every object is an instance of a class, and all classes except `Null` descend from [`Object`](https://api.dart.dev/stable/dart-core/Object-class.html). *Mixin-based inheritance* means that although every class (except for the [top class](https://dart.dev/null-safety/understanding-null-safety#top-and-bottom), `Object?`) has exactly one superclass, a class body can be reused in multiple class hierarchies. [Extension methods](https://dart.dev/language/extension-methods) are a way to add functionality to a class without changing the class or creating a subclass. [Class modifiers](https://dart.dev/language/class-modifiers) allow you to control how libraries can subtype a class.
Dart 是一种面向对象语言，具有类和基于 mixin 的继承。每个对象都是一个类的实例，除了 `Null` 之外，所有类都派生自 `Object` 。基于 mixin 的继承意味着虽然每个类（除了顶级类 `Object?` ）只有一个超类，但类主体可以在多个类层次结构中重复使用。扩展方法是一种向类添加功能的方法，而无需更改类或创建子类。类修饰符允许您控制库如何对类进行子类型化。

## Using class members 使用类成员

Objects have *members* consisting of functions and data (*methods* and *instance variables*, respectively). When you call a method, you *invoke* it on an object: the method has access to that object’s functions and data.
对象具有由函数和数据（分别为方法和实例变量）组成的成员。当您调用方法时，您在对象上调用它：该方法可以访问该对象的功能和数据。

Use a dot (`.`) to refer to an instance variable or method:
使用点 ( `.` ) 来引用实例变量或方法：

```dart
var p = Point(2, 2);

// Get the value of y.
assert(p.y == 2);

// Invoke distanceTo() on p.
double distance = p.distanceTo(Point(4, 4));
```

Use `?.` instead of `.` to avoid an exception when the leftmost operand is null:
使用 `?.` 代替 `.` 以避免在最左操作数为 null 时出现异常：

```dart
// If p is non-null, set a variable equal to its y value.
var a = p?.y;
```

## Using constructors 使用构造函数

You can create an object using a *constructor*. Constructor names can be either `*ClassName*` or `*ClassName*.*identifier*`. For example, the following code creates `Point` objects using the `Point()` and `Point.fromJson()` constructors:
您可以使用构造函数创建对象。构造函数名称可以是 `*ClassName*` 或 `*ClassName*.*identifier*` 。例如，以下代码使用 `Point()` 和 `Point.fromJson()` 构造函数创建 `Point` 对象：

```dart
var p1 = Point(2, 2);
var p2 = Point.fromJson({'x': 1, 'y': 2});
```

The following code has the same effect, but uses the optional `new` keyword before the constructor name:
以下代码具有相同的效果，但在构造函数名称之前使用了可选的 `new` 关键字：

```dart
var p1 = new Point(2, 2);
var p2 = new Point.fromJson({'x': 1, 'y': 2});
```

Some classes provide [constant constructors](https://dart.dev/language/constructors#constant-constructors). To create a compile-time constant using a constant constructor, put the `const` keyword before the constructor name:
一些类提供常量构造函数。要使用常量构造函数创建编译时常量，请在构造函数名称之前放置 `const` 关键字：

```dart
var p = const ImmutablePoint(2, 2);
```

Constructing two identical compile-time constants results in a single, canonical instance:
构造两个相同的编译时常量会产生一个单独的规范实例：

```dart
var a = const ImmutablePoint(1, 1);
var b = const ImmutablePoint(1, 1);

assert(identical(a, b)); // They are the same instance!
```

Within a *constant context*, you can omit the `const` before a constructor or literal. For example, look at this code, which creates a const map:
在常量上下文中，您可以在构造函数或字面量之前省略 `const` 。例如，请看以下代码，它创建了一个 const 映射：

```dart
// Lots of const keywords here.
const pointAndLine = const {
  'point': const [const ImmutablePoint(0, 0)],
  'line': const [const ImmutablePoint(1, 10), const ImmutablePoint(-2, 11)],
};
```

You can omit all but the first use of the `const` keyword:
您可以省略除第一个 `const` 关键字之外的所有关键字：

```dart
// Only one const, which establishes the constant context.
const pointAndLine = {
  'point': [ImmutablePoint(0, 0)],
  'line': [ImmutablePoint(1, 10), ImmutablePoint(-2, 11)],
};
```

If a constant constructor is outside of a constant context and is invoked without `const`, it creates a **non-constant object**:
如果常量构造函数位于常量上下文之外并且在没有 `const` 的情况下被调用，它将创建一个非常量对象：

```dart
var a = const ImmutablePoint(1, 1); // Creates a constant
var b = ImmutablePoint(1, 1); // Does NOT create a constant

assert(!identical(a, b)); // NOT the same instance!
```

## Getting an object’s type 获取对象的类型

To get an object’s type at runtime, you can use the `Object` property `runtimeType`, which returns a [`Type`](https://api.dart.dev/stable/dart-core/Type-class.html) object.
若要获取对象的类型，可以在运行时使用 `Object` 属性 `runtimeType` ，它会返回一个 `Type` 对象。

```dart
print('The type of a is ${a.runtimeType}');
```

*report_problem* Use a [type test operator](https://dart.dev/language/operators#type-test-operators) rather than `runtimeType` to test an object’s type. In production environments, the test `object is Type` is more stable than the test `object.runtimeType == Type`.
使用类型测试运算符而不是 `runtimeType` 来测试对象的类型。在生产环境中，测试 `object is Type` 比测试 `object.runtimeType == Type` 更稳定。

Up to here, you’ve seen how to *use* classes. The rest of this section shows how to *implement* classes.
到目前为止，您已经了解了如何使用类。本部分的其余内容将介绍如何实现类。

## Instance variables 实例变量

Here’s how you declare instance variables:
以下是声明实例变量的方法：

```dart
class Point {
  double? x; // Declare instance variable x, initially null.
  double? y; // Declare y, initially null.
  double z = 0; // Declare z, initially 0.
}
```

An uninitialized instance variable declared with a [nullable type](https://dart.dev/null-safety/understanding-null-safety#using-nullable-types) has the value `null`. Non-nullable instance variables [must be initialized](https://dart.dev/null-safety/understanding-null-safety#uninitialized-variables) at declaration.
使用可空类型声明的未初始化实例变量的值为 `null` 。不可空实例变量必须在声明时初始化。

All instance variables generate an implicit *getter* method. Non-final instance variables and `late final` instance variables without initializers also generate an implicit *setter* method. For details, check out [Getters and setters](https://dart.dev/language/methods#getters-and-setters).
所有实例变量都会生成一个隐式 getter 方法。非 final 实例变量和没有初始值设定项的 `late final` 实例变量也会生成一个隐式 setter 方法。有关详细信息，请参阅 Getter 和 Setter。

```dart
class Point {
  double? x; // Declare instance variable x, initially null.
  double? y; // Declare y, initially null.
}

void main() {
  var point = Point();
  point.x = 4; // Use the setter method for x.
  assert(point.x == 4); // Use the getter method for x.
  assert(point.y == null); // Values default to null.
}
```

Initializing a non-`late` instance variable where it’s declared sets the value when the instance is created, before the constructor and its initializer list execute. As a result, the initializing expression (after the `=`) of a non-`late` instance variable can’t access `this`.
在声明非 `late` 实例变量时对其进行初始化会在创建实例时设置值，在构造函数及其初始化程序列表执行之前。因此，非 `late` 实例变量的初始化表达式（在 `=` 之后）无法访问 `this` 。

```dart
double initialX = 1.5;

class Point {
  // OK, can access declarations that do not depend on `this`:
  double? x = initialX;

  // ERROR, can't access `this` in non-`late` initializer:
  double? y = this.x;

  // OK, can access `this` in `late` initializer:
  late double? z = this.x;

  // OK, `this.fieldName` is a parameter declaration, not an expression:
  Point(this.x, this.y);
}
```

Instance variables can be `final`, in which case they must be set exactly once. Initialize `final`, non-`late` instance variables at declaration, using a constructor parameter, or using a constructor’s [initializer list](https://dart.dev/language/constructors#initializer-list):
实例变量可以是 `final` ，在这种情况下，它们必须设置一次。使用构造函数参数或使用构造函数的初始化程序列表，在声明时初始化 `final` 、非 `late` 实例变量：

```dart
class ProfileMark {
  final String name;
  final DateTime start = DateTime.now();

  ProfileMark(this.name);
  ProfileMark.unnamed() : name = '';
}
```

If you need to assign the value of a `final` instance variable after the constructor body starts, you can use one of the following:
如果您需要在构造函数主体开始后分配 `final` 实例变量的值，可以使用以下方法之一：

- Use a [factory constructor](https://dart.dev/language/constructors#factory-constructors).
  使用工厂构造函数。
- Use `late final`, but [*be careful:*](https://dart.dev/effective-dart/design#avoid-public-late-final-fields-without-initializers) a `late final` without an initializer adds a setter to the API.
  使用 `late final` ，但要小心：没有初始化程序的 `late final` 会向 API 添加一个 setter。

## Implicit interfaces 隐式接口

Every class implicitly defines an interface containing all the instance members of the class and of any interfaces it implements. If you want to create a class A that supports class B’s API without inheriting B’s implementation, class A should implement the B interface.
每个类都隐式定义一个接口，其中包含该类的所有实例成员以及它实现的任何接口。如果您想创建一个支持类 B 的 API 但不继承 B 的实现的类 A，则类 A 应实现 B 接口。

A class implements one or more interfaces by declaring them in an `implements` clause and then providing the APIs required by the interfaces. For example:
类通过在 `implements` 子句中声明一个或多个接口，然后提供接口所需的 API 来实现这些接口。例如：

```dart
// A person. The implicit interface contains greet().
class Person {
  // In the interface, but visible only in this library.
  final String _name;

  // Not in the interface, since this is a constructor.
  Person(this._name);

  // In the interface.
  String greet(String who) => 'Hello, $who. I am $_name.';
}

// An implementation of the Person interface.
class Impostor implements Person {
  String get _name => '';

  String greet(String who) => 'Hi $who. Do you know who I am?';
}

String greetBob(Person person) => person.greet('Bob');

void main() {
  print(greetBob(Person('Kathy')));
  print(greetBob(Impostor()));
}
```

Here’s an example of specifying that a class implements multiple interfaces:
下面是一个指定类实现多个接口的示例：

```dart
class Point implements Comparable, Location {...}
```

## Class variables and methods 类变量和方法

Use the `static` keyword to implement class-wide variables and methods.
使用 `static` 关键字来实现类范围的变量和方法。

### Static variables 静态变量

Static variables (class variables) are useful for class-wide state and constants:
静态变量（类变量）对于类范围的状态和常量很有用：

```dart
class Queue {
  static const initialCapacity = 16;
  // ···
}

void main() {
  assert(Queue.initialCapacity == 16);
}
```

Static variables aren’t initialized until they’re used.
静态变量在使用前不会初始化。

*info* **Note:** This page follows the [style guide recommendation](https://dart.dev/effective-dart/style#identifiers) of preferring `lowerCamelCase` for constant names.
注意：此页面遵循样式指南的建议，更喜欢将 `lowerCamelCase` 用于常量名称。

### Static methods 静态方法

Static methods (class methods) don’t operate on an instance, and thus don’t have access to `this`. They do, however, have access to static variables. As the following example shows, you invoke static methods directly on a class:
静态方法（类方法）不作用于实例，因此无法访问 `this` 。但是，它们可以访问静态变量。如下例所示，您直接在类上调用静态方法：

```dart
import 'dart:math';

class Point {
  double x, y;
  Point(this.x, this.y);

  static double distanceBetween(Point a, Point b) {
    var dx = a.x - b.x;
    var dy = a.y - b.y;
    return sqrt(dx * dx + dy * dy);
  }
}

void main() {
  var a = Point(2, 2);
  var b = Point(4, 4);
  var distance = Point.distanceBetween(a, b);
  assert(2.8 < distance && distance < 2.9);
  print(distance);
}
```

*info* **Note:** Consider using top-level functions, instead of static methods, for common or widely used utilities and functionality.
注意：考虑使用顶级函数（而不是静态方法）来实现常见或广泛使用的实用程序和功能。

You can use static methods as compile-time constants. For example, you can pass a static method as a parameter to a constant constructor.
您可以将静态方法用作编译时常量。例如，您可以将静态方法作为参数传递给常量构造函数。
