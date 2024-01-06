+++
title = "Built-in types"
date = 2024-01-05T20:29:36+08:00
weight = 1
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文: [https://dart.dev/language/built-in-types](https://dart.dev/language/built-in-types)

# Built-in types 内置类型

The Dart language has special support for the following:
Dart 语言对以下内容有特殊支持：

- [Numbers](https://dart.dev/language/built-in-types#numbers) (`int`, `double`)
  数字 ( `int` , `double` )
- [Strings](https://dart.dev/language/built-in-types#strings) (`String`)
  字符串 ( `String` )
- [Booleans](https://dart.dev/language/built-in-types#booleans) (`bool`)
  布尔值 ( `bool` )
- [Records](https://dart.dev/language/records) (`(value1, value2)`)
  记录 ( `(value1, value2)` )
- [Lists](https://dart.dev/language/collections#lists) (`List`, also known as *arrays*)
  列表 ( `List` ，也称为数组)
- [Sets](https://dart.dev/language/collections#sets) (`Set`)
  集合 ( `Set` )
- [Maps](https://dart.dev/language/collections#maps) (`Map`)
  映射 ( `Map` )
- [Runes](https://dart.dev/language/built-in-types#runes-and-grapheme-clusters) (`Runes`; often replaced by the `characters` API)
  符文 ( `Runes` ；通常由 `characters` API 替换)
- [Symbols](https://dart.dev/language/built-in-types#symbols) (`Symbol`)
  符号 ( `Symbol` )
- The value `null` (`Null`)
  值 `null` ( `Null` )

This support includes the ability to create objects using literals. For example, `'this is a string'` is a string literal, and `true` is a boolean literal.
此支持包括使用字面量创建对象的功能。例如， `'this is a string'` 是字符串字面量， `true` 是布尔字面量。

Because every variable in Dart refers to an object—an instance of a *class*—you can usually use *constructors* to initialize variables. Some of the built-in types have their own constructors. For example, you can use the `Map()` constructor to create a map.
由于 Dart 中的每个变量都引用一个对象（一个类的实例），因此通常可以使用构造函数来初始化变量。某些内置类型有自己的构造函数。例如，可以使用 `Map()` 构造函数来创建映射。

Some other types also have special roles in the Dart language:
某些其他类型在 Dart 语言中也具有特殊作用：

- `Object`: The superclass of all Dart classes except `Null`.
  `Object` ：除 `Null` 之外所有 Dart 类的超类。
- `Enum`: The superclass of all enums.
  `Enum` ：所有枚举的超类。
- `Future` and `Stream`: Used in [asynchrony support](https://dart.dev/language/async).
  `Future` 和 `Stream` ：用于异步支持。
- `Iterable`: Used in [for-in loops](https://dart.dev/libraries/dart-core#iteration) and in synchronous [generator functions](https://dart.dev/language/functions#generators).
  `Iterable` ：用于 for-in 循环和同步生成器函数中。
- `Never`: Indicates that an expression can never successfully finish evaluating. Most often used for functions that always throw an exception.
  `Never` ：表示表达式永远无法成功完成求值。最常用于始终引发异常的函数。
- `dynamic`: Indicates that you want to disable static checking. Usually you should use `Object` or `Object?` instead.
  `dynamic` ：表示要禁用静态检查。通常应该改用 `Object` 或 `Object?` 。
- `void`: Indicates that a value is never used. Often used as a return type.
  `void` ：表示从不使用某个值。通常用作返回类型。

The `Object`, `Object?`, `Null`, and `Never` classes have special roles in the class hierarchy. Learn about these roles in [Understanding null safety](https://dart.dev/null-safety/understanding-null-safety#top-and-bottom).
`Object` 、 `Object?` 、 `Null` 和 `Never` 类在类层次结构中具有特殊作用。在了解 Null 安全性中了解这些作用。

## Numbers 数字

Dart numbers come in two flavors:
Dart 数字有两种类型：

- [`int`](https://api.dart.dev/stable/dart-core/int-class.html)

  Integer values no larger than 64 bits, [depending on the platform](https://dart.dev/guides/language/numbers). On native platforms, values can be from -263 to 263 - 1. On the web, integer values are represented as JavaScript numbers (64-bit floating-point values with no fractional part) and can be from -253 to 253 - 1. 整数值，最大为 64 位，具体取决于平台。在原生平台上，值可以介于 -2 63 到 2 63 - 1 之间。在网络上，整数值表示为 JavaScript 数字（没有小数部分的 64 位浮点值），可以介于 -2 53 到 2 53 - 1 之间。

- [`double`](https://api.dart.dev/stable/dart-core/double-class.html)

  64-bit (double-precision) floating-point numbers, as specified by the IEEE 754 standard. 64 位（双精度）浮点数，由 IEEE 754 标准指定。

Both `int` and `double` are subtypes of [`num`](https://api.dart.dev/stable/dart-core/num-class.html). The num type includes basic operators such as +, -, /, and *, and is also where you’ll find `abs()`,` ceil()`, and `floor()`, among other methods. (Bitwise operators, such as >>, are defined in the `int` class.) If num and its subtypes don’t have what you’re looking for, the [dart:math](https://api.dart.dev/stable/dart-math) library might.
Both `int` 和 `double` 是 `num` 的子类型。num 类型包括基本运算符，如 +、-、/ 和 *，您还可以在其中找到 `abs()` 、 `ceil()` 和 `floor()` 等方法。（按位运算符，如 >>，在 `int` 类中定义。）如果 num 及其子类型没有您要查找的内容，则 dart:math 库可能会有。

Integers are numbers without a decimal point. Here are some examples of defining integer literals:
整数是没有小数点的数字。以下是一些定义整数字面量的示例：

```dart
var x = 1;
var hex = 0xDEADBEEF;
```

If a number includes a decimal, it is a double. Here are some examples of defining double literals:
如果数字包含小数点，则它是一个双精度浮点数。以下是一些定义双精度浮点数字面量的示例：

```dart
var y = 1.1;
var exponents = 1.42e5;
```

You can also declare a variable as a num. If you do this, the variable can have both integer and double values.
您还可以将变量声明为 num。如果这样做，则该变量可以具有整数和双精度浮点数值。

```dart
num x = 1; // x can have both int and double values
x += 2.5;
```

Integer literals are automatically converted to doubles when necessary:
必要时，整数字面量会自动转换为双精度浮点数：

```dart
double z = 1; // Equivalent to double z = 1.0.
```

Here’s how you turn a string into a number, or vice versa:
以下是如何将字符串转换为数字，反之亦然：

```dart
// String -> int
var one = int.parse('1');
assert(one == 1);

// String -> double
var onePointOne = double.parse('1.1');
assert(onePointOne == 1.1);

// int -> String
String oneAsString = 1.toString();
assert(oneAsString == '1');

// double -> String
String piAsString = 3.14159.toStringAsFixed(2);
assert(piAsString == '3.14');
```

The `int` type specifies the traditional bitwise shift (`<<`, `>>`, `>>>`), complement (`~`), AND (`&`), OR (`|`), and XOR (`^`) operators, which are useful for manipulating and masking flags in bit fields. For example:
`int` 类型指定了传统的按位移位 ( `<<` 、 `>>` 、 `>>>` )、取反 ( `~` )、AND ( `&` )、OR ( `|` ) 和 XOR ( `^` ) 运算符，这些运算符可用于操作和屏蔽位字段中的标志。例如：

```dart
assert((3 << 1) == 6); // 0011 << 1 == 0110
assert((3 | 4) == 7); // 0011 | 0100 == 0111
assert((3 & 4) == 0); // 0011 & 0100 == 0000
```

For more examples, see the [bitwise and shift operator](https://dart.dev/language/operators#bitwise-and-shift-operators) section.
有关更多示例，请参阅按位和移位运算符部分。

Literal numbers are compile-time constants. Many arithmetic expressions are also compile-time constants, as long as their operands are compile-time constants that evaluate to numbers.
字面数字是编译时常量。只要其操作数是求值为数字的编译时常量，许多算术表达式也是编译时常量。

```dart
const msPerSecond = 1000;
const secondsUntilRetry = 5;
const msUntilRetry = secondsUntilRetry * msPerSecond;
```

For more information, see [Numbers in Dart](https://dart.dev/guides/language/numbers).
有关更多信息，请参阅 Dart 中的数字。

## Strings 字符串

A Dart string (`String` object) holds a sequence of UTF-16 code units. You can use either single or double quotes to create a string:
Dart 字符串（ `String` 对象）保存 UTF-16 代码单元序列。您可以使用单引号或双引号创建字符串：

```dart
var s1 = 'Single quotes work well for string literals.';
var s2 = "Double quotes work just as well.";
var s3 = 'It\'s easy to escape the string delimiter.';
var s4 = "It's even easier to use the other delimiter.";
```

You can put the value of an expression inside a string by using `${`*`expression`*`}`. If the expression is an identifier, you can skip the {}. To get the string corresponding to an object, Dart calls the object’s `toString()` method.
您可以使用 `${` `expression` `}` 将表达式的值放入字符串中。如果表达式是标识符，则可以跳过 {}。要获取与对象对应的字符串，Dart 会调用该对象的 `toString()` 方法。

```dart
var s = 'string interpolation';

assert('Dart has $s, which is very handy.' ==
    'Dart has string interpolation, '
        'which is very handy.');
assert('That deserves all caps. '
        '${s.toUpperCase()} is very handy!' ==
    'That deserves all caps. '
        'STRING INTERPOLATION is very handy!');
```

*info* **Note:** The `==` operator tests whether two objects are equivalent. Two strings are equivalent if they contain the same sequence of code units.
注意： `==` 运算符测试两个对象是否相等。如果两个字符串包含相同的代码单元序列，则它们相等。

You can concatenate strings using adjacent string literals or the `+` operator:
您可以使用相邻的字符串文字或 `+` 运算符连接字符串：

```dart
var s1 = 'String '
    'concatenation'
    " works even over line breaks.";
assert(s1 ==
    'String concatenation works even over '
        'line breaks.');

var s2 = 'The + operator ' + 'works, as well.';
assert(s2 == 'The + operator works, as well.');
```

To create a multi-line string, use a triple quote with either single or double quotation marks:
要创建多行字符串，请使用带有单引号或双引号的三重引号：

```dart
var s1 = '''
You can create
multi-line strings like this one.
''';

var s2 = """This is also a
multi-line string.""";
```

You can create a “raw” string by prefixing it with `r`:
您可以通过在字符串前加上 `r` 来创建“原始”字符串：

```dart
var s = r'In a raw string, not even \n gets special treatment.';
```

See [Runes and grapheme clusters](https://dart.dev/language/built-in-types#runes-and-grapheme-clusters) for details on how to express Unicode characters in a string.
有关如何在字符串中表示 Unicode 字符的详细信息，请参阅符文和音节簇。

Literal strings are compile-time constants, as long as any interpolated expression is a compile-time constant that evaluates to null or a numeric, string, or boolean value.
只要任何插值表达式是求值为 null 或数字、字符串或布尔值的编译时常量，文字字符串就是编译时常量。

```dart
// These work in a const string.
const aConstNum = 0;
const aConstBool = true;
const aConstString = 'a constant string';

// These do NOT work in a const string.
var aNum = 0;
var aBool = true;
var aString = 'a string';
const aConstList = [1, 2, 3];

const validConstString = '$aConstNum $aConstBool $aConstString';
// const invalidConstString = '$aNum $aBool $aString $aConstList';
```

For more information on using strings, check out [Strings and regular expressions](https://dart.dev/libraries/dart-core#strings-and-regular-expressions).
有关使用字符串的更多信息，请查看字符串和正则表达式。

## Booleans 布尔值

To represent boolean values, Dart has a type named `bool`. Only two objects have type bool: the boolean literals `true` and `false`, which are both compile-time constants.
为了表示布尔值，Dart 具有名为 `bool` 的类型。只有两个对象具有 bool 类型：布尔文字 `true` 和 `false` ，它们都是编译时常量。

Dart’s type safety means that you can’t use code like `if (*nonbooleanValue*)` or `assert (*nonbooleanValue*)`. Instead, explicitly check for values, like this:
Dart 的类型安全性意味着您不能使用类似 `if (*nonbooleanValue*)` 或 `assert (*nonbooleanValue*)` 的代码。相反，请显式检查值，如下所示：

```dart
// Check for an empty string.
var fullName = '';
assert(fullName.isEmpty);

// Check for zero.
var hitPoints = 0;
assert(hitPoints <= 0);

// Check for null.
var unicorn = null;
assert(unicorn == null);

// Check for NaN.
var iMeantToDoThis = 0 / 0;
assert(iMeantToDoThis.isNaN);
```

## Runes and grapheme clusters 符文和字形簇

In Dart, [runes](https://api.dart.dev/stable/dart-core/Runes-class.html) expose the Unicode code points of a string. You can use the [characters package](https://pub.dev/packages/characters) to view or manipulate user-perceived characters, also known as [Unicode (extended) grapheme clusters.](https://unicode.org/reports/tr29/#Grapheme_Cluster_Boundaries)
在 Dart 中，符文公开字符串的 Unicode 代码点。您可以使用 characters 包来查看或操作用户感知的字符，也称为 Unicode（扩展）字形簇。

Unicode defines a unique numeric value for each letter, digit, and symbol used in all of the world’s writing systems. Because a Dart string is a sequence of UTF-16 code units, expressing Unicode code points within a string requires special syntax. The usual way to express a Unicode code point is `\uXXXX`, where XXXX is a 4-digit hexadecimal value. For example, the heart character (♥) is `\u2665`. To specify more or less than 4 hex digits, place the value in curly brackets. For example, the laughing emoji (😆) is `\u{1f606}`.
Unicode 为所有世界书写系统中使用的每个字母、数字和符号定义了一个唯一的数字值。由于 Dart 字符串是 UTF-16 代码单元的序列，因此在字符串中表示 Unicode 代码点需要特殊语法。表示 Unicode 代码点的常用方法是 `\uXXXX` ，其中 XXXX 是一个 4 位十六进制值。例如，心形字符（♥）是 `\u2665` 。要指定多于或少于 4 位十六进制数字，请将值放在大括号中。例如，笑脸表情符号（😆）是 `\u{1f606}` 。

If you need to read or write individual Unicode characters, use the `characters` getter defined on String by the characters package. The returned [`Characters`](https://pub.dev/documentation/characters/latest/characters/Characters-class.html) object is the string as a sequence of grapheme clusters. Here’s an example of using the characters API:
如果您需要读取或写入单个 Unicode 字符，请使用 characters 包在 String 上定义的 `characters` getter。返回的 `Characters` 对象是字符串作为一系列字素簇。以下是如何使用 characters API 的示例：

```dart
import 'package:characters/characters.dart';

void main() {
  var hi = 'Hi 🇩🇰';
  print(hi);
  print('The end of the string: ${hi.substring(hi.length - 1)}');
  print('The last character: ${hi.characters.last}');
}
```

The output, depending on your environment, looks something like this:
输出（取决于您的环境）看起来像这样：

```
$ dart run bin/main.dart
Hi 🇩🇰
The end of the string: ???
The last character: 🇩🇰
```

For details on using the characters package to manipulate strings, see the [example](https://pub.dev/packages/characters/example) and [API reference](https://pub.dev/documentation/characters) for the characters package.
有关使用 characters 包来操作字符串的详细信息，请参阅 characters 包的示例和 API 参考。

## Symbols 符号

A [`Symbol`](https://api.dart.dev/stable/dart-core/Symbol-class.html) object represents an operator or identifier declared in a Dart program. You might never need to use symbols, but they’re invaluable for APIs that refer to identifiers by name, because minification changes identifier names but not identifier symbols.
A `Symbol` 对象表示在 Dart 程序中声明的运算符或标识符。您可能永远不需要使用符号，但对于通过名称引用标识符的 API 来说，它们非常有价值，因为混淆会更改标识符名称，但不会更改标识符符号。

To get the symbol for an identifier, use a symbol literal, which is just `#` followed by the identifier:
要获取标识符的符号，请使用符号文字，它只是 `#` 后跟标识符：

```nocode
#radix
#bar
```

Symbol literals are compile-time constants.
符号文字是编译时常量。
