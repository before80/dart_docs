+++
title = "Methods"
date = 2024-01-05T20:29:36+08:00
weight = 20
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文: [https://dart.dev/language/methods](https://dart.dev/language/methods)

# Methods 方法

Methods are functions that provide behavior for an object.
方法是为对象提供行为的函数。

## Instance methods 实例方法

Instance methods on objects can access instance variables and `this`. The `distanceTo()` method in the following sample is an example of an instance method:
对象上的实例方法可以访问实例变量和 `this` 。以下示例中的 `distanceTo()` 方法是实例方法的一个示例：

```dart
import 'dart:math';

class Point {
  final double x;
  final double y;

  Point(this.x, this.y);

  double distanceTo(Point other) {
    var dx = x - other.x;
    var dy = y - other.y;
    return sqrt(dx * dx + dy * dy);
  }
}
```

## Operators 运算符

Operators are instance methods with special names. Dart allows you to define operators with the following names:
运算符是具有特殊名称的实例方法。Dart 允许您使用以下名称定义运算符：

| `<`  | `+`  | `|`  | `>>>` |
| ---- | ---- | ---- | ----- |
| `>`  | `/`  | `^`  | `[]`  |
| `<=` | `~/` | `&`  | `[]=` |
| `>=` | `*`  | `<<` | `~`   |
| `-`  | `%`  | `>>` | `==`  |

*info* **Note:** You may have noticed that some [operators](https://dart.dev/language/operators), like `!=`, aren’t in the list of names. That’s because they’re just syntactic sugar. For example, the expression `e1 != e2` is syntactic sugar for `!(e1 == e2)`.
注意：您可能已经注意到某些运算符（如 `!=` ）不在名称列表中。这是因为它们只是语法糖。例如，表达式 `e1 != e2` 是 `!(e1 == e2)` 的语法糖。

An operator declaration is identified using the built-in identifier `operator`. The following example defines vector addition (`+`), subtraction (`-`), and equality (`==`):
运算符声明使用内置标识符 `operator` 标识。以下示例定义了向量加法 ( `+` )、减法 ( `-` ) 和相等 ( `==` )：

```dart
class Vector {
  final int x, y;

  Vector(this.x, this.y);

  Vector operator +(Vector v) => Vector(x + v.x, y + v.y);
  Vector operator -(Vector v) => Vector(x - v.x, y - v.y);

  @override
  bool operator ==(Object other) =>
      other is Vector && x == other.x && y == other.y;

  @override
  int get hashCode => Object.hash(x, y);
}

void main() {
  final v = Vector(2, 3);
  final w = Vector(2, 2);

  assert(v + w == Vector(4, 5));
  assert(v - w == Vector(0, 1));
}
```

## Getters and setters 获取器和设置器

Getters and setters are special methods that provide read and write access to an object’s properties. Recall that each instance variable has an implicit getter, plus a setter if appropriate. You can create additional properties by implementing getters and setters, using the `get` and `set` keywords:
Getter 和 setter 是提供对对象属性的读写访问权限的特殊方法。回想一下，每个实例变量都有一个隐式 getter，如果合适，还有一个 setter。您可以通过使用 `get` 和 `set` 关键字实现 getter 和 setter 来创建其他属性：

```dart
class Rectangle {
  double left, top, width, height;

  Rectangle(this.left, this.top, this.width, this.height);

  // Define two calculated properties: right and bottom.
  double get right => left + width;
  set right(double value) => left = value - width;
  double get bottom => top + height;
  set bottom(double value) => top = value - height;
}

void main() {
  var rect = Rectangle(3, 4, 20, 15);
  assert(rect.left == 3);
  rect.right = 12;
  assert(rect.left == -8);
}
```

With getters and setters, you can start with instance variables, later wrapping them with methods, all without changing client code.
使用 getter 和 setter，您可以从实例变量开始，稍后用方法包装它们，而无需更改客户端代码。

*info* **Note:** Operators such as increment (++) work in the expected way, whether or not a getter is explicitly defined. To avoid any unexpected side effects, the operator calls the getter exactly once, saving its value in a temporary variable.
注意：无论是否明确定义了 getter，诸如增量 (++) 之类的运算符都会按预期的方式工作。为了避免任何意外的副作用，运算符只调用 getter 一次，将其值保存在临时变量中。

## Abstract methods 抽象方法

Instance, getter, and setter methods can be abstract, defining an interface but leaving its implementation up to other classes. Abstract methods can only exist in [abstract classes](https://dart.dev/language/class-modifiers#abstract) or [mixins](https://dart.dev/language/mixins).
实例、getter 和 setter 方法可以是抽象的，定义一个接口，但将其实现留给其他类。抽象方法只能存在于抽象类或 mixin 中。

To make a method abstract, use a semicolon (;) instead of a method body:
要使方法变为抽象，请使用分号 (;) 代替方法体：

```dart
abstract class Doer {
  // Define instance variables and methods...

  void doSomething(); // Define an abstract method.
}

class EffectiveDoer extends Doer {
  void doSomething() {
    // Provide an implementation, so the method is not abstract here...
  }
}
```
