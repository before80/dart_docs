+++
title = "记录"
date = 2024-01-05T20:29:36+08:00
weight = 10
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文: [https://dart.dev/language/records](https://dart.dev/language/records)

## Records 记录

*merge_type* **Version note:** Records require a [language version](https://dart.dev/guides/language/evolution#language-versioning) of at least 3.0.

​	版本说明：记录至少需要 3.0 的语言版本。

Records are an anonymous, immutable, aggregate type. Like other [collection types](https://dart.dev/language/collections), they let you bundle multiple objects into a single object. Unlike other collection types, records are fixed-sized, heterogeneous, and typed.

​	记录是一种匿名、不可变的聚合类型。与其他集合类型一样，它们允许您将多个对象捆绑到单个对象中。与其他集合类型不同，记录是固定大小的、异构的和类型的。

Records are real values; you can store them in variables, nest them, pass them to and from functions, and store them in data structures such as lists, maps, and sets.

​	记录是真实值；您可以将它们存储在变量中、嵌套它们、将它们传递到函数并从函数中传递它们，并将它们存储在诸如列表、映射和集合等数据结构中。

## 记录语法 Record syntax 

*Records expressions* are comma-delimited lists of named or positional fields, enclosed in parentheses:

​	记录表达式是逗号分隔的命名或位置字段列表，用括号括起来：

```dart
var record = ('first', a: 2, b: true, 'last');
```

*Record type annotations* are comma-delimited lists of types enclosed in parentheses. You can use record type annotations to define return types and parameter types. For example, the following `(int, int)` statements are record type annotations:

​	记录类型注释是逗号分隔的类型列表，用括号括起来。您可以使用记录类型注释来定义返回类型和参数类型。例如，以下 `(int, int)` 语句是记录类型注释：

```dart
(int, int) swap((int, int) record) {
  var (a, b) = record;
  return (b, a);
}
```

Fields in record expressions and type annotations mirror how [parameters and arguments](https://dart.dev/language/functions#parameters) work in functions. Positional fields go directly inside the parentheses:

​	记录表达式和类型注释中的字段反映了函数中参数和参数的工作方式。位置字段直接放在括号内：

```dart
// Record type annotation in a variable declaration:
(String, int) record;

// Initialize it with a record expression:
record = ('A string', 123);
```

In a record type annotation, named fields go inside a curly brace-delimited section of type-and-name pairs, after all positional fields. In a record expression, the names go before each field value with a colon after:

​	在记录类型注释中，命名字段位于所有位置字段之后，位于用大括号分隔的类型和名称对部分内。在记录表达式中，名称位于每个字段值之前，后面带冒号：

```dart
// Record type annotation in a variable declaration:
({int a, bool b}) record;

// Initialize it with a record expression:
record = (a: 123, b: true);
```

The names of named fields in a record type are part of the [record’s type definition](https://dart.dev/language/records#record-types), or its *shape*. Two records with named fields with different names have different types:

​	记录类型中命名字段的名称是记录的类型定义或其形状的一部分。具有不同名称的命名字段的两个记录具有不同的类型：

```dart
({int a, int b}) recordAB = (a: 1, b: 2);
({int x, int y}) recordXY = (x: 3, y: 4);

// Compile error! These records don't have the same type.
// recordAB = recordXY;
```

In a record type annotation, you can also name the *positional* fields, but these names are purely for documentation and don’t affect the record’s type:

​	在记录类型注释中，您还可以命名位置字段，但这些名称纯粹用于文档，不会影响记录的类型：

```dart
(int a, int b) recordAB = (1, 2);
(int x, int y) recordXY = (3, 4);

recordAB = recordXY; // OK.
```

This is similar to how positional parameters in a function declaration or function typedef can have names but those names don’t affect the signature of the function.

​	这类似于函数声明或函数 typedef 中的位置参数可以具有名称，但这些名称不会影响函数的签名。

For more information and examples, check out [Record types](https://dart.dev/language/records#record-types) and [Record equality](https://dart.dev/language/records#record-equality).

​	有关更多信息和示例，请查看记录类型和记录相等性。

## 记录字段 Record fields 

Record fields are accessible through built-in getters. Records are immutable, so fields do not have setters.

​	记录字段可通过内置的 getter 访问。记录是不可变的，因此字段没有 setter。

Named fields expose getters of the same name. Positional fields expose getters of the name `$<position>`, skipping named fields:

​	命名字段公开同名 getter。位置字段公开名称为 `$<position>` 的 getter，跳过命名字段：

```dart
var record = ('first', a: 2, b: true, 'last');

print(record.$1); // Prints 'first'
print(record.a); // Prints 2
print(record.b); // Prints true
print(record.$2); // Prints 'last'
```

To streamline record field access even more, check out the page on [Patterns](https://dart.dev/language/patterns#destructuring-multiple-returns).

​	要进一步简化记录字段访问，请查看模式页面。

## 记录类型 Record types 

There is no type declaration for individual record types. Records are structurally typed based on the types of their fields. A record’s *shape* (the set of its fields, the fields’ types, and their names, if any) uniquely determines the type of a record.

​	对于各个记录类型，没有类型声明。记录根据其字段的类型进行结构化类型化。记录的形状（其字段的集合、字段的类型及其名称（如果有））唯一地确定了记录的类型。

Each field in a record has its own type. Field types can differ within the same record. The type system is aware of each field’s type wherever it is accessed from the record:

​	记录中的每个字段都有自己的类型。字段类型在同一记录中可能不同。类型系统知道从记录访问的每个字段的类型：

```dart
(num, Object) pair = (42, 'a');

var first = pair.$1; // Static type `num`, runtime type `int`.
var second = pair.$2; // Static type `Object`, runtime type `String`.
```

Consider two unrelated libraries that create records with the same set of fields. The type system understands that those records are the same type even though the libraries are not coupled to each other.

​	考虑两个创建具有相同字段集的记录的不相关库。即使库彼此不耦合，类型系统也会理解这些记录是相同的类型。

## 记录相等性 Record equality 

Two records are equal if they have the same *shape* (set of fields), and their corresponding fields have the same values. Since named field *order* is not part of a record’s shape, the order of named fields does not affect equality.

​	如果两个记录具有相同的形状（字段集），并且其相应字段具有相同的值，则这两个记录相等。由于命名字段顺序不是记录形状的一部分，因此命名字段的顺序不会影响相等性。

For example:

​	例如：

```dart
(int x, int y, int z) point = (1, 2, 3);
(int r, int g, int b) color = (1, 2, 3);

print(point == color); // Prints 'true'.
({int x, int y, int z}) point = (x: 1, y: 2, z: 3);
({int r, int g, int b}) color = (r: 1, g: 2, b: 3);

print(point == color); // Prints 'false'. Lint: Equals on unrelated types.
```

Records automatically define `hashCode` and `==` methods based on the structure of their fields.

​	记录根据其字段的结构自动定义 `hashCode` 和 `==` 方法。

## 多个返回值 Multiple returns 

Records allow functions to return multiple values bundled together. To retrieve record values from a return, destructure the values into local variables using [pattern matching](https://dart.dev/language/patterns#destructuring-multiple-returns).

​	记录允许函数返回捆绑在一起的多个值。要从返回值中检索记录值，请使用模式匹配将值解构为局部变量。

```dart
// Returns multiple values in a record:
(String, int) userInfo(Map<String, dynamic> json) {
  return (json['name'] as String, json['age'] as int);
}

final json = <String, dynamic>{
  'name': 'Dash',
  'age': 10,
  'color': 'blue',
};

// Destructures using a record pattern:
var (name, age) = userInfo(json);

/* Equivalent to:
  var info = userInfo(json);
  var name = info.$1;
  var age  = info.$2;
*/
```

You can return multiple values from a function without records, but other methods come with downsides. For example, creating a class is much more verbose, and using other collection types like `List` or `Map` loses type safety.

​	您可以从没有记录的函数返回多个值，但其他方法会带来负面影响。例如，创建类要冗长得多，而使用其他集合类型（如 `List` 或 `Map` ）会失去类型安全性。

*info* **Note:** Records’ multiple-return and heterogeneous-type characteristics enable parallelization of futures of different types, which you can read about in the [`dart:async` documentation](https://dart.dev/libraries/dart-async#handling-errors-for-multiple-futures).

​	注意：记录的多重返回和异构类型特性支持不同类型的期货并行化，您可以在 `dart:async` 文档中了解相关信息。
