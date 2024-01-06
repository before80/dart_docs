+++
title = "分支"
date = 2024-01-05T20:29:36+08:00
weight = 10
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文: [https://dart.dev/language/branches](https://dart.dev/language/branches)

## Branches 分支

This page shows how you can control the flow of your Dart code using branches:
此页面展示了如何使用分支控制 Dart 代码的流程：

- `if` statements and elements
  `if` 语句和元素
- `if-case` statements and elements
  `if-case` 语句和元素
- `switch` statements and expressions
  `switch` 语句和表达式

You can also manipulate control flow in Dart using:
您还可以使用以下方式在 Dart 中控制控制流：

- [Loops](https://dart.dev/language/loops), like `for` and `while`
  循环，例如 `for` 和 `while`
- [Exceptions](https://dart.dev/language/error-handling), like `try`, `catch`, and `throw`
  异常，如 `try` 、 `catch` 和 `throw`

## If

Dart supports `if` statements with optional `else` clauses. The condition in parentheses after `if` must be an expression that evaluates to a [boolean](https://dart.dev/language/built-in-types#booleans):
Dart 支持带有可选 `else` 子句的 `if` 语句。在 `if` 之后的括号中的条件必须是求值为布尔值的一个表达式：

```dart
if (isRaining()) {
  you.bringRainCoat();
} else if (isSnowing()) {
  you.wearJacket();
} else {
  car.putTopDown();
}
```

To learn how to use `if` in an expression context, check out [Conditional expressions](https://dart.dev/language/operators#conditional-expressions).
要了解如何在表达式上下文中使用 `if` ，请查看条件表达式。

### If-case

Dart `if` statements support `case` clauses followed by a [pattern](https://dart.dev/language/patterns):
Dart `if` 语句支持 `case` 子句，后跟一个模式：

```dart
if (pair case [int x, int y]) return Point(x, y);
```

If the pattern matches the value, then the branch executes with any variables the pattern defines in scope.
如果模式与值匹配，则该分支将使用模式在作用域中定义的任何变量执行。

In the previous example, the list pattern `[int x, int y]` matches the value `pair`, so the branch `return Point(x, y)` executes with the variables that the pattern defined, `x` and `y`.
在前面的示例中，列表模式 `[int x, int y]` 与值 `pair` 匹配，因此分支 `return Point(x, y)` 使用模式定义的变量 `x` 和 `y` 执行。

Otherwise, control flow progresses to the `else` branch to execute, if there is one:
否则，如果存在，控制流将继续执行 `else` 分支：

```dart
if (pair case [int x, int y]) {
  print('Was coordinate array $x,$y');
} else {
  throw FormatException('Invalid coordinates.');
}
```

The if-case statement provides a way to match and [destructure](https://dart.dev/language/patterns#destructuring) against a *single* pattern. To test a value against *multiple* patterns, use [switch](https://dart.dev/language/branches#switch).
if-case 语句提供了一种针对单个模式进行匹配和解构的方法。要针对多个模式测试值，请使用 switch。

*merge_type* **Version note:** Case clauses in if statements require a [language version](https://dart.dev/guides/language/evolution#language-versioning) of at least 3.0.
版本说明：if 语句中的 case 子句要求语言版本至少为 3.0。



## Switch statements Switch 语句

A `switch` statement evaluates a value expression against a series of cases. Each `case` clause is a [pattern](https://dart.dev/language/patterns) for the value to match against. You can use [any kind of pattern](https://dart.dev/language/pattern-types) for a case.
一个 `switch` 语句针对一系列 case 评估一个值表达式。每个 `case` 子句都是要与值匹配的模式。您可以对 case 使用任何类型的模式。

When the value matches a case’s pattern, the case body executes. Non-empty `case` clauses jump to the end of the switch after completion. They do not require a `break` statement. Other valid ways to end a non-empty `case` clause are a [`continue`](https://dart.dev/language/loops#break-and-continue), [`throw`](https://dart.dev/language/error-handling#throw), or [`return`](https://dart.dev/language/functions#return-values) statement.
当值与 case 的模式匹配时，case 主体将执行。非空 `case` 子句在完成后跳转到 switch 的末尾。它们不需要 `break` 语句。结束非空 `case` 子句的其他有效方法是 `continue` 、 `throw` 或 `return` 语句。

Use a `default` or [wildcard `_`](https://dart.dev/language/pattern-types#wildcard) clause to execute code when no `case` clause matches:
使用 `default` 或通配符 `_` 子句在没有 `case` 子句匹配时执行代码：

```dart
var command = 'OPEN';
switch (command) {
  case 'CLOSED':
    executeClosed();
  case 'PENDING':
    executePending();
  case 'APPROVED':
    executeApproved();
  case 'DENIED':
    executeDenied();
  case 'OPEN':
    executeOpen();
  default:
    executeUnknown();
}
```



Empty cases fall through to the next case, allowing cases to share a body. For an empty case that does not fall through, use [`break`](https://dart.dev/language/loops#break-and-continue) for its body. For non-sequential fall-through, you can use a [`continue` statement](https://dart.dev/language/loops#break-and-continue) and a label:
空 case 会进入下一个 case，允许 case 共享一个主体。对于不进入下一个 case 的空 case，请为其主体使用 `break` 。对于非顺序进入下一个 case，可以使用 `continue` 语句和一个标签：

```dart
switch (command) {
  case 'OPEN':
    executeOpen();
    continue newCase; // Continues executing at the newCase label.

  case 'DENIED': // Empty case falls through.
  case 'CLOSED':
    executeClosed(); // Runs for both DENIED and CLOSED,

  newCase:
  case 'PENDING':
    executeNowClosed(); // Runs for both OPEN and PENDING.
}
```

You can use [logical-or patterns](https://dart.dev/language/patterns#or-pattern-switch) to allow cases to share a body or a guard. To learn more about patterns and case clauses, check out the patterns documentation on [Switch statements and expressions](https://dart.dev/language/patterns#switch-statements-and-expressions).
可以使用逻辑或模式来允许 case 共享一个主体或一个保护。要详细了解模式和 case 子句，请查看 Switch 语句和表达式的模式文档。

### Switch expressions Switch 表达式

A `switch` *expression* produces a value based on the expression body of whichever case matches. You can use a switch expression wherever Dart allows expressions, *except* at the start of an expression statement. For example:
一个 `switch` 表达式会根据匹配的 case 的表达式主体生成一个值。可以在 Dart 允许表达式的任何地方使用 switch 表达式，但不能在表达式语句的开头使用。例如：

```
var x = switch (y) { ... };

print(switch (x) { ... });

return switch (x) { ... };
```

If you want to use a switch at the start of an expression statement, use a [switch statement](https://dart.dev/language/branches#switch-statements).
如果要在表达式语句的开头使用 switch，请使用 switch 语句。

Switch expressions allow you to rewrite a switch *statement* like this:
Switch 表达式允许你将类似这样的 switch 语句重写为：

```dart
// Where slash, star, comma, semicolon, etc., are constant variables...
switch (charCode) {
  case slash || star || plus || minus: // Logical-or pattern
    token = operator(charCode);
  case comma || semicolon: // Logical-or pattern
    token = punctuation(charCode);
  case >= digit0 && <= digit9: // Relational and logical-and patterns
    token = number();
  default:
    throw FormatException('Invalid');
}
```

Into an *expression*, like this:
一个表达式，类似这样：

```dart
token = switch (charCode) {
  slash || star || plus || minus => operator(charCode),
  comma || semicolon => punctuation(charCode),
  >= digit0 && <= digit9 => number(),
  _ => throw FormatException('Invalid')
};
```

The syntax of a `switch` expression differs from `switch` statement syntax:
`switch` 表达式的语法不同于 `switch` 语句语法：

- Cases *do not* start with the `case` keyword.
  Case 不会以 `case` 关键字开头。
- A case body is a single expression instead of a series of statements.
  案例主体是一个单独的表达式，而不是一系列语句。
- Each case must have a body; there is no implicit fallthrough for empty cases.
  每个案例都必须有一个主体；对于空案例，没有隐式贯穿。
- Case patterns are separated from their bodies using `=>` instead of `:`.
  案例模式使用 `=>` 与其主体分隔，而不是使用 `:` 。
- Cases are separated by `,` (and an optional trailing `,` is allowed).
  案例由 `,` 分隔（并且允许一个可选的尾随 `,` ）。
- Default cases can *only* use `_`, instead of allowing both `default` and `_`.
  默认案例只能使用 `_` ，而不是允许同时使用 `default` 和 `_` 。

*merge_type* **Version note:** Switch expressions require a [language version](https://dart.dev/guides/language/evolution#language-versioning) of at least 3.0.
版本说明：Switch 表达式至少需要 3.0 的语言版本。

### Exhaustiveness checking 详尽性检查

Exhaustiveness checking is a feature that reports a compile-time error if it’s possible for a value to enter a switch but not match any of the cases.
详尽性检查是一项功能，如果某个值有可能进入 switch 但与任何案例都不匹配，则会报告编译时错误。

```dart
// Non-exhaustive switch on bool?, missing case to match null possibility:
switch (nullableBool) {
  case true:
    print('yes');
  case false:
    print('no');
}
```

A default case (`default` or `_`) covers all possible values that can flow through a switch. This makes a switch on any type exhaustive.
默认案例（ `default` 或 `_` ）涵盖了所有可能通过 switch 流动值。这使得对任何类型的 switch 都详尽无遗。

[Enums](https://dart.dev/language/enums) and [sealed types](https://dart.dev/language/class-modifiers#sealed) are particularly useful for switches because, even without a default case, their possible values are known and fully enumerable. Use the [`sealed` modifier](https://dart.dev/language/class-modifiers#sealed) on a class to enable exhaustiveness checking when switching over subtypes of that class:
枚举和密封类型对于 switch 特别有用，因为即使没有默认案例，它们可能的值也是已知的并且完全可枚举的。在切换该类的子类型时，对类使用 `sealed` 修饰符以启用详尽性检查：

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

If anyone were to add a new subclass of `Shape`, this `switch` expression would be incomplete. Exhaustiveness checking would inform you of the missing subtype. This allows you to use Dart in a somewhat [functional algebraic datatype style](https://en.wikipedia.org/wiki/Algebraic_data_type).
如果有人要添加 `Shape` 的新子类，此 `switch` 表达式将不完整。详尽性检查会通知您缺少的子类型。这允许您以某种函数代数数据类型样式使用 Dart。



## Guard clause 卫语句

To set an optional guard clause after a `case` clause, use the keyword `when`. A guard clause can follow `if case`, and both `switch` statements and expressions.
要在 `case` 子句后设置可选的保护子句，请使用关键字 `when` 。保护子句可以紧跟 `if case` ，以及 `switch` 语句和表达式。

```
// Switch statement:
switch (something) {
  case somePattern when some || boolean || expression:
    //             ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^ Guard clause.
    body;
}

// Switch expression:
var value = switch (something) {
  somePattern when some || boolean || expression => body,
  //               ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^ Guard clause.
}

// If-case statement:
if (something case somePattern when some || boolean || expression) {
  //                           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^ Guard clause.
  body;
}
```

Guards evaluate an arbitrary boolean expression *after* matching. This allows you to add further constraints on whether a case body should execute. When the guard clause evaluates to false, execution proceeds to the next case rather than exiting the entire switch.
保护在匹配后评估任意布尔表达式。这允许您添加进一步的约束条件，以确定是否应执行 case 主体。当保护子句评估为 false 时，执行将继续进行到下一个 case，而不是退出整个 switch。
