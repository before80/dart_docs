+++
title = "可迭代集合"
date = 2024-01-05T20:29:36+08:00
weight = 20
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文: [https://dart.dev/codelabs/iterables](https://dart.dev/codelabs/iterables)

## Iterable collections 可迭代集合

This codelab teaches you how to use collections that implement the [Iterable](https://api.dart.dev/stable/dart-core/Iterable-class.html) class—for example [List](https://api.dart.dev/stable/dart-core/List-class.html) and [Set.](https://api.dart.dev/stable/dart-core/Set-class.html) Iterables are basic building blocks for all sorts of Dart applications, and you’re probably already using them, even without noticing. This codelab helps you make the most out of them.

​	此代码实验室教您如何使用实现 Iterable 类的集合，例如 List 和 Set。Iterable 是各种 Dart 应用程序的基本构建块，您可能已经在不知不觉中使用它们。此代码实验室可帮助您充分利用它们。

Using the embedded DartPad editors, you can test your knowledge by running example code and completing exercises.

​	使用嵌入式 DartPad 编辑器，您可以通过运行示例代码和完成练习来测试您的知识。

To get the most out of this codelab, you should have basic knowledge of [Dart syntax](https://dart.dev/language).

​	为了充分利用此代码实验室，您应该具备 Dart 语法的基本知识。

This codelab covers the following material:

​	此代码实验室涵盖以下内容：

- How to read elements of an Iterable.
- 如何读取 Iterable 的元素。
- How to check if the elements of an Iterable satisfy a condition.
- 如何检查 Iterable 的元素是否满足某个条件。
- How to filter the contents of an Iterable.
- 如何过滤 Iterable 的内容。
- How to map the contents of an Iterable to a different value.
- 如何将 Iterable 的内容映射到不同的值。

Estimated time to complete this codelab: 60 minutes.

​	完成此代码实验室的估计时间：60 分钟。

*info* **Note:** This page uses embedded DartPads to display examples and exercises. If you see empty boxes instead of DartPads, go to the [DartPad troubleshooting page](https://dart.dev/tools/dartpad/troubleshoot).

​	注意：此页面使用嵌入式 DartPad 来显示示例和练习。如果您看到的是空框而不是 DartPad，请转到 DartPad 故障排除页面。

## 什么是集合？ What are collections? 

A collection is an object that represents a group of objects, which are called *elements*. Iterables are a kind of collection.
集合是一个表示一组对象的的对象，这些对象称为元素。Iterable 是一种集合。

A collection can be empty, or it can contain many elements. Depending on the purpose, collections can have different structures and implementations. These are some of the most common collection types:
集合可以为空，也可以包含许多元素。根据用途，集合可以具有不同的结构和实现。以下是一些最常见的集合类型：

- [List:](https://api.dart.dev/stable/dart-core/List-class.html) Used to read elements by their indexes.
  列表：用于按其索引读取元素。
- [Set:](https://api.dart.dev/stable/dart-core/Set-class.html) Used to contain elements that can occur only once.
  集合：用于包含只能出现一次的元素。
- [Map:](https://api.dart.dev/stable/dart-core/Map-class.html) Used to read elements using a key.
  Map：用于使用键读取元素。

## 什么是可迭代对象？What is an Iterable? 

An `Iterable` is a collection of elements that can be accessed sequentially.

​	一个 `Iterable` 是一个可以按顺序访问的元素集合。

In Dart, an `Iterable` is an abstract class, meaning that you can’t instantiate it directly. However, you can create a new `Iterable` by creating a new `List` or `Set`.

​	在 Dart 中，一个 `Iterable` 是一个抽象类，这意味着您无法直接实例化它。但是，您可以通过创建一个新的 `Iterable` 或 `List` 来创建一个新的 `Iterable`。

Both `List` and `Set` are `Iterable`, so they have the same methods and properties as the `Iterable` class.

​	`List` 和 `Set` 都是 `Iterable` ，因此它们具有与 `Iterable` 类相同的方法和属性。

A `Map` uses a different data structure internally, depending on its implementation. For example, [HashMap](https://api.dart.dev/stable/dart-collection/HashMap-class.html) uses a hash table in which the elements (also called *values*) are obtained using a key. Elements of a `Map` can also be read as `Iterable` objects by using the map’s `entries` or `values` property.

​	一个 `Map` 在内部使用不同的数据结构，具体取决于它的实现。例如，HashMap 使用哈希表，其中元素（也称为值）是使用键获取的。 `Map` 的元素也可以使用映射的 `entries` 或 `values` 属性作为 `Iterable` 对象读取。

This example shows a `List` of `int`, which is also an `Iterable` of `int`:

​	此示例显示了一个 `int` 的 `List` ，它也是 `int` 的 `Iterable` ：

```dart
Iterable<int> iterable = [1, 2, 3];
```

The difference with a `List` is that with the `Iterable`, you can’t guarantee that reading elements by index will be efficient. `Iterable`, as opposed to `List`, doesn’t have the `[]` operator.

​	与 `List` 的区别在于，使用 `Iterable` 时，您无法保证按索引读取元素会很有效率。 `Iterable` 与 `List` 相反，没有 `[]` 运算符。

For example, consider the following code, which is **invalid**:

​	例如，考虑以下无效代码：

```dart
Iterable<int> iterable = [1, 2, 3];
int value = iterable[1];
```

If you read elements with `[]`, the compiler tells you that the operator `'[]'` isn’t defined for the class `Iterable`, which means that you can’t use `[index]` in this case.

​	如果您使用 `[]` 读取元素，编译器会告诉您运算符 `'[]'` 未针对类 `Iterable` 定义，这意味着您在此情况下无法使用 `[index]` 。

You can instead read elements with `elementAt()`, which steps through the elements of the iterable until it reaches that position.

​	您可以改用 `elementAt()` 读取元素，它会逐步处理可迭代对象的元素，直到到达该位置。

```dart
Iterable<int> iterable = [1, 2, 3];
int value = iterable.elementAt(1);
```

Continue to the next section to learn more about how to access elements of an `Iterable`.

​	继续转到下一部分，以详细了解如何访问 `Iterable` 的元素。

## 读取元素 Reading elements 

You can read the elements of an iterable sequentially, using a `for-in` loop.

​	您可以使用 `for-in` 循环按顺序读取可迭代对象的元素。

### 示例：使用 for-in 循环 Example: Using a for-in loop 

The following example shows you how to read elements using a `for-in` loop.

​	以下示例演示如何使用 `for-in` 循环读取元素。

```dart
void main() {
  const iterable = ['Salad', 'Popcorn', 'Toast'];
  for (final element in iterable) {
    print(element);
  }
}
```

<iframe src="https://dartpad.dev/embed-dart.html?theme=light&amp;run=dartpad&amp;split=false&amp;ga_id=for_in_loop" style="box-sizing: border-box; border: 1px solid rgb(204, 204, 204); margin-bottom: 1rem; min-height: 400px; resize: vertical; width: 896px;"></iframe>

*info* Behind the scenes, the `for-in` loop uses an *iterator.* You rarely see the [Iterator API](https://api.dart.dev/stable/dart-core/Iterator-class.html) used directly, however, because `for-in` is easier to read and understand, and is less prone to errors.

​	在后台， `for-in` 循环使用迭代器。但是，您很少会看到直接使用 Iterator API，因为 `for-in` 更易于阅读和理解，并且不易出错。

**Key terms:
关键词：**

- **Iterable**: The Dart [Iterable](https://api.dart.dev/stable/dart-core/Iterable-class.html) class.
- Iterable：Dart Iterable 类。
- **Iterator**: An object used by `for-in` to read elements from an `Iterable` object.
- 迭代器： `for-in` 用于从 `Iterable` 对象读取元素的对象。
- **`for-in` loop**: An easy way to sequentially read elements from an `Iterable`.
- `for-in` 循环：一种从 `Iterable` 中顺序读取元素的简单方法。

### 示例：使用 first 和 last - Example: Using first and last 

In some cases, you want to access only the first or the last element of an `Iterable`.

​	在某些情况下，您只想访问 `Iterable` 的第一个或最后一个元素。

With the `Iterable` class, you can’t access the elements directly, so you can’t call `iterable[0]` to access the first element. Instead, you can use `first`, which gets the first element.

​	使用 `Iterable` 类，您无法直接访问元素，因此无法调用 `iterable[0]` 来访问第一个元素。相反，您可以使用 `first` 来获取第一个元素。

Also, with the `Iterable` class, you can’t use the operator `[]` to access the last element, but you can use the `last` property.

​	此外，使用 `Iterable` 类，您无法使用运算符 `[]` 来访问最后一个元素，但可以使用 `last` 属性。

*report_problem* Because accessing the last element of an `Iterable` requires stepping through all the other elements, **`last` can be slow.** Using `first` or `last` on an **empty `Iterable`** results in a [StateError.](https://api.dart.dev/stable/dart-core/StateError-class.html)

​	由于访问 `Iterable` 的最后一个元素需要遍历所有其他元素，因此 `last` 可能会很慢。对空 `Iterable` 使用 `first` 或 `last` 会导致 StateError。

```dart
void main() {
  Iterable<String> iterable = const ['Salad', 'Popcorn', 'Toast'];
  print('The first element is ${iterable.first}');
  print('The last element is ${iterable.last}');
}
```

<iframe src="https://dartpad.dev/embed-dart.html?theme=light&amp;run=dartpad&amp;split=false&amp;ga_id=first_and_last" style="box-sizing: border-box; border: 1px solid rgb(204, 204, 204); margin-bottom: 1rem; min-height: 400px; resize: vertical; width: 896px;"></iframe>

In this example you saw how to use `first` and `last` to get the first and last elements of an `Iterable`. It’s also possible to find the first element that satisfies a condition. The next section shows how to do that using a method called `firstWhere()`.

​	在这个示例中，您看到了如何使用 `first` 和 `last` 来获取 `Iterable` 的第一个和最后一个元素。还可以找到满足条件的第一个元素。下一节将演示如何使用名为 `firstWhere()` 的方法来执行此操作。

### Example: Using firstWhere() 示例：使用 firstWhere()

You already saw that you can access the elements of an `Iterable` sequentially, and you can easily get the first or last element.

​	您已经看到您可以按顺序访问 `Iterable` 的元素，并且您可以轻松地获取第一个或最后一个元素。

Now, you learn how to use `firstWhere()` to find the first element that satisfies certain conditions. This method requires you to pass a *predicate*, which is a function that returns true if the input satisfies a certain condition.

​	现在，您将学习如何使用 `firstWhere()` 来查找满足特定条件的第一个元素。此方法要求您传递一个谓词，谓词是一个函数，如果输入满足某个条件，则返回 true。

```dart
String element = iterable.firstWhere((element) => element.length > 5);
```

For example, if you want to find the first `String` that has more than 5 characters, you must pass a predicate that returns true when the element size is greater than 5.

​	例如，如果您想查找第一个具有 5 个以上字符的 `String` ，则必须传递一个谓词，当元素大小大于 5 时返回 true。

Run the following example to see how `firstWhere()` works. Do you think all the functions will give the same result?

​	运行以下示例以查看 `firstWhere()` 的工作原理。您认为所有函数都会给出相同的结果吗？

```dart
bool predicate(String item) {
  return item.length > 5;
}

void main() {
  const items = ['Salad', 'Popcorn', 'Toast', 'Lasagne'];

  // You can find with a simple expression:
  var foundItem1 = items.firstWhere((item) => item.length > 5);
  print(foundItem1);

  // Or try using a function block:
  var foundItem2 = items.firstWhere((item) {
    return item.length > 5;
  });
  print(foundItem2);

  // Or even pass in a function reference:
  var foundItem3 = items.firstWhere(predicate);
  print(foundItem3);

  // You can also use an `orElse` function in case no value is found!
  var foundItem4 = items.firstWhere(
    (item) => item.length > 10,
    orElse: () => 'None!',
  );
  print(foundItem4);
}
```

<iframe src="https://dartpad.dev/embed-dart.html?theme=light&amp;run=dartpad&amp;split=false&amp;ga_id=using_firstwhere" style="box-sizing: border-box; border: 1px solid rgb(204, 204, 204); margin-bottom: 1rem; min-height: 400px; resize: vertical; width: 896px; height: 565px;"></iframe>

In this example, you can see three different ways to write a predicate:

​	在此示例中，您可以看到编写谓词的三种不同方法：

- **As an expression:** The test code has one line that uses arrow syntax (`=>`).
- 作为表达式：测试代码有一行使用箭头语法 ( `=>` )。
- **As a block:** The test code has multiple lines between brackets and a return statement.
- 作为块：测试代码在括号和 return 语句之间有多行。
- **As a function:** The test code is in an external function that’s passed to the `firstWhere()` method as a parameter.
- 作为函数：测试代码位于外部函数中，该函数作为参数传递给 `firstWhere()` 方法。

There is no right or wrong way. Use the way that works best for you, and that makes your code easier to read and understand.

​	没有正确或错误的方法。使用最适合您的方法，使您的代码更易于阅读和理解。

The final example calls `firstWhere()` with the optional named parameter `orElse`, which provides an alternative when an element isn’t found. In this case, the text `'None!'` is returned because no element satisfies the provided condition.

​	最后一个示例使用可选命名参数 `orElse` 调用 `firstWhere()` ，当找不到元素时，该参数提供一个备选方案。在这种情况下，返回文本 `'None!'` ，因为没有元素满足提供的条件。

*info* **Note:** If no element satisfies the test predicate and the `orElse` parameter isn’t provided, then `firstWhere()` throws a [StateError.](https://api.dart.dev/stable/dart-core/StateError-class.html)

​	注意：如果没有元素满足测试谓词并且未提供 `orElse` 参数，则 `firstWhere()` 会引发 StateError。

**Quick review:
快速回顾：**

- The elements of an `Iterable` must be accessed sequentially.
- `Iterable` 的元素必须按顺序访问。
- The easiest way to iterate through all the elements is using a `for-in` loop.
- 遍历所有元素的最简单方法是使用 `for-in` 循环。
- You can use the `first` and `last` getters to get the first and last elements.
- 您可以使用 `first` 和 `last` 获取器获取第一个和最后一个元素。
- You can also find the first element that satisfies a condition with `firstWhere()`.
- 您还可以使用 `firstWhere()` 找到满足条件的第一个元素。
- You can write test predicates as expressions, blocks, or functions.
- 您可以将测试谓词写成表达式、块或函数。

**Key terms:
关键术语：**

- **Predicate:** A function that returns `true` when a certain condition is satisfied.
- 谓词：当满足某个条件时返回 `true` 的函数。

### 练习：练习编写测试谓词 Exercise: Practice writing a test predicate 

The following exercise is a failing unit test that contains a partially complete code snippet. Your task is to complete the exercise by writing code to make the tests pass. You don’t need to implement `main()`.

​	以下练习是一个失败的单元测试，其中包含一个部分完成的代码片段。您的任务是通过编写代码使测试通过来完成练习。您不需要实现 `main()` 。

This exercise introduces `singleWhere()` This method works similarly to `firstWhere()`, but in this case it expects only one element of the `Iterable` to satisfy the predicate. If more than one or no element in the `Iterable` satisfies the predicate condition, then the method throws a [StateError](https://api.dart.dev/stable/dart-core/StateError-class.html) exception.

​	此练习介绍 `singleWhere()` 此方法的工作方式类似于 `firstWhere()` ，但在此情况下，它仅期望 `Iterable` 的一个元素满足谓词。如果 `Iterable` 中有多个或没有元素满足谓词条件，则该方法会引发 StateError 异常。

*report_problem* `singleWhere()` steps through the whole `Iterable` until the last element, which can cause problems if the `Iterable` is infinite or contains a large collection of elements.

​	`singleWhere()` 遍历整个 `Iterable` 直到最后一个元素，如果 `Iterable` 无限或包含大量元素，这可能会导致问题。

Your goal is to implement the predicate for `singleWhere()` that satisfies the following conditions:
您的目标是为 `singleWhere()` 实现满足以下条件的谓词：

- The element contains the character `'a'`.
- 元素包含字符 `'a'` 。
- The element starts with the character `'M'`.
- 元素以字符 `'M'` 开头。

All the elements in the test data are [strings](https://api.dart.dev/stable/dart-core/String-class.html); you can check the class documentation for help.

​	测试数据中的所有元素都是字符串；您可以查看类文档以获取帮助。


```dart
// Implement the predicate of singleWhere
// with the following conditions
// * The element contains the character `'a'`
// * The element starts with the character `'M'`
String singleWhere(Iterable<String> items) {
  return items.singleWhere(TODO('Implement predicate'));
}

```

<iframe src="https://dartpad.dev/embed-dart.html?theme=dark&amp;run=dartpad&amp;split=false&amp;ga_id=practice_writing_a_test_predicate" style="box-sizing: border-box; border: 1px solid rgb(204, 204, 204); margin-bottom: 1rem; min-height: 400px; resize: vertical; width: 896px;"></iframe>

## 检查条件 Checking conditions 

When working with `Iterable`, sometimes you need to verify that all the elements of a collection satisfy some condition.

​	在使用 `Iterable` 时，有时您需要验证集合的所有元素是否满足某个条件。

You might be tempted to write a solution using a `for-in` loop like this one:

​	您可能会尝试编写一个使用 `for-in` 循环的解决方案，如下所示：

```dart
for (final item in items) {
  if (item.length < 5) {
    return false;
  }
}
return true;
```

However, you can accomplish the same using the `every()` method:

​	但是，您可以使用 `every()` 方法实现相同的目的：

```dart
return items.every((item) => item.length >= 5);
```

Using the `every()` method results in code that is more readable, compact, and less error-prone.

​	使用 `every()` 方法会生成更具可读性、更紧凑且更不易出错的代码。

### 示例：使用 any() 和 every() - Example: Using any() and every() 

The `Iterable` class provides two methods that you can use to verify conditions:

​	`Iterable` 类提供了两种可用于验证条件的方法：

- `any()`: Returns true if at least one element satisfies the condition.
- `any()` ：如果至少一个元素满足条件，则返回 true。
- `every()`: Returns true if all elements satisfy the condition.
- `every()` ：如果所有元素都满足条件，则返回 true。

Run this exercise to see them in action.

​	运行此练习以查看它们在实际中的效果。

```dart
void main() {
  const items = ['Salad', 'Popcorn', 'Toast'];

  if (items.any((item) => item.contains('a'))) {
    print('At least one item contains "a"');
  }

  if (items.every((item) => item.length >= 5)) {
    print('All items have length >= 5');
  }
}
```

<iframe src="https://dartpad.dev/embed-dart.html?theme=light&amp;run=dartpad&amp;split=false&amp;ga_id=using_any_and_every" style="box-sizing: border-box; border: 1px solid rgb(204, 204, 204); margin-bottom: 1rem; min-height: 400px; resize: vertical; width: 896px; height: 255px;"></iframe>

In the example, `any()` verifies that at least one element contains the character `a`, and `every()` verifies that all elements have a length equal to or greater than 5.

​	在示例中， `any()` 验证至少一个元素包含字符 `a` ，而 `every()` 验证所有元素的长度都等于或大于 5。

After running the code, try changing the predicate of `any()` so it returns false:

​	运行代码后，尝试更改 `any()` 的谓词，使其返回 false：

```dart
if (items.any((item) => item.contains('Z'))) {
  print('At least one item contains "Z"');
} else {
  print('No item contains "Z"');
}
```

You can also use `any()` to verify that no element of an `Iterable` satisfies a certain condition.

​	您还可以使用 `any()` 验证 `Iterable` 的任何元素是否满足某个条件。

### 练习：验证 Iterable 是否满足条件 Exercise: Verify that an Iterable satisfies a condition 

The following exercise provides practice using the `any()` and `every()` methods, described in the previous example. In this case, you work with a group of users, represented by `User` objects that have the member field `age`.

​	以下练习提供了使用 `any()` 和 `every()` 方法的实践，如上一个示例中所述。在此情况下，您使用一组用户，由具有成员字段 `age` 的 `User` 对象表示。

Use `any()` and `every()` to implement two functions:

​	使用 `any()` 和 `every()` 来实现两个函数：

- Part 1: Implement `anyUserUnder18()`.

- 
  第 1 部分：实现 `anyUserUnder18()` 。
  
  - Return `true` if at least one user is 17 or younger.
  - 如果至少有一个用户年龄在 17 岁或以下，则返回 `true` 。
  
- Part 2: Implement `everyUserOver13()`.

- 
  第 2 部分：实现 `everyUserOver13()` 。
  
  - Return `true` if all users are 14 or older.
  - 如果所有用户年龄在 14 岁或以上，则返回 `true` 。


```dart
bool anyUserUnder18(Iterable<User> users) {
  TODO('Implement this method');
}

bool everyUserOver13(Iterable<User> users) {
  TODO('Implement this method');
}

class User {
  String name;
  int age;

  User(
    this.name,
    this.age,
  );
}

```

<iframe src="https://dartpad.dev/embed-dart.html?theme=dark&amp;run=dartpad&amp;split=false&amp;ga_id=verify_iterable" style="box-sizing: border-box; border: 1px solid rgb(204, 204, 204); margin-bottom: 1rem; min-height: 400px; resize: vertical; width: 896px; height: 395px;"></iframe>

**Quick review:
快速回顾：**

- Although you can use `for-in` loops to check conditions, there are better ways to do that.
- 虽然您可以使用 `for-in` 循环来检查条件，但还有更好的方法可以做到这一点。
- The method `any()` enables you to check whether any element satisfies a condition.
- 方法 `any()` 使您能够检查是否有任何元素满足某个条件。
- The method `every()` enables you to verify that all elements satisfy a condition.
- 方法 `every()` 使您能够验证所有元素是否满足某个条件。

## Filtering 过滤

The previous sections cover methods like `firstWhere()` or `singleWhere()` that can help you find an element that satisfies a certain predicate.

​	前面的部分介绍了诸如 `firstWhere()` 或 `singleWhere()` 之类的方法，这些方法可以帮助您找到满足某个谓词的元素。

But what if you want to find all the elements that satisfy a certain condition? You can accomplish that using the `where()` method.

​	但是，如果您想找到满足某个条件的所有元素，该怎么办？您可以使用 `where()` 方法来实现。

```dart
var evenNumbers = numbers.where((number) => number.isEven);
```

In this example, `numbers` contains an `Iterable` with multiple `int` values, and `where()` finds all the numbers that are even.

​	在此示例中， `numbers` 包含一个具有多个 `int` 值的 `Iterable` ，而 `where()` 找到所有偶数。

The output of `where()` is another `Iterable`, and you can use it as such to iterate over it or apply other `Iterable` methods. In the next example, the output of `where()` is used directly inside the `for-in` loop.

​	`where()` 的输出是另一个 `Iterable` ，您可以使用它来对其进行迭代或应用其他 `Iterable` 方法。在下一个示例中， `where()` 的输出直接在 `for-in` 循环中使用。

```dart
var evenNumbers = numbers.where((number) => number.isEven);
for (final number in evenNumbers) {
  print('$number is even');
}
```

### 示例：使用 where() - Example: Using where() 

Run this example to see how `where()` can be used together with other methods like `any()`.

​	运行此示例以了解如何将 `where()` 与其他方法（如 `any()` ）结合使用。

```dart
void main() {
  var evenNumbers = const [1, -2, 3, 42].where((number) => number.isEven);

  for (final number in evenNumbers) {
    print('$number is even.');
  }

  if (evenNumbers.any((number) => number.isNegative)) {
    print('evenNumbers contains negative numbers.');
  }

  // If no element satisfies the predicate, the output is empty.
  var largeNumbers = evenNumbers.where((number) => number > 1000);
  if (largeNumbers.isEmpty) {
    print('largeNumbers is empty!');
  }
}
```

<iframe src="https://dartpad.dev/embed-dart.html?theme=light&amp;run=dartpad&amp;split=false&amp;ga_id=using_where" style="box-sizing: border-box; border: 1px solid rgb(204, 204, 204); margin-bottom: 1rem; min-height: 400px; resize: vertical; width: 896px; height: 380px;"></iframe>

In this example, `where()` is used to find all numbers that are even, then `any()` is used to check if the results contain a negative number.

​	在此示例中， `where()` 用于查找所有偶数，然后使用 `any()` 检查结果是否包含负数。

Later in the example, `where()` is used again to find all numbers larger than 1000. Because there are none, the result is an empty `Iterable`.

​	稍后在示例中， `where()` 再次用于查找所有大于 1000 的数字。由于没有这样的数字，因此结果是一个空 `Iterable` 。

*info* **Note:** If no element satisfies the predicate in `where()`, then the method returns an empty `Iterable`. Unlike `singleWhere()` or `firstWhere()`, `where()` doesn’t throw a [StateError](https://api.dart.dev/stable/dart-core/StateError-class.html) exception.

注意：如果 `where()` 中的谓词不满足任何元素，则该方法会返回一个空 `Iterable` 。与 `singleWhere()` 或 `firstWhere()` 不同， `where()` 不会抛出 StateError 异常。

### 示例：使用 takeWhile - Example: Using takeWhile 

The methods `takeWhile()` and `skipWhile()` can also help you filter elements from an `Iterable`.

​	方法 `takeWhile()` 和 `skipWhile()` 还可以帮助您从 `Iterable` 中过滤元素。

Run this example to see how `takeWhile()` and `skipWhile()` can split an `Iterable` containing numbers.

​	运行此示例以了解 `takeWhile()` 和 `skipWhile()` 如何拆分包含数字的 `Iterable` 。

```dart
void main() {
  const numbers = [1, 3, -2, 0, 4, 5];

  var numbersUntilZero = numbers.takeWhile((number) => number != 0);
  print('Numbers until 0: $numbersUntilZero');

  var numbersStartingAtZero = numbers.skipWhile((number) => number != 0);
  print('Numbers starting at 0: $numbersStartingAtZero');
}
```

<iframe src="https://dartpad.dev/embed-dart.html?theme=light&amp;run=dartpad&amp;split=false&amp;ga_id=using_takewhile" style="box-sizing: border-box; border: 1px solid rgb(204, 204, 204); margin-bottom: 1rem; min-height: 400px; resize: vertical; width: 896px;"></iframe>

In this example, `takeWhile()` returns an `Iterable` that contains all the elements before the one that satisfies the predicate. On the other hand, `skipWhile()` returns an `Iterable` that contains all elements after and including the first one that *doesn’t* satisfy the predicate.

​	在此示例中， `takeWhile()` 返回一个 `Iterable` ，其中包含满足谓词的元素之前的所有元素。另一方面， `skipWhile()` 返回一个 `Iterable` ，其中包含第一个不满足谓词的元素及之后的元素。

After running the example, change `takeWhile()` to take elements until it reaches the first negative number.

​	运行示例后，将 `takeWhile()` 更改为获取元素，直到达到第一个负数。

```dart
var numbersUntilNegative =
    numbers.takeWhile((number) => !number.isNegative);
```

Notice that the condition `number.isNegative` is negated with `!`.

​	请注意，条件 `number.isNegative` 使用 `!` 取反。

### 练习：从列表中过滤元素 Exercise: Filtering elements from a list 

The following exercise provides practice using the `where()` method with the class `User` from the previous exercise.

​	以下练习提供了使用 `where()` 方法与前一个练习中的类 `User` 的实践。

Use `where()` to implement two functions:

​	使用 `where()` 实现两个函数：

- Part 1: Implement `filterOutUnder21()`.

- 
  第一部分：实现 `filterOutUnder21()` 。
  
  - Return an `Iterable` containing all users of age 21 or more.
  - 返回一个包含所有年龄为 21 岁或以上的用户的 `Iterable` 。
  
- Part 2: Implement `findShortNamed()`.

- 
  第二部分：实现 `findShortNamed()` 。
  
  - Return an `Iterable` containing all users with names of length 3 or less.
  - 返回一个包含所有姓名长度为 3 或更少的用户的 `Iterable` 。

```dart
Iterable<User> filterOutUnder21(Iterable<User> users) {
  TODO('Implement this method');
}

Iterable<User> findShortNamed(Iterable<User> users) {
  TODO('Implement this method');
}

class User {
  String name;
  int age;

  User(
    this.name,
    this.age,
  );
}

```

<iframe src="https://dartpad.dev/embed-dart.html?theme=dark&amp;run=dartpad&amp;split=false&amp;ga_id=filtering_elements_from_a_list" style="box-sizing: border-box; border: 1px solid rgb(204, 204, 204); margin-bottom: 1rem; min-height: 400px; resize: vertical; width: 896px; height: 380px;"></iframe>

**Quick review:
快速回顾：**

- Filter the elements of an `Iterable` with `where()`.
- 使用 `where()` 过滤 `Iterable` 的元素。
- The output of `where()` is another `Iterable`.
- `where()` 的输出是另一个 `Iterable` 。
- Use `takeWhile()` and `skipWhile()` to obtain elements until or after a condition is met.
- 使用 `takeWhile()` 和 `skipWhile()` 来获取元素，直到或在满足条件后。
- The output of these methods can be an empty `Iterable`.
- 这些方法的输出可以是空的 `Iterable` 。

## Mapping 映射

Mapping `Iterables` with the method `map()` enables you to apply a function over each of the elements, replacing each element with a new one.

​	使用 `map()` 方法映射 `Iterables` 使您能够对每个元素应用一个函数，用一个新元素替换每个元素。

```dart
Iterable<int> output = numbers.map((number) => number * 10);
```

In this example, each element of the `Iterable` numbers is multiplied by 10.

​	在此示例中， `Iterable` numbers 的每个元素都乘以 10。

You can also use `map()` to transform an element into a different object—for example, to convert all `int` to `String`, as you can see in the following example:

​	您还可以使用 `map()` 将元素转换为不同的对象，例如，将所有 `int` 转换为 `String` ，如您在以下示例中看到的那样：

```dart
Iterable<String> output = numbers.map((number) => number.toString());
```

*info* **Note:** `map()` returns a *lazy* `Iterable`, meaning that the supplied function is called only when the elements are iterated.

​	注意： `map()` 返回一个惰性 `Iterable` ，这意味着仅在迭代元素时才调用提供的函数。

### 示例：使用 map 更改元素 Example: Using map to change elements 

Run this example to see how to use `map()` to multiply all the elements of an `Iterable` by 2. What do you think the output will be?

​	运行此示例以了解如何使用 `map()` 将 `Iterable` 的所有元素乘以 2。您认为输出会是什么？

```dart
void main() {
  var numbersByTwo = const [1, -2, 3, 42].map((number) => number * 2);
  print('Numbers: $numbersByTwo');
}
```

<iframe src="https://dartpad.dev/embed-dart.html?theme=light&amp;run=dartpad&amp;split=false&amp;ga_id=using_map" style="box-sizing: border-box; border: 1px solid rgb(204, 204, 204); margin-bottom: 1rem; min-height: 400px; resize: vertical; width: 896px;"></iframe>

### 练习：映射到不同类型 Exercise: Mapping to a different type 

In the previous example, you multiplied the elements of an `Iterable` by 2. Both the input and the output of that operation were an `Iterable` of `int`.

​	在前面的示例中，您将 `Iterable` 的元素乘以 2。该操作的输入和输出都是 `Iterable` 的 `int` 。

In this exercise, your code takes an `Iterable` of `User`, and you need to return an `Iterable` that contains strings containing each user’s name and age.

​	在此练习中，您的代码采用 `User` 的 `Iterable` ，您需要返回一个 `Iterable` ，其中包含包含每个用户姓名和年龄的字符串。

Each string in the `Iterable` must follow this format: `'{name} is {age}'`—for example `'Alice is 21'`.

​	`Iterable` 中的每个字符串都必须遵循此格式： `'{name} is {age}'` — 例如 `'Alice is 21'` 。

```dart
Iterable<String> getNameAndAges(Iterable<User> users) {
  TODO('Implement this method');
}

class User {
  String name;
  int age;

  User(
    this.name,
    this.age,
  );
}

```

<iframe src="https://dartpad.dev/embed-dart.html?theme=dark&amp;run=dartpad&amp;split=false&amp;ga_id=mapping_to_a_different_type" style="box-sizing: border-box; border: 1px solid rgb(204, 204, 204); margin-bottom: 1rem; min-height: 400px; resize: vertical; width: 896px; height: 310px;"></iframe>

**Quick review:
快速回顾：**

- `map()` applies a function to all the elements of an `Iterable`.
- `map()` 将函数应用于 `Iterable` 的所有元素。
- The output of `map()` is another `Iterable`.
- `map()` 的输出是另一个 `Iterable` 。
- The function isn’t evaluated until the `Iterable` is iterated.
- 在迭代 `Iterable` 之前不会计算该函数。

## 练习：将所有内容放在一起 Exercise: Putting it all together 

It’s time to practice what you learned, in one final exercise.

​	现在是时候在最后一个练习中练习您所学的内容了。

This exercise provides the class `EmailAddress`, which has a constructor that takes a string. Another provided function is `isValidEmailAddress()`, which tests whether an email address is valid.

​	此练习提供了类 `EmailAddress` ，它具有一个采用字符串的构造函数。另一个提供的函数是 `isValidEmailAddress()` ，它测试电子邮件地址是否有效。

| Constructor/function 构造函数/函数 | Type signature 类型签名                  | Description 说明                                             |
| ---------------------------------- | ---------------------------------------- | ------------------------------------------------------------ |
| EmailAddress()                     | `EmailAddress(String address)`           | Creates an `EmailAddress` for the specified address. 为指定地址创建一个 `EmailAddress` 。 |
| isValidEmailAddress()              | `bool isValidEmailAddress(EmailAddress)` | Returns `true` if the provided `EmailAddress` is valid. 如果提供的 `EmailAddress` 有效，则返回 `true` 。 |

Write the following code:

​	编写以下代码：

Part 1: Implement `parseEmailAddresses()`.

​	第 1 部分：实现 `parseEmailAddresses()` 。

- Write the function `parseEmailAddresses()`, which takes an `Iterable<String>` containing email addresses, and returns an `Iterable<EmailAddress>`.
- 编写函数 `parseEmailAddresses()` ，该函数采用包含电子邮件地址的 `Iterable<String>` ，并返回 `Iterable<EmailAddress>` 。
- Use the method `map()` to map from a `String` to `EmailAddress`.
- 使用 `map()` 方法从 `String` 映射到 `EmailAddress` 。
- Create the `EmailAddress` objects using the constructor `EmailAddress(String)`.
- 使用构造函数 `EmailAddress(String)` 创建 `EmailAddress` 对象。

Part 2: Implement `anyInvalidEmailAddress()`.

​	第 2 部分：实现 `anyInvalidEmailAddress()` 。

- Write the function `anyInvalidEmailAddress()`, which takes an `Iterable<EmailAddress>` and returns `true` if any `EmailAddress` in the `Iterable` isn’t valid.
- 编写函数 `anyInvalidEmailAddress()` ，该函数接受一个 `Iterable<EmailAddress>` 并返回 `true` ，如果 `Iterable` 中的任何 `EmailAddress` 无效。
- Use the method `any()` together with the provided function `isValidEmailAddress()`.
- 将方法 `any()` 与提供的函数 `isValidEmailAddress()` 一起使用。

Part 3: Implement `validEmailAddresses()`.

​	第 3 部分：实现 `validEmailAddresses()` 。

- Write the function `validEmailAddresses()`, which takes an `Iterable<EmailAddress>` and returns another `Iterable<EmailAddress>` containing only valid addresses.
- 编写函数 `validEmailAddresses()` ，该函数接受一个 `Iterable<EmailAddress>` 并返回另一个仅包含有效地址的 `Iterable<EmailAddress>` 。
- Use the method `where()` to filter the `Iterable<EmailAddress>`.
- 使用 `where()` 方法过滤 `Iterable<EmailAddress>` 。
- Use the provided function `isValidEmailAddress()` to evaluate whether an `EmailAddress` is valid.
- 使用提供的函数 `isValidEmailAddress()` 来评估 `EmailAddress` 是否有效。

```dart
Iterable<EmailAddress> parseEmailAddresses(Iterable<String> strings) {
  TODO('Implement this method');
}

bool anyInvalidEmailAddress(Iterable<EmailAddress> emails) {
  TODO('Implement this method');
}

Iterable<EmailAddress> validEmailAddresses(Iterable<EmailAddress> emails) {
  TODO('Implement this method');
}

class EmailAddress {
  final String address;

  EmailAddress(this.address);

  @override
  bool operator ==(Object other) =>
      identical(this, other) ||
          other is EmailAddress &&
              address == other.address;

  @override
  int get hashCode => address.hashCode;

  @override
  String toString() {
    return 'EmailAddress{address: $address}';
  }
}

```

<iframe src="https://dartpad.dev/embed-dart.html?theme=dark&amp;run=dartpad&amp;split=false&amp;ga_id=putting_it_all_together" style="box-sizing: border-box; border: 1px solid rgb(204, 204, 204); margin-bottom: 1rem; min-height: 400px; resize: vertical; width: 896px; height: 600px;"></iframe>

## What’s next

Congratulations, you finished the codelab! If you want to learn more, here are some suggestions for where to go next:

​	恭喜，您已完成代码实验室！如果您想了解更多信息，这里有一些关于下一步去向的建议：

- Play with [DartPad.](https://dartpad.dev/)
- 使用 DartPad 玩耍。
- Try another [codelab](https://dart.dev/codelabs).
- 尝试另一个代码实验室。
- Read the [Iterable API reference](https://api.dart.dev/stable/dart-core/Iterable-class.html) to learn about methods not covered by this codelab.
- 阅读 Iterable API 参考以了解本代码实验室未涵盖的方法。
