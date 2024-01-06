+++
title = "扩展类"
date = 2024-01-05T20:29:36+08:00
weight = 30
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文: [https://dart.dev/language/extend](https://dart.dev/language/extend)

## Extend a class 扩展类

Use `extends` to create a subclass, and `super` to refer to the superclass:
使用 `extends` 创建子类，使用 `super` 引用超类：

```dart
class Television {
  void turnOn() {
    _illuminateDisplay();
    _activateIrSensor();
  }
  // ···
}

class SmartTelevision extends Television {
  void turnOn() {
    super.turnOn();
    _bootNetworkInterface();
    _initializeMemory();
    _upgradeApps();
  }
  // ···
}
```

For another usage of `extends`, see the discussion of [parameterized types](https://dart.dev/language/generics#restricting-the-parameterized-type) on the Generics page.
有关 `extends` 的另一种用法，请参阅泛型页面上的参数化类型讨论。

## Overriding members 覆盖成员

Subclasses can override instance methods (including [operators](https://dart.dev/language/methods#operators)), getters, and setters. You can use the `@override` annotation to indicate that you are intentionally overriding a member:
子类可以覆盖实例方法（包括运算符）、getter 和 setter。您可以使用 `@override` 注释来表明您有意覆盖某个成员：

```dart
class Television {
  // ···
  set contrast(int value) {...}
}

class SmartTelevision extends Television {
  @override
  set contrast(num value) {...}
  // ···
}
```

An overriding method declaration must match the method (or methods) that it overrides in several ways:
覆盖方法声明必须在以下几个方面与它所覆盖的方法（或方法）相匹配：

- The return type must be the same type as (or a subtype of) the overridden method’s return type.
  返回类型必须与被覆盖方法的返回类型相同（或为其子类型）。
- Argument types must be the same type as (or a supertype of) the overridden method’s argument types. In the preceding example, the `contrast` setter of `SmartTelevision` changes the argument type from `int` to a supertype, `num`.
  参数类型必须与被覆盖方法的参数类型相同（或为其超类型）。在前面的示例中， `SmartTelevision` 的 `contrast` setter 将参数类型从 `int` 更改为超类型 `num` 。
- If the overridden method accepts *n* positional parameters, then the overriding method must also accept *n* positional parameters.
  如果被覆盖方法接受 n 个位置参数，那么覆盖方法也必须接受 n 个位置参数。
- A [generic method](https://dart.dev/language/generics#using-generic-methods) can’t override a non-generic one, and a non-generic method can’t override a generic one.
  泛型方法不能覆盖非泛型方法，非泛型方法不能覆盖泛型方法。

Sometimes you might want to narrow the type of a method parameter or an instance variable. This violates the normal rules, and it’s similar to a downcast in that it can cause a type error at runtime. Still, narrowing the type is possible if the code can guarantee that a type error won’t occur. In this case, you can use the [`covariant` keyword](https://dart.dev/guides/language/sound-problems#the-covariant-keyword) in a parameter declaration. For details, see the [Dart language specification](https://dart.dev/guides/language/spec).
有时您可能希望缩小方法参数或实例变量的类型。这违反了常规规则，并且类似于向下转型，因为它可能在运行时导致类型错误。不过，如果代码可以保证不会发生类型错误，则可以缩小类型。在这种情况下，您可以在参数声明中使用 `covariant` 关键字。有关详细信息，请参阅 Dart 语言规范。

*report_problem* **Warning:** If you override `==`, you should also override Object’s `hashCode` getter. For an example of overriding `==` and `hashCode`, check out [Implementing map keys](https://dart.dev/libraries/dart-core#implementing-map-keys).
警告：如果您重写 `==` ，还应重写 Object 的 `hashCode` getter。有关重写 `==` 和 `hashCode` 的示例，请查看实现映射键。

## noSuchMethod()

To detect or react whenever code attempts to use a non-existent method or instance variable, you can override `noSuchMethod()`:
要检测或响应代码尝试使用不存在的方法或实例变量的情况，您可以重写 `noSuchMethod()` ：

```dart
class A {
  // Unless you override noSuchMethod, using a
  // non-existent member results in a NoSuchMethodError.
  @override
  void noSuchMethod(Invocation invocation) {
    print('You tried to use a non-existent member: '
        '${invocation.memberName}');
  }
}
```

You **can’t invoke** an unimplemented method unless **one** of the following is true:
除非满足以下条件之一，否则无法调用未实现的方法：

- The receiver has the static type `dynamic`.
  接收器具有静态类型 `dynamic` 。
- The receiver has a static type that defines the unimplemented method (abstract is OK), and the dynamic type of the receiver has an implementation of `noSuchMethod()` that’s different from the one in class `Object`.
  接收器具有定义未实现方法的静态类型（抽象是可以的），并且接收器的动态类型具有与类 `Object` 中的实现不同的 `noSuchMethod()` 实现。

For more information, see the informal [noSuchMethod forwarding specification.](https://github.com/dart-lang/language/blob/main/archive/feature-specifications/nosuchmethod-forwarding.md)
有关更多信息，请参阅非正式的 noSuchMethod 转发规范。
