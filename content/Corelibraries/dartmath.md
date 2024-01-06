+++
title = "dart:math"
date = 2024-01-05T20:29:36+08:00
weight = 30
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文: [https://dart.dev/libraries/dart-math](https://dart.dev/libraries/dart-math)

## dart:math

The dart:math library ([API reference](https://api.dart.dev/stable/dart-math/dart-math-library.html)) provides common functionality such as sine and cosine, maximum and minimum, and constants such as *pi* and *e*. Most of the functionality in the Math library is implemented as top-level functions.
dart:math 库（API 参考）提供常见的功能，例如正弦和余弦、最大值和最小值，以及诸如 pi 和 e 之类的常量。Math 库中的大多数功能都作为顶级函数实现。

To use this library in your app, import dart:math.
要在您的应用中使用此库，请导入 dart:math。

```dart
import 'dart:math';
```

### Trigonometry 三角学

The Math library provides basic trigonometric functions:
Math 库提供基本三角函数：

```dart
// Cosine
assert(cos(pi) == -1.0);

// Sine
var degrees = 30;
var radians = degrees * (pi / 180);
// radians is now 0.52359.
var sinOf30degrees = sin(radians);
// sin 30° = 0.5
assert((sinOf30degrees - 0.5).abs() < 0.01);
```

*info* **Note:** These functions use radians, not degrees!
注意：这些函数使用弧度，而不是角度！

### Maximum and minimum 最大值和最小值

The Math library provides `max()` and `min()` methods:
Math 库提供 `max()` 和 `min()` 方法：

```dart
assert(max(1, 1000) == 1000);
assert(min(1, -1000) == -1000);
```

### Math constants 数学常量

Find your favorite constants—*pi*, *e*, and more—in the Math library:
在 Math 库中找到您最喜欢的常量——pi、e 等：

```dart
// See the Math library for additional constants.
print(e); // 2.718281828459045
print(pi); // 3.141592653589793
print(sqrt2); // 1.4142135623730951
```

### Random numbers 随机数

Generate random numbers with the [Random](https://api.dart.dev/stable/dart-math/Random-class.html) class. You can optionally provide a seed to the Random constructor.
使用 Random 类生成随机数。您可以选择向 Random 构造函数提供种子。

```dart
var random = Random();
random.nextDouble(); // Between 0.0 and 1.0: [0, 1)
random.nextInt(10); // Between 0 and 9.
```

You can even generate random booleans:
您甚至可以生成随机布尔值：

```dart
var random = Random();
random.nextBool(); // true or false
```

*report_problem* **Warning:** The default implementation of `Random` supplies a stream of pseudorandom bits that are unsuitable for cryptographic purposes. To create a cryptographically secure random number generator, use the [`Random.secure()`](https://api.dart.dev/stable/dart-math/Random/Random.secure.html) constructor.
警告： `Random` 的默认实现提供了一系列伪随机位，不适合用于加密目的。要创建一个加密安全的随机数生成器，请使用 `Random.secure()` 构造函数。

### More information 更多信息

Refer to the [Math API reference](https://api.dart.dev/stable/dart-math/dart-math-library.html) for a full list of methods. Also see the API reference for [num,](https://api.dart.dev/stable/dart-core/num-class.html) [int,](https://api.dart.dev/stable/dart-core/int-class.html) and [double.](https://api.dart.dev/stable/dart-core/double-class.html)
有关方法的完整列表，请参阅 Math API 参考。另请参阅 num、int 和 double 的 API 参考。
