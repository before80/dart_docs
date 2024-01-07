+++
title = "模式的概览和用法"
date = 2024-01-05T20:29:36+08:00
weight = 1
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文: [https://dart.dev/language/patterns](https://dart.dev/language/patterns)

# Patterns 模式

*merge_type* **Version note:** Patterns require a [language version](https://dart.dev/guides/language/evolution#language-versioning) of at least 3.0.

​	版本说明：模式需要至少为 3.0 的语言版本。

Patterns are a syntactic category in the Dart language, like statements and expressions. A pattern represents the shape of a set of values that it may match against actual values.

​	模式是 Dart 语言中的一个语法类别，例如语句和表达式。模式表示一组值的形式，它可以与实际值进行匹配。

This page describes:

​	此页面介绍：

- What patterns do.
- 模式的作用。
- Where patterns are allowed in Dart code.
- 模式在 Dart 代码中允许出现的位置。
- What the common use cases for patterns are.
- 模式的常见用例。

To learn about the different kinds of patterns, visit the [pattern types](https://dart.dev/language/pattern-types) page.

​	要了解不同种类的模式，请访问模式类型页面。

## 模式的作用 What patterns do 

In general, a pattern may **match** a value, **destructure** a value, or both, depending on the context and shape of the pattern.

​	通常，模式可以匹配值、解构值或同时执行这两项操作，具体取决于模式的上下文和形式。

First, *pattern matching* allows you to check whether a given value:

​	首先，模式匹配允许您检查给定值：

- Has a certain shape.
- 是否具有某种形状。
- Is a certain constant.
- 是否为某个常量。
- Is equal to something else.
- 是否等于其他内容。
- Has a certain type.
- 是否具有某种类型。

Then, *pattern destructuring* provides you with a convenient declarative syntax to break that value into its constituent parts. The same pattern can also let you bind variables to some or all of those parts in the process.

​	然后，模式解构为您提供了一种便捷的声明性语法，可以将该值分解为其组成部分。同一个模式还可以让您将变量绑定到这些部分中的一些或全部部分。

### 匹配 Matching 

A pattern always tests against a value to determine if the value has the form you expect. In other words, you are checking if the value *matches* the pattern.

​	模式始终针对值进行测试，以确定该值是否具有您期望的形式。换句话说，您正在检查该值是否与模式匹配。

What constitutes a match depends on [what kind of pattern](https://dart.dev/language/pattern-types) you are using. For example, a constant pattern matches if the value is equal to the pattern’s constant:

​	什么构成匹配取决于您使用哪种模式。例如，如果值等于模式的常量，则常量模式匹配：

```dart
switch (number) {
  // Constant pattern matches if 1 == number.
  case 1:
    print('one');
}
```

Many patterns make use of subpatterns, sometimes called *outer* and *inner* patterns, respectively. Patterns match recursively on their subpatterns. For example, the individual fields of any [collection-type](https://dart.dev/language/collections) pattern could be [variable patterns](https://dart.dev/language/pattern-types#variable) or [constant patterns](https://dart.dev/language/pattern-types#constant):

​	许多模式使用子模式，有时分别称为外部模式和内部模式。模式在其子模式上递归匹配。例如，任何集合类型模式的各个字段都可以是变量模式或常量模式：

```dart
const a = 'a';
const b = 'b';
switch (obj) {
  // List pattern [a, b] matches obj first if obj is a list with two fields,
  // then if its fields match the constant subpatterns 'a' and 'b'.
  case [a, b]:
    print('$a, $b');
}
```

To ignore parts of a matched value, you can use a [wildcard pattern](https://dart.dev/language/pattern-types#wildcard) as a placeholder. In the case of list patterns, you can use a [rest element](https://dart.dev/language/pattern-types#rest-element).

​	要忽略匹配值的部分，您可以使用通配符模式作为占位符。对于列表模式，可以使用 rest 元素。

### 解构 Destructuring 

When an object and pattern match, the pattern can then access the object’s data and extract it in parts. In other words, the pattern *destructures* the object:

​	当对象和模式匹配时，模式可以访问对象的数据并将其分解成各个部分。换句话说，模式对对象进行解构：

```dart
var numList = [1, 2, 3];
// List pattern [a, b, c] destructures the three elements from numList...
var [a, b, c] = numList;
// ...and assigns them to new variables.
print(a + b + c);
```

You can nest [any kind of pattern](https://dart.dev/language/pattern-types) inside a destructuring pattern. For example, this case pattern matches and destructures a two-element list whose first element is `'a'` or `'b'`:

​	您可以在解构模式中嵌套任何类型的模式。例如，此 case 模式匹配并解构了一个由两个元素组成的列表，其第一个元素是 `'a'` 或 `'b'` ：

```dart
switch (list) {
  case ['a' || 'b', var c]:
    print(c);
}
```

## 模式可以出现的位置 Places patterns can appear 

You can use patterns in several places in the Dart language:

​	您可以在 Dart 语言的几个地方使用模式：

- Local variable [declarations](https://dart.dev/language/patterns#variable-declaration) and [assignments](https://dart.dev/language/patterns#variable-assignment)
- 局部变量声明和赋值
- [for and for-in loops](https://dart.dev/language/loops#for-loops)
- for 和 for-in 循环
- [if-case](https://dart.dev/language/branches#if-case) and [switch-case](https://dart.dev/language/branches#switch-statements)
- if-case 和 switch-case
- Control flow in [collection literals](https://dart.dev/language/collections#control-flow-operators)
- 集合字面量中的控制流

This section describes common use cases for matching and destructuring with patterns.

​	本部分介绍使用模式进行匹配和解构的常见用例。

### 变量声明 Variable declaration 

You can use a *pattern variable declaration* anywhere Dart allows local variable declaration. The pattern matches against the value on the right of the declaration. Once matched, it destructures the value and binds it to new local variables:

​	您可以在 Dart 允许本地变量声明的任何地方使用模式变量声明。该模式与声明右侧的值进行匹配。匹配后，它会解构该值并将其绑定到新的本地变量：

```dart
// Declares new variables a, b, and c.
var (a, [b, c]) = ('str', [1, 2]);
```

A pattern variable declaration must start with either `var` or `final`, followed by a pattern.

​	模式变量声明必须以 `var` 或 `final` 开头，后跟一个模式。

### 变量赋值 Variable assignment 

A *variable assignment pattern* falls on the left side of an assignment. First, it destructures the matched object. Then it assigns the values to *existing* variables, instead of binding new ones.

​	变量赋值模式位于赋值的左侧。首先，它会解构匹配的对象。然后，它将值分配给现有变量，而不是绑定新变量。

Use a variable assignment pattern to swap the values of two variables without declaring a third temporary one:
使用变量赋值模式可以交换两个变量的值，而无需声明第三个临时变量：

```dart
var (a, b) = ('left', 'right');
(b, a) = (a, b); // Swap.
print('$a $b'); // Prints "right left".
```

### switch 语句和表达式 Switch statements and expressions 

Every case clause contains a pattern. This applies to [switch statements](https://dart.dev/language/branches#switch-statements) and [expressions](https://dart.dev/language/branches#switch-expressions), as well as [if-case statements](https://dart.dev/language/branches#if-case). You can use [any kind of pattern](https://dart.dev/language/pattern-types) in a case.

​	每个 case 子句都包含一个模式。这适用于 switch 语句和表达式，也适用于 if-case 语句。您可以在 case 中使用任何类型的模式。

*Case patterns* are [refutable](https://dart.dev/resources/glossary#refutable-pattern). They allow control flow to either:

​	Case 模式是可反驳的。它们允许控制流：

- Match and destructure the object being switched on.
- 匹配并解构正在切换的对象。
- Continue execution if the object doesn’t match.
- 如果对象不匹配，则继续执行。

The values that a pattern destructures in a case become local variables. Their scope is only within the body of that case.

​	模式在 case 中解构的值变为局部变量。它们的范围仅限于该 case 的主体中。

```dart
switch (obj) {
  // Matches if 1 == obj.
  case 1:
    print('one');

  // Matches if the value of obj is between the
  // constant values of 'first' and 'last'.
  case >= first && <= last:
    print('in range');

  // Matches if obj is a record with two fields,
  // then assigns the fields to 'a' and 'b'.
  case (var a, var b):
    print('a = $a, b = $b');

  default:
}
```



[Logical-or patterns](https://dart.dev/language/pattern-types#logical-or) are useful for having multiple cases share a body in switch expressions or statements:

​	逻辑或模式对于让多个 case 在 switch 表达式或语句中共享主体非常有用：

```dart
var isPrimary = switch (color) {
  Color.red || Color.yellow || Color.blue => true,
  _ => false
};
```

Switch statements can have multiple cases share a body [without using logical-or patterns](https://dart.dev/language/branches#switch-share), but they are still uniquely useful for allowing multiple cases to share a [guard](https://dart.dev/language/branches#guard-clause):

​	Switch 语句可以使用逻辑或模式让多个 case 共享主体，但它们对于允许多个 case 共享保护仍然非常有用：

```dart
switch (shape) {
  case Square(size: var s) || Circle(size: var s) when s > 0:
    print('Non-empty symmetric shape');
}
```

[Guard clauses](https://dart.dev/language/branches#guard-clause) evaluate an arbitrary conditon as part of a case, without exiting the switch if the condition is false (like using an `if` statement in the case body would cause).

​	守卫子句评估任意条件作为 case 的一部分，如果条件为假，则不会退出 switch（就像在 case 主体中使用 `if` 语句会造成的那样）。

```dart
switch (pair) {
  case (int a, int b):
    if (a > b) print('First element greater');
  // If false, prints nothing and exits the switch.
  case (int a, int b) when a > b:
    // If false, prints nothing but proceeds to next case.
    print('First element greater');
  case (int a, int b):
    print('First element not greater');
}
```

### for 和 for-in 循环 - For and for-in loops 

You can use patterns in [for and for-in loops](https://dart.dev/language/loops#for-loops) to iterate-over and destructure values in a collection.

​	您可以在 for 和 for-in 循环中使用模式来迭代和解构集合中的值。

This example uses [object destructuring](https://dart.dev/language/pattern-types#object) in a for-in loop to destructure the [`MapEntry`](https://api.dart.dev/stable/dart-core/MapEntry-class.html) objects that a `<Map>.entries` call returns:

​	此示例在 for-in 循环中使用对象解构来解构 `<Map>.entries` 调用返回的 `MapEntry` 对象：

```dart
Map<String, int> hist = {
  'a': 23,
  'b': 100,
};

for (var MapEntry(key: key, value: count) in hist.entries) {
  print('$key occurred $count times');
}
```

The object pattern checks that `hist.entries` has the named type `MapEntry`, and then recurses into the named field subpatterns `key` and `value`. It calls the `key` getter and `value` getter on the `MapEntry` in each iteration, and binds the results to local variables `key` and `count`, respectively.

​	对象模式检查 `hist.entries` 是否具有命名类型 `MapEntry` ，然后递归到命名字段子模式 `key` 和 `value` 。它在每次迭代中调用 `MapEntry` 上的 `key` getter 和 `value` getter，并将结果分别绑定到局部变量 `key` 和 `count` 。

Binding the result of a getter call to a variable of the same name is a common use case, so object patterns can also infer the getter name from the [variable subpattern](https://dart.dev/language/pattern-types#variable). This allows you to simplify the variable pattern from something redundant like `key: key` to just `:key`:

​	将 getter 调用的结果绑定到同名变量是一种常见用例，因此对象模式还可以从变量子模式推断 getter 名称。这允许您将变量模式从冗余内容（如 `key: key` ）简化为 `:key` ：

```dart
for (var MapEntry(:key, value: count) in hist.entries) {
  print('$key occurred $count times');
}
```

## 模式的用例 Use cases for patterns 

The [previous section](https://dart.dev/language/patterns#places-patterns-can-appear) describes *how* patterns fit into other Dart code constructs. You saw some interesting use cases as examples, like [swapping](https://dart.dev/language/patterns#variable-assignment) the values of two variables, or [destructuring key-value pairs](https://dart.dev/language/patterns#for-and-for-in-loops) in a map. This section describes even more use cases, answering:

​	上一节介绍了模式如何适应其他 Dart 代码结构。您看到了一些有趣的用例作为示例，例如交换两个变量的值，或解构映射中的键值对。本节介绍了更多用例，回答了以下问题：

- *When and why* you might want to use patterns.
- 您何时以及为何可能想要使用模式。
- What kinds of problems they solve.
- 它们解决哪些类型的问题。
- Which idioms they best suit.
- 最适合哪些习语。

### 解构多个返回值 Destructuring multiple returns 

[Records](https://dart.dev/language/records) allow aggregating and returning multiple values from a single function call. Patterns add the ability to destructure a record’s fields directly into local variables, inline with the function call.

​	记录允许从单个函数调用聚合并返回多个值。模式增加了将记录的字段直接解构到局部变量中的能力，与函数调用保持一致。

Instead of individually declaring new local variables for each record field, like this:

​	不必为每个记录字段单独声明新的局部变量，如下所示：

```dart
var info = userInfo(json);
var name = info.$1;
var age = info.$2;
```

You can destructure the fields of a record that a function returns into local variables using a [variable declaration](https://dart.dev/language/patterns#variable-declaration) or [assigment pattern](https://dart.dev/language/patterns#variable-assignment), and a record pattern as its subpattern:

​	您可以使用变量声明或赋值模式以及记录模式作为其子模式，将函数返回的记录的字段解构到局部变量中：

```dart
var (name, age) = userInfo(json);
```

### 解构类实例 Destructuring class instances 

[Object patterns](https://dart.dev/language/pattern-types#object) match against named object types, allowing you to destructure their data using the getters the object’s class already exposes.

​	对象模式与命名的对象类型匹配，允许您使用对象类已经公开的 getter 解构其数据。

To destructure an instance of a class, use the named type, followed by the properties to destructure enclosed in parentheses:
要解构类的实例，请使用命名类型，后跟用括号括起来要解构的属性：

```dart
final Foo myFoo = Foo(one: 'one', two: 2);
var Foo(:one, :two) = myFoo;
print('one $one, two $two');
```

### 代数数据类型 Algebraic data types 

Object destructuring and switch cases are conducive to writing code in an [algebraic data type](https://en.wikipedia.org/wiki/Algebraic_data_type) style. Use this method when:

​	对象解构和 switch 用例有利于以代数数据类型样式编写代码。在以下情况下使用此方法：

- You have a family of related types.
- 您有一系列相关类型。
- You have an operation that needs specific behavior for each type.
- 您有一个需要针对每种类型执行特定行为的操作。
- You want to group that behavior in one place instead of spreading it across all the different type definitions.
- 您想将该行为分组在一个地方，而不是将其分散到所有不同的类型定义中。

Instead of implementing the operation as an instance method for every type, keep the operation’s variations in a single function that switches over the subtypes:

​	不要将操作实现为每种类型的实例方法，而是将操作的变化保存在一个函数中，该函数在子类型上进行切换：

```dart
sealed class Shape {}

class Square implements Shape {
  final double length;
  Square(this.length);
}

class Circle implements Shape {
  final double radius;
  Circle(this.radius);
}

double calculateArea(Shape shape) => switch (shape) {
      Square(length: var l) => l * l,
      Circle(radius: var r) => math.pi * r * r
    };
```

### 验证传入的 JSON - Validating incoming JSON 

[Map](https://dart.dev/language/pattern-types#map) and [list](https://dart.dev/language/pattern-types#list) patterns work well for destructuring key-value pairs in JSON data:

​	映射和列表模式非常适合解构 JSON 数据中的键值对：

```dart
var json = {
  'user': ['Lily', 13]
};
var {'user': [name, age]} = json;
```

If you know that the JSON data has the structure you expect, the previous example is realistic. But data typically comes from an external source, like over the network. You need to validate it first to confirm its structure.

​	如果您知道 JSON 数据具有您期望的结构，那么前面的示例是切合实际的。但数据通常来自外部来源，例如网络。您需要先对其进行验证以确认其结构。

Without patterns, validation is verbose:

​	如果没有模式，验证就会很冗长：

```dart
if (json is Map<String, Object?> &&
    json.length == 1 &&
    json.containsKey('user')) {
  var user = json['user'];
  if (user is List<Object> &&
      user.length == 2 &&
      user[0] is String &&
      user[1] is int) {
    var name = user[0] as String;
    var age = user[1] as int;
    print('User $name is $age years old.');
  }
}
```

A single [case pattern](https://dart.dev/language/patterns#switch-statements-and-expressions) can achieve the same validation. Single cases work best as [if-case](https://dart.dev/language/branches#if-case) statements. Patterns provide a more declarative, and much less verbose method of validating JSON:

​	一个单一 case 模式可以实现相同的验证。单一 case 最好作为 if-case 语句使用。模式提供了一种更具声明性且冗长性更低的方法来验证 JSON：

```dart
if (json case {'user': [String name, int age]}) {
  print('User $name is $age years old.');
}
```

This case pattern simultaneously validates that:

​	此案例模式同时验证：

- `json` is a map, because it must first match the outer map pattern to proceed.
- `json` 是一个映射，因为它必须首先匹配外部映射模式才能继续。
  - And, since it’s a map, it also confirms `json` is not null.
  - 并且，由于它是一个映射，它还确认 `json` 不为 null。
- `json` contains a key `user`.
- `json` 包含一个键 `user` 。
- The key `user` pairs with a list of two values.
- 键 `user` 与两个值的列表配对。
- The types of the list values are `String` and `int`.
- 列表值类型为 `String` 和 `int` 。
- The new local variables to hold the values are `String` and `int`.
- 用于保存值的新的局部变量为 `String` 和 `int` 。
