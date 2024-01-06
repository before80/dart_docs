+++
title = "Enums"
date = 2024-01-05T20:29:36+08:00
weight = 50
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文: [https://dart.dev/language/enums](https://dart.dev/language/enums)

# Enumerated types 枚举类型

Enumerated types, often called *enumerations* or *enums*, are a special kind of class used to represent a fixed number of constant values.
枚举类型（通常称为枚举或枚举）是一种特殊的类，用于表示固定数量的常量值。

*info* **Note:** All enums automatically extend the [`Enum`](https://api.dart.dev/stable/dart-core/Enum-class.html) class. They are also sealed, meaning they cannot be subclassed, implemented, mixed in, or otherwise explicitly instantiated.
注意：所有枚举都会自动扩展 `Enum` 类。它们也是密封的，这意味着它们不能被子类化、实现、混合或以其他方式显式实例化。

Abstract classes and mixins can explicitly implement or extend `Enum`, but unless they are then implemented by or mixed into an enum declaration, no objects can actually implement the type of that class or mixin.
抽象类和 mixin 可以显式实现或扩展 `Enum` ，但除非它们随后由枚举声明实现或混合，否则没有任何对象可以实际实现该类或 mixin 的类型。

## Declaring simple enums 声明简单枚举

To declare a simple enumerated type, use the `enum` keyword and list the values you want to be enumerated:
要声明一个简单的枚举类型，请使用 `enum` 关键字并列出要枚举的值：

```dart
enum Color { red, green, blue }
```

*tips_and_updates* **Tip:** You can also use [trailing commas](https://dart.dev/language/collections#lists) when declaring an enumerated type to help prevent copy-paste errors.
提示：您还可以在声明枚举类型时使用尾随逗号，以帮助防止复制粘贴错误。

## Declaring enhanced enums 声明增强枚举

Dart also allows enum declarations to declare classes with fields, methods, and const constructors which are limited to a fixed number of known constant instances.
Dart 还允许枚举声明声明具有字段、方法和常量构造函数的类，这些类仅限于固定数量的已知常量实例。

To declare an enhanced enum, follow a syntax similar to normal [classes](https://dart.dev/language/classes), but with a few extra requirements:
要声明增强枚举，请遵循类似于普通类的语法，但有一些额外的要求：

- Instance variables must be `final`, including those added by [mixins](https://dart.dev/language/mixins).
  实例变量必须是 `final` ，包括由 mixin 添加的变量。
- All [generative constructors](https://dart.dev/language/constructors#constant-constructors) must be constant.
  所有生成构造函数都必须是常量。
- [Factory constructors](https://dart.dev/language/constructors#factory-constructors) can only return one of the fixed, known enum instances.
  工厂构造函数只能返回一个固定的已知枚举实例。
- No other class can be extended as [`Enum`](https://api.dart.dev/stable/dart-core/Enum-class.html) is automatically extended.
  没有其他类可以扩展为 `Enum` ，因为 `Enum` 会自动扩展。
- There cannot be overrides for `index`, `hashCode`, the equality operator `==`.
  不能对 `index` 、 `hashCode` 、相等运算符 `==` 进行重写。
- A member named `values` cannot be declared in an enum, as it would conflict with the automatically generated static `values` getter.
  枚举中不能声明名为 `values` 的成员，因为它会与自动生成的静态 `values` getter 发生冲突。
- All instances of the enum must be declared in the beginning of the declaration, and there must be at least one instance declared.
  枚举的所有实例都必须在声明的开头声明，并且必须至少声明一个实例。

Instance methods in an enhanced enum can use `this` to reference the current enum value.
增强枚举中的实例方法可以使用 `this` 来引用当前枚举值。

Here is an example that declares an enhanced enum with multiple instances, instance variables, getters, and an implemented interface:
这是一个声明具有多个实例、实例变量、getter 和已实现接口的增强枚举的示例：

```dart
enum Vehicle implements Comparable<Vehicle> {
  car(tires: 4, passengers: 5, carbonPerKilometer: 400),
  bus(tires: 6, passengers: 50, carbonPerKilometer: 800),
  bicycle(tires: 2, passengers: 1, carbonPerKilometer: 0);

  const Vehicle({
    required this.tires,
    required this.passengers,
    required this.carbonPerKilometer,
  });

  final int tires;
  final int passengers;
  final int carbonPerKilometer;

  int get carbonFootprint => (carbonPerKilometer / passengers).round();

  bool get isTwoWheeled => this == Vehicle.bicycle;

  @override
  int compareTo(Vehicle other) => carbonFootprint - other.carbonFootprint;
}
```

*merge_type* **Version note:** Enhanced enums require a [language version](https://dart.dev/guides/language/evolution#language-versioning) of at least 2.17.
版本说明：增强枚举需要至少为 2.17 的语言版本。

## Using enums 使用枚举

Access the enumerated values like any other [static variable](https://dart.dev/language/classes#class-variables-and-methods):
像访问任何其他静态变量一样访问枚举值：

```dart
final favoriteColor = Color.blue;
if (favoriteColor == Color.blue) {
  print('Your favorite color is blue!');
}
```

Each value in an enum has an `index` getter, which returns the zero-based position of the value in the enum declaration. For example, the first value has index 0, and the second value has index 1.
枚举中的每个值都有一个 `index` getter，它返回该值在枚举声明中的基于零的位置。例如，第一个值具有索引 0，第二个值具有索引 1。

```dart
assert(Color.red.index == 0);
assert(Color.green.index == 1);
assert(Color.blue.index == 2);
```

To get a list of all the enumerated values, use the enum’s `values` constant.
要获取所有枚举值列表，请使用枚举的 `values` 常量。

```dart
List<Color> colors = Color.values;
assert(colors[2] == Color.blue);
```

You can use enums in [switch statements](https://dart.dev/language/branches#switch), and you’ll get a warning if you don’t handle all of the enum’s values:
您可以在 switch 语句中使用枚举，如果您没有处理枚举的所有值，您将收到警告：

```dart
var aColor = Color.blue;

switch (aColor) {
  case Color.red:
    print('Red as roses!');
  case Color.green:
    print('Green as grass!');
  default: // Without this, you see a WARNING.
    print(aColor); // 'Color.blue'
}
```

If you need to access the name of an enumerated value, such as `'blue'` from `Color.blue`, use the `.name` property:
如果您需要访问枚举值的名称，例如 `'blue'` 来自 `Color.blue` ，请使用 `.name` 属性：

```dart
print(Color.blue.name); // 'blue'
```

You can access a member of an enum value like you would on a normal object:
您可以像访问普通对象一样访问枚举值的一个成员：

```dart
print(Vehicle.car.carbonFootprint);
```
