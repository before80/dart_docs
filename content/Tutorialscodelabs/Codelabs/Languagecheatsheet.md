+++
title = "语言速查表"
date = 2024-01-05T20:29:36+08:00
weight = 10
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文: [https://dart.dev/codelabs/dart-cheatsheet](https://dart.dev/codelabs/dart-cheatsheet)

## Dart cheatsheet codelab Dart 速查表代码实验室

The Dart language is designed to be easy to learn for coders coming from other languages, but it has a few unique features. This codelab walks you through the most important of these language features.

​	Dart 语言旨在让来自其他语言的编码人员易于学习，但它有一些独特的功能。本代码实验室将引导您了解这些语言功能中最重要的功能。

The embedded editors in this codelab have partially completed code snippets. You can use these editors to test your knowledge by completing the code and clicking the **Run** button. The editors also contain thorough test code; **don’t edit the test code**, but feel free to study it to learn about testing.

​	本代码实验室中的嵌入式编辑器具有部分完成的代码片段。您可以使用这些编辑器通过完成代码并单击“运行”按钮来测试您的知识。编辑器还包含全面的测试代码；不要编辑测试代码，但可以随意研究它以了解测试。

If you need help, expand the **Solution for…** dropdown beneath each DartPad for an explanation and the answer. To run the code formatter ([`dart format`](https://dart.dev/tools/dart-format)), click **Format**. The **Reset** button erases your work and restores the editor to its original state.

​	如果您需要帮助，展开每个 DartPad 下方的“解决方案”下拉列表，以获取说明和答案。要运行代码格式化程序 ( `dart format` )，请点击“格式化”。“重置”按钮会清除您的工作并使编辑器恢复到其原始状态。

*info* **Note:** This page uses embedded DartPads to display runnable examples. If you see empty boxes instead of DartPads, go to the [DartPad troubleshooting page](https://dart.dev/tools/dartpad/troubleshoot).

​	注意：此页面使用嵌入式 DartPad 来显示可运行的示例。如果您看到的是空框而不是 DartPad，请转到 DartPad 故障排除页面。

## 字符串插值 String interpolation 

To put the value of an expression inside a string, use `${expression}`. If the expression is an identifier, you can omit the `{}`.

​	要将表达式的值放入字符串中，请使用 `${expression}` 。如果表达式是标识符，您可以省略 `{}` 。

Here are some examples of using string interpolation:

​	以下是一些使用字符串插值示例：

| String 字符串               |      | Result 结果                        |
| --------------------------- | ---- | ---------------------------------- |
| `'${3 + 2}'`                |      | `'5'`                              |
| `'${"word".toUpperCase()}'` |      | `'WORD'`                           |
| `'$myObject'`               |      | The value of `myObject.toString()` |

### 代码示例 Code example 

The following function takes two integers as parameters. Make it return a string containing both integers separated by a space. For example, `stringify(2, 3)` should return `'2 3'`.

​	以下函数采用两个整数作为参数。使其返回一个字符串，其中包含用空格分隔的两个整数。例如， `stringify(2, 3)` 应返回 `'2 3'` 。

```dart
String stringify(int x, int y) {
  return '$x $y';
}

// Tests your solution (Don't edit!):
void main() {
  assert(stringify(2, 3) == '2 3',
      "Your stringify method returned '${stringify(2, 3)}' instead of '2 3'");
  print('Success!');
}
```




## 可空变量 Nullable variables 

Dart enforces sound null safety. This means values can’t be null unless you say they can be. In other words, types default to non-nullable.

​	Dart 强制执行健全的空安全。这意味着除非您说值可以为 null，否则值不能为 null。换句话说，类型默认为非空。

For example, consider the following code. With null safety, this code returns an error. A variable of type `int` can’t have the value `null`:

​	例如，考虑以下代码。使用空安全，此代码会返回一个错误。类型为 `int` 的变量不能具有值 `null` :

```dart
int a = null; // INVALID.
```

When creating a variable, add `?` to the type to indicate that the variable can be `null`:

​	创建变量时，将 `?` 添加到类型以指示变量可以为 `null` :

```dart
int? a = null; // Valid.
```

You can simplify that code a bit because, in all versions of Dart, `null` is the default value for uninitialized variables:

​	您可以稍微简化该代码，因为在所有版本的 Dart 中， `null` 是未初始化变量的默认值：

```dart
int? a; // The initial value of a is null.
```

To learn more about null safety in Dart, read the [sound null safety guide](https://dart.dev/null-safety).

​	要详细了解 Dart 中的空安全，请阅读健全空安全指南。

### 代码示例 Code example 

Try to declare two variables below:

​	尝试在下面声明两个变量：

- A nullable `String` named `name` with the value `'Jane'`.
- 一个名为 `String` 的可空变量，其值为 `'Jane'` 。
- A nullable `String` named `address` with the value `null`.
- 一个名为 `String` 的可空变量，其值为 `null` 。

Ignore all initial errors in the DartPad.

​	忽略 DartPad 中的所有初始错误。

```dart
String? name = 'Jane';
String? address;

// Tests your solution (Don't edit!):
void main() {
  try {
    if (name == 'Jane' && address == null) {
      // verify that "name" is nullable
      name = null;
      print('Success!');
    } else {
      print('Not quite right, try again!');
    }
  } catch (e) {
    print('Exception: ${e.runtimeType}');
  }
}

```




## 空感知运算符 Null-aware operators 

Dart offers some handy operators for dealing with values that might be null. One is the `??=` assignment operator, which assigns a value to a variable only if that variable is currently null:

​	Dart 提供了一些方便的运算符来处理可能为 null 的值。其中一个是 `??=` 赋值运算符，它仅在变量当前为 null 时才将值分配给该变量：

```dart
int? a; // = null
a ??= 3;
print(a); // <-- Prints 3.

a ??= 5;
print(a); // <-- Still prints 3.
```

Another null-aware operator is `??`, which returns the expression on its left unless that expression’s value is null, in which case it evaluates and returns the expression on its right:

​	另一个空感知运算符是 `??` ，它返回其左侧的表达式，除非该表达式的值为 null，在这种情况下，它将计算并返回其右侧的表达式：

```dart
print(1 ?? 3); // <-- Prints 1.
print(null ?? 12); // <-- Prints 12.
```

### 代码示例 Code example 

Try substituting in the `??=` and `??` operators to implement the described behavior in the following snippet.

​	尝试在以下代码段中替换 `??=` 和 `??` 运算符以实现所述行为。

Ignore all initial errors in the DartPad.

​	忽略 DartPad 中的所有初始错误。

```dart
String? foo = 'a string';
String? bar; // = null

// Substitute an operator that makes 'a string' be assigned to baz.
String? baz = foo ?? bar;

void updateSomeVars() {
  // Substitute an operator that makes 'a string' be assigned to bar.
  bar ??= 'a string';
}

// Tests your solution (Don't edit!):
void main() {
  try {
    updateSomeVars();

    if (foo != 'a string') {
      print('Looks like foo somehow ended up with the wrong value.');
    } else if (bar != 'a string') {
      print('Looks like bar ended up with the wrong value.');
    } else if (baz != 'a string') {
      print('Looks like baz ended up with the wrong value.');
    } else {
      print('Success!');
    }
  } catch (e) {
    print('Exception: ${e.runtimeType}.');
  }
}

```

## 条件属性访问 Conditional property access 

To guard access to a property or method of an object that might be null, put a question mark (`?`) before the dot (`.`):

​	为了保护对可能为 null 的对象的属性或方法的访问，请在点 ( `.` ) 之前放置一个问号 ( `?` )：

```dart
myObject?.someProperty
```

The preceding code is equivalent to the following:

​	前面的代码等效于以下代码：

```dart
(myObject != null) ? myObject.someProperty : null
```

You can chain multiple uses of `?.` together in a single expression:

​	您可以在单个表达式中将 `?.` 的多个用法链接在一起：

```dart
myObject?.someProperty?.someMethod()
```

The preceding code returns null (and never calls `someMethod()`) if either `myObject` or `myObject.someProperty` is null.

​	如果 `myObject` 或 `myObject.someProperty` 为 null，则前一个代码返回 null（并且从不调用 `someMethod()` ）。

### 代码示例 Code example 

The following function takes a nullable string as a parameter. Try using conditional property access to make it return the uppercase version of `str`, or `null` if `str` is `null`.

​	以下函数将可为 null 的字符串作为参数。尝试使用条件属性访问使其返回 `str` 的大写版本，或在 `str` 为 `null` 时返回 `null` 。

```dart
String? upperCaseIt(String? str) {
  return str?.toUpperCase();
}

// Tests your solution (Don't edit!):
void main() {
  try {
    String? one = upperCaseIt(null);
    if (one != null) {
      print('Looks like you\'re not returning null for null inputs.');
    } else {
      print('Success when str is null!');
    }
  } catch (e) {
    print(
        'Tried calling upperCaseIt(null) and got an exception: \n ${e.runtimeType}.');
  }

  try {
    String? two = upperCaseIt('a string');
    if (two == null) {
      print('Looks like you\'re returning null even when str has a value.');
    } else if (two != 'A STRING') {
      print(
          'Tried upperCaseIt(\'a string\'), but didn\'t get \'A STRING\' in response.');
    } else {
      print('Success when str is not null!');
    }
  } catch (e) {
    print(
        'Tried calling upperCaseIt(\'a string\') and got an exception: \n ${e.runtimeType}.');
  }
}

```




## 集合字面量 Collection literals 

Dart has built-in support for lists, maps, and sets. You can create them using literals:

​	Dart 内置对列表、映射和集合的支持。您可以使用字面量创建它们：

```dart
final aListOfStrings = ['one', 'two', 'three'];
final aSetOfStrings = {'one', 'two', 'three'};
final aMapOfStringsToInts = {
  'one': 1,
  'two': 2,
  'three': 3,
};
```

Dart’s type inference can assign types to these variables for you. In this case, the inferred types are `List<String>`, `Set<String>`, and `Map<String, int>`.

​	Dart 的类型推断可以为您将类型分配给这些变量。在这种情况下，推断的类型是 `List<String>` 、 `Set<String>` 和 `Map<String, int>` 。

Or you can specify the type yourself:

​	或者，您可以自己指定类型：

```dart
final aListOfInts = <int>[];
final aSetOfInts = <int>{};
final aMapOfIntToDouble = <int, double>{};
```

Specifying types is handy when you initialize a list with contents of a subtype, but still want the list to be `List<BaseType>`:

​	当您使用子类型的內容初始化列表，但仍希望列表是 `List<BaseType>` 时，指定类型非常方便：

```dart
final aListOfBaseType = <BaseType>[SubType(), SubType()];
```

### 代码示例 Code example 

Try setting the following variables to the indicated values. Replace the existing null values.

​	尝试将以下变量设置为指示的值。替换现有的 null 值。

```dart
// Assign this a list containing 'a', 'b', and 'c' in that order:
final aListOfStrings = ['a', 'b', 'c'];

// Assign this a set containing 3, 4, and 5:
final aSetOfInts = {3, 4, 5};

// Assign this a map of String to int so that aMapOfStringsToInts['myKey'] returns 12:
final aMapOfStringsToInts = {'myKey': 12};

// Assign this an empty List<double>:
final anEmptyListOfDouble = <double>[];

// Assign this an empty Set<String>:
final anEmptySetOfString = <String>{};

// Assign this an empty Map of double to int:
final anEmptyMapOfDoublesToInts = <double, int>{};

// Tests your solution (Don't edit!):
void main() {
  final errs = <String>[];

  if (aListOfStrings is! List<String>) {
    errs.add('aListOfStrings should have the type List<String>.');
  } else if (aListOfStrings.length != 3) {
    errs.add(
        'aListOfStrings has ${aListOfStrings.length} items in it, \n rather than the expected 3.');
  } else if (aListOfStrings[0] != 'a' ||
      aListOfStrings[1] != 'b' ||
      aListOfStrings[2] != 'c') {
    errs.add(
        'aListOfStrings doesn\'t contain the correct values (\'a\', \'b\', \'c\').');
  }

  if (aSetOfInts is! Set<int>) {
    errs.add('aSetOfInts should have the type Set<int>.');
  } else if (aSetOfInts.length != 3) {
    errs.add(
        'aSetOfInts has ${aSetOfInts.length} items in it, \n rather than the expected 3.');
  } else if (!aSetOfInts.contains(3) ||
      !aSetOfInts.contains(4) ||
      !aSetOfInts.contains(5)) {
    errs.add('aSetOfInts doesn\'t contain the correct values (3, 4, 5).');
  }

  if (aMapOfStringsToInts is! Map<String, int>) {
    errs.add('aMapOfStringsToInts should have the type Map<String, int>.');
  } else if (aMapOfStringsToInts['myKey'] != 12) {
    errs.add(
        'aMapOfStringsToInts doesn\'t contain the correct values (\'myKey\': 12).');
  }

  if (anEmptyListOfDouble is! List<double>) {
    errs.add('anEmptyListOfDouble should have the type List<double>.');
  } else if (anEmptyListOfDouble.isNotEmpty) {
    errs.add('anEmptyListOfDouble should be empty.');
  }

  if (anEmptySetOfString is! Set<String>) {
    errs.add('anEmptySetOfString should have the type Set<String>.');
  } else if (anEmptySetOfString.isNotEmpty) {
    errs.add('anEmptySetOfString should be empty.');
  }

  if (anEmptyMapOfDoublesToInts is! Map<double, int>) {
    errs.add(
        'anEmptyMapOfDoublesToInts should have the type Map<double, int>.');
  } else if (anEmptyMapOfDoublesToInts.isNotEmpty) {
    errs.add('anEmptyMapOfDoublesToInts should be empty.');
  }

  if (errs.isEmpty) {
    print('Success!');
  } else {
    errs.forEach(print);
  }

  // ignore_for_file: unnecessary_type_check
}

```

## 箭头语法 Arrow syntax 

You might have seen the `=>` symbol in Dart code. This arrow syntax is a way to define a function that executes the expression to its right and returns its value.

​	您可能在 Dart 代码中看到过 `=>` 符号。此箭头语法是一种定义函数的方法，该函数执行其右侧的表达式并返回其值。

For example, consider this call to the `List` class’s `any()` method:

​	例如，考虑对 `List` 类的方法 `any()` 的此调用：

```dart
bool hasEmpty = aListOfStrings.any((s) {
  return s.isEmpty;
});
```

Here’s a simpler way to write that code:

​	下面是编写该代码的更简单方法：

```dart
bool hasEmpty = aListOfStrings.any((s) => s.isEmpty);
```

### 代码示例 Code example 

Try finishing the following statements, which use arrow syntax.

​	尝试完成以下使用箭头语法的语句。

```dart
class MyClass {
  int value1 = 2;
  int value2 = 3;
  int value3 = 5;

  // Returns the product of the above values:
  int get product => value1 * value2 * value3;

  // Adds 1 to value1:
  void incrementValue1() => value1++;

  // Returns a string containing each item in the
  // list, separated by commas (e.g. 'a,b,c'):
  String joinWithCommas(List<String> strings) => strings.join(',');
}

// Tests your solution (Don't edit!):
void main() {
  final obj = MyClass();
  final errs = <String>[];

  try {
    final product = obj.product;

    if (product != 30) {
      errs.add(
          'The product property returned $product \n instead of the expected value (30).');
    }
  } catch (e) {
    print(
        'Tried to use MyClass.product, but encountered an exception: \n ${e.runtimeType}.');
    return;
  }

  try {
    obj.incrementValue1();

    if (obj.value1 != 3) {
      errs.add(
          'After calling incrementValue, value1 was ${obj.value1} \n instead of the expected value (3).');
    }
  } catch (e) {
    print(
        'Tried to use MyClass.incrementValue1, but encountered an exception: \n ${e.runtimeType}.');
    return;
  }

  try {
    final joined = obj.joinWithCommas(['one', 'two', 'three']);

    if (joined != 'one,two,three') {
      errs.add(
          'Tried calling joinWithCommas([\'one\', \'two\', \'three\']) \n and received $joined instead of the expected value (\'one,two,three\').');
    }
  } catch (e) {
    print(
        'Tried to use MyClass.joinWithCommas, but encountered an exception: \n ${e.runtimeType}.');
    return;
  }

  if (errs.isEmpty) {
    print('Success!');
  } else {
    errs.forEach(print);
  }
}

```

## 级联 Cascades 

To perform a sequence of operations on the same object, use cascades (`..`). We’ve all seen an expression like this:

​	若要对同一对象执行一系列操作，请使用级联（ `..` ）。我们都见过类似这样的表达式：

```dart
myObject.someMethod()
```

It invokes `someMethod()` on `myObject`, and the result of the expression is the return value of `someMethod()`.

​	它在 `myObject` 上调用 `someMethod()` ，表达式的结果是 `someMethod()` 的返回值。

Here’s the same expression with a cascade:

​	下面是带有级联的相同表达式：

```dart
myObject..someMethod()
```

Although it still invokes `someMethod()` on `myObject`, the result of the expression **isn’t** the return value—it’s a reference to `myObject`!

​	虽然它仍然在 `myObject` 上调用 `someMethod()` ，但表达式的结果不是返回值，而是对 `myObject` 的引用！

Using cascades, you can chain together operations that would otherwise require separate statements. For example, consider the following code, which uses the conditional member access operator (`?.`) to read properties of `button` if it isn’t `null`:

​	使用级联，您可以将原本需要单独语句的操作链接在一起。例如，考虑以下代码，它使用条件成员访问运算符 ( `?.` ) 来读取 `button` 的属性（如果它不是 `null` ）：

```dart
var button = querySelector('#confirm');
button?.text = 'Confirm';
button?.classes.add('important');
button?.onClick.listen((e) => window.alert('Confirmed!'));
button?.scrollIntoView();
```

To instead use cascades, you can start with the *null-shorting* cascade (`?..`), which guarantees that none of the cascade operations are attempted on a `null` object. Using cascades shortens the code and makes the `button` variable unnecessary:

​	相反，要使用级联，您可以从 null 缩短级联 ( `?..` ) 开始，这可确保不会对 `null` 对象尝试任何级联操作。使用级联可缩短代码，并且使 `button` 变量变得不必要：

```dart
querySelector('#confirm')
  ?..text = 'Confirm'
  ..classes.add('important')
  ..onClick.listen((e) => window.alert('Confirmed!'))
  ..scrollIntoView();
```

### 代码示例 Code example 

Use cascades to create a single statement that sets the `anInt`, `aString`, and `aList` properties of a `BigObject` to `1`, `'String!'`, and `[3.0]` (respectively) and then calls `allDone()`.

​	使用级联创建单个语句，将 `BigObject` 的 `anInt` 、 `aString` 和 `aList` 属性分别设置为 `1` 、 `'String!'` 和 `[3.0]` ，然后调用 `allDone()` 。

```dart
class BigObject {
  int anInt = 0;
  String aString = '';
  List<double> aList = [];
  bool _done = false;

  void allDone() {
    _done = true;
  }
}

BigObject fillBigObject(BigObject obj) {
  return obj
    ..anInt = 1
    ..aString = 'String!'
    ..aList.add(3)
    ..allDone();
}

// Tests your solution (Don't edit!):
void main() {
  BigObject obj;

  try {
    obj = fillBigObject(BigObject());
  } catch (e) {
    print(
        'Caught an exception of type ${e.runtimeType} \n while running fillBigObject');
    return;
  }

  final errs = <String>[];

  if (obj.anInt != 1) {
    errs.add(
        'The value of anInt was ${obj.anInt} \n rather than the expected (1).');
  }

  if (obj.aString != 'String!') {
    errs.add(
        'The value of aString was \'${obj.aString}\' \n rather than the expected (\'String!\').');
  }

  if (obj.aList.length != 1) {
    errs.add(
        'The length of aList was ${obj.aList.length} \n rather than the expected value (1).');
  } else {
    if (obj.aList[0] != 3.0) {
      errs.add(
          'The value found in aList was ${obj.aList[0]} \n rather than the expected (3.0).');
    }
  }

  if (!obj._done) {
    errs.add('It looks like allDone() wasn\'t called.');
  }

  if (errs.isEmpty) {
    print('Success!');
  } else {
    errs.forEach(print);
  }
}

```

## 获取器和设置器 Getters and setters 

You can define getters and setters whenever you need more control over a property than a simple field allows.

​	只要您需要对属性进行比简单字段更多的控制，就可以定义获取器和设置器。

For example, you can make sure a property’s value is valid:

​	例如，您可以确保属性的值有效：

```dart
class MyClass {
  int _aProperty = 0;

  int get aProperty => _aProperty;

  set aProperty(int value) {
    if (value >= 0) {
      _aProperty = value;
    }
  }
}
```

You can also use a getter to define a computed property:

​	您还可以使用 getter 来定义计算属性：

```dart
class MyClass {
  final List<int> _values = [];

  void addValue(int value) {
    _values.add(value);
  }

  // A computed property.
  int get count {
    return _values.length;
  }
}
```

### 代码示例 Code example 

Imagine you have a shopping cart class that keeps a private `List<double>` of prices. Add the following:

​	想象一下，您有一个购物车类，其中包含一个私有 `List<double>` 价格。添加以下内容：

- A getter called `total` that returns the sum of the prices
- 一个名为 `total` 的 getter，用于返回价格的总和
- A setter that replaces the list with a new one, as long as the new list doesn’t contain any negative prices (in which case the setter should throw an `InvalidPriceException`).
- 一个 setter，用于将列表替换为一个新列表，只要新列表不包含任何负价格（在这种情况下，setter 应抛出 `InvalidPriceException` ）。

Ignore all initial errors in the DartPad.

​	忽略 DartPad 中的所有初始错误。

```dart
class InvalidPriceException {}

class ShoppingCart {
  List<double> _prices = [];

  // Add a "total" getter here:
  double get total => _prices.fold(0, (e, t) => e + t);

  // Add a "prices" setter here:
  set prices(List<double> value) {
    if (value.any((p) => p < 0)) {
      throw InvalidPriceException();
    }

    _prices = value;
  }
}

// Tests your solution (Don't edit!):
void main() {
  var foundException = false;

  try {
    final cart = ShoppingCart();
    cart.prices = [12.0, 12.0, -23.0];
  } on InvalidPriceException {
    foundException = true;
  } catch (e) {
    print(
        'Tried setting a negative price and received a ${e.runtimeType} \n instead of an InvalidPriceException.');
    return;
  }

  if (!foundException) {
    print(
        'Tried setting a negative price \n and didn\'t get an InvalidPriceException.');
    return;
  }

  final secondCart = ShoppingCart();

  try {
    secondCart.prices = [1.0, 2.0, 3.0];
  } catch (e) {
    print(
        'Tried setting prices with a valid list, \n but received an exception: ${e.runtimeType}.');
    return;
  }

  if (secondCart._prices.length != 3) {
    print(
        'Tried setting prices with a list of three values, \n but _prices ended up having length ${secondCart._prices.length}.');
    return;
  }

  if (secondCart._prices[0] != 1.0 ||
      secondCart._prices[1] != 2.0 ||
      secondCart._prices[2] != 3.0) {
    final vals = secondCart._prices.map((p) => p.toString()).join(', ');
    print(
        'Tried setting prices with a list of three values (1, 2, 3), \n but incorrect ones ended up in the price list ($vals) .');
    return;
  }

  var sum = 0.0;

  try {
    sum = secondCart.total;
  } catch (e) {
    print('Tried to get total, but received an exception: ${e.runtimeType}.');
    return;
  }

  if (sum != 6.0) {
    print(
        'After setting prices to (1, 2, 3), total returned $sum instead of 6.');
    return;
  }

  print('Success!');
}

```

## 可选位置参数 Optional positional parameters 

Dart has two kinds of function parameters: positional and named. Positional parameters are the kind you’re likely familiar with:

​	Dart 有两种函数参数：位置参数和命名参数。位置参数是您可能熟悉的那种：

```dart
int sumUp(int a, int b, int c) {
  return a + b + c;
}
// ···
  int total = sumUp(1, 2, 3);
```

With Dart, you can make these positional parameters optional by wrapping them in brackets:

​	使用 Dart，您可以通过将这些位置参数括起来使其成为可选参数：

```dart
int sumUpToFive(int a, [int? b, int? c, int? d, int? e]) {
  int sum = a;
  if (b != null) sum += b;
  if (c != null) sum += c;
  if (d != null) sum += d;
  if (e != null) sum += e;
  return sum;
}
// ···
  int total = sumUpToFive(1, 2);
  int otherTotal = sumUpToFive(1, 2, 3, 4, 5);
```

Optional positional parameters are always last in a function’s parameter list. Their default value is null unless you provide another default value:

​	可选位置参数始终位于函数的参数列表的最后。除非您提供另一个默认值，否则它们的默认值为 null：

```dart
int sumUpToFive(int a, [int b = 2, int c = 3, int d = 4, int e = 5]) {
// ···
}
// ···
  int newTotal = sumUpToFive(1);
  print(newTotal); // <-- prints 15
```

### 代码示例 Code example 

Implement a function called `joinWithCommas()` that accepts one to five integers, then returns a string of those numbers separated by commas. Here are some examples of function calls and returned values:

​	实现一个名为 `joinWithCommas()` 的函数，该函数接受一到五个整数，然后返回一个由逗号分隔的数字字符串。以下是函数调用和返回值的一些示例：

| Function call 函数调用          |      | Returned value 返回值 |
| ------------------------------- | ---- | --------------------- |
| `joinWithCommas(1)`             |      | `'1'`                 |
| `joinWithCommas(1, 2, 3)`       |      | `'1,2,3'`             |
| `joinWithCommas(1, 1, 1, 1, 1)` |      | `'1,1,1,1,1'`         |



```dart
String joinWithCommas(int a, [int? b, int? c, int? d, int? e]) {
  var total = '$a';
  if (b != null) total = '$total,$b';
  if (c != null) total = '$total,$c';
  if (d != null) total = '$total,$d';
  if (e != null) total = '$total,$e';
  return total;
}

// Tests your solution (Don't edit!):
void main() {
  final errs = <String>[];

  try {
    final value = joinWithCommas(1);

    if (value != '1') {
      errs.add(
          'Tried calling joinWithCommas(1) \n and got $value instead of the expected (\'1\').');
    }
  } on UnimplementedError {
    print(
        'Tried to call joinWithCommas but failed. \n Did you implement the method?');
    return;
  } catch (e) {
    print(
        'Tried calling joinWithCommas(1), \n but encountered an exception: ${e.runtimeType}.');
    return;
  }

  try {
    final value = joinWithCommas(1, 2, 3);

    if (value != '1,2,3') {
      errs.add(
          'Tried calling joinWithCommas(1, 2, 3) \n and got $value instead of the expected (\'1,2,3\').');
    }
  } on UnimplementedError {
    print(
        'Tried to call joinWithCommas but failed. \n Did you implement the method?');
    return;
  } catch (e) {
    print(
        'Tried calling joinWithCommas(1, 2 ,3), \n but encountered an exception: ${e.runtimeType}.');
    return;
  }

  try {
    final value = joinWithCommas(1, 2, 3, 4, 5);

    if (value != '1,2,3,4,5') {
      errs.add(
          'Tried calling joinWithCommas(1, 2, 3, 4, 5) \n and got $value instead of the expected (\'1,2,3,4,5\').');
    }
  } on UnimplementedError {
    print(
        'Tried to call joinWithCommas but failed. \n Did you implement the method?');
    return;
  } catch (e) {
    print(
        'Tried calling stringify(1, 2, 3, 4 ,5), \n but encountered an exception: ${e.runtimeType}.');
    return;
  }

  if (errs.isEmpty) {
    print('Success!');
  } else {
    errs.forEach(print);
  }
}

```



## 命名参数 Named parameters 

Using a curly brace syntax at the end of the parameter list, you can define parameters that have names.

​	在参数列表末尾使用大括号语法，可以定义具有名称的参数。

Named parameters are optional unless they’re explicitly marked as `required`.

​	命名参数是可选的，除非它们被明确标记为 `required` 。

```dart
void printName(String firstName, String lastName, {String? middleName}) {
  print('$firstName ${middleName ?? ''} $lastName');
}
// ···
  printName('Dash', 'Dartisan');
  printName('John', 'Smith', middleName: 'Who');
  // Named arguments can be placed anywhere in the argument list
  printName('John', middleName: 'Who', 'Smith');
```

As you might expect, the default value of a nullable named parameter is `null`, but you can provide a custom default value.

​	正如你所料，可空命名参数的默认值为 `null` ，但你可以提供自定义默认值。

If the type of a parameter is non-nullable, then you must either provide a default value (as shown in the following code) or mark the parameter as `required` (as shown in the [constructor section](https://dart.dev/codelabs/dart-cheatsheet#using-this-in-a-constructor)).

​	如果参数的类型为非空，则你必须提供一个默认值（如下面的代码所示）或将参数标记为 `required` （如构造函数部分所示）。

```dart
void printName(String firstName, String lastName, {String middleName = ''}) {
  print('$firstName $middleName $lastName');
}
```

A function can’t have both optional positional and named parameters.

​	一个函数不能同时具有可选的位置参数和命名参数。

### 代码示例 Code example 

Add a `copyWith()` instance method to the `MyDataObject` class. It should take three named, nullable parameters:

​	向 `MyDataObject` 类添加一个 `copyWith()` 实例方法。它应采用三个命名的可空参数：

- `int? newInt`
- `String? newString`
- `double? newDouble`

Your `copyWith()` method should return a new `MyDataObject` based on the current instance, with data from the preceding parameters (if any) copied into the object’s properties. For example, if `newInt` is non-null, then copy its value into `anInt`.

​	您的 `copyWith()` 方法应基于当前实例返回一个新的 `MyDataObject` ，其中将前一个参数（如果有）中的数据复制到对象的属性中。例如，如果 `newInt` 为非空，则将其值复制到 `anInt` 中。

Ignore all initial errors in the DartPad.

​	忽略 DartPad 中的所有初始错误。

```dart
class MyDataObject {
  final int anInt;
  final String aString;
  final double aDouble;

  MyDataObject({
    this.anInt = 1,
    this.aString = 'Old!',
    this.aDouble = 2.0,
  });

  MyDataObject copyWith({int? newInt, String? newString, double? newDouble}) {
    return MyDataObject(
      anInt: newInt ?? this.anInt,
      aString: newString ?? this.aString,
      aDouble: newDouble ?? this.aDouble,
    );
  }
}

// Tests your solution (Don't edit!):
void main() {
  final source = MyDataObject();
  final errs = <String>[];

  try {
    final copy = source.copyWith(newInt: 12, newString: 'New!', newDouble: 3.0);

    if (copy.anInt != 12) {
      errs.add(
          'Called copyWith(newInt: 12, newString: \'New!\', newDouble: 3.0), \n and the new object\'s anInt was ${copy.anInt} rather than the expected value (12).');
    }

    if (copy.aString != 'New!') {
      errs.add(
          'Called copyWith(newInt: 12, newString: \'New!\', newDouble: 3.0), \n and the new object\'s aString was ${copy.aString} rather than the expected value (\'New!\').');
    }

    if (copy.aDouble != 3) {
      errs.add(
          'Called copyWith(newInt: 12, newString: \'New!\', newDouble: 3.0), \n and the new object\'s aDouble was ${copy.aDouble} rather than the expected value (3).');
    }
  } catch (e) {
    print(
        'Called copyWith(newInt: 12, newString: \'New!\', newDouble: 3.0) \n and got an exception: ${e.runtimeType}');
  }

  try {
    final copy = source.copyWith();

    if (copy.anInt != 1) {
      errs.add(
          'Called copyWith(), and the new object\'s anInt was ${copy.anInt} \n rather than the expected value (1).');
    }

    if (copy.aString != 'Old!') {
      errs.add(
          'Called copyWith(), and the new object\'s aString was ${copy.aString} \n rather than the expected value (\'Old!\').');
    }

    if (copy.aDouble != 2) {
      errs.add(
          'Called copyWith(), and the new object\'s aDouble was ${copy.aDouble} \n rather than the expected value (2).');
    }
  } catch (e) {
    print('Called copyWith() and got an exception: ${e.runtimeType}');
  }

  if (errs.isEmpty) {
    print('Success!');
  } else {
    errs.forEach(print);
  }
}

```

## 异常 Exceptions 

Dart code can throw and catch exceptions. In contrast to Java, all of Dart’s exceptions are unchecked exceptions. Methods don’t declare which exceptions they might throw, and you aren’t required to catch any exceptions.

​	Dart 代码可以抛出和捕获异常。与 Java 不同，Dart 的所有异常都是未检查异常。方法不会声明它们可能抛出的异常，并且您不需要捕获任何异常。

Dart provides `Exception` and `Error` types, but you’re allowed to throw any non-null object:

​	Dart 提供 `Exception` 和 `Error` 类型，但允许您抛出任何非空对象：

```dart
throw Exception('Something bad happened.');
throw 'Waaaaaaah!';
```

Use the `try`, `on`, and `catch` keywords when handling exceptions:

​	处理异常时使用 `try` 、 `on` 和 `catch` 关键字：

```dart
try {
  breedMoreLlamas();
} on OutOfLlamasException {
  // A specific exception
  buyMoreLlamas();
} on Exception catch (e) {
  // Anything else that is an exception
  print('Unknown exception: $e');
} catch (e) {
  // No specified type, handles all
  print('Something really unknown: $e');
}
```

The `try` keyword works as it does in most other languages. Use the `on` keyword to filter for specific exceptions by type, and the `catch` keyword to get a reference to the exception object.

​	关键字 `try` 的作用与大多数其他语言中的作用相同。使用关键字 `on` 按类型过滤特定异常，使用关键字 `catch` 获取对异常对象的引用。

If you can’t completely handle the exception, use the `rethrow` keyword to propagate the exception:

​	如果无法完全处理异常，请使用关键字 `rethrow` 传播异常：

```dart
try {
  breedMoreLlamas();
} catch (e) {
  print('I was just trying to breed llamas!');
  rethrow;
}
```

To execute code whether or not an exception is thrown, use `finally`:
无论是否引发异常，都要执行代码，请使用 `finally` ：

```dart
try {
  breedMoreLlamas();
} catch (e) {
  // ... handle exception ...
} finally {
  // Always clean up, even if an exception is thrown.
  cleanLlamaStalls();
}
```

### 代码示例 Code example 

Implement `tryFunction()` below. It should execute an untrustworthy method and then do the following:

​	在下面实现 `tryFunction()` 。它应该执行一个不可信方法，然后执行以下操作：

- If `untrustworthy()` throws an `ExceptionWithMessage`, call `logger.logException` with the exception type and message (try using `on` and `catch`).
- 如果 `untrustworthy()` 引发 `ExceptionWithMessage` ，则使用异常类型和消息调用 `logger.logException` （尝试使用 `on` 和 `catch` ）。
- If `untrustworthy()` throws an `Exception`, call `logger.logException` with the exception type (try using `on` for this one).
- 如果 `untrustworthy()` 引发 `Exception` ，则使用异常类型调用 `logger.logException` （尝试为此使用 `on` ）。
- If `untrustworthy()` throws any other object, don’t catch the exception.
- 如果 `untrustworthy()` 引发任何其他对象，则不捕获异常。
- After everything’s caught and handled, call `logger.doneLogging` (try using `finally`).
- 在捕获并处理所有内容后，调用 `logger.doneLogging` （尝试使用 `finally` ）。

```dart
typedef VoidFunction = void Function();

class ExceptionWithMessage {
  final String message;
  const ExceptionWithMessage(this.message);
}

// Call logException to log an exception, and doneLogging when finished.
abstract class Logger {
  void logException(Type t, [String? msg]);
  void doneLogging();
}

void tryFunction(VoidFunction untrustworthy, Logger logger) {
  try {
    untrustworthy();
  } on ExceptionWithMessage catch (e) {
    logger.logException(e.runtimeType, e.message);
  } on Exception {
    logger.logException(Exception);
  } finally {
    logger.doneLogging();
  }
}

// Tests your solution (Don't edit!):
class MyLogger extends Logger {
  Type? lastType;
  String lastMessage = '';
  bool done = false;

  void logException(Type t, [String? message]) {
    lastType = t;
    lastMessage = message ?? lastMessage;
  }

  void doneLogging() => done = true;
}

void main() {
  final errs = <String>[];
  var logger = MyLogger();

  try {
    tryFunction(() => throw Exception(), logger);

    if ('${logger.lastType}' != 'Exception' &&
        '${logger.lastType}' != '_Exception') {
      errs.add(
          'Untrustworthy threw an Exception, but a different type was logged: \n ${logger.lastType}.');
    }

    if (logger.lastMessage != '') {
      errs.add(
          'Untrustworthy threw an Exception with no message, but a message \n was logged anyway: \'${logger.lastMessage}\'.');
    }

    if (!logger.done) {
      errs.add(
          'Untrustworthy threw an Exception, \n and doneLogging() wasn\'t called afterward.');
    }
  } catch (e) {
    print(
        'Untrustworthy threw an exception, and an exception of type \n ${e.runtimeType} was unhandled by tryFunction.');
  }

  logger = MyLogger();

  try {
    tryFunction(() => throw ExceptionWithMessage('Hey!'), logger);

    if (logger.lastType != ExceptionWithMessage) {
      errs.add(
          'Untrustworthy threw an ExceptionWithMessage(\'Hey!\'), but a \n different type was logged: ${logger.lastType}.');
    }

    if (logger.lastMessage != 'Hey!') {
      errs.add(
          'Untrustworthy threw an ExceptionWithMessage(\'Hey!\'), but a \n different message was logged: \'${logger.lastMessage}\'.');
    }

    if (!logger.done) {
      errs.add(
          'Untrustworthy threw an ExceptionWithMessage(\'Hey!\'), \n and doneLogging() wasn\'t called afterward.');
    }
  } catch (e) {
    print(
        'Untrustworthy threw an ExceptionWithMessage(\'Hey!\'), \n and an exception of type ${e.runtimeType} was unhandled by tryFunction.');
  }

  logger = MyLogger();
  bool caughtStringException = false;

  try {
    tryFunction(() => throw 'A String', logger);
  } on String {
    caughtStringException = true;
  }

  if (!caughtStringException) {
    errs.add(
        'Untrustworthy threw a string, and it was incorrectly handled inside tryFunction().');
  }

  logger = MyLogger();

  try {
    tryFunction(() {}, logger);

    if (logger.lastType != null) {
      errs.add(
          'Untrustworthy didn\'t throw an Exception, \n but one was logged anyway: ${logger.lastType}.');
    }

    if (logger.lastMessage != '') {
      errs.add(
          'Untrustworthy didn\'t throw an Exception with no message, \n but a message was logged anyway: \'${logger.lastMessage}\'.');
    }

    if (!logger.done) {
      errs.add(
          'Untrustworthy didn\'t throw an Exception, \n but doneLogging() wasn\'t called afterward.');
    }
  } catch (e) {
    print(
        'Untrustworthy didn\'t throw an exception, \n but an exception of type ${e.runtimeType} was unhandled by tryFunction anyway.');
  }

  if (errs.isEmpty) {
    print('Success!');
  } else {
    errs.forEach(print);
  }
}

```

## 在构造函数中使用 `this` Using `this` in a constructor 

Dart provides a handy shortcut for assigning values to properties in a constructor: use `this.propertyName` when declaring the constructor:

​	Dart 提供了一个便捷的快捷方式，用于在构造函数中为属性分配值：在声明构造函数时使用 `this.propertyName` ：

```dart
class MyColor {
  int red;
  int green;
  int blue;

  MyColor(this.red, this.green, this.blue);
}

final color = MyColor(80, 80, 128);
```

This technique works for named parameters, too. Property names become the names of the parameters:

​	此技术也适用于命名参数。属性名称变为参数的名称：

```dart
class MyColor {
  ...

  MyColor({required this.red, required this.green, required this.blue});
}

final color = MyColor(red: 80, green: 80, blue: 80);
```

In the preceding code, `red`, `green`, and `blue` are marked as `required` because these `int` values can’t be null. If you add default values, you can omit `required`:

​	在前面的代码中， `red` 、 `green` 和 `blue` 被标记为 `required` ，因为这些 `int` 值不能为 null。如果添加默认值，则可以省略 `required` ：

```dart
MyColor([this.red = 0, this.green = 0, this.blue = 0]);
// or
MyColor({this.red = 0, this.green = 0, this.blue = 0});
```

### 代码示例 Code example 

Add a one-line constructor to `MyClass` that uses `this.` syntax to receive and assign values for all three properties of the class.

​	为 `MyClass` 添加一个单行构造函数，该函数使用 `this.` 语法来接收和分配类的所有三个属性的值。

Ignore all initial errors in the DartPad.

​	忽略 DartPad 中的所有初始错误。

```dart
class MyClass {
  final int anInt;
  final String aString;
  final double aDouble;

  MyClass(this.anInt, this.aString, this.aDouble);
}

// Tests your solution (Don't edit!):
void main() {
  final errs = <String>[];

  try {
    final obj = MyClass(1, 'two', 3);

    if (obj.anInt != 1) {
      errs.add(
          'Called MyClass(1, \'two\', 3) and got an object with anInt of ${obj.anInt} \n instead of the expected value (1).');
    }

    if (obj.anInt != 1) {
      errs.add(
          'Called MyClass(1, \'two\', 3) and got an object with aString of \'${obj.aString}\' \n instead of the expected value (\'two\').');
    }

    if (obj.anInt != 1) {
      errs.add(
          'Called MyClass(1, \'two\', 3) and got an object with aDouble of ${obj.aDouble} \n instead of the expected value (3).');
    }
  } catch (e) {
    print(
        'Called MyClass(1, \'two\', 3) and got an exception \n of type ${e.runtimeType}.');
  }

  if (errs.isEmpty) {
    print('Success!');
  } else {
    errs.forEach(print);
  }
}

```

## 初始化器列表 Initializer lists 

Sometimes when you implement a constructor, you need to do some setup before the constructor body executes. For example, final fields must have values before the constructor body executes. Do this work in an initializer list, which goes between the constructor’s signature and its body:

​	有时，在实现构造函数时，您需要在构造函数主体执行之前进行一些设置。例如，final 字段必须在构造函数主体执行之前具有值。在初始化器列表中执行此操作，该列表位于构造函数的签名及其主体之间：

```dart
Point.fromJson(Map<String, double> json)
    : x = json['x']!,
      y = json['y']! {
  print('In Point.fromJson(): ($x, $y)');
}
```

The initializer list is also a handy place to put asserts, which run only during development:

​	初始化器列表也是放置断言的便捷位置，断言仅在开发期间运行：

```dart
NonNegativePoint(this.x, this.y)
    : assert(x >= 0),
      assert(y >= 0) {
  print('I just made a NonNegativePoint: ($x, $y)');
}
```

### 代码示例 Code example 

Complete the `FirstTwoLetters` constructor below. Use an initializer list to assign the first two characters in `word` to the `letterOne` and `LetterTwo` properties. For extra credit, add an `assert` to catch words of less than two characters.

​	完成下面的 `FirstTwoLetters` 构造函数。使用初始化器列表将 `word` 中的前两个字符分配给 `letterOne` 和 `LetterTwo` 属性。为了获得额外积分，添加一个 `assert` 来捕获少于两个字符的单词。

Ignore all initial errors in the DartPad.

​	忽略 DartPad 中的所有初始错误。

```dart
class FirstTwoLetters {
  final String letterOne;
  final String letterTwo;

  FirstTwoLetters(String word)
      : assert(word.length >= 2),
        letterOne = word[0],
        letterTwo = word[1];
}

// Tests your solution (Don't edit!):
void main() {
  final errs = <String>[];

  try {
    final result = FirstTwoLetters('My String');

    if (result.letterOne != 'M') {
      errs.add(
          'Called FirstTwoLetters(\'My String\') and got an object with \n letterOne equal to \'${result.letterOne}\' instead of the expected value (\'M\').');
    }

    if (result.letterTwo != 'y') {
      errs.add(
          'Called FirstTwoLetters(\'My String\') and got an object with \n letterTwo equal to \'${result.letterTwo}\' instead of the expected value (\'y\').');
    }
  } catch (e) {
    errs.add(
        'Called FirstTwoLetters(\'My String\') and got an exception \n of type ${e.runtimeType}.');
  }

  bool caughtException = false;

  try {
    FirstTwoLetters('');
  } catch (e) {
    caughtException = true;
  }

  if (!caughtException) {
    errs.add(
        'Called FirstTwoLetters(\'\') and didn\'t get an exception \n from the failed assertion.');
  }

  if (errs.isEmpty) {
    print('Success!');
  } else {
    errs.forEach(print);
  }
}

```

## 命名构造函数 Named constructors 

To allow classes to have multiple constructors, Dart supports named constructors:

​	为了允许类具有多个构造函数，Dart 支持命名构造函数：

```dart
class Point {
  double x, y;

  Point(this.x, this.y);

  Point.origin()
      : x = 0,
        y = 0;
}
```

To use a named constructor, invoke it using its full name:

​	要使用命名构造函数，请使用其全名调用它：

```dart
final myPoint = Point.origin();
```

### 代码示例 Code example 

Give the `Color` class a constructor named `Color.black` that sets all three properties to zero.

​	为 `Color` 类提供一个名为 `Color.black` 的构造函数，该构造函数将所有三个属性都设置为零。

Ignore all initial errors in the DartPad.

​	忽略 DartPad 中的所有初始错误。

```dart
class Color {
  int red;
  int green;
  int blue;

  Color(this.red, this.green, this.blue);

  Color.black()
      : red = 0,
        green = 0,
        blue = 0;
}

// Tests your solution (Don't edit!):
void main() {
  final errs = <String>[];

  try {
    final result = Color.black();

    if (result.red != 0) {
      errs.add(
          'Called Color.black() and got a Color with red equal to \n ${result.red} instead of the expected value (0).');
    }

    if (result.green != 0) {
      errs.add(
          'Called Color.black() and got a Color with green equal to \n ${result.green} instead of the expected value (0).');
    }

    if (result.blue != 0) {
      errs.add(
          'Called Color.black() and got a Color with blue equal to \n ${result.blue} instead of the expected value (0).');
    }
  } catch (e) {
    print(
        'Called Color.black() and got an exception of type \n ${e.runtimeType}.');
    return;
  }

  if (errs.isEmpty) {
    print('Success!');
  } else {
    errs.forEach(print);
  }
}

```

## 工厂构造函数 Factory constructors 

Dart supports factory constructors, which can return subtypes or even null. To create a factory constructor, use the `factory` keyword:

​	Dart 支持工厂构造函数，它可以返回子类型甚至 null。要创建工厂构造函数，请使用 `factory` 关键字：

```dart
class Square extends Shape {}

class Circle extends Shape {}

class Shape {
  Shape();

  factory Shape.fromTypeName(String typeName) {
    if (typeName == 'square') return Square();
    if (typeName == 'circle') return Circle();

    throw ArgumentError('Unrecognized $typeName');
  }
}
```

### 代码示例 Code example 

Fill in the factory constructor named `IntegerHolder.fromList`, making it do the following:

​	填写名为 `IntegerHolder.fromList` 的工厂构造函数，使其执行以下操作：

- If the list has **one** value, create an `IntegerSingle` with that value.
- 如果列表有一个值，则创建一个具有该值 `IntegerSingle` 。
- If the list has **two** values, create an `IntegerDouble` with the values in order.
- 如果列表有两个值，则创建一个具有按顺序排列的值的 `IntegerDouble` 。
- If the list has **three** values, create an `IntegerTriple` with the values in order.
- 如果列表有三个值，则按顺序使用这些值创建一个 `IntegerTriple` 。
- Otherwise, throw an `Error`.
- 否则，抛出 `Error` 。

```dart
class IntegerHolder {
  IntegerHolder();

  factory IntegerHolder.fromList(List<int> list) {
    if (list.length == 1) {
      return IntegerSingle(list[0]);
    } else if (list.length == 2) {
      return IntegerDouble(list[0], list[1]);
    } else if (list.length == 3) {
      return IntegerTriple(list[0], list[1], list[2]);
    } else {
      throw Error();
    }
  }
}

class IntegerSingle extends IntegerHolder {
  final int a;
  IntegerSingle(this.a);
}

class IntegerDouble extends IntegerHolder {
  final int a;
  final int b;
  IntegerDouble(this.a, this.b);
}

class IntegerTriple extends IntegerHolder {
  final int a;
  final int b;
  final int c;
  IntegerTriple(this.a, this.b, this.c);
}

// Tests your solution (Don't edit!):
void main() {
  final errs = <String>[];

  bool _throwed = false;
  try {
    IntegerHolder.fromList([]);
  } on UnimplementedError {
    print('Test failed. Did you implement the method?');
    return;
  } on Error {
    _throwed = true;
  } catch (e) {
    print(
        'Called IntegerSingle.fromList([]) and got an exception of \n type ${e.runtimeType}.');
    return;
  }

  if (!_throwed) {
    errs.add('Called IntegerSingle.fromList([]) and didn\'t throw Error.');
  }

  try {
    final obj = IntegerHolder.fromList([1]);

    if (obj is! IntegerSingle) {
      errs.add(
          'Called IntegerHolder.fromList([1]) and got an object of type \n ${obj.runtimeType} instead of IntegerSingle.');
    } else {
      if (obj.a != 1) {
        errs.add(
            'Called IntegerHolder.fromList([1]) and got an IntegerSingle with \n  an \'a\' value of ${obj.a} instead of the expected (1).');
      }
    }
  } catch (e) {
    print(
        'Called IntegerHolder.fromList([]) and got an exception of \n type ${e.runtimeType}.');
    return;
  }

  try {
    final obj = IntegerHolder.fromList([1, 2]);

    if (obj is! IntegerDouble) {
      errs.add(
          'Called IntegerHolder.fromList([1, 2]) and got an object of type \n ${obj.runtimeType} instead of IntegerDouble.');
    } else {
      if (obj.a != 1) {
        errs.add(
            'Called IntegerHolder.fromList([1, 2]) and got an IntegerDouble \n with an \'a\' value of ${obj.a} instead of the expected (1).');
      }

      if (obj.b != 2) {
        errs.add(
            'Called IntegerHolder.fromList([1, 2]) and got an IntegerDouble \n with an \'b\' value of ${obj.b} instead of the expected (2).');
      }
    }
  } catch (e) {
    print(
        'Called IntegerHolder.fromList([1, 2]) and got an exception \n of type ${e.runtimeType}.');
    return;
  }

  try {
    final obj = IntegerHolder.fromList([1, 2, 3]);

    if (obj is! IntegerTriple) {
      errs.add(
          'Called IntegerHolder.fromList([1, 2, 3]) and got an object of type \n ${obj.runtimeType} instead of IntegerTriple.');
    } else {
      if (obj.a != 1) {
        errs.add(
            'Called IntegerHolder.fromList([1, 2, 3]) and got an IntegerTriple \n with an \'a\' value of ${obj.a} instead of the expected (1).');
      }

      if (obj.b != 2) {
        errs.add(
            'Called IntegerHolder.fromList([1, 2, 3]) and got an IntegerTriple \n with an \'a\' value of ${obj.b} instead of the expected (2).');
      }

      if (obj.c != 3) {
        errs.add(
            'Called IntegerHolder.fromList([1, 2, 3]) and got an IntegerTriple \n with an \'a\' value of ${obj.b} instead of the expected (2).');
      }
    }
  } catch (e) {
    print(
        'Called IntegerHolder.fromList([1, 2, 3]) and got an exception \n of type ${e.runtimeType}.');
    return;
  }

  if (errs.isEmpty) {
    print('Success!');
  } else {
    errs.forEach(print);
  }
}

```

## 重定向构造函数 Redirecting constructors 

Sometimes a constructor’s only purpose is to redirect to another constructor in the same class. A redirecting constructor’s body is empty, with the constructor call appearing after a colon (`:`).

​	有时，构造函数的唯一目的是重定向到同一类中的另一个构造函数。重定向构造函数的主体为空，构造函数调用出现在冒号之后 ( `:` )。

```dart
class Automobile {
  String make;
  String model;
  int mpg;

  // The main constructor for this class.
  Automobile(this.make, this.model, this.mpg);

  // Delegates to the main constructor.
  Automobile.hybrid(String make, String model) : this(make, model, 60);

  // Delegates to a named constructor
  Automobile.fancyHybrid() : this.hybrid('Futurecar', 'Mark 2');
}
```

### 代码示例 Code example 

Remember the `Color` class from above? Create a named constructor called `black`, but rather than manually assigning the properties, redirect it to the default constructor with zeros as the arguments.

​	还记得上面的 `Color` 类吗？创建一个名为 `black` 的命名构造函数，但不要手动分配属性，而是使用零作为参数将其重定向到默认构造函数。

Ignore all initial errors in the DartPad.

​	忽略 DartPad 中的所有初始错误。

```dart
class Color {
  int red;
  int green;
  int blue;

  Color(this.red, this.green, this.blue);

  Color.black() : this(0, 0, 0);
}

// Tests your solution (Don't edit!):
void main() {
  final errs = <String>[];

  try {
    final result = Color.black();

    if (result.red != 0) {
      errs.add(
          'Called Color.black() and got a Color with red equal to \n ${result.red} instead of the expected value (0).');
    }

    if (result.green != 0) {
      errs.add(
          'Called Color.black() and got a Color with green equal to \n ${result.green} instead of the expected value (0).');
    }

    if (result.blue != 0) {
      errs.add(
          'Called Color.black() and got a Color with blue equal to \n ${result.blue} instead of the expected value (0).');
    }
  } catch (e) {
    print(
        'Called Color.black() and got an exception of type ${e.runtimeType}.');
    return;
  }

  if (errs.isEmpty) {
    print('Success!');
  } else {
    errs.forEach(print);
  }
}

```

## 常量构造函数 Const constructors 

If your class produces objects that never change, you can make these objects compile-time constants. To do this, define a `const` constructor and make sure that all instance variables are final.

​	如果您的类生成永不更改的对象，则可以将这些对象设为编译时常量。为此，请定义一个 `const` 构造函数，并确保所有实例变量都是 final。

```dart
class ImmutablePoint {
  static const ImmutablePoint origin = ImmutablePoint(0, 0);

  final int x;
  final int y;

  const ImmutablePoint(this.x, this.y);
}
```

### 代码示例 Code example 

Modify the `Recipe` class so its instances can be constants, and create a constant constructor that does the following:

​	修改 `Recipe` 类，使其实例可以是常量，并创建一个执行以下操作的常量构造函数：

- Has three parameters: `ingredients`, `calories`, and `milligramsOfSodium` (in that order).
- 具有三个参数： `ingredients` 、 `calories` 和 `milligramsOfSodium` （按此顺序）。
- Uses `this.` syntax to automatically assign the parameter values to the object properties of the same name.
- 使用 `this.` 语法将参数值自动分配给同名对象属性。
- Is constant, with the `const` keyword just before `Recipe` in the constructor declaration.
- 是常量，在构造函数声明中 `Recipe` 前面带有 `const` 关键字。

Ignore all initial errors in the DartPad.

忽略 DartPad 中的所有初始错误。

```dart
class Recipe {
  final List<String> ingredients;
  final int calories;
  final double milligramsOfSodium;

  const Recipe(this.ingredients, this.calories, this.milligramsOfSodium);
}

// Tests your solution (Don't edit!):
void main() {
  final errs = <String>[];

  try {
    const obj = Recipe(['1 egg', 'Pat of butter', 'Pinch salt'], 120, 200);

    if (obj.ingredients.length != 3) {
      errs.add(
          'Called Recipe([\'1 egg\', \'Pat of butter\', \'Pinch salt\'], 120, 200) \n and got an object with ingredient list of length ${obj.ingredients.length} rather than the expected length (3).');
    }

    if (obj.calories != 120) {
      errs.add(
          'Called Recipe([\'1 egg\', \'Pat of butter\', \'Pinch salt\'], 120, 200) \n and got an object with a calorie value of ${obj.calories} rather than the expected value (120).');
    }

    if (obj.milligramsOfSodium != 200) {
      errs.add(
          'Called Recipe([\'1 egg\', \'Pat of butter\', \'Pinch salt\'], 120, 200) \n and got an object with a milligramsOfSodium value of ${obj.milligramsOfSodium} rather than the expected value (200).');
    }
  } catch (e) {
    print(
        'Tried calling Recipe([\'1 egg\', \'Pat of butter\', \'Pinch salt\'], 120, 200) \n and received a null.');
  }

  if (errs.isEmpty) {
    print('Success!');
  } else {
    errs.forEach(print);
  }
}

```



## What’s next? 

We hope you enjoyed using this codelab to learn or test your knowledge of some of the most interesting features of the Dart language. Here are some suggestions for what to do now:

​	我们希望您喜欢使用此代码实验室来学习或测试您对 Dart 语言一些最有趣特性的了解。以下是一些关于现在该做什么的建议：

- Try [other Dart codelabs](https://dart.dev/codelabs).
- 尝试其他 Dart 代码实验室。
- Read the [Dart language tour](https://dart.dev/language).
- 阅读 Dart 语言之旅。
- Play with [DartPad.](https://dartpad.dev/)
- 使用 DartPad 玩耍。
- [Get the Dart SDK](https://dart.dev/get-dart).
- 获取 Dart SDK。
