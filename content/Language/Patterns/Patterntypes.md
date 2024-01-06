+++
title = "模式类型"
date = 2024-01-05T20:29:36+08:00
weight = 10
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文: [https://dart.dev/language/pattern-types](https://dart.dev/language/pattern-types)

## Pattern types 模式类型

This page is a reference for the different kinds of patterns. For an overview of how patterns work, where you can use them in Dart, and common use cases, visit the main [Patterns](https://dart.dev/language/patterns) page.
此页面是不同类型模式的参考。有关模式的工作原理、可以在 Dart 中使用它们的位置以及常见用例的概述，请访问主模式页面。

#### Pattern precedence 模式优先级

Similar to [operator precedence](https://dart.dev/language/operators#operator-precedence-example), pattern evaluation adheres to precedence rules. You can use [parenthesized patterns](https://dart.dev/language/pattern-types#parenthesized) to evaluate lower-precedence patterns first.
与运算符优先级类似，模式评估遵循优先级规则。您可以使用带括号的模式来首先评估较低优先级的模式。

This document lists the pattern types in ascending order of precedence:
此文档按升序列出了模式类型：

- [Logical-or](https://dart.dev/language/pattern-types#logical-or) patterns are lower-precedence than [logical-and](https://dart.dev/language/pattern-types#logical-and), logical-and patterns are lower-precedence than [relational](https://dart.dev/language/pattern-types#relational) patterns, and so on.
  逻辑或模式的优先级低于逻辑与，逻辑与模式的优先级低于关系模式，依此类推。
- Post-fix unary patterns ([cast](https://dart.dev/language/pattern-types#cast), [null-check](https://dart.dev/language/pattern-types#null-check), and [null-assert](https://dart.dev/language/pattern-types#null-assert)) share the same level of precedence.
  后缀一元模式（强制转换、空检查和空断言）具有相同的优先级。
- The remaining primary patterns share the highest precedence. Collection-type ([record](https://dart.dev/language/pattern-types#record), [list](https://dart.dev/language/pattern-types#list), and [map](https://dart.dev/language/pattern-types#map)) and [Object](https://dart.dev/language/pattern-types#object) patterns encompass other data, so are evaluated first as outer-patterns.
  其余的基本模式具有最高的优先级。集合类型（记录、列表和映射）和对象模式包含其他数据，因此首先作为外部模式进行评估。

## Logical-or 逻辑或

```
subpattern1 || subpattern2
```

A logical-or pattern separates subpatterns by `||` and matches if any of the branches match. Branches are evaluated left-to-right. Once a branch matches, the rest are not evaluated.
逻辑或模式通过 `||` 分隔子模式，如果任何分支匹配，则匹配。分支从左到右进行评估。一旦某个分支匹配，则不会评估其余分支。

```dart
var isPrimary = switch (color) {
  Color.red || Color.yellow || Color.blue => true,
  _ => false
};
```

Subpatterns in a logical-or pattern can bind variables, but the branches must define the same set of variables, because only one branch will be evaluated when the pattern matches.
逻辑或模式中的子模式可以绑定变量，但分支必须定义相同的变量集，因为在模式匹配时只会评估一个分支。

## Logical-and 逻辑与

```
subpattern1 && subpattern2
```

A pair of patterns separated by `&&` matches only if both subpatterns match. If the left branch does not match, the right branch is not evaluated.
由 `&&` 分隔的一对模式仅在两个子模式都匹配时才匹配。如果左分支不匹配，则不会评估右分支。

Subpatterns in a logical-and pattern can bind variables, but the variables in each subpattern must not overlap, because they will both be bound if the pattern matches:
逻辑与模式中的子模式可以绑定变量，但每个子模式中的变量不得重叠，因为如果模式匹配，它们都将被绑定：

```dart
switch ((1, 2)) {
  // Error, both subpatterns attempt to bind 'b'.
  case (var a, var b) && (var b, var c): // ...
}
```

## Relational 关系

```
== expression
< expression
```

Relational patterns compare the matched value to a given constant using any of the equality or relational operators: `==`, `!=`, `<`, `>`, `<=`, and `>=`.
关系模式使用任何相等或关系运算符将匹配的值与给定常量进行比较： `==` 、 `!=` 、 `<` 、 `>` 、 `<=` 和 `>=` 。

The pattern matches when calling the appropriate operator on the matched value with the constant as an argument returns `true`.
当使用常量作为参数对匹配的值调用适当的运算符时，模式匹配返回 `true` 。

Relational patterns are useful for matching on numeric ranges, especially when combined with the [logical-and pattern](https://dart.dev/language/pattern-types#logical-and):
关系模式对于匹配数值范围非常有用，尤其是与逻辑与模式结合使用时：

```dart
String asciiCharType(int char) {
  const space = 32;
  const zero = 48;
  const nine = 57;

  return switch (char) {
    < space => 'control',
    == space => 'space',
    > space && < zero => 'punctuation',
    >= zero && <= nine => 'digit',
    _ => ''
  };
}
```

## Cast 强制类型转换

```
foo as String
```

A cast pattern lets you insert a [type cast](https://dart.dev/language/operators#type-test-operators) in the middle of destructuring, before passing the value to another subpattern:
强制类型转换模式允许你在解构过程中插入一个类型转换，在将值传递给另一个子模式之前：

```dart
(num, Object) record = (1, 's');
var (i as int, s as String) = record;
```

Cast patterns will [throw](https://dart.dev/language/error-handling#throw) if the value doesn’t have the stated type. Like the [null-assert pattern](https://dart.dev/language/pattern-types#null-assert), this lets you forcibly assert the expected type of some destructured value.
如果值没有声明的类型，强制类型转换模式会抛出异常。与 null 断言模式一样，这允许你强制断言某个解构值预期的类型。

## Null-check 空值检查

```
subpattern?
```

Null-check patterns match first if the value is not null, and then match the inner pattern against that same value. They let you bind a variable whose type is the non-nullable base type of the nullable value being matched.
如果值不为 null，空值检查模式首先匹配，然后针对相同的值匹配内部模式。它们允许你绑定一个变量，其类型是正在匹配的可空值的非空基类型。

To treat `null` values as match failures without throwing, use the null-check pattern.
要将 `null` 值视为匹配失败而不抛出异常，请使用空值检查模式。

```dart
String? maybeString = 'nullable with base type String';
switch (maybeString) {
  case var s?:
  // 's' has type non-nullable String here.
}
```

To match when the value *is* null, use the [constant pattern](https://dart.dev/language/pattern-types#constant) `null`.
要匹配值为空的情况，请使用常量模式 `null` 。

## Null-assert 空值断言

```
subpattern!
```

Null-assert patterns match first if the object is not null, then on the value. They permit non-null values to flow through, but [throw](https://dart.dev/language/error-handling#throw) if the matched value is null.
如果对象不为 null，空值断言模式首先匹配，然后匹配值。它们允许非空值通过，但如果匹配的值为 null，则会抛出异常。

To ensure `null` values are not silently treated as match failures, use a null-assert pattern while matching:
要确保 `null` 值不会被静默地视为匹配失败，请在匹配时使用空值断言模式：

```dart
List<String?> row = ['user', null];
switch (row) {
  case ['user', var name!]: // ...
  // 'name' is a non-nullable string here.
}
```

To eliminate `null` values from variable declaration patterns, use the null-assert pattern:
要从变量声明模式中消除 `null` 值，请使用 null 断言模式：

```dart
(int?, int?) position = (2, 3);

var (x!, y!) = position;
```

To match when the value *is* null, use the [constant pattern](https://dart.dev/language/pattern-types#constant) `null`.
要匹配值为空时，请使用常量模式 `null` 。

## Constant 常量

```
123, null, 'string', math.pi, SomeClass.constant, const Thing(1, 2), const (1 + 2)
```

Constant patterns match when the value is equal to the constant:
常量模式在值等于常量时匹配：

```dart
switch (number) {
  // Matches if 1 == number.
  case 1: // ...
}
```

You can use simple literals and references to named constants directly as constant patterns:
您可以直接使用简单文字和对命名常量的引用作为常量模式：

- Number literals (`123`, `45.56`)
  数字文字（ `123` 、 `45.56` ）
- Boolean literals (`true`)
  布尔文字（ `true` ）
- String literals (`'string'`)
  字符串文字（ `'string'` ）
- Named constants (`someConstant`, `math.pi`, `double.infinity`)
  命名常量 ( `someConstant` , `math.pi` , `double.infinity` )
- Constant constructors (`const Point(0, 0)`)
  常量构造函数 ( `const Point(0, 0)` )
- Constant collection literals (`const []`, `const {1, 2}`)
  常量集合字面量 ( `const []` , `const {1, 2}` )

More complex constant expressions must be parenthesized and prefixed with `const` (`const (1 + 2)`):
更复杂的常量表达式必须用括号括起来并以 `const` ( `const (1 + 2)` ) 为前缀：

```dart
// List or map pattern:
case [a, b]: // ...

// List or map literal:
case const [a, b]: // ...
```

## Variable 变量

```
var bar, String str, final int _
```

Variable patterns bind new variables to values that have been matched or destructured. They usually occur as part of a [destructuring pattern](https://dart.dev/language/patterns#destructuring) to capture a destructured value.
变量模式将新变量绑定到已匹配或解构的值。它们通常作为解构模式的一部分出现，以捕获解构的值。

The variables are in scope in a region of code that is only reachable when the pattern has matched.
变量在仅在模式匹配时才能到达的代码区域中处于作用域中。

```dart
switch ((1, 2)) {
  // 'var a' and 'var b' are variable patterns that bind to 1 and 2, respectively.
  case (var a, var b): // ...
  // 'a' and 'b' are in scope in the case body.
}
```

A *typed* variable pattern only matches if the matched value has the declared type, and fails otherwise:
类型化变量模式仅在匹配的值具有声明的类型时才匹配，否则失败：

```dart
switch ((1, 2)) {
  // Does not match.
  case (int a, String b): // ...
}
```

You can use a [wildcard pattern](https://dart.dev/language/pattern-types#wildcard) as a variable pattern.
您可以将通配符模式用作变量模式。

## Identifier 标识符

```
foo, _
```

Identifier patterns may behave like a [constant pattern](https://dart.dev/language/pattern-types#constant) or like a [variable pattern](https://dart.dev/language/pattern-types#variable), depending on the context where they appear:
标识符模式可能会表现得像常量模式或变量模式，具体取决于它们出现的上下文：

- [Declaration](https://dart.dev/language/patterns#variable-declaration) context: declares a new variable with identifier name: `var (a, b) = (1, 2);`
  声明上下文：声明一个具有标识符名称的新变量： `var (a, b) = (1, 2);`

- [Assignment](https://dart.dev/language/patterns#variable-assignment) context: assigns to existing variable with identifier name: `(a, b) = (3, 4);`
  赋值上下文：赋值给具有标识符名称的现有变量： `(a, b) = (3, 4);`

- [Matching](https://dart.dev/language/patterns#matching) context: treated as a named constant pattern (unless its name is `_`):
  匹配上下文：视为命名常量模式（除非其名称为 `_` ）：

  ```dart
  const c = 1;
  switch (2) {
    case c:
      print('match $c');
    default:
      print('no match'); // Prints "no match".
  }
  ```

- [Wildcard](https://dart.dev/language/pattern-types#wildcard) identifier in any context: matches any value and discards it: `case [_, var y, _]: print('The middle element is $y');`
  任何上下文中的通配符标识符：匹配任何值并将其丢弃： `case [_, var y, _]: print('The middle element is $y');`

## Parenthesized 带括号的

```
(subpattern)
```

Like parenthesized expressions, parentheses in a pattern let you control [pattern precedence](https://dart.dev/language/pattern-types#pattern-precedence) and insert a lower-precedence pattern where a higher precedence one is expected.
与带括号的表达式一样，模式中的括号允许您控制模式优先级并在预期出现较高优先级模式的位置插入较低优先级模式。

For example, imagine the boolean constants `x`, `y`, and `z` are equal to `true`, `true`, and `false`, respectively:
例如，假设布尔常量 `x` 、 `y` 和 `z` 分别等于 `true` 、 `true` 和 `false` ：

```dart
// ...
x || y && z => 'matches true',
(x || y) && z => 'matches false',
// ...
```

In the first case, the logical-and pattern `y && z` evaluates first because logical-and patterns have higher precedence than logical-or. In the next case, the logical-or pattern is parenthesized. It evaluates first, which results in a different match.
在第一种情况下，逻辑与模式 `y && z` 首先求值，因为逻辑与模式的优先级高于逻辑或。在下一个案例中，逻辑或模式带括号。它首先求值，从而导致不同的匹配。

## List 列表

```
[subpattern1, subpattern2]
```

A list pattern matches values that implement [`List`](https://dart.dev/language/collections#lists), and then recursively matches its subpatterns against the list’s elements to destructure them by position:
列表模式匹配实现 `List` 的值，然后递归地将其子模式与列表的元素进行匹配，按位置对其进行解构：

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

List patterns require that the number of elements in the pattern match the entire list. You can, however, use a [rest element](https://dart.dev/language/pattern-types#rest-element) as a place holder to account for any number of elements in a list.
列表模式要求模式中的元素数量与整个列表匹配。但是，您可以使用剩余元素作为占位符来解释列表中的任意数量的元素。

### Rest element 剩余元素

List patterns can contain *one* rest element (`...`) which allows matching lists of arbitrary lengths.
列表模式可以包含一个剩余元素 ( `...` )，该元素允许匹配任意长度的列表。

```dart
var [a, b, ..., c, d] = [1, 2, 3, 4, 5, 6, 7];
// Prints "1 2 6 7".
print('$a $b $c $d');
```

A rest element can also have a subpattern that collects elements that don’t match the other subpatterns in the list, into a new list:
剩余元素还可以具有一个子模式，该子模式将不匹配列表中其他子模式的元素收集到一个新列表中：

```dart
var [a, b, ...rest, c, d] = [1, 2, 3, 4, 5, 6, 7];
// Prints "1 2 [3, 4, 5] 6 7".
print('$a $b $rest $c $d');
```

## Map

```
{"key": subpattern1, someConst: subpattern2}
```

Map patterns match values that implement [`Map`](https://dart.dev/language/collections#maps), and then recursively match its subpatterns against the map’s keys to destructure them.
映射模式匹配实现 `Map` 的值，然后递归地将其子模式与映射的键进行匹配，以按位置对其进行解构。

Map patterns don’t require the pattern to match the entire map. A map pattern ignores any keys that the map contains that aren’t matched by the pattern.
映射模式不要求模式与整个映射匹配。映射模式会忽略映射中包含的任何未由模式匹配的键。

## Record 记录

```
(subpattern1, subpattern2)
(x: subpattern1, y: subpattern2)
```

Record patterns match a [record](https://dart.dev/language/records) object and destructure its fields. If the value isn’t a record with the same [shape](https://dart.dev/language/records#record-types) as the pattern, the match fails. Otherwise, the field subpatterns are matched against the corresponding fields in the record.
记录模式匹配记录对象并对其字段进行解构。如果该值不是与模式具有相同形状的记录，则匹配失败。否则，字段子模式将与记录中的相应字段进行匹配。

Record patterns require that the pattern match the entire record. To destructure a record with *named* fields using a pattern, include the field names in the pattern:
记录模式要求模式与整个记录匹配。要使用模式对具有命名字段的记录进行解构，请在模式中包含字段名称：

```dart
var (myString: foo, myNumber: bar) = (myString: 'string', myNumber: 1);
```

The getter name can be omitted and inferred from the [variable pattern](https://dart.dev/language/pattern-types#variable) or [identifier pattern](https://dart.dev/language/pattern-types#identifier) in the field subpattern. These pairs of patterns are each equivalent:
可以省略 getter 名称，并从字段子模式中的变量模式或标识符模式推断出来。以下每对模式都等效：

```dart
// Record pattern with variable subpatterns:
var (untyped: untyped, typed: int typed) = record;
var (:untyped, :int typed) = record;

switch (record) {
  case (untyped: var untyped, typed: int typed): // ...
  case (:var untyped, :int typed): // ...
}

// Record pattern wih null-check and null-assert subpatterns:
switch (record) {
  case (checked: var checked?, asserted: var asserted!): // ...
  case (:var checked?, :var asserted!): // ...
}

// Record pattern wih cast subpattern:
var (untyped: untyped as int, typed: typed as String) = record;
var (:untyped as int, :typed as String) = record;
```

## Object 对象

```
SomeClass(x: subpattern1, y: subpattern2)
```

Object patterns check the matched value against a given named type to destructure data using getters on the object’s properties. They are [refuted](https://dart.dev/resources/glossary#refutable-pattern) if the value doesn’t have the same type.
对象模式使用对象属性上的 getter 来检查匹配值与给定命名类型是否一致，以解构数据。如果值没有相同的类型，则会驳回它们。

```dart
switch (shape) {
  // Matches if shape is of type Rect, and then against the properties of Rect.
  case Rect(width: var w, height: var h): // ...
}
```

The getter name can be omitted and inferred from the [variable pattern](https://dart.dev/language/pattern-types#variable) or [identifier pattern](https://dart.dev/language/pattern-types#identifier) in the field subpattern:
可以省略 getter 名称，并从字段子模式中的变量模式或标识符模式推断出来：

```dart
// Binds new variables x and y to the values of Point's x and y properties.
var Point(:x, :y) = Point(1, 2);
```

Object patterns don’t require the pattern to match the entire object. If an object has extra fields that the pattern doesn’t destructure, it can still match.
对象模式不要求模式与整个对象匹配。如果对象具有模式未解构的额外字段，它仍然可以匹配。

## Wildcard 通配符

```
_
```

A pattern named `_` is a wildcard, either a [variable pattern](https://dart.dev/language/pattern-types#variable) or [identifier pattern](https://dart.dev/language/pattern-types#identifier), that doesn’t bind or assign to any variable.
名为 `_` 的模式是通配符，即变量模式或标识符模式，不会绑定或分配给任何变量。

It’s useful as a placeholder in places where you need a subpattern in order to destructure later positional values:
在需要子模式才能解构后续位置值的地方，它可用作占位符：

```dart
var list = [1, 2, 3];
var [_, two, _] = list;
```

A wildcard name with a type annotation is useful when you want to test a value’s type but not bind the value to a name:
当您想测试值类型但不将值绑定到名称时，带有类型注释的通配符名称非常有用：

```dart
switch (record) {
  case (int _, String _):
    print('First field is int and second is String.');
}
```
