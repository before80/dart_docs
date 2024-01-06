+++
title = "dart:core"
date = 2024-01-05T20:29:36+08:00
weight = 10
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文: [https://dart.dev/libraries/dart-core](https://dart.dev/libraries/dart-core)

## dart:core

The dart:core library ([API reference](https://api.dart.dev/stable/dart-core/dart-core-library.html)) provides a small but critical set of built-in functionality. This library is automatically imported into every Dart program.
dart:core 库（API 参考）提供了一组精简但至关重要的内置功能。此库会自动导入到每个 Dart 程序中。

### Printing to the console 打印到控制台

The top-level `print()` method takes a single argument (any Object) and displays that object’s string value (as returned by `toString()`) in the console.
顶级 `print()` 方法接受一个参数（任何对象），并在控制台中显示该对象的字符串值（由 `toString()` 返回）。

```dart
print(anObject);
print('I drink $tea.');
```

For more information on basic strings and `toString()`, see [Strings](https://dart.dev/language/built-in-types#strings) in the language tour.
有关基本字符串和 `toString()` 的更多信息，请参阅语言之旅中的字符串。

### Numbers 数字

The dart:core library defines the num, int, and double classes, which have some basic utilities for working with numbers.
dart:core 库定义了 num、int 和 double 类，这些类具有一些用于处理数字的基本实用工具。

You can convert a string into an integer or double with the `parse()` methods of int and double, respectively:
您可以使用 int 和 double 的 `parse()` 方法将字符串转换为整数或双精度浮点数：

```dart
assert(int.parse('42') == 42);
assert(int.parse('0x42') == 66);
assert(double.parse('0.50') == 0.5);
```

Or use the parse() method of num, which creates an integer if possible and otherwise a double:
或者使用 num 的 parse() 方法，如果可能，它会创建一个整数，否则创建一个双精度浮点数：

```dart
assert(num.parse('42') is int);
assert(num.parse('0x42') is int);
assert(num.parse('0.50') is double);
```

To specify the base of an integer, add a `radix` parameter:
要指定整数的基数，请添加 `radix` 参数：

```dart
assert(int.parse('42', radix: 16) == 66);
```

Use the `toString()` method to convert an int or double to a string. To specify the number of digits to the right of the decimal, use [toStringAsFixed().](https://api.dart.dev/stable/dart-core/num/toStringAsFixed.html) To specify the number of significant digits in the string, use [toStringAsPrecision():](https://api.dart.dev/stable/dart-core/num/toStringAsPrecision.html)
使用 `toString()` 方法将 int 或 double 转换为字符串。要指定小数点右边的数字位数，请使用 toStringAsFixed()。要指定字符串中的有效数字位数，请使用 toStringAsPrecision()：

```dart
// Convert an int to a string.
assert(42.toString() == '42');

// Convert a double to a string.
assert(123.456.toString() == '123.456');

// Specify the number of digits after the decimal.
assert(123.456.toStringAsFixed(2) == '123.46');

// Specify the number of significant figures.
assert(123.456.toStringAsPrecision(2) == '1.2e+2');
assert(double.parse('1.2e+2') == 120.0);
```

For more information, see the API documentation for [int,](https://api.dart.dev/stable/dart-core/int-class.html) [double,](https://api.dart.dev/stable/dart-core/double-class.html) and [num.](https://api.dart.dev/stable/dart-core/num-class.html) Also see the [dart:math section](https://dart.dev/libraries/dart-math)
有关更多信息，请参阅 int、double 和 num 的 API 文档。另请参阅 dart:math 部分

### Strings and regular expressions 字符串和正则表达式

A string in Dart is an immutable sequence of UTF-16 code units. The language tour has more information about [strings](https://dart.dev/language/built-in-types#strings). You can use regular expressions (RegExp objects) to search within strings and to replace parts of strings.
Dart 中的字符串是 UTF-16 代码单元的不可变序列。语言之旅中有更多有关字符串的信息。您可以使用正则表达式 (RegExp 对象) 在字符串中搜索并替换字符串的各个部分。

The String class defines such methods as `split()`, `contains()`, `startsWith()`, `endsWith()`, and more.
String 类定义了诸如 `split()` 、 `contains()` 、 `startsWith()` 、 `endsWith()` 等方法。

#### Searching inside a string 在字符串中搜索

You can find particular locations within a string, as well as check whether a string begins with or ends with a particular pattern. For example:
您可以在字符串中查找特定位置，以及检查字符串是否以特定模式开头或结尾。例如：

```dart
// Check whether a string contains another string.
assert('Never odd or even'.contains('odd'));

// Does a string start with another string?
assert('Never odd or even'.startsWith('Never'));

// Does a string end with another string?
assert('Never odd or even'.endsWith('even'));

// Find the location of a string inside a string.
assert('Never odd or even'.indexOf('odd') == 6);
```

#### Extracting data from a string 从字符串中提取数据

You can get the individual characters from a string as Strings or ints, respectively. To be precise, you actually get individual UTF-16 code units; high-numbered characters such as the treble clef symbol (‘\u{1D11E}’) are two code units apiece.
您可以分别将字符串中的各个字符作为字符串或整数获取。确切地说，您实际上获取的是各个 UTF-16 代码单元；高编号字符（例如高音谱号符号 (‘\u{1D11E}’））是两个代码单元。

You can also extract a substring or split a string into a list of substrings:
您还可以提取子字符串或将字符串拆分为子字符串列表：

```dart
// Grab a substring.
assert('Never odd or even'.substring(6, 9) == 'odd');

// Split a string using a string pattern.
var parts = 'progressive web apps'.split(' ');
assert(parts.length == 3);
assert(parts[0] == 'progressive');

// Get a UTF-16 code unit (as a string) by index.
assert('Never odd or even'[0] == 'N');

// Use split() with an empty string parameter to get
// a list of all characters (as Strings); good for
// iterating.
for (final char in 'hello'.split('')) {
  print(char);
}

// Get all the UTF-16 code units in the string.
var codeUnitList = 'Never odd or even'.codeUnits.toList();
assert(codeUnitList[0] == 78);
```

*info* **Note:** In many cases, you want to work with Unicode grapheme clusters as opposed to pure code units. These are characters as they are perceived by the user (for example, “🇬🇧” is one user-perceived character but several UTF-16 code units). For this, the Dart team provides the [`characters` package.](https://pub.dev/packages/characters)
注意：在许多情况下，您希望使用 Unicode 字符簇而不是纯代码单元。这些是用户感知到的字符（例如，“🇬🇧”是一个用户感知到的字符，但有几个 UTF-16 代码单元）。为此，Dart 团队提供了 `characters` 包。

#### Converting to uppercase or lowercase 转换为大写或小写

You can easily convert strings to their uppercase and lowercase variants:
您可以轻松地将字符串转换为其大写和小写变体：

```dart
// Convert to uppercase.
assert('web apps'.toUpperCase() == 'WEB APPS');

// Convert to lowercase.
assert('WEB APPS'.toLowerCase() == 'web apps');
```

*info* **Note:** These methods don’t work for every language. For example, the Turkish alphabet’s dotless *I* is converted incorrectly.
注意：这些方法不适用于每种语言。例如，土耳其语字母的无点 I 转换不正确。

#### Trimming and empty strings 修剪和空字符串

Remove all leading and trailing white space with `trim()`. To check whether a string is empty (length is zero), use `isEmpty`.
使用 `trim()` 删除所有前导和尾随空格。要检查字符串是否为空（长度为零），请使用 `isEmpty` 。

```dart
// Trim a string.
assert('  hello  '.trim() == 'hello');

// Check whether a string is empty.
assert(''.isEmpty);

// Strings with only white space are not empty.
assert('  '.isNotEmpty);
```

#### Replacing part of a string 替换字符串的一部分

Strings are immutable objects, which means you can create them but you can’t change them. If you look closely at the [String API reference,](https://api.dart.dev/stable/dart-core/String-class.html) you’ll notice that none of the methods actually changes the state of a String. For example, the method `replaceAll()` returns a new String without changing the original String:
字符串是不可变对象，这意味着您可以创建它们，但不能更改它们。如果您仔细查看 String API 参考，您会注意到没有一个方法真正更改 String 的状态。例如，方法 `replaceAll()` 返回一个新的 String，而不更改原始 String：

```dart
var greetingTemplate = 'Hello, NAME!';
var greeting = greetingTemplate.replaceAll(RegExp('NAME'), 'Bob');

// greetingTemplate didn't change.
assert(greeting != greetingTemplate);
```

#### Building a string 构建字符串

To programmatically generate a string, you can use StringBuffer. A StringBuffer doesn’t generate a new String object until `toString()` is called. The `writeAll()` method has an optional second parameter that lets you specify a separator—in this case, a space.
要以编程方式生成字符串，可以使用 StringBuffer。StringBuffer 不会生成新的 String 对象，直到调用 `toString()` 。 `writeAll()` 方法有一个可选的第二个参数，允许您指定分隔符——在本例中，是一个空格。

```dart
var sb = StringBuffer();
sb
  ..write('Use a StringBuffer for ')
  ..writeAll(['efficient', 'string', 'creation'], ' ')
  ..write('.');

var fullString = sb.toString();

assert(fullString == 'Use a StringBuffer for efficient string creation.');
```

#### Regular expressions 正则表达式

The RegExp class provides the same capabilities as JavaScript regular expressions. Use regular expressions for efficient searching and pattern matching of strings.
RegExp 类提供与 JavaScript 正则表达式相同的功能。使用正则表达式可高效地搜索和匹配字符串模式。

```dart
// Here's a regular expression for one or more digits.
var numbers = RegExp(r'\d+');

var allCharacters = 'llamas live fifteen to twenty years';
var someDigits = 'llamas live 15 to 20 years';

// contains() can use a regular expression.
assert(!allCharacters.contains(numbers));
assert(someDigits.contains(numbers));

// Replace every match with another string.
var exedOut = someDigits.replaceAll(numbers, 'XX');
assert(exedOut == 'llamas live XX to XX years');
```

You can work directly with the RegExp class, too. The Match class provides access to a regular expression match.
您也可以直接使用 RegExp 类。Match 类提供对正则表达式匹配的访问权限。

```dart
var numbers = RegExp(r'\d+');
var someDigits = 'llamas live 15 to 20 years';

// Check whether the reg exp has a match in a string.
assert(numbers.hasMatch(someDigits));

// Loop through all matches.
for (final match in numbers.allMatches(someDigits)) {
  print(match.group(0)); // 15, then 20
}
```

#### More information 更多信息

Refer to the [String API reference](https://api.dart.dev/stable/dart-core/String-class.html) for a full list of methods. Also see the API reference for [StringBuffer,](https://api.dart.dev/stable/dart-core/StringBuffer-class.html) [Pattern,](https://api.dart.dev/stable/dart-core/Pattern-class.html) [RegExp,](https://api.dart.dev/stable/dart-core/RegExp-class.html) and [Match.](https://api.dart.dev/stable/dart-core/Match-class.html)
有关方法的完整列表，请参阅 String API 参考。另请参阅 StringBuffer、Pattern、RegExp 和 Match 的 API 参考。

### Collections 集合

Dart ships with a core collections API, which includes classes for lists, sets, and maps.
Dart 附带一个核心集合 API，其中包括列表、集合和映射的类。

*tips_and_updates* **Tip:** To practice using APIs that are available to both lists and sets, follow the [Iterable collections codelab](https://dart.dev/codelabs/iterables).
提示：要练习使用同时适用于列表和集合的 API，请按照 Iterable 集合 codelab 进行操作。

#### Lists 列表

As the language tour shows, you can use literals to create and initialize [lists](https://dart.dev/language/collections#lists). Alternatively, use one of the List constructors. The List class also defines several methods for adding items to and removing items from lists.
正如语言之旅所示，您可以使用文字创建并初始化列表。或者，使用其中一个 List 构造函数。List 类还定义了几个用于向列表中添加项和从列表中删除项的方法。

```dart
// Create an empty list of strings.
var grains = <String>[];
assert(grains.isEmpty);

// Create a list using a list literal.
var fruits = ['apples', 'oranges'];

// Add to a list.
fruits.add('kiwis');

// Add multiple items to a list.
fruits.addAll(['grapes', 'bananas']);

// Get the list length.
assert(fruits.length == 5);

// Remove a single item.
var appleIndex = fruits.indexOf('apples');
fruits.removeAt(appleIndex);
assert(fruits.length == 4);

// Remove all elements from a list.
fruits.clear();
assert(fruits.isEmpty);

// You can also create a List using one of the constructors.
var vegetables = List.filled(99, 'broccoli');
assert(vegetables.every((v) => v == 'broccoli'));
```

Use `indexOf()` to find the index of an object in a list:
使用 `indexOf()` 查找列表中对象的索引：

```dart
var fruits = ['apples', 'oranges'];

// Access a list item by index.
assert(fruits[0] == 'apples');

// Find an item in a list.
assert(fruits.indexOf('apples') == 0);
```

Sort a list using the `sort()` method. You can provide a sorting function that compares two objects. This sorting function must return < 0 for *smaller*, 0 for the *same*, and > 0 for *bigger*. The following example uses `compareTo()`, which is defined by [Comparable](https://api.dart.dev/stable/dart-core/Comparable-class.html) and implemented by String.
使用 `sort()` 方法对列表进行排序。您可以提供一个比较两个对象的排序函数。此排序函数必须返回 < 0 表示较小，0 表示相同，> 0 表示较大。以下示例使用 `compareTo()` ，它由 Comparable 定义并由 String 实现。

```dart
var fruits = ['bananas', 'apples', 'oranges'];

// Sort a list.
fruits.sort((a, b) => a.compareTo(b));
assert(fruits[0] == 'apples');
```

Lists are parameterized types ([generics](https://dart.dev/language/generics)), so you can specify the type that a list should contain:
列表是参数化类型（泛型），因此您可以指定列表应包含的类型：

```dart
// This list should contain only strings.
var fruits = <String>[];

fruits.add('apples');
var fruit = fruits[0];
assert(fruit is String);
fruits.add(5); // Error: 'int' can't be assigned to 'String'
```

*info* **Note:** In many cases, you don’t need to explicitly specify generic types, because Dart will [infer](https://dart.dev/language/type-system#type-inference) them for you. A list like `['Dash', 'Dart']` is understood to be a `List<String>` (read: list of strings).
注意：在许多情况下，您无需显式指定泛型类型，因为 Dart 会为您推断它们。像 `['Dash', 'Dart']` 这样的列表被理解为 `List<String>` （即字符串列表）。

But there are times when you *should* specify the generic type. Like, for example, when Dart doesn’t have anything to infer from: `[]` could be a list of any combination of things. That’s often not what you want, so you write `<String>[]` or `<Person>[]` or something similar.
但在某些情况下，您应该指定泛型类型。例如，当 Dart 无从推断时： `[]` 可以是任何事物的组合列表。这通常不是您想要的，因此您编写 `<String>[]` 或 `<Person>[]` 或类似内容。

Refer to the [List API reference](https://api.dart.dev/stable/dart-core/List-class.html) for a full list of methods.
有关方法的完整列表，请参阅 List API 参考。

#### Sets 组

A set in Dart is an unordered collection of unique items. Because a set is unordered, you can’t get a set’s items by index (position).
Dart 中的集合是一个无序的唯一项集合。由于集合是无序的，因此无法按索引（位置）获取集合的项。

```dart
// Create an empty set of strings.
var ingredients = <String>{};

// Add new items to it.
ingredients.addAll(['gold', 'titanium', 'xenon']);
assert(ingredients.length == 3);

// Adding a duplicate item has no effect.
ingredients.add('gold');
assert(ingredients.length == 3);

// Remove an item from a set.
ingredients.remove('gold');
assert(ingredients.length == 2);

// You can also create sets using
// one of the constructors.
var atomicNumbers = Set.from([79, 22, 54]);
```

Use `contains()` and `containsAll()` to check whether one or more objects are in a set:
使用 `contains()` 和 `containsAll()` 检查一个或多个对象是否在集合中：

```dart
var ingredients = Set<String>();
ingredients.addAll(['gold', 'titanium', 'xenon']);

// Check whether an item is in the set.
assert(ingredients.contains('titanium'));

// Check whether all the items are in the set.
assert(ingredients.containsAll(['titanium', 'xenon']));
```

An intersection is a set whose items are in two other sets.
交集是一个项在两个其他集合中的集合。

```dart
var ingredients = Set<String>();
ingredients.addAll(['gold', 'titanium', 'xenon']);

// Create the intersection of two sets.
var nobleGases = Set.from(['xenon', 'argon']);
var intersection = ingredients.intersection(nobleGases);
assert(intersection.length == 1);
assert(intersection.contains('xenon'));
```

Refer to the [Set API reference](https://api.dart.dev/stable/dart-core/Set-class.html) for a full list of methods.
有关方法的完整列表，请参阅 Set API 参考。

#### Maps 映射

A map, commonly known as a *dictionary* or *hash*, is an unordered collection of key-value pairs. Maps associate a key to some value for easy retrieval. Unlike in JavaScript, Dart objects are not maps.
映射（通常称为字典或哈希）是无序的键值对集合。映射将键与某个值相关联，以便于检索。与 JavaScript 不同，Dart 对象不是映射。

You can declare a map using a terse literal syntax, or you can use a traditional constructor:
可以使用简洁的文字语法声明映射，也可以使用传统构造函数：

```dart
// Maps often use strings as keys.
var hawaiianBeaches = {
  'Oahu': ['Waikiki', 'Kailua', 'Waimanalo'],
  'Big Island': ['Wailea Bay', 'Pololu Beach'],
  'Kauai': ['Hanalei', 'Poipu']
};

// Maps can be built from a constructor.
var searchTerms = Map();

// Maps are parameterized types; you can specify what
// types the key and value should be.
var nobleGases = Map<int, String>();
```

You add, get, and set map items using the bracket syntax. Use `remove()` to remove a key and its value from a map.
您可以使用方括号语法添加、获取和设置映射项。使用 `remove()` 从映射中移除键及其值。

```dart
var nobleGases = {54: 'xenon'};

// Retrieve a value with a key.
assert(nobleGases[54] == 'xenon');

// Check whether a map contains a key.
assert(nobleGases.containsKey(54));

// Remove a key and its value.
nobleGases.remove(54);
assert(!nobleGases.containsKey(54));
```

You can retrieve all the values or all the keys from a map:
您可以从映射中检索所有值或所有键：

```dart
var hawaiianBeaches = {
  'Oahu': ['Waikiki', 'Kailua', 'Waimanalo'],
  'Big Island': ['Wailea Bay', 'Pololu Beach'],
  'Kauai': ['Hanalei', 'Poipu']
};

// Get all the keys as an unordered collection
// (an Iterable).
var keys = hawaiianBeaches.keys;

assert(keys.length == 3);
assert(Set.from(keys).contains('Oahu'));

// Get all the values as an unordered collection
// (an Iterable of Lists).
var values = hawaiianBeaches.values;
assert(values.length == 3);
assert(values.any((v) => v.contains('Waikiki')));
```

To check whether a map contains a key, use `containsKey()`. Because map values can be null, you cannot rely on simply getting the value for the key and checking for null to determine the existence of a key.
要检查映射是否包含某个键，请使用 `containsKey()` 。由于映射值可以为 null，因此您不能仅仅获取键的值并检查 null 来确定键是否存在。

```dart
var hawaiianBeaches = {
  'Oahu': ['Waikiki', 'Kailua', 'Waimanalo'],
  'Big Island': ['Wailea Bay', 'Pololu Beach'],
  'Kauai': ['Hanalei', 'Poipu']
};

assert(hawaiianBeaches.containsKey('Oahu'));
assert(!hawaiianBeaches.containsKey('Florida'));
```

Use the `putIfAbsent()` method when you want to assign a value to a key if and only if the key does not already exist in a map. You must provide a function that returns the value.
当您想仅在键尚不存在于映射中时为其分配值时，请使用 `putIfAbsent()` 方法。您必须提供一个返回该值的函数。

```dart
var teamAssignments = <String, String>{};
teamAssignments.putIfAbsent('Catcher', () => pickToughestKid());
assert(teamAssignments['Catcher'] != null);
```

Refer to the [Map API reference](https://api.dart.dev/stable/dart-core/Map-class.html) for a full list of methods.
有关方法的完整列表，请参阅 Map API 参考。

#### Common collection methods 通用集合方法

List, Set, and Map share common functionality found in many collections. Some of this common functionality is defined by the Iterable class, which List and Set implement.
列表、集合和映射共享许多集合中常见的通用功能。其中一些通用功能由 Iterable 类定义，List 和 Set 实现该类。

*info* **Note:** Although Map doesn’t implement Iterable, you can get Iterables from it using the Map `keys` and `values` properties.
注意：尽管 Map 未实现 Iterable，但您可以使用 Map `keys` 和 `values` 属性从中获取 Iterable。

Use `isEmpty` or `isNotEmpty` to check whether a list, set, or map has items:
使用 `isEmpty` 或 `isNotEmpty` 检查列表、集合或映射中是否有项：

```dart
var coffees = <String>[];
var teas = ['green', 'black', 'chamomile', 'earl grey'];
assert(coffees.isEmpty);
assert(teas.isNotEmpty);
```

To apply a function to each item in a list, set, or map, you can use `forEach()`:
要将函数应用于列表、集合或映射中的每个项，可以使用 `forEach()` ：

```dart
var teas = ['green', 'black', 'chamomile', 'earl grey'];

teas.forEach((tea) => print('I drink $tea'));
```

When you invoke `forEach()` on a map, your function must take two arguments (the key and value):
当您对映射调用 `forEach()` 时，您的函数必须采用两个参数（键和值）：

```dart
hawaiianBeaches.forEach((k, v) {
  print('I want to visit $k and swim at $v');
  // I want to visit Oahu and swim at
  // [Waikiki, Kailua, Waimanalo], etc.
});
```

Iterables provide the `map()` method, which gives you all the results in a single object:
可迭代对象提供 `map()` 方法，该方法可让您在单个对象中获得所有结果：

```dart
var teas = ['green', 'black', 'chamomile', 'earl grey'];

var loudTeas = teas.map((tea) => tea.toUpperCase());
loudTeas.forEach(print);
```

*info* **Note:** The object returned by `map()` is an Iterable that’s *lazily evaluated*: your function isn’t called until you ask for an item from the returned object.
注意： `map()` 返回的对象是一个惰性求值的可迭代对象：在您请求返回对象中的某个项目之前，您的函数不会被调用。

To force your function to be called immediately on each item, use `map().toList()` or `map().toSet()`:
若要强制您的函数立即对每个项目进行调用，请使用 `map().toList()` 或 `map().toSet()` ：

```dart
var loudTeas = teas.map((tea) => tea.toUpperCase()).toList();
```

Use Iterable’s `where()` method to get all the items that match a condition. Use Iterable’s `any()` and `every()` methods to check whether some or all items match a condition.
使用 Iterable 的 `where()` 方法获取满足条件的所有项目。使用 Iterable 的 `any()` 和 `every()` 方法检查某些或所有项目是否满足条件。

```dart
var teas = ['green', 'black', 'chamomile', 'earl grey'];

// Chamomile is not caffeinated.
bool isDecaffeinated(String teaName) => teaName == 'chamomile';

// Use where() to find only the items that return true
// from the provided function.
var decaffeinatedTeas = teas.where((tea) => isDecaffeinated(tea));
// or teas.where(isDecaffeinated)

// Use any() to check whether at least one item in the
// collection satisfies a condition.
assert(teas.any(isDecaffeinated));

// Use every() to check whether all the items in a
// collection satisfy a condition.
assert(!teas.every(isDecaffeinated));
```

For a full list of methods, refer to the [Iterable API reference,](https://api.dart.dev/stable/dart-core/Iterable-class.html) as well as those for [List,](https://api.dart.dev/stable/dart-core/List-class.html) [Set,](https://api.dart.dev/stable/dart-core/Set-class.html) and [Map.](https://api.dart.dev/stable/dart-core/Map-class.html)
有关方法的完整列表，请参阅 Iterable API 参考，以及 List、Set 和 Map 的 API 参考。

### URIs URI

The [Uri class](https://api.dart.dev/stable/dart-core/Uri-class.html) provides functions to encode and decode strings for use in URIs (which you might know as *URLs*). These functions handle characters that are special for URIs, such as `&` and `=`. The Uri class also parses and exposes the components of a URI—host, port, scheme, and so on.
Uri 类提供用于对字符串进行编码和解码的函数，以便在 URI（您可能称为 URL）中使用。这些函数处理对 URI 具有特殊意义的字符，例如 `&` 和 `=` 。Uri 类还会解析并公开 URI 的组件，例如主机、端口、方案等。

#### Encoding and decoding fully qualified URIs 对完全限定的 URI 进行编码和解码

To encode and decode characters *except* those with special meaning in a URI (such as `/`, `:`, `&`, `#`), use the `encodeFull()` and `decodeFull()` methods. These methods are good for encoding or decoding a fully qualified URI, leaving intact special URI characters.
若要对 URI 中具有特殊含义的字符（例如 `/` 、 `:` 、 `&` 、 `#` ）以外的字符进行编码和解码，请使用 `encodeFull()` 和 `decodeFull()` 方法。这些方法适用于对完全限定的 URI 进行编码或解码，保持 URI 特殊字符不变。

```dart
var uri = 'https://example.org/api?foo=some message';

var encoded = Uri.encodeFull(uri);
assert(encoded == 'https://example.org/api?foo=some%20message');

var decoded = Uri.decodeFull(encoded);
assert(uri == decoded);
```

Notice how only the space between `some` and `message` was encoded.
注意，只有 `some` 和 `message` 之间的空格被编码。

#### Encoding and decoding URI components 编码和解码 URI 组件

To encode and decode all of a string’s characters that have special meaning in a URI, including (but not limited to) `/`, `&`, and `:`, use the `encodeComponent()` and `decodeComponent()` methods.
要对字符串中所有在 URI 中具有特殊含义的字符进行编码和解码，包括（但不限于） `/` 、 `&` 和 `:` ，请使用 `encodeComponent()` 和 `decodeComponent()` 方法。

```dart
var uri = 'https://example.org/api?foo=some message';

var encoded = Uri.encodeComponent(uri);
assert(
    encoded == 'https%3A%2F%2Fexample.org%2Fapi%3Ffoo%3Dsome%20message');

var decoded = Uri.decodeComponent(encoded);
assert(uri == decoded);
```

Notice how every special character is encoded. For example, `/` is encoded to `%2F`.
注意，每个特殊字符都已编码。例如， `/` 已编码为 `%2F` 。

#### Parsing URIs 解析 URI

If you have a Uri object or a URI string, you can get its parts using Uri fields such as `path`. To create a Uri from a string, use the `parse()` static method:
如果您有 Uri 对象或 URI 字符串，则可以使用 Uri 字段（例如 `path` ）获取其部分。要从字符串创建 Uri，请使用 `parse()` 静态方法：

```dart
var uri = Uri.parse('https://example.org:8080/foo/bar#frag');

assert(uri.scheme == 'https');
assert(uri.host == 'example.org');
assert(uri.path == '/foo/bar');
assert(uri.fragment == 'frag');
assert(uri.origin == 'https://example.org:8080');
```

See the [Uri API reference](https://api.dart.dev/stable/dart-core/Uri-class.html) for more URI components that you can get.
请参阅 Uri API 参考，以了解您可以获取的更多 URI 组件。

#### Building URIs 构建 URI

You can build up a URI from individual parts using the `Uri()` constructor:
您可以使用 `Uri()` 构造函数从各个部分构建 URI：

```dart
var uri = Uri(
    scheme: 'https',
    host: 'example.org',
    path: '/foo/bar',
    fragment: 'frag',
    queryParameters: {'lang': 'dart'});
assert(uri.toString() == 'https://example.org/foo/bar?lang=dart#frag');
```

If you don’t need to specify a fragment, to create a URI with a http or https scheme, you can instead use the [`Uri.http`](https://api.dart.dev/stable/dart-core/Uri/Uri.http.html) or [`Uri.https`](https://api.dart.dev/stable/dart-core/Uri/Uri.https.html) factory constructors:
如果您不需要指定片段，则可以改用 `Uri.http` 或 `Uri.https` 工厂构造函数来创建具有 http 或 https 方案的 URI：

```dart
var httpUri = Uri.http('example.org', '/foo/bar', {'lang': 'dart'});
var httpsUri = Uri.https('example.org', '/foo/bar', {'lang': 'dart'});

assert(httpUri.toString() == 'http://example.org/foo/bar?lang=dart');
assert(httpsUri.toString() == 'https://example.org/foo/bar?lang=dart');
```

### Dates and times 日期和时间

A DateTime object is a point in time. The time zone is either UTC or the local time zone.
DateTime 对象是一个时间点。时区是 UTC 或本地时区。

You can create DateTime objects using several constructors and methods:
您可以使用多个构造函数和方法创建 DateTime 对象：

```dart
// Get the current date and time.
var now = DateTime.now();

// Create a new DateTime with the local time zone.
var y2k = DateTime(2000); // January 1, 2000

// Specify the month and day.
y2k = DateTime(2000, 1, 2); // January 2, 2000

// Specify the date as a UTC time.
y2k = DateTime.utc(2000); // 1/1/2000, UTC

// Specify a date and time in ms since the Unix epoch.
y2k = DateTime.fromMillisecondsSinceEpoch(946684800000, isUtc: true);

// Parse an ISO 8601 date in the UTC time zone.
y2k = DateTime.parse('2000-01-01T00:00:00Z');

// Create a new DateTime from an existing one, adjusting just some properties:
var sameTimeLastYear = now.copyWith(year: now.year - 1);
```

*report_problem* **Warning:** `DateTime` operations might give unexpected results related to Daylight Savings Time and other non-standard time adjustments.
警告： `DateTime` 操作可能会导致与夏令时和其他非标准时间调整相关的意外结果。

The `millisecondsSinceEpoch` property of a date returns the number of milliseconds since the “Unix epoch”—January 1, 1970, UTC:
日期的 `millisecondsSinceEpoch` 属性返回自“Unix 纪元”（1970 年 1 月 1 日，UTC）以来的毫秒数：

```dart
// 1/1/2000, UTC
var y2k = DateTime.utc(2000);
assert(y2k.millisecondsSinceEpoch == 946684800000);

// 1/1/1970, UTC
var unixEpoch = DateTime.utc(1970);
assert(unixEpoch.millisecondsSinceEpoch == 0);
```

Use the Duration class to calculate the difference between two dates and to shift a date forward or backward:
使用 Duration 类计算两个日期之间的差值，并向前或向后移动日期：

```dart
var y2k = DateTime.utc(2000);

// Add one year.
var y2001 = y2k.add(const Duration(days: 366));
assert(y2001.year == 2001);

// Subtract 30 days.
var december2000 = y2001.subtract(const Duration(days: 30));
assert(december2000.year == 2000);
assert(december2000.month == 12);

// Calculate the difference between two dates.
// Returns a Duration object.
var duration = y2001.difference(y2k);
assert(duration.inDays == 366); // y2k was a leap year.
```

*report_problem* **Warning:** Using a Duration to shift a DateTime by days can be problematic, due to clock shifts (to daylight saving time, for example). Use UTC dates if you must shift days.
警告：由于时钟变化（例如，夏令时），使用 Duration 按天移动 DateTime 可能会有问题。如果您必须移动天数，请使用 UTC 日期。

For a full list of methods, refer to the API reference for [DateTime](https://api.dart.dev/stable/dart-core/DateTime-class.html) and [Duration.](https://api.dart.dev/stable/dart-core/Duration-class.html)
有关方法的完整列表，请参阅 DateTime 和 Duration 的 API 参考。

### Utility classes 实用程序类

The core library contains various utility classes, useful for sorting, mapping values, and iterating.
核心库包含各种实用程序类，可用于对值进行排序、映射和迭代。

#### Comparing objects 比较对象

Implement the [Comparable](https://api.dart.dev/stable/dart-core/Comparable-class.html) interface to indicate that an object can be compared to another object, usually for sorting. The `compareTo()` method returns < 0 for *smaller*, 0 for the *same*, and > 0 for *bigger*.
实现 Comparable 接口以指示对象可以与另一个对象进行比较，通常用于排序。 `compareTo()` 方法返回 < 0 表示较小，0 表示相同，> 0 表示较大。

```dart
class Line implements Comparable<Line> {
  final int length;
  const Line(this.length);

  @override
  int compareTo(Line other) => length - other.length;
}

void main() {
  var short = const Line(1);
  var long = const Line(100);
  assert(short.compareTo(long) < 0);
}
```

#### Implementing map keys 实现映射键

Each object in Dart automatically provides an integer hash code, and thus can be used as a key in a map. However, you can override the `hashCode` getter to generate a custom hash code. If you do, you might also want to override the `==` operator. Objects that are equal (via `==`) must have identical hash codes. A hash code doesn’t have to be unique, but it should be well distributed.
Dart 中的每个对象都会自动提供一个整数哈希码，因此可以将其用作映射中的键。但是，您可以覆盖 `hashCode` getter 以生成自定义哈希码。如果您这样做，您可能还想覆盖 `==` 运算符。相等的对象（通过 `==` ）必须具有相同的哈希码。哈希码不必是唯一的，但应该分布均匀。

*tips_and_updates* **Tip:** To consistently and easily implement the `hashCode` getter, consider using the static hashing methods provided by the `Object` class.
提示：为了始终如一且轻松地实现 `hashCode` getter，请考虑使用 `Object` 类提供的静态哈希方法。

To generate a single hash code for multiple properties of an object, you can use [`Object.hash()`](https://api.dart.dev/stable/dart-core/Object/hash.html). To generate a hash code for a collection, you can use either [`Object.hashAll()`](https://api.dart.dev/stable/dart-core/Object/hashAll.html) (if element order matters) or [`Object.hashAllUnordered()`](https://api.dart.dev/stable/dart-core/Object/hashAllUnordered.html).
要为对象的多个属性生成单个哈希码，可以使用 `Object.hash()` 。要为集合生成哈希码，可以使用 `Object.hashAll()` （如果元素顺序很重要）或 `Object.hashAllUnordered()` 。

```dart
class Person {
  final String firstName, lastName;

  Person(this.firstName, this.lastName);

  // Override hashCode using the static hashing methods
  // provided by the `Object` class.
  @override
  int get hashCode => Object.hash(firstName, lastName);

  // You should generally implement operator `==` if you
  // override `hashCode`.
  @override
  bool operator ==(Object other) {
    return other is Person &&
        other.firstName == firstName &&
        other.lastName == lastName;
  }
}

void main() {
  var p1 = Person('Bob', 'Smith');
  var p2 = Person('Bob', 'Smith');
  var p3 = 'not a person';
  assert(p1.hashCode == p2.hashCode);
  assert(p1 == p2);
  assert(p1 != p3);
}
```

#### Iteration 迭代

The [Iterable](https://api.dart.dev/stable/dart-core/Iterable-class.html) and [Iterator](https://api.dart.dev/stable/dart-core/Iterator-class.html) classes support sequential access to a collection of values. To practice using these collections, follow the [Iterable collections codelab](https://dart.dev/codelabs/iterables).
Iterable 和 Iterator 类支持对值集合进行顺序访问。要练习使用这些集合，请按照 Iterable 集合代码实验室进行操作。

If you create a class that can provide Iterators for use in for-in loops, extend (if possible) or implement Iterable. Implement Iterator to define the actual iteration ability.
如果您创建了一个可以为 for-in 循环提供 Iterator 的类，请扩展（如果可能）或实现 Iterable。实现 Iterator 以定义实际的迭代能力。

```dart
class Process {
  // Represents a process...
}

class ProcessIterator implements Iterator<Process> {
  @override
  Process get current => ...
  @override
  bool moveNext() => ...
}

// A mythical class that lets you iterate through all
// processes. Extends a subclass of [Iterable].
class Processes extends IterableBase<Process> {
  @override
  final Iterator<Process> iterator = ProcessIterator();
}

void main() {
  // Iterable objects can be used with for-in.
  for (final process in Processes()) {
    // Do something with the process.
  }
}
```

### Exceptions 异常

The Dart core library defines many common exceptions and errors. Exceptions are considered conditions that you can plan ahead for and catch. Errors are conditions that you don’t expect or plan for.
Dart 核心库定义了许多常见的异常和错误。异常被视为您可以提前计划并捕获的条件。错误是您不期望或不计划的条件。

A couple of the most common errors are:
最常见的错误包括：

- [NoSuchMethodError](https://api.dart.dev/stable/dart-core/NoSuchMethodError-class.html)

  Thrown when a receiving object (which might be null) does not implement a method. 当接收对象（可能为 null）未实现方法时引发。

- [ArgumentError](https://api.dart.dev/stable/dart-core/ArgumentError-class.html)

  Can be thrown by a method that encounters an unexpected argument. 可能会由遇到意外参数的方法引发。

Throwing an application-specific exception is a common way to indicate that an error has occurred. You can define a custom exception by implementing the Exception interface:
引发特定于应用程序的异常是指示发生错误的常用方法。您可以通过实现 Exception 接口来定义自定义异常：

```dart
class FooException implements Exception {
  final String? msg;

  const FooException([this.msg]);

  @override
  String toString() => msg ?? 'FooException';
}
```

For more information, see [Exceptions](https://dart.dev/language/error-handling#exceptions) (in the language tour) and the [Exception API reference.](https://api.dart.dev/stable/dart-core/Exception-class.html)
有关更多信息，请参阅异常（在语言游览中）和 Exception API 参考。

### Weak references and finalizers 弱引用和终结器

Dart is a [garbage-collected](https://medium.com/flutter/flutter-dont-fear-the-garbage-collector-d69b3ff1ca30) language, which means that any Dart object that isn’t referenced can be disposed by the garbage collector. This default behavior might not be desirable in some scenarios involving native resources or if the target object can’t be modified.
Dart 是一种垃圾回收语言，这意味着任何未引用的 Dart 对象都可以由垃圾回收器处理。在涉及本机资源或无法修改目标对象的一些场景中，这种默认行为可能不理想。

A [WeakReference](https://api.dart.dev/stable/dart-core/WeakReference-class.html) stores a reference to the target object that does not affect how it is collected by the garbage collector. Another option is to use an [Expando](https://api.dart.dev/stable/dart-core/Expando-class.html) to add properties to an object.
WeakReference 存储对目标对象的引用，该引用不影响垃圾回收器如何收集它。另一种选择是使用 Expando 向对象添加属性。

A [Finalizer](https://api.dart.dev/stable/dart-core/Finalizer-class.html) can be used to execute a callback function after an object is no longer referenced. However, it is not guaranteed to execute this callback.
Finalizer 可用于在不再引用对象后执行回调函数。但是，无法保证执行此回调。

A [NativeFinalizer](https://api.dart.dev/stable/dart-ffi/NativeFinalizer-class.html) provides stronger guarantees for interacting with native code using [dart:ffi](https://dart.dev/guides/libraries/c-interop); its callback is invoked at least once after the object is no longer referenced. Also, it can be used to close native resources such as a database connection or open files.
NativeFinalizer 提供了更强的保证，可使用 dart:ffi 与本机代码进行交互；在不再引用对象后，至少会调用一次其回调。此外，它可用于关闭本机资源，例如数据库连接或打开的文件。

To ensure that an object won’t be garbage collected and finalized too early, classes can implement the [Finalizable](https://api.dart.dev/stable/dart-ffi/Finalizable-class.html) interface. When a local variable is Finalizable, it won’t be garbage collected until the code block where it is declared has exited.
为了确保不会过早地对对象进行垃圾回收和最终处理，类可以实现 Finalizable 接口。当局部变量为 Finalizable 时，在声明它的代码块退出之前，不会对它进行垃圾回收。

*merge_type* **Version note:** Support for weak references and finalizers was added in Dart 2.17.
版本说明：Dart 2.17 中添加了对弱引用和终结器的支持。
