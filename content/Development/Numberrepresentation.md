+++
title = "Dart 中的数字表示"
date = 2024-01-05T20:29:36+08:00
weight = 20
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文: [https://dart.dev/guides/language/numbers](https://dart.dev/guides/language/numbers)

## Numbers in Dart - Dart 中的数字

Dart apps often target multiple platforms. For example, a Flutter app might target iOS, Android, and the web. The code can be the same, as long as the app doesn’t rely on platform-specific libraries or use numbers in a way that’s platform dependent.
Dart 应用通常针对多个平台。例如，Flutter 应用可能针对 iOS、Android 和网络。只要应用不依赖于特定于平台的库或以依赖于平台的方式使用数字，代码就可以相同。

This page has details about the differences between native and web number implementations, and how to write code so that those differences don’t matter.
此页面详细介绍了本机数字实现和网络数字实现之间的差异，以及如何编写代码以消除这些差异的影响。

**Number implementations in Dart and other languages
Dart 和其他语言中的数字实现**

Dart has always allowed platform-specific representations and semantics for numbers, for reasons of performance, code size, and platform interoperability.
出于性能、代码大小和平台互操作性的原因，Dart 一直允许针对数字使用特定于平台的表示和语义。

Similarly, in C/C++ the commonly used `int` type for integer values is platform-specific to best map to the native machine architecture (16-, 32-, or 64-bit). In Java, the `float` and `double` types for fractional values were originally designed to strictly follow IEEE 754 on all platforms, but this constraint was loosened almost immediately for efficiency reasons (`strictfp` is required for exact coherence).
同样，在 C/C++ 中，用于整数值的常用 `int` 类型是特定于平台的，以便最适合本机机器架构（16 位、32 位或 64 位）。在 Java 中，用于分数值的 `float` 和 `double` 类型最初设计为在所有平台上严格遵循 IEEE 754，但出于效率原因，此约束几乎立即放宽（ `strictfp` 是精确一致性所必需的）。

## Dart number representation Dart 数字表示

In Dart, all numbers are part of the common `Object` type hierarchy, and there are two concrete, user-visible numeric types: `int`, representing integer values, and `double`, representing fractional values.
在 Dart 中，所有数字都是通用 `Object` 类型层次结构的一部分，并且有两个具体的、用户可见的数字类型： `int` ，表示整数值，和 `double` ，表示分数值。

![Object is the parent of num, which is the parent of int and double](./Numberrepresentation_img/number-class-hierarchy.svg+xml)

Depending on the platform, those numeric types have different, hidden implementations. In particular, Dart has two very different types of targets it compiles to:
根据平台的不同，这些数字类型具有不同的隐藏实现。特别是，Dart 有两种非常不同的目标类型进行编译：

- **Native:** Most often, a 64-bit mobile or desktop processor.
  本机：最常见的是 64 位移动或桌面处理器。
- **Web:** JavaScript as the primary execution engine.
  网络：JavaScript 作为主要的执行引擎。

The following table shows how Dart numbers are usually implemented:
下表显示了 Dart 数字的通常实现方式：

| Representation 表示                                          | Native `int` 本机 `int` | Native `double` 原生 `double` | Web `int` | Web `double` |
| ------------------------------------------------------------ | ----------------------- | ----------------------------- | --------- | ------------ |
| [64-bit signed two's complement 64 位带符号的二进制补码](https://en.wikipedia.org/wiki/Two's_complement) | ✅                       |                               |           |              |
| [64-bit floating point 64 位浮点数](https://en.wikipedia.org/wiki/Double-precision_floating-point_format) |                         | ✅                             | ✅         | ✅            |

For native targets, you can assume that `int` maps to a signed 64-bit integer representation and `double` maps to a 64-bit IEEE floating-point representation that matches the underlying processor.
对于原生目标，您可以假设 `int` 映射到有符号 64 位整数表示形式，而 `double` 映射到与底层处理器匹配的 64 位 IEEE 浮点表示形式。

But on the web, where Dart compiles to and interoperates with JavaScript, there is a single numeric representation: a 64-bit double-precision floating-point value. For efficiency, Dart maps both `int` and `double` to this single representation. The visible type hierarchy remains the same, but the underlying hidden implementation types are different and intertwined.
但在 Web 上，Dart 编译为 JavaScript 并与其互操作，存在一个数字表示形式：64 位双精度浮点值。为了提高效率，Dart 将 `int` 和 `double` 都映射到此单一表示形式。可见的类型层次结构保持不变，但底层隐藏的实现类型不同且相互交织。

The following figure illustrates the platform-specific types (in blue) for native and web targets. As the figure shows, the concrete type for `int` on native implements only the `int` interface. However, the concrete type for `int` on the web implements both `int` and `double`.
下图说明了原生和 Web 目标的特定于平台的类型（以蓝色显示）。如图所示，原生上的 `int` 的具体类型仅实现 `int` 接口。但是，Web 上的 `int` 的具体类型同时实现了 `int` 和 `double` 。

![Implementation classes vary by platform; for JavaScript, the class that implements int also implements double](./Numberrepresentation_img/number-platform-specific.svg+xml)

*info* **Note:** Dart represents `int` and `double` in a few different ways for efficiency, but these implementation classes (in blue, above) are hidden. In general, you can ignore the platform-specific types, and think of `int` and `double` as concrete types.
注意：Dart 以几种不同的方式表示 `int` 和 `double` 以提高效率，但这些实现类（如上所示，以蓝色显示）是隐藏的。通常，您可以忽略特定于平台的类型，并将 `int` 和 `double` 视为具体类型。

An `int` on the web is represented as a double-precision floating-point value with no fractional part. In practice, this works pretty well: double-precision floating point provides 53 bits of integer precision. However, `int` values are always also `double` values, which can lead to some surprises.
Web 上的 `int` 表示为没有小数部分的双精度浮点值。实际上，这种方法效果很好：双精度浮点提供 53 位整数精度。但是， `int` 值始终也是 `double` 值，这可能会带来一些意外情况。

## Differences in behavior 行为差异

Most integer and double arithmetic has essentially the same behavior. There are, however, important differences—particularly when your code has strict expectations about precision, string formatting, or underlying runtime types.
大多数整数和双精度算术具有基本相同的行为。但是，存在一些重要的差异，尤其是当您的代码对精度、字符串格式或底层运行时类型有严格要求时。

When arithmetic results differ, as described in this section, the behavior is **platform specific** and **subject to change**.
当算术结果不同时（如本节所述），行为取决于平台，并且可能会发生变化。

*info* **Note:** Any platform-specific behavior that this page describes might change to be less surprising, more consistent, or more performant.
注意：此页面描述的任何特定于平台的行为都可能会发生变化，以减少意外情况，提高一致性或性能。

### Precision 精度

The following table demonstrates how some numerical expressions differ due to precision. Here, `math` represents the `dart:math` library, and `math.pow(2, 53)` is 253.
下表演示了由于精度不同，一些数值表达式如何有所差异。此处， `math` 表示 `dart:math` 库， `math.pow(2, 53)` 为 2 53 。

On the web, integers lose precision past 53 bits. In particular, 253 and 253+1 map to the same value due to truncation. On native, these values can still be differentiated because native numbers have 64 bits—63 bits for the value and 1 for the sign.
在网络上，整数在超过 53 位时会失去精度。具体来说，由于截断，2 53 和 2 53 +1 映射到相同的值。在原生环境中，这些值仍然可以区分，因为原生数字有 64 位——63 位用于值，1 位用于符号。

The effect of overflow is visible when comparing 263-1 to 263. On native, the latter overflows to -263, as expected for two’s-complement arithmetic. On the web, these values do not overflow because they are represented differently; they’re approximations due to the loss of precision.
比较 2 63 -1 和 2 63 时，溢出的效果是可见的。在原生环境中，后者溢出为 -2 63 ，这是二进制补码算术的预期结果。在网络上，这些值不会溢出，因为它们以不同的方式表示；由于精度的损失，它们是近似值。

| Expression 表达式     | Native 本机            | Web                    |
| --------------------- | ---------------------- | ---------------------- |
| `math.pow(2, 53) - 1` | `9007199254740991`     | `9007199254740991`     |
| `math.pow(2, 53)`     | `9007199254740992`     | `9007199254740992`     |
| `math.pow(2, 53) + 1` | `9007199254740993`     | `9007199254740992`     |
| `math.pow(2, 62)`     | `4611686018427387904`  | `4611686018427388000`  |
| `math.pow(2, 63) - 1` | `9223372036854775807`  | `9223372036854776000`  |
| `math.pow(2, 63)`     | `-9223372036854775808` | `9223372036854776000`  |
| `math.pow(2, 64)`     | `0`                    | `18446744073709552000` |

### Identity 标识

On native platforms, `double` and `int` are distinct types: no value can be both a `double` and an `int` at the same time. On the web, that isn’t true. Because of this difference, identity can differ between platforms, although equality (`==`) doesn’t.
在本地平台上， `double` 和 `int` 是不同的类型：没有值可以同时是 `double` 和 `int` 。在网络上，情况并非如此。由于这种差异，标识可能因平台而异，尽管相等（ `==` ）不会。

The following table shows some expressions that use equality and identity. The equality expressions are the same on native and web; the identity expressions are usually different.
下表显示了一些使用相等和标识的表达式。相等表达式在本地和网络上是相同的；标识表达式通常是不同的。

| Expression 表达式                             | Native 本机 | Web     |
| --------------------------------------------- | ----------- | ------- |
| `1.0 == 1`                                    | `true`      | `true`  |
| `identical(1.0, 1)`                           | `false`     | `true`  |
| `0.0 == -0.0`                                 | `true`      | `true`  |
| `identical(0.0, -0.0)`                        | `false`     | `true`  |
| `double.nan == double.nan`                    | `false`     | `false` |
| `identical(double.nan, double.nan)`           | `true`      | `false` |
| `double.infinity == double.infinity`          | `true`      | `true`  |
| `identical(double.infinity, double.infinity)` | `true`      | `true`  |

### Types and type checking 类型和类型检查

On the web, the underlying `int` type is like a subtype of `double`: it’s a double-precision value without a fractional part. In fact, a type check on the web of the form `x is int` returns true if `x` is a number (`double`) with a zero-valued fractional part.
在网络上，基础 `int` 类型就像 `double` 的子类型：它是一个没有小数部分的双精度值。事实上，如果 `x` 是一个带有零值小数部分的数字 ( `double` )，则网络上的形式 `x is int` 的类型检查返回 true。

As a result, the following are true on the web:
因此，以下内容在网络上为真：

- All Dart numbers (values of type `num`) are `double`.
  所有 Dart 数字（类型 `num` 的值）都是 `double` 。
- A Dart number can be both a `double` and an `int` at the same time.
  一个 Dart 数字可以同时是 `double` 和 `int` 。

These facts affect `is` checks and `runtimeType` properties. A side effect is that `double.infinity` is interpreted as an `int`. Because this is a platform-specific behavior, it might change in the future.
这些事实影响 `is` 检查和 `runtimeType` 属性。一个副作用是 `double.infinity` 被解释为 `int` 。由于这是一个特定于平台的行为，因此将来可能会发生变化。

| Expression 表达式        | Native 本机 | Web      |
| ------------------------ | ----------- | -------- |
| `1 is int`               | `true`      | `true`   |
| `1 is double`            | `false`     | `true`   |
| `1.0 is int`             | `false`     | `true`   |
| `1.0 is double`          | `true`      | `true`   |
| `(0.5 + 0.5) is int`     | `false`     | `true`   |
| `(0.5 + 0.5) is double`  | `true`      | `true`   |
| `3.14 is int`            | `false`     | `false`  |
| `3.14 is double`         | `true`      | `true`   |
| `double.infinity is int` | `false`     | `true`   |
| `double.nan is int`      | `false`     | `false`  |
| `1.0.runtimeType`        | `double`    | `int`    |
| `1.runtimeType`          | `int`       | `int`    |
| `1.5.runtimeType`        | `double`    | `double` |

### Bitwise operations 按位运算

For performance reasons on the web, bitwise (`&`, `|`, `^`, `~`) and shift (`<<`,`>>`, `>>>`) operators on `int` use the native JavaScript equivalents. In JavaScript, the operands are truncated to 32-bit integers that are treated as unsigned. This treatment can lead to surprising results on larger numbers. In particular, if operands are negative or don’t fit into 32 bits, they’re likely to produce different results between native and web.
出于 Web 上的性能原因，按位（ `&` 、 `|` 、 `^` 、 `~` ）和移位（ `<<` 、 `>>` 、 `>>>` ）运算符在 `int` 上使用原生 JavaScript 等价项。在 JavaScript 中，操作数被截断为 32 位整数，并被视为无符号。这种处理可能会对较大的数字产生令人惊讶的结果。特别是，如果操作数为负数或不适合 32 位，则它们很可能在原生和 Web 之间产生不同的结果。

The following table shows how native and web platforms treat bitwise and shift operators when the operands are either negative or close to 32 bits:
下表显示了当操作数为负数或接近 32 位时，原生平台和 Web 平台如何处理按位运算符和移位运算符：

| Expression 表达式                  | Native 本机  | Web          |
| ---------------------------------- | ------------ | ------------ |
| `-1 >> 0`                          | `-1`         | `4294967295` |
| `-1 ^ 2`                           | `-3`         | `4294967293` |
| `math.pow(2, 32).toInt()`          | `4294967296` | `4294967296` |
| `math.pow(2, 32).toInt() >> 1`     | `2147483648` | `0`          |
| `(math.pow(2, 32).toInt()-1) >> 1` | `2147483647` | `2147483647` |

### String representation 字符串表示

On the web, Dart generally defers to JavaScript to convert a number to a string (for example, for a `print`). The following table demonstrates how converting the expressions in the first column can lead to different results.
在 Web 上，Dart 通常推迟到 JavaScript 将数字转换为字符串（例如，对于 `print` ）。下表演示了如何将第一列中的表达式转换为不同的结果。

| Expression 表达式 | Native `toString()` 原生 `toString()` | Web `toString()`           |
| ----------------- | ------------------------------------- | -------------------------- |
| `1`               | `"1"`                                 | `"1"`                      |
| `1.0`             | `"1.0"`                               | `"1"`                      |
| `(0.5 + 0.5)`     | `"1.0"`                               | `"1"`                      |
| `1.5`             | `"1.5"`                               | `"1.5"`                    |
| `-0`              | `"0"`                                 | `"-0.0"`                   |
| `math.pow(2, 0)`  | `"1"`                                 | `"1"`                      |
| `math.pow(2, 80)` | `"0"`                                 | `"1.2089258196146292e+24"` |

## What should you do? 您应该怎么做？

Usually, you don’t need to change your numeric code. Dart code has been running on both native and web platforms for years, and number implementation differences are rarely a problem. Common, typical code—such as iterating through a range of small integers and indexing a list—behaves the same.
通常，您无需更改数字代码。Dart 代码已经在原生平台和网络平台上运行多年，并且数字实现差异很少会成为问题。常见的典型代码（例如遍历一系列小整数和索引列表）的行为相同。

If you have tests or assertions that compare string results, write them in a platform-resilient manner. For example, suppose you’re testing the value of string expressions that have embedded numbers:
如果您有比较字符串结果的测试或断言，请以平台弹性方式编写它们。例如，假设您正在测试包含嵌入数字的字符串表达式的值：

```
void main() {
  var count = 10.0 * 2;
  var message = "$count cows";
  if (message != "20.0 cows") throw Exception("Unexpected: $message");
}
```

The preceding code succeeds on native platforms but throws on the web because `message` is `"20 cows"` (no decimal) on the web. As an alternative, you might write the condition as follows, so it passes on both native and web platforms:
前述代码在原生平台上成功，但在网络上抛出异常，因为 `message` 在网络上是 `"20 cows"` （无小数）。作为替代，您可以按如下方式编写条件，以便它在原生平台和网络平台上都通过：

```
if (message != "${20.0} cows") throw ... 
```

For bit manipulation, consider explicitly operating on 32-bit chunks, which are consistent on all platforms. To force a signed interpretation of a 32-bit chunk, use `int.toSigned(32)`.
对于位操作，请考虑显式操作 32 位块，这些块在所有平台上都一致。要强制对 32 位块进行有符号解释，请使用 `int.toSigned(32)` 。

For other cases where precision matters, consider other numeric types. The [`BigInt`](https://api.dart.dev/stable/dart-core/BigInt-class.html) type provides arbitrary-precision integers on both native and web. The [`fixnum`](https://pub.dev/packages/fixnum) package provides strict 64-bit signed numbers, even on the web. Use these types with care, though: they often result in significantly bigger and slower code.
对于精度很重要的其他情况，请考虑其他数字类型。 `BigInt` 类型在原生和网络上都提供任意精度的整数。 `fixnum` 包提供严格的 64 位有符号数字，即使在网络上也是如此。但请谨慎使用这些类型：它们通常会导致代码明显变大且变慢。
