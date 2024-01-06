+++
title = "Collections"
date = 2024-01-05T20:29:36+08:00
weight = 20
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文: [https://dart.dev/language/collections](https://dart.dev/language/collections)

# Collections 集合

Dart has built-in support for list, set, and map [collections](https://dart.dev/libraries/dart-core#collections). To learn more about configuring the types collections contain, check out [Generics](https://dart.dev/language/generics).
Dart 内置对列表、集合和映射集合的支持。要详细了解如何配置类型集合包含的内容，请查看泛型。

## Lists 列表

Perhaps the most common collection in nearly every programming language is the *array*, or ordered group of objects. In Dart, arrays are [`List`](https://api.dart.dev/stable/dart-core/List-class.html) objects, so most people just call them *lists*.
几乎每种编程语言中最常见的集合可能是数组，或有序的对象组。在 Dart 中，数组是 `List` 对象，因此大多数人只称它们为列表。

Dart list literals are denoted by a comma separated list of expressions or values, enclosed in square brackets (`[]`). Here’s a simple Dart list:
Dart 列表字面量由用逗号分隔的表达式或值列表表示，并用方括号 ( `[]` ) 括起来。这是一个简单的 Dart 列表：

```dart
var list = [1, 2, 3];
```

*info* **Note:** Dart infers that `list` has type `List<int>`. If you try to add non-integer objects to this list, the analyzer or runtime raises an error. For more information, read about [type inference](https://dart.dev/language/type-system#type-inference).
注意：Dart 推断 `list` 的类型为 `List<int>` 。如果您尝试向此列表添加非整数对象，分析器或运行时会引发错误。有关更多信息，请阅读有关类型推断的内容。

You can add a comma after the last item in a Dart collection literal. This *trailing comma* doesn’t affect the collection, but it can help prevent copy-paste errors.
您可以在 Dart 集合字面量的最后一个项目后添加逗号。此尾随逗号不会影响集合，但可以帮助防止复制粘贴错误。

```dart
var list = [
  'Car',
  'Boat',
  'Plane',
];
```

Lists use zero-based indexing, where 0 is the index of the first value and `list.length - 1` is the index of the last value. You can get a list’s length using the `.length` property and access a list’s values using the subscript operator (`[]`):
列表使用从零开始的索引，其中 0 是第一个值索引， `list.length - 1` 是最后一个值的索引。您可以使用 `.length` 属性获取列表的长度，并使用下标运算符 ( `[]` ) 访问列表的值：

```dart
var list = [1, 2, 3];
assert(list.length == 3);
assert(list[1] == 2);

list[1] = 1;
assert(list[1] == 1);
```

To create a list that’s a compile-time constant, add `const` before the list literal:
要创建一个编译时常量的列表，请在列表字面量之前添加 `const` ：

```dart
var constantList = const [1, 2, 3];
// constantList[1] = 1; // This line will cause an error.
```

For more information about lists, refer to the Lists section of the [`dart:core` documentation](https://dart.dev/libraries/dart-core#lists).
有关列表的更多信息，请参阅 `dart:core` 文档的列表部分。

## Sets 集合

A set in Dart is an unordered collection of unique items. Dart support for sets is provided by set literals and the [`Set`](https://api.dart.dev/stable/dart-core/Set-class.html) type.
Dart 中的集合是唯一项的无序集合。Dart 对集合的支持由集合字面量和 `Set` 类型提供。

Here is a simple Dart set, created using a set literal:
这是一个使用集合字面量创建的简单 Dart 集合：

```dart
var halogens = {'fluorine', 'chlorine', 'bromine', 'iodine', 'astatine'};
```

*info* **Note:** Dart infers that `halogens` has the type `Set<String>`. If you try to add the wrong type of value to the set, the analyzer or runtime raises an error. For more information, read about [type inference.](https://dart.dev/language/type-system#type-inference)
注意：Dart 推断 `halogens` 的类型为 `Set<String>` 。如果您尝试向集合添加错误类型的数值，分析器或运行时会引发错误。有关更多信息，请阅读有关类型推断的内容。

To create an empty set, use `{}` preceded by a type argument, or assign `{}` to a variable of type `Set`:
要创建一个空集合，请使用带有类型参数的 `{}` ，或将 `{}` 赋值给类型为 `Set` 的变量：

```dart
var names = <String>{};
// Set<String> names = {}; // This works, too.
// var names = {}; // Creates a map, not a set.
```

*info* **Set or map?** The syntax for map literals is similar to that for set literals. Because map literals came first, `{}` defaults to the `Map` type. If you forget the type annotation on `{}` or the variable it’s assigned to, then Dart creates an object of type `Map<dynamic, dynamic>`.
集合还是映射？映射字面量的语法类似于集合字面量的语法。由于映射字面量先出现，因此 `{}` 默认类型为 `Map` 。如果您忘记了 `{}` 或其所赋值变量的类型注释，那么 Dart 会创建一个类型为 `Map<dynamic, dynamic>` 的对象。

Add items to an existing set using the `add()` or `addAll()` methods:
使用 `add()` 或 `addAll()` 方法向现有集合添加项：

```dart
var elements = <String>{};
elements.add('fluorine');
elements.addAll(halogens);
```

Use `.length` to get the number of items in the set:
使用 `.length` 获取集合中的项数：

```dart
var elements = <String>{};
elements.add('fluorine');
elements.addAll(halogens);
assert(elements.length == 5);
```

To create a set that’s a compile-time constant, add `const` before the set literal:
要创建一个编译时常量的集合，请在集合字面量之前添加 `const` ：

```dart
final constantSet = const {
  'fluorine',
  'chlorine',
  'bromine',
  'iodine',
  'astatine',
};
// constantSet.add('helium'); // This line will cause an error.
```

For more information about sets, refer to the Sets section of the [`dart:core` documentation](https://dart.dev/libraries/dart-core#sets).
有关集合的更多信息，请参阅 `dart:core` 文档的集合部分。

## Maps 映射

In general, a map is an object that associates keys and values. Both keys and values can be any type of object. Each *key* occurs only once, but you can use the same *value* multiple times. Dart support for maps is provided by map literals and the [`Map`](https://api.dart.dev/stable/dart-core/Map-class.html) type.
通常，映射是一个将键与值相关联的对象。键和值都可以是任何类型的对象。每个键只出现一次，但您可以多次使用相同的值。Dart 对映射的支持由映射字面量和 `Map` 类型提供。

Here are a couple of simple Dart maps, created using map literals:
以下是使用映射字面量创建的几个简单的 Dart 映射：

```dart
var gifts = {
  // Key:    Value
  'first': 'partridge',
  'second': 'turtledoves',
  'fifth': 'golden rings'
};

var nobleGases = {
  2: 'helium',
  10: 'neon',
  18: 'argon',
};
```

*info* **Note:** Dart infers that `gifts` has the type `Map<String, String>` and `nobleGases` has the type `Map<int, String>`. If you try to add the wrong type of value to either map, the analyzer or runtime raises an error. For more information, read about [type inference](https://dart.dev/language/type-system#type-inference).
注意：Dart 推断 `gifts` 的类型为 `Map<String, String>` ， `nobleGases` 的类型为 `Map<int, String>` 。如果您尝试向任一映射添加错误类型的值，分析器或运行时会引发错误。有关更多信息，请阅读有关类型推断的内容。

You can create the same objects using a Map constructor:
您可以使用 Map 构造函数创建相同对象：

```dart
var gifts = Map<String, String>();
gifts['first'] = 'partridge';
gifts['second'] = 'turtledoves';
gifts['fifth'] = 'golden rings';

var nobleGases = Map<int, String>();
nobleGases[2] = 'helium';
nobleGases[10] = 'neon';
nobleGases[18] = 'argon';
```

*info* **Note:** If you come from a language like C# or Java, you might expect to see `new Map()` instead of just `Map()`. In Dart, the `new` keyword is optional. For details, see [Using constructors](https://dart.dev/language/classes#using-constructors).
注意：如果您来自 C# 或 Java 等语言，您可能希望看到 `new Map()` 而不仅仅是 `Map()` 。在 Dart 中， `new` 关键字是可选的。有关详细信息，请参阅使用构造函数。

Add a new key-value pair to an existing map using the subscript assignment operator (`[]=`):
使用下标赋值运算符 ( `[]=` ) 向现有映射添加新的键值对：

```dart
var gifts = {'first': 'partridge'};
gifts['fourth'] = 'calling birds'; // Add a key-value pair
```

Retrieve a value from a map using the subscript operator (`[]`):
使用下标运算符 ( `[]` ) 从映射中检索值：

```dart
var gifts = {'first': 'partridge'};
assert(gifts['first'] == 'partridge');
```

If you look for a key that isn’t in a map, you get `null` in return:
如果您查找地图中不存在的键，您将获得 `null` 作为返回值：

```dart
var gifts = {'first': 'partridge'};
assert(gifts['fifth'] == null);
```

Use `.length` to get the number of key-value pairs in the map:
使用 `.length` 获取地图中的键值对数量：

```dart
var gifts = {'first': 'partridge'};
gifts['fourth'] = 'calling birds';
assert(gifts.length == 2);
```

To create a map that’s a compile-time constant, add `const` before the map literal:
要创建编译时常量的映射，请在映射字面量前添加 `const` ：

```dart
final constantMap = const {
  2: 'helium',
  10: 'neon',
  18: 'argon',
};

// constantMap[2] = 'Helium'; // This line will cause an error.
```

For more information about maps, refer to the Maps section of the [`dart:core` documentation](https://dart.dev/libraries/dart-core#maps).
有关映射的更多信息，请参阅 `dart:core` 文档的映射部分。

## Operators 运算符

### Spread operators 展开运算符

Dart supports the **spread operator** (`...`) and the **null-aware spread operator** (`...?`) in list, map, and set literals. Spread operators provide a concise way to insert multiple values into a collection.
Dart 在列表、映射和集合字面量中支持展开运算符 ( `...` ) 和空感知展开运算符 ( `...?` )。展开运算符提供了一种将多个值插入到集合中的简洁方法。

For example, you can use the spread operator (`...`) to insert all the values of a list into another list:
例如，您可以使用展开运算符 ( `...` ) 将列表的所有值插入到另一个列表中：

```dart
var list = [1, 2, 3];
var list2 = [0, ...list];
assert(list2.length == 4);
```

If the expression to the right of the spread operator might be null, you can avoid exceptions by using a null-aware spread operator (`...?`):
如果展开运算符右侧的表达式可能为 null，您可以通过使用空感知展开运算符 ( `...?` ) 来避免异常：

```dart
var list2 = [0, ...?list];
assert(list2.length == 1);
```

For more details and examples of using the spread operator, see the [spread operator proposal.](https://github.com/dart-lang/language/blob/main/accepted/2.3/spread-collections/feature-specification.md)
有关使用展开运算符的更多详细信息和示例，请参阅展开运算符提案。



### Control-flow operators 控制流运算符

Dart offers **collection if** and **collection for** for use in list, map, and set literals. You can use these operators to build collections using conditionals (`if`) and repetition (`for`).
Dart 提供集合 if 和集合 for，用于列表、映射和集合字面量。您可以使用这些运算符使用条件 ( `if` ) 和重复 ( `for` ) 来构建集合。

Here’s an example of using **collection if** to create a list with three or four items in it:
以下是如何使用集合 if 创建包含三或四个项目的列表的示例：

```dart
var nav = ['Home', 'Furniture', 'Plants', if (promoActive) 'Outlet'];
```

Dart also supports [if-case](https://dart.dev/language/branches#if-case) inside collection literals:
Dart 还支持集合字面量中的 if-case：

```
var nav = ['Home', 'Furniture', 'Plants', if (login case 'Manager') 'Inventory'];
```

Here’s an example of using **collection for** to manipulate the items of a list before adding them to another list:
以下是如何使用集合 for 在将项目添加到另一个列表之前操作该列表的项目的示例：

```dart
var listOfInts = [1, 2, 3];
var listOfStrings = ['#0', for (var i in listOfInts) '#$i'];
assert(listOfStrings[1] == '#1');
```

For more details and examples of using collection `if` and `for`, see the [control flow collections proposal.](https://github.com/dart-lang/language/blob/main/accepted/2.3/control-flow-collections/feature-specification.md)
有关使用集合 `if` 和 `for` 的更多详细信息和示例，请参阅控制流集合提案。
