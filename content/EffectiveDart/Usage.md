+++
title = "有效的 Dart：用法"
date = 2024-01-05T20:29:36+08:00
weight = 30
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文: [https://dart.dev/effective-dart/usage](https://dart.dev/effective-dart/usage)

## Effective Dart: Usage 有效的 Dart：用法

You can use these guidelines every day in the bodies of your Dart code. *Users* of your library may not be able to tell that you’ve internalized the ideas here, but *maintainers* of it sure will.
您可以在 Dart 代码的主体中每天使用这些准则。库的用户可能无法分辨您是否已内化了此处的想法，但维护人员肯定能分辨出来。

## Libraries 库

These guidelines help you compose your program out of multiple files in a consistent, maintainable way. To keep these guidelines brief, they use “import” to cover `import` and `export` directives. The guidelines apply equally to both.
这些准则可帮助您以一致、可维护的方式将程序组合成多个文件。为了使这些准则简明扼要，它们使用“import”来涵盖 `import` 和 `export` 指令。这些准则同样适用于两者。

### DO use strings in `part of` directives 在 `part of` 指令中使用字符串

Linter rule: [use_string_in_part_of_directives](https://dart.dev/tools/linter-rules/use_string_in_part_of_directives)
Linter 规则：use_string_in_part_of_directives

Many Dart developers avoid using `part` entirely. They find it easier to reason about their code when each library is a single file. If you do choose to use `part` to split part of a library out into another file, Dart requires the other file to in turn indicate which library it’s a part of.
许多 Dart 开发人员完全避免使用 `part` 。当每个库都是一个单独的文件时，他们发现更容易理解其代码。如果您确实选择使用 `part` 将库的一部分拆分为另一个文件，Dart 要求另一个文件反过来指明它属于哪个库。

Dart allows the `part of` directive to use the *name* of a library. Naming libraries is a legacy feature that is now [discouraged](https://dart.dev/effective-dart/style#dont-explicitly-name-libraries). Library names can introduce ambiguity when determining which library a part belongs to.
Dart 允许 `part of` 指令使用库的名称。命名库是一项不推荐使用的旧功能。在确定某个部分属于哪个库时，库名称可能会引起歧义。

The preferred syntax is to use a URI string that points directly to the library file. If you have some library, `my_library.dart`, that contains:
首选语法是使用直接指向库文件的 URI 字符串。如果您有一些库， `my_library.dart` ，其中包含：

```dart
library my_library;

part 'some/other/file.dart';
```

Then the part file should use the library file’s URI string:
那么部分文件应使用库文件的 URI 字符串：

```dart
part of '../../my_library.dart';
```

Not the library name:
而不是库名称：

```dart
part of my_library;
```

### DON’T import libraries that are inside the `src` directory of another package 不要导入另一个软件包的 `src` 目录中的库

Linter rule: [implementation_imports](https://dart.dev/tools/linter-rules/implementation_imports)
Linter 规则：implementation_imports

The `src` directory under `lib` [is specified](https://dart.dev/tools/pub/package-layout) to contain libraries private to the package’s own implementation. The way package maintainers version their package takes this convention into account. They are free to make sweeping changes to code under `src` without it being a breaking change to the package.
在 `lib` 下的 `src` 目录被指定为包含软件包自己的实现专用的库。软件包维护者对软件包进行版本控制的方式考虑了此约定。他们可以自由地对 `src` 下的代码进行彻底的更改，而不会导致软件包发生重大更改。

That means that if you import some other package’s private library, a minor, theoretically non-breaking point release of that package could break your code.
这意味着，如果您导入其他一些软件包的专用库，则该软件包的次要理论上不会中断的重点版本可能会破坏您的代码。

### DON’T allow an import path to reach into or out of `lib` 不要允许导入路径到达或离开 `lib`

Linter rule: [avoid_relative_lib_imports](https://dart.dev/tools/linter-rules/avoid_relative_lib_imports)
Linter 规则：avoid_relative_lib_imports

A `package:` import lets you access a library inside a package’s `lib` directory without having to worry about where the package is stored on your computer. For this to work, you cannot have imports that require the `lib` to be in some location on disk relative to other files. In other words, a relative import path in a file inside `lib` can’t reach out and access a file outside of the `lib` directory, and a library outside of `lib` can’t use a relative path to reach into the `lib` directory. Doing either leads to confusing errors and broken programs.
通过 `package:` 导入，您可以访问软件包 `lib` 目录中的库，而无需担心该软件包存储在计算机上的位置。为此，您不能有需要 `lib` 相对于其他文件位于磁盘上某个位置的导入。换句话说， `lib` 中的文件中的相对导入路径无法访问 `lib` 目录之外的文件，而 `lib` 之外的库无法使用相对路径访问 `lib` 目录。执行任一操作都会导致令人困惑的错误和程序崩溃。

For example, say your directory structure looks like this:
例如，假设你的目录结构如下所示：

```
my_package
└─ lib
   └─ api.dart
   test
   └─ api_test.dart
```

And say `api_test.dart` imports `api.dart` in two ways:
并且假设 `api_test.dart` 以两种方式导入 `api.dart` ：

```
import 'package:my_package/api.dart';
import '../lib/api.dart';
```

Dart thinks those are imports of two completely unrelated libraries. To avoid confusing Dart and yourself, follow these two rules:
Dart 认为这些是两个完全不相关的库的导入。为了避免让 Dart 和你自己感到困惑，请遵循以下两条规则：

- Don’t use `/lib/` in import paths.
  不要在导入路径中使用 `/lib/` 。
- Don’t use `../` to escape the `lib` directory.
  不要使用 `../` 来转义 `lib` 目录。

Instead, when you need to reach into a package’s `lib` directory (even from the same package’s `test` directory or any other top-level directory), use a `package:` import.
相反，当你需要访问包的 `lib` 目录时（即使是从同一个包的 `test` 目录或任何其他顶级目录），请使用 `package:` 导入。

```
import 'package:my_package/api.dart';
```

A package should never reach *out* of its `lib` directory and import libraries from other places in the package.
包绝不应超出其 `lib` 目录，并从包中的其他位置导入库。

### PREFER relative import paths 首选相对导入路径

Linter rule: [prefer_relative_imports](https://dart.dev/tools/linter-rules/prefer_relative_imports)
Linter 规则：prefer_relative_imports

Whenever the previous rule doesn’t come into play, follow this one. When an import does *not* reach across `lib`, prefer using relative imports. They’re shorter. For example, say your directory structure looks like this:
只要前一条规则不起作用，请遵循这一条。当导入不会跨越 `lib` 时，最好使用相对导入。它们更短。例如，假设你的目录结构如下所示：

```
my_package
└─ lib
   ├─ src
   │  └─ stuff.dart
   │  └─ utils.dart
   └─ api.dart
   test
   │─ api_test.dart
   └─ test_utils.dart
```

Here is how the various libraries should import each other:
各种库应如何相互导入：

**lib/api.dart:
lib/api.dart：**

```
import 'src/stuff.dart';
import 'src/utils.dart';
```

**lib/src/utils.dart:
lib/src/utils.dart：**

```
import '../api.dart';
import 'stuff.dart';
```

**test/api_test.dart:
test/api_test.dart：**

```
import 'package:my_package/api.dart'; // Don't reach into 'lib'.

import 'test_utils.dart'; // Relative within 'test' is fine.
```

## Null

### DON’T explicitly initialize variables to `null` 不要显式将变量初始化为 `null`

Linter rule: [avoid_init_to_null](https://dart.dev/tools/linter-rules/avoid_init_to_null)
Linter 规则：avoid_init_to_null

If a variable has a non-nullable type, Dart reports a compile error if you try to use it before it has been definitely initialized. If the variable is nullable, then it is implicitly initialized to `null` for you. There’s no concept of “uninitialized memory” in Dart and no need to explicitly initialize a variable to `null` to be “safe”.
如果变量具有非空类型，则在尝试在明确初始化之前使用它时，Dart 会报告一个编译错误。如果变量可为空，则会隐式将其初始化为 `null` 。Dart 中没有“未初始化内存”的概念，也不需要显式将变量初始化为 `null` 以确保“安全”。

```dart
Item? bestDeal(List<Item> cart) {
  Item? bestItem;

  for (final item in cart) {
    if (bestItem == null || item.price < bestItem.price) {
      bestItem = item;
    }
  }

  return bestItem;
}
Item? bestDeal(List<Item> cart) {
  Item? bestItem = null;

  for (final item in cart) {
    if (bestItem == null || item.price < bestItem.price) {
      bestItem = item;
    }
  }

  return bestItem;
}
```

### DON’T use an explicit default value of `null` 不要使用显式默认值 `null`

Linter rule: [avoid_init_to_null](https://dart.dev/tools/linter-rules/avoid_init_to_null)
Linter 规则：avoid_init_to_null

If you make a nullable parameter optional but don’t give it a default value, the language implicitly uses `null` as the default, so there’s no need to write it.
如果将可为空参数设为可选但未为其提供默认值，则该语言会隐式使用 `null` 作为默认值，因此无需编写它。

```dart
void error([String? message]) {
  stderr.write(message ?? '\n');
}
void error([String? message = null]) {
  stderr.write(message ?? '\n');
}
```



### DON’T use `true` or `false` in equality operations 在相等运算中不要使用 `true` 或 `false`

Using the equality operator to evaluate a *non-nullable* boolean expression against a boolean literal is redundant. It’s always simpler to eliminate the equality operator, and use the unary negation operator `!` if necessary:
使用相等运算符来评估非空布尔表达式与布尔文字之间的关系是多余的。消除相等运算符并根据需要使用一元否定运算符 `!` 总是更简单：

```dart
if (nonNullableBool) { ... }

if (!nonNullableBool) { ... }
if (nonNullableBool == true) { ... }

if (nonNullableBool == false) { ... }
```

To evaluate a boolean expression that *is nullable*, you should use `??` or an explicit `!= null` check.
要评估一个可为空的布尔表达式，您应该使用 `??` 或显式 `!= null` 检查。

```dart
// If you want null to result in false:
if (nullableBool ?? false) { ... }

// If you want null to result in false
// and you want the variable to type promote:
if (nullableBool != null && nullableBool) { ... }
// Static error if null:
if (nullableBool) { ... }

// If you want null to be false:
if (nullableBool == true) { ... }
```

`nullableBool == true` is a viable expression, but shouldn’t be used for several reasons:
`nullableBool == true` 是一个可行的表达式，但出于以下几个原因不应使用：

- It doesn’t indicate the code has anything to do with `null`.
  它没有表明代码与 `null` 有任何关系。
- Because it’s not evidently `null` related, it can easily be mistaken for the non-nullable case, where the equality operator is redundant and can be removed. That’s only true when the boolean expression on the left has no chance of producing null, but not when it can.
  因为它显然与 `null` 无关，所以很容易将其误认为是不可为空的情况，在这种情况下，相等运算符是多余的，可以将其删除。只有当左侧的布尔表达式没有机会生成 null 时，这才是正确的，但当它可以生成 null 时，就不是这样了。
- The boolean logic is confusing. If `nullableBool` is null, then `nullableBool == true` means the condition evaluates to `false`.
  布尔逻辑令人困惑。如果 `nullableBool` 为 null，则 `nullableBool == true` 表示条件评估为 `false` 。

The `??` operator makes it clear that something to do with null is happening, so it won’t be mistaken for a redundant operation. The logic is much clearer too; the result of the expression being `null` is the same as the boolean literal.
`??` 运算符清楚地表明与 null 有关的事情正在发生，因此不会将其误认为是多余的操作。逻辑也更加清晰；表达式结果为 `null` 与布尔文字相同。

Using a null-aware operator such as `??` on a variable inside a condition doesn’t promote the variable to a non-nullable type. If you want the variable to be promoted inside the body of the `if` statement, it’s better to use an explicit `!= null` check instead of `??`.
在条件中对变量使用空感知运算符（例如 `??` ）不会将变量提升为非空类型。如果希望在 `if` 语句的主体中提升变量，最好使用显式 `!= null` 检查，而不是 `??` 。

### AVOID `late` variables if you need to check whether they are initialized 如果需要检查 `late` 变量是否已初始化，请避免使用

Dart offers no way to tell if a `late` variable has been initialized or assigned to. If you access it, it either immediately runs the initializer (if it has one) or throws an exception. Sometimes you have some state that’s lazily initialized where `late` might be a good fit, but you also need to be able to *tell* if the initialization has happened yet.
Dart 无法判断 `late` 变量是否已初始化或已赋值。如果访问该变量，它会立即运行初始化程序（如果有）或引发异常。有时，您有一些延迟初始化的状态，其中 `late` 可能很合适，但您还需要能够判断是否已完成初始化。

Although you could detect initialization by storing the state in a `late` variable and having a separate boolean field that tracks whether the variable has been set, that’s redundant because Dart *internally* maintains the initialized status of the `late` variable. Instead, it’s usually clearer to make the variable non-`late` and nullable. Then you can see if the variable has been initialized by checking for `null`.
虽然您可以通过将状态存储在 `late` 变量中并使用单独的布尔字段来跟踪变量是否已设置来检测初始化，但这很冗余，因为 Dart 在内部维护 `late` 变量的初始化状态。相反，通常更清楚的做法是使变量为非 `late` 且可空。然后，您可以通过检查 `null` 来查看变量是否已初始化。

Of course, if `null` is a valid initialized value for the variable, then it probably does make sense to have a separate boolean field.
当然，如果 `null` 是变量的有效初始化值，那么单独设置一个布尔字段可能是有意义的。

### CONSIDER assigning a nullable field to a local variable to enable type promotion 考虑将可空字段赋值给局部变量以启用类型提升

Checking that a nullable variable is not equal to `null` promotes the variable to a non-nullable type. That lets you access members on the variable and pass it to functions expecting a non-nullable type.
检查可空变量是否不等于 `null` 会将变量提升为不可空类型。这样，您就可以访问变量上的成员并将其传递给需要不可空类型的函数。

Type promotion is only supported, however, for local variables, parameters, and private final fields. Values that are open to manipulation [can’t be type promoted](https://dart.dev/tools/non-promotion-reasons).
但是，仅对局部变量、参数和私有 final 字段支持类型提升。不能对可操作的值进行类型提升。

Declaring members [private](https://dart.dev/effective-dart/design#prefer-making-declarations-private) and [final](https://dart.dev/effective-dart/design#prefer-making-fields-and-top-level-variables-final), as we generally recommend, is often enough to bypass these limitations. But, that’s not always an option. One pattern to work around this is to assign the field’s value to a local variable. Null checks on that variable will promote, so you can safely treat it as non-nullable.
按照我们通常建议的那样，将成员声明为私有且 final 通常足以绕过这些限制。但是，这并不总是可行的。一种解决此问题的模式是将字段的值分配给局部变量。对该变量进行空检查会进行提升，因此您可以安全地将其视为不可空。

```dart
class UploadException {
  final Response? response;

  UploadException([this.response]);

  @override
  String toString() {
    final response = this.response;
    if (response != null) {
      return 'Could not complete upload to ${response.url} '
          '(error code ${response.errorCode}): ${response.reason}.';
    }

    return 'Could not upload (no response).';
  }
}
```

Assigning to a local variable can be cleaner and safer than using `!` every time you need to treat the value as non-null:
在每次需要将值视为非空时，分配给局部变量可能比每次都使用 `!` 更简洁、更安全：

```dart
class UploadException {
  final Response? response;

  UploadException([this.response]);

  @override
  String toString() {
    if (response != null) {
      return 'Could not complete upload to ${response!.url} '
          '(error code ${response!.errorCode}): ${response!.reason}.';
    }

    return 'Could not upload (no response).';
  }
}
```

Be careful when using a local variable. If you need to write back to the field, make sure that you don’t write back to the local variable instead. (Making the local variable [`final`](https://dart.dev/effective-dart/usage#do-follow-a-consistent-rule-for-var-and-final-on-local-variables) can prevent such mistakes.) Also, if the field might change while the local is still in scope, then the local might have a stale value. Sometimes it’s best to simply [use `!`](https://dart.dev/null-safety/understanding-null-safety#non-null-assertion-operator) on the field.
在使用局部变量时要小心。如果您需要写回字段，请确保您没有写回局部变量。（使局部变量 `final` 可以防止此类错误。）此外，如果字段在局部变量仍在作用域内时可能发生变化，则局部变量可能具有陈旧的值。有时，最好直接在字段上使用 `!` 。

## Strings 字符串

Here are some best practices to keep in mind when composing strings in Dart.
以下是 Dart 中编写字符串时需要牢记的一些最佳做法。

### DO use adjacent strings to concatenate string literals 使用相邻字符串连接字符串文字

Linter rule: [prefer_adjacent_string_concatenation](https://dart.dev/tools/linter-rules/prefer_adjacent_string_concatenation)
Linter 规则：prefer_adjacent_string_concatenation

If you have two string literals—not values, but the actual quoted literal form—you do not need to use `+` to concatenate them. Just like in C and C++, simply placing them next to each other does it. This is a good way to make a single long string that doesn’t fit on one line.
如果您有两个字符串文字（不是值，而是实际的带引号的文字形式），则无需使用 `+` 连接它们。就像在 C 和 C++ 中一样，只需将它们放在彼此旁边即可。这是制作不适合一行显示的单个长字符串的好方法。

```dart
raiseAlarm('ERROR: Parts of the spaceship are on fire. Other '
    'parts are overrun by martians. Unclear which are which.');
raiseAlarm('ERROR: Parts of the spaceship are on fire. Other ' +
    'parts are overrun by martians. Unclear which are which.');
```

### PREFER using interpolation to compose strings and values 首选使用插值来组合字符串和值

Linter rule: [prefer_interpolation_to_compose_strings](https://dart.dev/tools/linter-rules/prefer_interpolation_to_compose_strings)
Linter 规则：prefer_interpolation_to_compose_strings

If you’re coming from other languages, you’re used to using long chains of `+` to build a string out of literals and other values. That does work in Dart, but it’s almost always cleaner and shorter to use interpolation:
如果您来自其他语言，您习惯于使用长链 `+` 从文字和其他值构建字符串。这在 Dart 中确实可行，但使用插值几乎总是更简洁、更短：

```dart
'Hello, $name! You are ${year - birth} years old.';
'Hello, ' + name + '! You are ' + (year - birth).toString() + ' y...';
```

Note that this guideline applies to combining *multiple* literals and values. It’s fine to use `.toString()` when converting only a single object to a string.
请注意，此准则适用于组合多个文字和值。仅将单个对象转换为字符串时，可以使用 `.toString()` 。

### AVOID using curly braces in interpolation when not needed 在不必要时避免在插值中使用大括号

Linter rule: [unnecessary_brace_in_string_interps](https://dart.dev/tools/linter-rules/unnecessary_brace_in_string_interps)
Linter 规则：unnecessary_brace_in_string_interps

If you’re interpolating a simple identifier not immediately followed by more alphanumeric text, the `{}` should be omitted.
如果您正在插值一个简单的标识符，其后没有紧跟更多字母数字文本，则应省略 `{}` 。

```dart
var greeting = 'Hi, $name! I love your ${decade}s costume.';
var greeting = 'Hi, ${name}! I love your ${decade}s costume.';
```

## Collections 集合

Out of the box, Dart supports four collection types: lists, maps, queues, and sets. The following best practices apply to collections.
开箱即用，Dart 支持四种集合类型：列表、映射、队列和集合。以下最佳做法适用于集合。

### DO use collection literals when possible 尽可能使用集合文字

Linter rule: [prefer_collection_literals](https://dart.dev/tools/linter-rules/prefer_collection_literals)
Linter 规则：prefer_collection_literals

Dart has three core collection types: List, Map, and Set. The Map and Set classes have unnamed constructors like most classes do. But because these collections are used so frequently, Dart has nicer built-in syntax for creating them:
Dart 有三种核心集合类型：List、Map 和 Set。Map 和 Set 类具有未命名构造函数，就像大多数类一样。但由于这些集合使用非常频繁，因此 Dart 提供了更友好的内置语法来创建它们：

```dart
var points = <Point>[];
var addresses = <String, Address>{};
var counts = <int>{};
var addresses = Map<String, Address>();
var counts = Set<int>();
```

Note that this guideline doesn’t apply to the *named* constructors for those classes. `List.from()`, `Map.fromIterable()`, and friends all have their uses. (The List class also has an unnamed constructor, but it is prohibited in null safe Dart.)
请注意，此准则不适用于这些类的命名构造函数。 `List.from()` 、 `Map.fromIterable()` 及其朋友都有各自的用途。（List 类也具有未命名构造函数，但禁止在 null 安全的 Dart 中使用。）

Collection literals are particularly powerful in Dart because they give you access to the [spread operator](https://dart.dev/language/collections#spread-operators) for including the contents of other collections, and [`if` and `for`](https://dart.dev/language/collections#control-flow-operators) for performing control flow while building the contents:
集合字面量在 Dart 中特别强大，因为它们允许您访问展开运算符以包含其他集合的内容，以及 `if` 和 `for` ，以便在构建内容时执行控制流：

```dart
var arguments = [
  ...options,
  command,
  ...?modeFlags,
  for (var path in filePaths)
    if (path.endsWith('.dart')) path.replaceAll('.dart', '.js')
];
var arguments = <String>[];
arguments.addAll(options);
arguments.add(command);
if (modeFlags != null) arguments.addAll(modeFlags);
arguments.addAll(filePaths
    .where((path) => path.endsWith('.dart'))
    .map((path) => path.replaceAll('.dart', '.js')));
```

### DON’T use `.length` to see if a collection is empty 不要使用 `.length` 来查看集合是否为空

Linter rules: [prefer_is_empty](https://dart.dev/tools/linter-rules/prefer_is_empty), [prefer_is_not_empty](https://dart.dev/tools/linter-rules/prefer_is_not_empty)
Linter 规则：prefer_is_empty、prefer_is_not_empty

The [Iterable](https://api.dart.dev/stable/dart-core/Iterable-class.html) contract does not require that a collection know its length or be able to provide it in constant time. Calling `.length` just to see if the collection contains *anything* can be painfully slow.
Iterable 契约不要求集合知道其长度或能够在恒定时间内提供长度。调用 `.length` 仅仅是为了查看集合是否包含任何内容，这可能会非常慢。

Instead, there are faster and more readable getters: `.isEmpty` and `.isNotEmpty`. Use the one that doesn’t require you to negate the result.
相反，有更快且更易读的 getter： `.isEmpty` 和 `.isNotEmpty` 。使用不需要您否定结果的那个。

```dart
if (lunchBox.isEmpty) return 'so hungry...';
if (words.isNotEmpty) return words.join(' ');
if (lunchBox.length == 0) return 'so hungry...';
if (!words.isEmpty) return words.join(' ');
```

### AVOID using `Iterable.forEach()` with a function literal 避免将 `Iterable.forEach()` 与函数字面量一起使用

Linter rule: [avoid_function_literals_in_foreach_calls](https://dart.dev/tools/linter-rules/avoid_function_literals_in_foreach_calls)
Linter 规则：avoid_function_literals_in_foreach_calls

`forEach()` functions are widely used in JavaScript because the built in `for-in` loop doesn’t do what you usually want. In Dart, if you want to iterate over a sequence, the idiomatic way to do that is using a loop.
`forEach()` 函数在 JavaScript 中被广泛使用，因为内置的 `for-in` 循环不会执行您通常想要的操作。在 Dart 中，如果您想迭代序列，惯用的方法是使用循环。

```dart
for (final person in people) {
  ...
}
people.forEach((person) {
  ...
});
```

Note that this guideline specifically says “function *literal*”. If you want to invoke some *already existing* function on each element, `forEach()` is fine.
请注意，此准则明确指出“函数字面量”。如果您想对每个元素调用某个已存在的函数，则 `forEach()` 可以。

```dart
people.forEach(print);
```

Also note that it’s always OK to use `Map.forEach()`. Maps aren’t iterable, so this guideline doesn’t apply.
还要注意，始终可以使用 `Map.forEach()` 。映射不可迭代，因此此准则不适用。

### DON’T use `List.from()` unless you intend to change the type of the result 除非您打算更改结果的类型，否则不要使用 `List.from()`

Given an Iterable, there are two obvious ways to produce a new List that contains the same elements:
给定一个 Iterable，有两种明显的方法可以生成包含相同元素的新 List：

```dart
var copy1 = iterable.toList();
var copy2 = List.from(iterable);
```

The obvious difference is that the first one is shorter. The *important* difference is that the first one preserves the type argument of the original object:
明显的区别是第一个更短。重要的区别是第一个保留了原始对象的类型参数：

```dart
// Creates a List<int>:
var iterable = [1, 2, 3];

// Prints "List<int>":
print(iterable.toList().runtimeType);
// Creates a List<int>:
var iterable = [1, 2, 3];

// Prints "List<dynamic>":
print(List.from(iterable).runtimeType);
```

If you *want* to change the type, then calling `List.from()` is useful:
如果您想更改类型，那么调用 `List.from()` 会很有用：

```dart
var numbers = [1, 2.3, 4]; // List<num>.
numbers.removeAt(1); // Now it only contains integers.
var ints = List<int>.from(numbers);
```

But if your goal is just to copy the iterable and preserve its original type, or you don’t care about the type, then use `toList()`.
但如果您的目标只是复制可迭代对象并保留其原始类型，或者您不关心类型，那么请使用 `toList()` 。

### DO use `whereType()` to filter a collection by type 使用 `whereType()` 根据类型过滤集合

Linter rule: [prefer_iterable_whereType](https://dart.dev/tools/linter-rules/prefer_iterable_whereType)
假定您有一个包含混合对象的列表，并且您只想从中获取整数。您可以像这样使用 ：

Let’s say you have a list containing a mixture of objects, and you want to get just the integers out of it. You could use `where()` like this:
这很冗长，但更糟糕的是，它返回的可迭代对象的类型可能不是您想要的。在此示例中，它返回一个 `where()` ，即使您可能想要一个 ，因为这是您对其进行过滤的类型。

```dart
var objects = [1, 'a', 2, 'b', 3];
var ints = objects.where((e) => e is int);
```

This is verbose, but, worse, it returns an iterable whose type probably isn’t what you want. In the example here, it returns an `Iterable<Object>` even though you likely want an `Iterable<int>` since that’s the type you’re filtering it to.
有时您会看到通过添加 `Iterable<Object>` 来“更正”上述错误的代码：

Sometimes you see code that “corrects” the above error by adding `cast()`:
这很冗长，并且会导致创建两个包装器，其中包含两层间接和冗余的运行时检查。幸运的是，核心库具有适用于此确切用例的 `cast()` 方法：

```dart
var objects = [1, 'a', 2, 'b', 3];
var ints = objects.where((e) => e is int).cast<int>();
```

That’s verbose and causes two wrappers to be created, with two layers of indirection and redundant runtime checking. Fortunately, the core library has the [`whereType()`](https://api.dart.dev/stable/dart-core/Iterable/whereType.html) method for this exact use case:
使用 `whereType()` 简洁明了，会生成所需类型的 Iterable，并且没有不必要的包装级别。

```dart
var objects = [1, 'a', 2, 'b', 3];
var ints = objects.whereType<int>();
```

Using `whereType()` is concise, produces an [Iterable](https://api.dart.dev/stable/dart-core/Iterable-class.html) of the desired type, and has no unnecessary levels of wrapping.

### DON’T use `cast()` when a nearby operation will do 当附近的操作可以完成时，不要使用 `cast()`

Often when you’re dealing with an iterable or stream, you perform several transformations on it. At the end, you want to produce an object with a certain type argument. Instead of tacking on a call to `cast()`, see if one of the existing transformations can change the type.
通常，当您处理可迭代对象或流时，您会对其执行多个转换。最后，您希望生成具有特定类型参数的对象。不要附加对 `cast()` 的调用，请查看现有转换之一是否可以更改类型。

If you’re already calling `toList()`, replace that with a call to [`List.from()`](https://api.dart.dev/stable/dart-core/List/List.from.html) where `T` is the type of resulting list you want.
如果您已经在调用 `toList()` ，请用对 `List<T>.from()` 的调用替换它，其中 `T` 是您想要的最终列表的类型。

```dart
var stuff = <dynamic>[1, 2];
var ints = List<int>.from(stuff);
var stuff = <dynamic>[1, 2];
var ints = stuff.toList().cast<int>();
```

If you are calling `map()`, give it an explicit type argument so that it produces an iterable of the desired type. Type inference often picks the correct type for you based on the function you pass to `map()`, but sometimes you need to be explicit.
如果您正在调用 `map()` ，请为其提供一个显式类型参数，以便它生成所需类型的可迭代对象。类型推断通常会根据您传递给 `map()` 的函数为您选择正确的类型，但有时您需要明确指定。

```dart
var stuff = <dynamic>[1, 2];
var reciprocals = stuff.map<double>((n) => 1 / n);
var stuff = <dynamic>[1, 2];
var reciprocals = stuff.map((n) => 1 / n).cast<double>();
```

### AVOID using `cast()` 避免使用 `cast()`

This is the softer generalization of the previous rule. Sometimes there is no nearby operation you can use to fix the type of some object. Even then, when possible avoid using `cast()` to “change” a collection’s type.
这是上一条规则的较弱概括。有时，没有可以用来修复某些对象类型的附近操作。即使这样，在可能的情况下，也避免使用 `cast()` 来“更改”集合的类型。

Prefer any of these options instead:
而是优先选择以下任何选项：

- **Create it with the right type.** Change the code where the collection is first created so that it has the right type.
  使用正确的类型创建它。更改集合首次创建的代码，使其具有正确的类型。

- **Cast the elements on access.** If you immediately iterate over the collection, cast each element inside the iteration.
  在访问时强制转换元素。如果您立即迭代集合，请在迭代中强制转换每个元素。

- **Eagerly cast using `List.from()`.** If you’ll eventually access most of the elements in the collection, and you don’t need the object to be backed by the original live object, convert it using `List.from()`.
  使用 `List.from()` 急切强制转换。如果您最终会访问集合中的大多数元素，并且不需要对象由原始实时对象支持，请使用 `List.from()` 进行转换。

  The `cast()` method returns a lazy collection that checks the element type on *every operation*. If you perform only a few operations on only a few elements, that laziness can be good. But in many cases, the overhead of lazy validation and of wrapping outweighs the benefits.
  `cast()` 方法返回一个惰性集合，该集合在每次操作时都会检查元素类型。如果您仅对少数元素执行少数操作，那么这种惰性可能是好的。但在许多情况下，惰性验证和包装的开销大于好处。

Here is an example of **creating it with the right type:**
以下是如何使用正确类型创建它的示例：

```dart
List<int> singletonList(int value) {
  var list = <int>[];
  list.add(value);
  return list;
}
List<int> singletonList(int value) {
  var list = []; // List<dynamic>.
  list.add(value);
  return list.cast<int>();
}
```

Here is **casting each element on access:**
以下是在访问时强制转换每个元素：

```dart
void printEvens(List<Object> objects) {
  // We happen to know the list only contains ints.
  for (final n in objects) {
    if ((n as int).isEven) print(n);
  }
}
void printEvens(List<Object> objects) {
  // We happen to know the list only contains ints.
  for (final n in objects.cast<int>()) {
    if (n.isEven) print(n);
  }
}
```

Here is **casting eagerly using `List.from()`:**
以下是如何使用 `List.from()` 急切强制转换：

```dart
int median(List<Object> objects) {
  // We happen to know the list only contains ints.
  var ints = List<int>.from(objects);
  ints.sort();
  return ints[ints.length ~/ 2];
}
int median(List<Object> objects) {
  // We happen to know the list only contains ints.
  var ints = objects.cast<int>();
  ints.sort();
  return ints[ints.length ~/ 2];
}
```

These alternatives don’t always work, of course, and sometimes `cast()` is the right answer. But consider that method a little risky and undesirable—it can be slow and may fail at runtime if you aren’t careful.
当然，这些替代方法并不总是有效，有时 `cast()` 是正确的答案。但请考虑一下这种方法有点冒险且不受欢迎——它可能会很慢，如果您不小心，可能会在运行时失败。

## Functions 函数

In Dart, even functions are objects. Here are some best practices involving functions.
在 Dart 中，即使函数也是对象。以下是一些涉及函数的最佳做法。

### DO use a function declaration to bind a function to a name 请使用函数声明将函数绑定到名称

Linter rule: [prefer_function_declarations_over_variables](https://dart.dev/tools/linter-rules/prefer_function_declarations_over_variables)
Linter 规则：prefer_function_declarations_over_variables

Modern languages have realized how useful local nested functions and closures are. It’s common to have a function defined inside another one. In many cases, this function is used as a callback immediately and doesn’t need a name. A function expression is great for that.
现代语言已经意识到本地嵌套函数和闭包有多么有用。通常在一个函数内部定义另一个函数。在许多情况下，此函数被立即用作回调，不需要名称。函数表达式非常适合这种情况。

But, if you do need to give it a name, use a function declaration statement instead of binding a lambda to a variable.
但是，如果您确实需要给它一个名称，请使用函数声明语句，而不是将 lambda 绑定到变量。

```dart
void main() {
  void localFunction() {
    ...
  }
}
void main() {
  var localFunction = () {
    ...
  };
}
```

### DON’T create a lambda when a tear-off will do 不要在可以使用 tear-off 时创建 lambda

Linter rule: [unnecessary_lambdas](https://dart.dev/tools/linter-rules/unnecessary_lambdas)
Linter 规则：unnecessary_lambdas

When you refer to a function, method, or named constructor but omit the parentheses, Dart creates a *tear-off*—a closure that takes the same parameters as the function and invokes the underlying function when you call it. If all you need is a closure that invokes a named function with the same parameters as the closure accepts, don’t manually wrap the call in a lambda.
当您引用函数、方法或命名构造函数，但省略括号时，Dart 会创建一个 tear-off，即一个与函数采用相同参数的闭包，并在您调用它时调用底层函数。如果您只需要一个闭包，它会使用与闭包接受的相同参数调用一个命名函数，请不要手动将调用包装在 lambda 中。

```dart
var charCodes = [68, 97, 114, 116];
var buffer = StringBuffer();

// Function:
charCodes.forEach(print);

// Method:
charCodes.forEach(buffer.write);

// Named constructor:
var strings = charCodes.map(String.fromCharCode);

// Unnamed constructor:
var buffers = charCodes.map(StringBuffer.new);
var charCodes = [68, 97, 114, 116];
var buffer = StringBuffer();

// Function:
charCodes.forEach((code) {
  print(code);
});

// Method:
charCodes.forEach((code) {
  buffer.write(code);
});

// Named constructor:
var strings = charCodes.map((code) => String.fromCharCode(code));

// Unnamed constructor:
var buffers = charCodes.map((code) => StringBuffer(code));
```

## Variables 变量

The following best practices describe how to best use variables in Dart.
以下最佳做法介绍了如何在 Dart 中最佳地使用变量。

### DO follow a consistent rule for `var` and `final` on local variables 请遵循本地变量的 `var` 和 `final` 的一致规则

Most local variables shouldn’t have type annotations and should be declared using just `var` or `final`. There are two rules in wide use for when to use one or the other:
大多数本地变量不应具有类型注释，而应仅使用 `var` 或 `final` 声明。对于何时使用其中之一或另一个，有两种广泛使用的规则：

- Use `final` for local variables that are not reassigned and `var` for those that are.
  对于不会重新分配的局部变量，使用 `final` ；对于会重新分配的局部变量，使用 `var` 。
- Use `var` for all local variables, even ones that aren’t reassigned. Never use `final` for locals. (Using `final` for fields and top-level variables is still encouraged, of course.)
  对所有局部变量使用 `var` ，即使是不会重新分配的变量。切勿对局部变量使用 `final` 。（当然，仍然建议对字段和顶级变量使用 `final` 。）

Either rule is acceptable, but pick *one* and apply it consistently throughout your code. That way when a reader sees `var`, they know whether it means that the variable is assigned later in the function.
两种规则都可以接受，但选择一种并在整个代码中一致应用。这样，当读者看到 `var` 时，他们就知道它是否意味着该变量在函数中稍后被分配。

### AVOID storing what you can calculate 避免存储可以计算的内容

When designing a class, you often want to expose multiple views into the same underlying state. Often you see code that calculates all of those views in the constructor and then stores them:
在设计类时，通常希望将多个视图公开到相同的基础状态。通常会看到在构造函数中计算所有这些视图然后存储它们的代码：

```dart
class Circle {
  double radius;
  double area;
  double circumference;

  Circle(double radius)
      : radius = radius,
        area = pi * radius * radius,
        circumference = pi * 2.0 * radius;
}
```

This code has two things wrong with it. First, it’s likely wasting memory. The area and circumference, strictly speaking, are *caches*. They are stored calculations that we could recalculate from other data we already have. They are trading increased memory for reduced CPU usage. Do we know we have a performance problem that merits that trade-off?
此代码有两个问题。首先，它可能会浪费内存。严格来说，面积和周长是缓存。它们是存储的计算，我们可以从已有的其他数据重新计算它们。它们用增加的内存来换取减少的 CPU 使用率。我们知道我们有一个值得进行这种权衡的性能问题吗？

Worse, the code is *wrong*. The problem with caches is *invalidation*—how do you know when the cache is out of date and needs to be recalculated? Here, we never do, even though `radius` is mutable. You can assign a different value and the `area` and `circumference` will retain their previous, now incorrect values.
更糟糕的是，代码是错误的。缓存的问题在于失效——您如何知道缓存已过期并需要重新计算？在此，我们从未执行此操作，即使 `radius` 是可变的。您可以分配一个不同的值，而 `area` 和 `circumference` 将保留其先前的、现在不正确的值。

To correctly handle cache invalidation, we would need to do this:
要正确处理缓存失效，我们需要执行以下操作：

```dart
class Circle {
  double _radius;
  double get radius => _radius;
  set radius(double value) {
    _radius = value;
    _recalculate();
  }

  double _area = 0.0;
  double get area => _area;

  double _circumference = 0.0;
  double get circumference => _circumference;

  Circle(this._radius) {
    _recalculate();
  }

  void _recalculate() {
    _area = pi * _radius * _radius;
    _circumference = pi * 2.0 * _radius;
  }
}
```

That’s an awful lot of code to write, maintain, debug, and read. Instead, your first implementation should be:
这是大量需要编写、维护、调试和阅读的代码。相反，您的第一个实现应该是：

```dart
class Circle {
  double radius;

  Circle(this.radius);

  double get area => pi * radius * radius;
  double get circumference => pi * 2.0 * radius;
}
```

This code is shorter, uses less memory, and is less error-prone. It stores the minimal amount of data needed to represent the circle. There are no fields to get out of sync because there is only a single source of truth.
此代码更短，使用更少的内存，并且不易出错。它存储表示圆所需的最小数据量。没有字段会不同步，因为只有一个真实来源。

In some cases, you may need to cache the result of a slow calculation, but only do that after you know you have a performance problem, do it carefully, and leave a comment explaining the optimization.
在某些情况下，您可能需要缓存慢速计算的结果，但仅在您知道存在性能问题后才执行此操作，请小心执行此操作，并留下解释优化的注释。

## Members 成员

In Dart, objects have members which can be functions (methods) or data (instance variables). The following best practices apply to an object’s members.
在 Dart 中，对象具有可以是函数（方法）或数据（实例变量）的成员。以下最佳做法适用于对象的成员。

### DON’T wrap a field in a getter and setter unnecessarily 不要不必要地用 getter 和 setter 包装字段

Linter rule: [unnecessary_getters_setters](https://dart.dev/tools/linter-rules/unnecessary_getters_setters)
Linter 规则：不必要的 getter 和 setter

In Java and C#, it’s common to hide all fields behind getters and setters (or properties in C#), even if the implementation just forwards to the field. That way, if you ever need to do more work in those members, you can without needing to touch the call sites. This is because calling a getter method is different than accessing a field in Java, and accessing a property isn’t binary-compatible with accessing a raw field in C#.
在 Java 和 C# 中，即使实现只是转发到字段，也通常将所有字段隐藏在 getter 和 setter（或 C# 中的属性）后面。这样，如果您以后需要在这些成员中执行更多工作，则无需接触调用站点即可完成。这是因为在 Java 中调用 getter 方法不同于访问字段，并且在 C# 中访问属性与访问原始字段在二进制上不兼容。

Dart doesn’t have this limitation. Fields and getters/setters are completely indistinguishable. You can expose a field in a class and later wrap it in a getter and setter without having to touch any code that uses that field.
Dart 没有此限制。字段和 getter/setter 完全无法区分。您可以在类中公开一个字段，然后在不接触使用该字段的任何代码的情况下，将其包装在 getter 和 setter 中。

```dart
class Box {
  Object? contents;
}
class Box {
  Object? _contents;
  Object? get contents => _contents;
  set contents(Object? value) {
    _contents = value;
  }
}
```

### PREFER using a `final` field to make a read-only property 更喜欢使用 `final` 字段来创建只读属性

If you have a field that outside code should be able to see but not assign to, a simple solution that works in many cases is to simply mark it `final`.
如果您有一个字段，外部代码应该能够看到但不能分配给该字段，那么一种在许多情况下可行的简单解决方案是简单地将其标记为 `final` 。

```dart
class Box {
  final contents = [];
}
class Box {
  Object? _contents;
  Object? get contents => _contents;
}
```

Of course, if you need to internally assign to the field outside of the constructor, you may need to do the “private field, public getter” pattern, but don’t reach for that until you need to.
当然，如果您需要在构造函数之外对字段进行内部分配，则可能需要执行“私有字段、公有 getter”模式，但不要在需要之前这样做。

### CONSIDER using `=>` for simple members 考虑对简单成员使用 `=>`

Linter rule: [prefer_expression_function_bodies](https://dart.dev/tools/linter-rules/prefer_expression_function_bodies)
Linter 规则：prefer_expression_function_bodies

In addition to using `=>` for function expressions, Dart also lets you define members with it. That style is a good fit for simple members that just calculate and return a value.
除了将 `=>` 用于函数表达式外，Dart 还允许您用它定义成员。这种风格非常适合只计算并返回一个值的简单成员。

```dart
double get area => (right - left) * (bottom - top);

String capitalize(String name) =>
    '${name[0].toUpperCase()}${name.substring(1)}';
```

People *writing* code seem to love `=>`, but it’s very easy to abuse it and end up with code that’s hard to *read*. If your declaration is more than a couple of lines or contains deeply nested expressions—cascades and conditional operators are common offenders—do yourself and everyone who has to read your code a favor and use a block body and some statements.
编写代码的人似乎很喜欢 `=>` ，但很容易滥用它，最终导致代码难以阅读。如果您的声明超过几行或包含深度嵌套的表达式（级联和条件运算符是常见的罪魁祸首），那么请为自己和所有必须阅读您的代码的人一个好处，使用块正文和一些语句。

```dart
Treasure? openChest(Chest chest, Point where) {
  if (_opened.containsKey(chest)) return null;

  var treasure = Treasure(where);
  treasure.addAll(chest.contents);
  _opened[chest] = treasure;
  return treasure;
}
Treasure? openChest(Chest chest, Point where) => _opened.containsKey(chest)
    ? null
    : _opened[chest] = (Treasure(where)..addAll(chest.contents));
```

You can also use `=>` on members that don’t return a value. This is idiomatic when a setter is small and has a corresponding getter that uses `=>`.
您还可以在不返回值的成员上使用 `=>` 。当一个 setter 很小并且有一个使用 `=>` 的相应 getter 时，这是惯用的用法。

```dart
num get x => center.x;
set x(num value) => center = Point(value, center.y);
```

### DON’T use `this.` except to redirect to a named constructor or to avoid shadowing 不要使用 `this.` ，除非是为了重定向到一个命名构造函数或避免遮蔽

Linter rule: [unnecessary_this](https://dart.dev/tools/linter-rules/unnecessary_this)
Linter 规则：unnecessary_this

JavaScript requires an explicit `this.` to refer to members on the object whose method is currently being executed, but Dart—like C++, Java, and C#—doesn’t have that limitation.
JavaScript 需要一个显式的 `this.` 来引用当前正在执行其方法的对象上的成员，但 Dart（如 C++、Java 和 C#）没有这种限制。

There are only two times you need to use `this.`. One is when a local variable with the same name shadows the member you want to access:
您只需要在两种情况下使用 `this.` 。一种情况是，当具有相同名称的局部变量遮蔽您想要访问的成员时：

```dart
class Box {
  Object? value;

  void clear() {
    this.update(null);
  }

  void update(Object? value) {
    this.value = value;
  }
}
class Box {
  Object? value;

  void clear() {
    update(null);
  }

  void update(Object? value) {
    this.value = value;
  }
}
```

The other time to use `this.` is when redirecting to a named constructor:
使用 `this.` 的另一个时机是在重定向到命名构造函数时：

```dart
class ShadeOfGray {
  final int brightness;

  ShadeOfGray(int val) : brightness = val;

  ShadeOfGray.black() : this(0);

  // This won't parse or compile!
  // ShadeOfGray.alsoBlack() : black();
}
class ShadeOfGray {
  final int brightness;

  ShadeOfGray(int val) : brightness = val;

  ShadeOfGray.black() : this(0);

  // But now it will!
  ShadeOfGray.alsoBlack() : this.black();
}
```

Note that constructor parameters never shadow fields in constructor initializer lists:
请注意，构造函数参数绝不会在构造函数初始化器列表中隐藏字段：

```dart
class Box extends BaseBox {
  Object? value;

  Box(Object? value)
      : value = value,
        super(value);
}
```

This looks surprising, but works like you want. Fortunately, code like this is relatively rare thanks to initializing formals and super initializers.
这看起来令人惊讶，但会按您希望的方式运行。幸运的是，由于初始化形式参数和超级初始化器，此类代码相对少见。

### DO initialize fields at their declaration when possible 尽可能在声明时初始化字段

If a field doesn’t depend on any constructor parameters, it can and should be initialized at its declaration. It takes less code and avoids duplication when the class has multiple constructors.
如果字段不依赖任何构造函数参数，则可以在其声明时对其进行初始化，也应该这样做。当类具有多个构造函数时，这会减少代码量并避免重复。

```dart
class ProfileMark {
  final String name;
  final DateTime start;

  ProfileMark(this.name) : start = DateTime.now();
  ProfileMark.unnamed()
      : name = '',
        start = DateTime.now();
}
class ProfileMark {
  final String name;
  final DateTime start = DateTime.now();

  ProfileMark(this.name);
  ProfileMark.unnamed() : name = '';
}
```

Some fields can’t be initialized at their declarations because they need to reference `this`—to use other fields or call methods, for example. However, if the field is marked `late`, then the initializer *can* access `this`.
某些字段无法在其声明时初始化，因为它们需要引用 `this` —例如，使用其他字段或调用方法。但是，如果字段标记为 `late` ，则初始化器可以访问 `this` 。

Of course, if a field depends on constructor parameters, or is initialized differently by different constructors, then this guideline does not apply.
当然，如果字段依赖于构造函数参数，或者由不同的构造函数以不同的方式初始化，则此准则不适用。

## Constructors 构造函数

The following best practices apply to declaring constructors for a class.
以下最佳做法适用于为类声明构造函数。

### DO use initializing formals when possible 尽可能使用初始化形式参数

Linter rule: [prefer_initializing_formals](https://dart.dev/tools/linter-rules/prefer_initializing_formals)
Linter 规则：prefer_initializing_formals

Many fields are initialized directly from a constructor parameter, like:
许多字段直接从构造函数参数初始化，例如：

```dart
class Point {
  double x, y;
  Point(double x, double y)
      : x = x,
        y = y;
}
```

We’ve got to type `x` *four* times here to define a field. We can do better:
我们必须在此处键入 `x` 四次才能定义一个字段。我们可以做得更好：

```dart
class Point {
  double x, y;
  Point(this.x, this.y);
}
```

This `this.` syntax before a constructor parameter is called an “initializing formal”. You can’t always take advantage of it. Sometimes you want to have a named parameter whose name doesn’t match the name of the field you are initializing. But when you *can* use initializing formals, you *should*.
在构造函数参数之前调用此 `this.` 语法称为“初始化形式”。您不能总是利用它。有时您希望有一个命名参数，其名称与您要初始化的字段的名称不匹配。但是，当您可以使用初始化形式时，您应该使用。

### DON’T use `late` when a constructor initializer list will do 当构造函数初始化程序列表可以执行时，不要使用 `late`

Dart requires you to initialize non-nullable fields before they can be read. Since fields can be read inside the constructor body, this means you get an error if you don’t initialize a non-nullable field before the body runs.
Dart 要求您在读取非空字段之前对其进行初始化。由于可以在构造函数体中读取字段，这意味着如果您在运行正文之前未初始化非空字段，则会收到错误。

You can make this error go away by marking the field `late`. That turns the compile-time error into a *runtime* error if you access the field before it is initialized. That’s what you need in some cases, but often the right fix is to initialize the field in the constructor initializer list:
您可以通过标记字段 `late` 来消除此错误。如果在初始化字段之前访问该字段，则会将编译时错误变为运行时错误。在某些情况下，您需要这样做，但通常正确的修复方法是在构造函数初始化程序列表中初始化字段：

```dart
class Point {
  double x, y;
  Point.polar(double theta, double radius)
      : x = cos(theta) * radius,
        y = sin(theta) * radius;
}
class Point {
  late double x, y;
  Point.polar(double theta, double radius) {
    x = cos(theta) * radius;
    y = sin(theta) * radius;
  }
}
```

The initializer list gives you access to constructor parameters and lets you initialize fields before they can be read. So, if it’s possible to use an initializer list, that’s better than making the field `late` and losing some static safety and performance.
初始化器列表让你可以访问构造函数参数，并让你在字段被读取之前对其进行初始化。因此，如果可以使用初始化器列表，那么这比创建字段 `late` 并损失一些静态安全性和性能要好。

### DO use `;` instead of `{}` for empty constructor bodies 对于空构造函数体，请使用 `;` 而不是 `{}`

Linter rule: [empty_constructor_bodies](https://dart.dev/tools/linter-rules/empty_constructor_bodies)
Linter 规则：empty_constructor_bodies

In Dart, a constructor with an empty body can be terminated with just a semicolon. (In fact, it’s required for const constructors.)
在 Dart 中，具有空体的构造函数可以用分号终止。（事实上，const 构造函数需要这样做。）

```dart
class Point {
  double x, y;
  Point(this.x, this.y);
}
class Point {
  double x, y;
  Point(this.x, this.y) {}
}
```

### DON’T use `new` 不要使用 `new`

Linter rule: [unnecessary_new](https://dart.dev/tools/linter-rules/unnecessary_new)
Linter 规则：unnecessary_new

The `new` keyword is optional when calling a constructor. Its meaning is not clear because factory constructors mean a `new` invocation may not actually return a new object.
在调用构造函数时， `new` 关键字是可选的。它的含义不清晰，因为工厂构造函数意味着 `new` 调用可能实际上不会返回一个新对象。

The language still permits `new`, but consider it deprecated and avoid using it in your code.
该语言仍然允许 `new` ，但请认为它已弃用，并避免在代码中使用它。

```dart
Widget build(BuildContext context) {
  return Row(
    children: [
      RaisedButton(
        child: Text('Increment'),
      ),
      Text('Click!'),
    ],
  );
}
Widget build(BuildContext context) {
  return new Row(
    children: [
      new RaisedButton(
        child: new Text('Increment'),
      ),
      new Text('Click!'),
    ],
  );
}
```

### DON’T use `const` redundantly 不要冗余地使用 `const`

Linter rule: [unnecessary_const](https://dart.dev/tools/linter-rules/unnecessary_const)
Linter 规则：unnecessary_const

In contexts where an expression *must* be constant, the `const` keyword is implicit, doesn’t need to be written, and shouldn’t. Those contexts are any expression inside:
在表达式必须为常量的上下文中， `const` 关键字是隐式的，不需要编写，也不应该编写。这些上下文是：

- A const collection literal.
  const 集合字面量内部。
- A const constructor call
  const 构造函数调用
- A metadata annotation.
  元数据注释。
- The initializer for a const variable declaration.
  const 变量声明的初始化程序。
- A switch case expression—the part right after `case` before the `:`, not the body of the case.
  switch case 表达式—— `case` 之后的右部分，在 `:` 之前，而不是 case 的主体。

(Default values are not included in this list because future versions of Dart may support non-const default values.)
（默认值不包含在此列表中，因为未来版本的 Dart 可能支持非 const 默认值。）

Basically, any place where it would be an error to write `new` instead of `const`, Dart allows you to omit the `const`.
基本上，在任何地方编写 `new` 而不是 `const` 会出错，Dart 都允许您省略 `const` 。

```dart
const primaryColors = [
  Color('red', [255, 0, 0]),
  Color('green', [0, 255, 0]),
  Color('blue', [0, 0, 255]),
];
const primaryColors = const [
  const Color('red', const [255, 0, 0]),
  const Color('green', const [0, 255, 0]),
  const Color('blue', const [0, 0, 255]),
];
```

## Error handling 错误处理

Dart uses exceptions when an error occurs in your program. The following best practices apply to catching and throwing exceptions.
当程序中发生错误时，Dart 会使用异常。以下最佳做法适用于捕获和抛出异常。

### AVOID catches without `on` clauses 避免使用没有 `on` 子句的 catch

Linter rule: [avoid_catches_without_on_clauses](https://dart.dev/tools/linter-rules/avoid_catches_without_on_clauses)
Linter 规则：avoid_catches_without_on_clauses

A catch clause with no `on` qualifier catches *anything* thrown by the code in the try block. [Pokémon exception handling](https://blog.codinghorror.com/new-programming-jargon/) is very likely not what you want. Does your code correctly handle [StackOverflowError](https://api.dart.dev/stable/dart-core/StackOverflowError-class.html) or [OutOfMemoryError](https://api.dart.dev/stable/dart-core/OutOfMemoryError-class.html)? If you incorrectly pass the wrong argument to a method in that try block do you want to have your debugger point you to the mistake or would you rather that helpful [ArgumentError](https://api.dart.dev/stable/dart-core/ArgumentError-class.html) get swallowed? Do you want any `assert()` statements inside that code to effectively vanish since you’re catching the thrown [AssertionError](https://api.dart.dev/stable/dart-core/AssertionError-class.html)s?
没有 `on` 限定符的 catch 子句会捕获 try 块中的代码抛出的任何内容。神奇宝贝异常处理很可能不是您想要的。您的代码是否正确处理了 StackOverflowError 或 OutOfMemoryError？如果您错误地将错误的参数传递给该 try 块中的某个方法，您希望调试器指出错误，还是希望有用的 ArgumentError 被吞掉？您是否希望该代码中的任何 `assert()` 语句有效消失，因为您正在捕获抛出的 AssertionErrors？

The answer is probably “no”, in which case you should filter the types you catch. In most cases, you should have an `on` clause that limits you to the kinds of runtime failures you are aware of and are correctly handling.
答案可能是“否”，在这种情况下，您应该过滤捕获的类型。在大多数情况下，您应该有一个 `on` 子句，将您限制为已知且正确处理的运行时故障类型。

In rare cases, you may wish to catch any runtime error. This is usually in framework or low-level code that tries to insulate arbitrary application code from causing problems. Even here, it is usually better to catch [Exception](https://api.dart.dev/stable/dart-core/Exception-class.html) than to catch all types. Exception is the base class for all *runtime* errors and excludes errors that indicate *programmatic* bugs in the code.
在极少数情况下，您可能希望捕获任何运行时错误。这通常在框架或低级代码中，该代码尝试隔离任意应用程序代码以避免造成问题。即使在此处，通常最好捕获 Exception，而不是捕获所有类型。Exception 是所有运行时错误的基类，并且不包括指示代码中编程错误的错误。

### DON’T discard errors from catches without `on` clauses 不要在没有 `on` 子句的情况下丢弃捕获的错误

If you really do feel you need to catch *everything* that can be thrown from a region of code, *do something* with what you catch. Log it, display it to the user or rethrow it, but do not silently discard it.
如果您真的觉得需要捕获从代码区域抛出的所有内容，请对捕获的内容执行某些操作。记录它、向用户显示它或重新抛出它，但不要默默地丢弃它。

### DO throw objects that implement `Error` only for programmatic errors 仅对编程错误抛出实现 `Error` 的对象

The [Error](https://api.dart.dev/stable/dart-core/Error-class.html) class is the base class for *programmatic* errors. When an object of that type or one of its subinterfaces like [ArgumentError](https://api.dart.dev/stable/dart-core/ArgumentError-class.html) is thrown, it means there is a *bug* in your code. When your API wants to report to a caller that it is being used incorrectly throwing an Error sends that signal clearly.
Error 类是编程错误的基本类。当抛出该类型或其某个子接口（如 ArgumentError）的对象时，意味着您的代码中存在错误。当您的 API 想向调用者报告其使用不正确时，抛出 Error 会清楚地发送该信号。

Conversely, if the exception is some kind of runtime failure that doesn’t indicate a bug in the code, then throwing an Error is misleading. Instead, throw one of the core Exception classes or some other type.
相反，如果异常是某种运行时故障，不表示代码中的错误，那么抛出 Error 会产生误导。相反，抛出核心 Exception 类或其他类型之一。

### DON’T explicitly catch `Error` or types that implement it 不要显式捕获 `Error` 或实现它的类型

Linter rule: [avoid_catching_errors](https://dart.dev/tools/linter-rules/avoid_catching_errors)
Linter 规则：avoid_catching_errors

This follows from the above. Since an Error indicates a bug in your code, it should unwind the entire callstack, halt the program, and print a stack trace so you can locate and fix the bug.
这源于上述内容。由于 Error 表示代码中的错误，因此它应该展开整个调用堆栈、停止程序并打印堆栈跟踪，以便您可以找到并修复错误。

Catching errors of these types breaks that process and masks the bug. Instead of *adding* error-handling code to deal with this exception after the fact, go back and fix the code that is causing it to be thrown in the first place.
捕获这些类型的错误会中断该进程并掩盖错误。不要在事后添加错误处理代码来处理此异常，而是返回并修复导致首先引发此异常的代码。

### DO use `rethrow` to rethrow a caught exception DO 使用 `rethrow` 重新抛出捕获的异常

Linter rule: [use_rethrow_when_possible](https://dart.dev/tools/linter-rules/use_rethrow_when_possible)
Linter 规则：use_rethrow_when_possible

If you decide to rethrow an exception, prefer using the `rethrow` statement instead of throwing the same exception object using `throw`. `rethrow` preserves the original stack trace of the exception. `throw` on the other hand resets the stack trace to the last thrown position.
如果您决定重新抛出异常，请使用 `rethrow` 语句，而不是使用 `throw` 抛出相同的异常对象。 `rethrow` 保留异常的原始堆栈跟踪。 `throw` 另一方面，将堆栈跟踪重置为最后抛出的位置。

```dart
try {
  somethingRisky();
} catch (e) {
  if (!canHandle(e)) throw e;
  handle(e);
}
try {
  somethingRisky();
} catch (e) {
  if (!canHandle(e)) rethrow;
  handle(e);
}
```

## Asynchrony 异步

Dart has several language features to support asynchronous programming. The following best practices apply to asynchronous coding.
Dart 有多种语言功能来支持异步编程。以下最佳做法适用于异步编码。

### PREFER async/await over using raw futures 优先使用 async/await 而非原始期货

Asynchronous code is notoriously hard to read and debug, even when using a nice abstraction like futures. The `async`/`await` syntax improves readability and lets you use all of the Dart control flow structures within your async code.
即使使用像期货这样的良好抽象，异步代码也出了名的难以阅读和调试。 `async` / `await` 语法提高了可读性，并允许您在异步代码中使用所有 Dart 控制流结构。

```dart
Future<int> countActivePlayers(String teamName) async {
  try {
    var team = await downloadTeam(teamName);
    if (team == null) return 0;

    var players = await team.roster;
    return players.where((player) => player.isActive).length;
  } catch (e) {
    log.error(e);
    return 0;
  }
}
Future<int> countActivePlayers(String teamName) {
  return downloadTeam(teamName).then((team) {
    if (team == null) return Future.value(0);

    return team.roster.then((players) {
      return players.where((player) => player.isActive).length;
    });
  }).catchError((e) {
    log.error(e);
    return 0;
  });
}
```

### DON’T use `async` when it has no useful effect 不要在 `async` 没有有用效果时使用它

It’s easy to get in the habit of using `async` on any function that does anything related to asynchrony. But in some cases, it’s extraneous. If you can omit the `async` without changing the behavior of the function, do so.
在任何与异步相关的函数上使用 `async` 很容易养成习惯。但在某些情况下，它是多余的。如果您可以在不改变函数行为的情况下省略 `async` ，请这样做。

```dart
Future<int> fastestBranch(Future<int> left, Future<int> right) {
  return Future.any([left, right]);
}
Future<int> fastestBranch(Future<int> left, Future<int> right) async {
  return Future.any([left, right]);
}
```

Cases where `async` *is* useful include:
可以使用 `async` 的情况包括：

- You are using `await`. (This is the obvious one.)
  您正在使用 `await` 。（这是显而易见的。）
- You are returning an error asynchronously. `async` and then `throw` is shorter than `return Future.error(...)`.
  您正在异步返回错误。 `async` 然后 `throw` 比 `return Future.error(...)` 短。
- You are returning a value and you want it implicitly wrapped in a future. `async` is shorter than `Future.value(...)`.
  您正在返回一个值，并且希望将其隐式包装在 future 中。 `async` 比 `Future.value(...)` 短。

```dart
Future<void> usesAwait(Future<String> later) async {
  print(await later);
}

Future<void> asyncError() async {
  throw 'Error!';
}

Future<String> asyncValue() async => 'value';
```

### CONSIDER using higher-order methods to transform a stream 考虑使用高阶方法转换流

This parallels the above suggestion on iterables. Streams support many of the same methods and also handle things like transmitting errors, closing, etc. correctly.
这与上述关于可迭代对象的建议相似。流支持许多相同的方法，并且还正确地处理诸如传输错误、关闭等事项。

### AVOID using Completer directly 避免直接使用 Completer

Many people new to asynchronous programming want to write code that produces a future. The constructors in Future don’t seem to fit their need so they eventually find the Completer class and use that.
许多异步编程新手希望编写生成 future 的代码。Future 中的构造函数似乎不适合他们的需求，因此他们最终找到了 Completer 类并使用它。

```dart
Future<bool> fileContainsBear(String path) {
  var completer = Completer<bool>();

  File(path).readAsString().then((contents) {
    completer.complete(contents.contains('bear'));
  });

  return completer.future;
}
```

Completer is needed for two kinds of low-level code: new asynchronous primitives, and interfacing with asynchronous code that doesn’t use futures. Most other code should use async/await or [`Future.then()`](https://api.dart.dev/stable/dart-async/Future/then.html), because they’re clearer and make error handling easier.
Completer 对于两种低级代码是必需的：新的异步原语，以及与不使用 future 的异步代码进行接口。大多数其他代码应该使用 async/await 或 `Future.then()` ，因为它们更清晰，并且使错误处理更容易。

```dart
Future<bool> fileContainsBear(String path) {
  return File(path).readAsString().then((contents) {
    return contents.contains('bear');
  });
}
Future<bool> fileContainsBear(String path) async {
  var contents = await File(path).readAsString();
  return contents.contains('bear');
}
```

### DO test for `Future<T>` when disambiguating a `FutureOr<T>` whose type argument could be `Object` 在消除歧义时测试 `Future<T>` ，其类型参数可能是 `Object`

Before you can do anything useful with a `FutureOr<T>`, you typically need to do an `is` check to see if you have a `Future<T>` or a bare `T`. If the type argument is some specific type as in `FutureOr<int>`, it doesn’t matter which test you use, `is int` or `is Future<int>`. Either works because those two types are disjoint.
在对 `FutureOr<T>` 执行任何有用的操作之前，通常需要执行 `is` 检查，以查看您是否有 `Future<T>` 或裸 `T` 。如果类型参数是某个特定类型，如 `FutureOr<int>` 中，则您使用哪个测试（ `is int` 或 `is Future<int>` ）并不重要。两种方法都适用，因为这两种类型是分离的。

However, if the value type is `Object` or a type parameter that could possibly be instantiated with `Object`, then the two branches overlap. `Future<Object>` itself implements `Object`, so `is Object` or `is T` where `T` is some type parameter that could be instantiated with `Object` returns true even when the object is a future. Instead, explicitly test for the `Future` case:
但是，如果值类型是 `Object` 或可能使用 `Object` 实例化的类型参数，则这两个分支重叠。 `Future<Object>` 本身实现了 `Object` ，因此 `is Object` 或 `is T` （其中 `T` 是可能使用 `Object` 实例化的某个类型参数）即使对象是 future 也返回 true。相反，显式测试 `Future` 的情况：

```dart
Future<T> logValue<T>(FutureOr<T> value) async {
  if (value is Future<T>) {
    var result = await value;
    print(result);
    return result;
  } else {
    print(value);
    return value;
  }
}
Future<T> logValue<T>(FutureOr<T> value) async {
  if (value is T) {
    print(value);
    return value;
  } else {
    var result = await value;
    print(result);
    return result;
  }
}
```

In the bad example, if you pass it a `Future<Object>`, it incorrectly treats it like a bare, synchronous value.
在不好的示例中，如果您向其传递 `Future<Object>` ，它会错误地将其视为裸的同步值。
