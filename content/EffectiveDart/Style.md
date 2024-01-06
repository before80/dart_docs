+++
title = "有效的 Dart：样式"
date = 2024-01-05T20:29:36+08:00
weight = 10
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文: [https://dart.dev/effective-dart/style](https://dart.dev/effective-dart/style)

## Effective Dart: Style 有效的 Dart：样式

A surprisingly important part of good code is good style. Consistent naming, ordering, and formatting helps code that *is* the same *look* the same. It takes advantage of the powerful pattern-matching hardware most of us have in our ocular systems. If we use a consistent style across the entire Dart ecosystem, it makes it easier for all of us to learn from and contribute to each others’ code.
良好代码的一个非常重要的部分是良好的样式。一致的命名、排序和格式有助于使相同的代码看起来相同。它利用了我们大多数人在视觉系统中拥有的强大的模式匹配硬件。如果我们在整个 Dart 生态系统中使用一致的样式，那么我们所有人都可以更轻松地相互学习和为彼此的代码做出贡献。

## Identifiers 标识符

Identifiers come in three flavors in Dart.
Dart 中的标识符有三种形式。

- `UpperCamelCase` names capitalize the first letter of each word, including the first.
  `UpperCamelCase` 名称将每个单词的首字母大写，包括第一个字母。
- `lowerCamelCase` names capitalize the first letter of each word, *except* the first which is always lowercase, even if it’s an acronym.
  `lowerCamelCase` 名称首字母大写，除了第一个字母始终小写，即使它是缩写。
- `lowercase_with_underscores` names use only lowercase letters, even for acronyms, and separate words with `_`.
  `lowercase_with_underscores` 名称仅使用小写字母，即使是缩写，并用 `_` 分隔单词。

### DO name types using `UpperCamelCase` 使用 `UpperCamelCase`

Linter rule: [camel_case_types](https://dart.dev/tools/linter-rules/camel_case_types)
Linter 规则：camel_case_types

Classes, enum types, typedefs, and type parameters should capitalize the first letter of each word (including the first word), and use no separators.
命名类型类、枚举类型、typedef 和类型参数应将每个单词的首字母大写（包括第一个单词），并且不使用分隔符。

```dart
class SliderMenu { ... }

class HttpRequest { ... }

typedef Predicate<T> = bool Function(T value);
```

This even includes classes intended to be used in metadata annotations.
这甚至包括旨在用于元数据注释的类。

```dart
class Foo {
  const Foo([Object? arg]);
}

@Foo(anArg)
class A { ... }

@Foo()
class B { ... }
```

If the annotation class’s constructor takes no parameters, you might want to create a separate `lowerCamelCase` constant for it.
如果注释类的构造函数不采用任何参数，您可能需要为其创建一个单独的 `lowerCamelCase` 常量。

```dart
const foo = Foo();

@foo
class C { ... }
```

### DO name extensions using `UpperCamelCase` 使用 `UpperCamelCase`

Linter rule: [camel_case_extensions](https://dart.dev/tools/linter-rules/camel_case_extensions)
Linter 规则：camel_case_extensions

Like types, [extensions](https://dart.dev/language/extension-methods) should capitalize the first letter of each word (including the first word), and use no separators.
命名扩展名与类型一样，应将每个单词的首字母大写（包括第一个单词），并且不使用分隔符。

```dart
extension MyFancyList<T> on List<T> { ... }

extension SmartIterable<T> on Iterable<T> { ... }
```



### DO name packages, directories, and source files using `lowercase_with_underscores` 使用 `lowercase_with_underscores`

Linter rules: [file_names](https://dart.dev/tools/linter-rules/file_names), [package_names](https://dart.dev/tools/linter-rules/package_names)
Linter 规则：file_names、package_names

Some file systems are not case-sensitive, so many projects require filenames to be all lowercase. Using a separating character allows names to still be readable in that form. Using underscores as the separator ensures that the name is still a valid Dart identifier, which may be helpful if the language later supports symbolic imports.
对名称包、目录和源文件进行命名

```
my_package
└─ lib
   └─ file_system.dart
   └─ slider_menu.dart
mypackage
└─ lib
   └─ file-system.dart
   └─ SliderMenu.dart
```

### DO name import prefixes using `lowercase_with_underscores` 某些文件系统不区分大小写，因此许多项目要求文件名全部采用小写。使用分隔符允许名称仍以该形式可读。使用下划线作为分隔符可确保名称仍然是有效的 Dart 标识符，如果该语言以后支持符号导入，这可能会有所帮助。

Linter rule: [library_prefixes](https://dart.dev/tools/linter-rules/library_prefixes)
使用

```dart
import 'dart:math' as math;
import 'package:angular_components/angular_components.dart' as angular_components;
import 'package:js/js.dart' as js;
import 'dart:math' as Math;
import 'package:angular_components/angular_components.dart' as angularComponents;
import 'package:js/js.dart' as JS;
```

### DO name other identifiers using `lowerCamelCase` Linter 规则：library_prefixes

Linter rule: [non_constant_identifier_names](https://dart.dev/tools/linter-rules/non_constant_identifier_names)
对导入前缀进行命名

Class members, top-level definitions, variables, parameters, and named parameters should capitalize the first letter of each word *except* the first word, and use no separators.
使用

```dart
var count = 3;

HttpRequest httpRequest;

void align(bool clearItems) {
  // ...
}
```

### PREFER using `lowerCamelCase` for constant names Linter 规则：non_constant_identifier_names

Linter rule: [constant_identifier_names](https://dart.dev/tools/linter-rules/constant_identifier_names)
对其他标识符进行命名

In new code, use `lowerCamelCase` for constant variables, including enum values.
在新的代码中，对常量变量（包括枚举值）使用 `lowerCamelCase` 。

```dart
const pi = 3.14;
const defaultTimeout = 1000;
final urlScheme = RegExp('^([a-z]+):');

class Dice {
  static final numberGenerator = Random();
}
const PI = 3.14;
const DefaultTimeout = 1000;
final URL_SCHEME = RegExp('^([a-z]+):');

class Dice {
  static final NUMBER_GENERATOR = Random();
}
```

You may use `SCREAMING_CAPS` for consistency with existing code, as in the following cases:
为了与现有代码保持一致，您可以在以下情况下使用 `SCREAMING_CAPS` ：

- When adding code to a file or library that already uses `SCREAMING_CAPS`.
  向已经使用 `SCREAMING_CAPS` 的文件或库中添加代码时。
- When generating Dart code that’s parallel to Java code—for example, in enumerated types generated from [protobufs.](https://pub.dev/packages/protobuf)
  生成与 Java 代码平行的 Dart 代码时，例如，从 protobufs 生成的枚举类型。

*info* **Note:** We initially used Java’s `SCREAMING_CAPS` style for constants. We changed for a few reasons:
注意：我们最初对常量使用 Java 的 `SCREAMING_CAPS` 样式。我们出于以下几个原因进行了更改：

- `SCREAMING_CAPS` looks bad for many cases, particularly enum values for things like CSS colors.
  `SCREAMING_CAPS` 在许多情况下看起来很糟糕，尤其是对于 CSS 颜色等内容的枚举值。
- Constants are often changed to final non-const variables, which would necessitate a name change.
  常量通常会更改为最终的非 const 变量，这将需要更改名称。
- The `values` property automatically defined on an enum type is const and lowercase.
  在枚举类型上自动定义的 `values` 属性是 const 和小写的。

### DO capitalize acronyms and abbreviations longer than two letters like words 像单词一样对两个字母以上的首字母缩写词和缩写词进行大写

Capitalized acronyms can be hard to read, and multiple adjacent acronyms can lead to ambiguous names. For example, given a name that starts with `HTTPSFTP`, there’s no way to tell if it’s referring to HTTPS FTP or HTTP SFTP.
大写的首字母缩写词可能难以阅读，并且多个相邻的首字母缩写词可能导致名称不明确。例如，给定一个以 `HTTPSFTP` 开头的名称，无法判断它指的是 HTTPS FTP 还是 HTTP SFTP。

To avoid this, acronyms and abbreviations are capitalized like regular words.
为避免这种情况，首字母缩略词和缩写词像普通单词一样大写。

**Exception:** Two-letter *acronyms* like IO (input/output) are fully capitalized: `IO`. On the other hand, two-letter *abbreviations* like ID (identification) are still capitalized like regular words: `Id`.
例外：像 IO（输入/输出）这样的两个字母的缩略词完全大写： `IO` 。另一方面，像 ID（标识）这样的两个字母的缩写词仍然像普通单词一样大写： `Id` 。

```
class HttpConnection {}
class DBIOPort {}
class TVVcr {}
class MrRogers {}

var httpRequest = ...
var uiHandler = ...
var userId = ...
Id id;
class HTTPConnection {}
class DbIoPort {}
class TvVcr {}
class MRRogers {}

var hTTPRequest = ...
var uIHandler = ...
var userID = ...
ID iD;
```

### PREFER using `_`, `__`, etc. for unused callback parameters 优先使用 `_` 、 `__` 等作为未使用的回调参数

Sometimes the type signature of a callback function requires a parameter, but the callback implementation doesn’t *use* the parameter. In this case, it’s idiomatic to name the unused parameter `_`. If the function has multiple unused parameters, use additional underscores to avoid name collisions: `__`, `___`, etc.
有时回调函数的类型签名需要一个参数，但回调实现不使用该参数。在这种情况下，惯用做法是将未使用的参数命名为 `_` 。如果函数有多个未使用的参数，请使用其他下划线来避免名称冲突： `__` 、 `___` 等。

```dart
futureOfVoid.then((_) {
  print('Operation complete.');
});
```

This guideline is only for functions that are both *anonymous and local*. These functions are usually used immediately in a context where it’s clear what the unused parameter represents. In contrast, top-level functions and method declarations don’t have that context, so their parameters must be named so that it’s clear what each parameter is for, even if it isn’t used.
此准则仅适用于匿名且本地的函数。这些函数通常在明确未使用的参数表示什么的上下文中立即使用。相比之下，顶级函数和方法声明没有该上下文，因此必须为其参数命名，以便明确每个参数的用途，即使它未使用。

### DON’T use a leading underscore for identifiers that aren’t private 不要对非私有标识符使用前导下划线

Dart uses a leading underscore in an identifier to mark members and top-level declarations as private. This trains users to associate a leading underscore with one of those kinds of declarations. They see “_” and think “private”.
Dart 在标识符中使用前导下划线将成员和顶级声明标记为私有。这训练用户将前导下划线与其中一种声明相关联。他们看到“_”并想到“私有”。

There is no concept of “private” for local variables, parameters, local functions, or library prefixes. When one of those has a name that starts with an underscore, it sends a confusing signal to the reader. To avoid that, don’t use leading underscores in those names.
对于局部变量、参数、局部函数或库前缀，没有“私有”的概念。当其中一个以下划线开头时，它会向读者发送一个令人困惑的信号。为了避免这种情况，请不要在这些名称中使用前导下划线。

### DON’T use prefix letters 不要使用前缀字母

[Hungarian notation](https://en.wikipedia.org/wiki/Hungarian_notation) and other schemes arose in the time of BCPL, when the compiler didn’t do much to help you understand your code. Because Dart can tell you the type, scope, mutability, and other properties of your declarations, there’s no reason to encode those properties in identifier names.
匈牙利符号法和其他方案出现在 BCPL 时代，当时编译器并没有做太多帮助您理解代码的事情。由于 Dart 可以告诉您声明的类型、范围、可变性和其他属性，因此没有理由在标识符名称中对这些属性进行编码。

```
defaultTimeout
kDefaultTimeout
```

### DON’T explicitly name libraries 不要显式命名库

Appending a name to the `library` directive is technically possible, but is a legacy feature and discouraged.
在 `library` 指令中附加名称在技术上是可行的，但这是一个遗留特性，不建议使用。

Dart generates a unique tag for each library based on its path and filename. Naming libraries overrides this generated URI. Without the URI, it can be harder for tools to find the main library file in question.
Dart 会根据库的路径和文件名生成一个唯一的标记。命名库会覆盖此生成的 URI。如果没有 URI，工具可能更难找到有问题的库主文件。

```dart
library my_library;
/// A really great test library.
@TestOn('browser')
library;
```

## Ordering 排序

To keep the preamble of your file tidy, we have a prescribed order that directives should appear in. Each “section” should be separated by a blank line.
为了保持文件的前言部分整洁，我们规定了指令应出现的顺序。每个“部分”应由空行分隔。

A single linter rule handles all the ordering guidelines: [directives_ordering.](https://dart.dev/tools/linter-rules/directives_ordering)
单个 linter 规则处理所有排序准则：directives_ordering。

### DO place `dart:` imports before other imports 在其他导入之前放置 `dart:` 导入

Linter rule: [directives_ordering](https://dart.dev/tools/linter-rules/directives_ordering)
Linter 规则：directives_ordering

```dart
import 'dart:async';
import 'dart:html';

import 'package:bar/bar.dart';
import 'package:foo/foo.dart';
```

### DO place `package:` imports before relative imports 在相对导入之前放置 `package:` 导入

Linter rule: [directives_ordering](https://dart.dev/tools/linter-rules/directives_ordering)
Linter 规则：directives_ordering

```dart
import 'package:bar/bar.dart';
import 'package:foo/foo.dart';

import 'util.dart';
```

### DO specify exports in a separate section after all imports 在所有导入之后单独的部分中指定导出

Linter rule: [directives_ordering](https://dart.dev/tools/linter-rules/directives_ordering)
Linter 规则：directives_ordering

```dart
import 'src/error.dart';
import 'src/foo_bar.dart';

export 'src/error.dart';
import 'src/error.dart';
export 'src/error.dart';
import 'src/foo_bar.dart';
```

### DO sort sections alphabetically 按字母顺序对部分进行排序

Linter rule: [directives_ordering](https://dart.dev/tools/linter-rules/directives_ordering)
Linter 规则：directives_ordering

```dart
import 'package:bar/bar.dart';
import 'package:foo/foo.dart';

import 'foo.dart';
import 'foo/foo.dart';
import 'package:foo/foo.dart';
import 'package:bar/bar.dart';

import 'foo/foo.dart';
import 'foo.dart';
```

## Formatting 格式化

Like many languages, Dart ignores whitespace. However, *humans* don’t. Having a consistent whitespace style helps ensure that human readers see code the same way the compiler does.
与许多语言一样，Dart 忽略空格。但是，人类不会。采用一致的空格样式有助于确保人类读者以编译器相同的方式查看代码。

### DO format your code using `dart format` 使用 `dart format` 格式化您的代码

Formatting is tedious work and is particularly time-consuming during refactoring. Fortunately, you don’t have to worry about it. We provide a sophisticated automated code formatter called [`dart format`](https://dart.dev/tools/dart-format) that does it for you. We have [some documentation](https://github.com/dart-lang/dart_style/wiki/Formatting-Rules) on the rules it applies, but the official whitespace-handling rules for Dart are *whatever `dart format` produces*.
格式化是一项繁琐的工作，在重构期间尤其耗时。幸运的是，您不必担心它。我们提供了一个名为 `dart format` 的复杂自动代码格式化程序，它可以为您完成此操作。我们有一些关于它应用的规则的文档，但 Dart 的官方空白处理规则是 `dart format` 生成的任何内容。

The remaining formatting guidelines are for the few things `dart format` cannot fix for you.
其余的格式化指南适用于 `dart format` 无法为您修复的少数内容。

### CONSIDER changing your code to make it more formatter-friendly 考虑更改您的代码以使其更易于格式化

The formatter does the best it can with whatever code you throw at it, but it can’t work miracles. If your code has particularly long identifiers, deeply nested expressions, a mixture of different kinds of operators, etc. the formatted output may still be hard to read.
格式化程序会尽最大努力处理您抛给它的任何代码，但它无法创造奇迹。如果您的代码具有特别长的标识符、深度嵌套的表达式、不同类型的运算符的混合等，则格式化的输出可能仍然难以阅读。

When that happens, reorganize or simplify your code. Consider shortening a local variable name or hoisting out an expression into a new local variable. In other words, make the same kinds of modifications that you’d make if you were formatting the code by hand and trying to make it more readable. Think of `dart format` as a partnership where you work together, sometimes iteratively, to produce beautiful code.
发生这种情况时，请重新组织或简化您的代码。考虑缩短局部变量名或将表达式提升到新的局部变量中。换句话说，进行与手动格式化代码并尝试使其更具可读性时相同的修改。将 `dart format` 视为一种合作关系，您有时会反复合作以生成漂亮的代码。

### AVOID lines longer than 80 characters 避免超过 80 个字符的行

Linter rule: [lines_longer_than_80_chars](https://dart.dev/tools/linter-rules/lines_longer_than_80_chars)
Linter 规则：lines_longer_than_80_chars

Readability studies show that long lines of text are harder to read because your eye has to travel farther when moving to the beginning of the next line. This is why newspapers and magazines use multiple columns of text.
可读性研究表明，长文本行更难阅读，因为在移动到下一行开头时，您的眼睛必须走得更远。这就是报纸和杂志使用多列文本的原因。

If you really find yourself wanting lines longer than 80 characters, our experience is that your code is likely too verbose and could be a little more compact. The main offender is usually `VeryLongCamelCaseClassNames`. Ask yourself, “Does each word in that type name tell me something critical or prevent a name collision?” If not, consider omitting it.
如果您真的发现自己想要超过 80 个字符的行，我们的经验是您的代码可能过于冗长，可以更紧凑一些。主要的罪魁祸首通常是 `VeryLongCamelCaseClassNames` 。问问自己，“类型名称中的每个单词是否都告诉我一些关键信息或防止名称冲突？”如果不是，请考虑省略它。

Note that `dart format` does 99% of this for you, but the last 1% is you. It does not split long string literals to fit in 80 columns, so you have to do that manually.
请注意， `dart format` 为您完成了其中的 99%，但剩下的 1% 由您完成。它不会拆分长字符串文字以使其适合 80 列，因此您必须手动执行此操作。

**Exception:** When a URI or file path occurs in a comment or string (usually in an import or export), it may remain whole even if it causes the line to go over 80 characters. This makes it easier to search source files for a path.
例外情况：当 URI 或文件路径出现在注释或字符串中（通常在导入或导出中）时，即使它导致该行超过 80 个字符，它也可能保持完整。这使得在源文件中搜索路径变得更加容易。

**Exception:** Multi-line strings can contain lines longer than 80 characters because newlines are significant inside the string and splitting the lines into shorter ones can alter the program.
异常：多行字符串可以包含超过 80 个字符的行，因为换行符在字符串中很重要，将行拆分为较短的行可能会更改程序。

### DO use curly braces for all flow control statements 对所有流程控制语句使用大括号

Linter rule: [curly_braces_in_flow_control_structures](https://dart.dev/tools/linter-rules/curly_braces_in_flow_control_structures)
Linter 规则：curly_braces_in_flow_control_structures

Doing so avoids the [dangling else](https://en.wikipedia.org/wiki/Dangling_else) problem.
这样做可以避免悬空 else 问题。

```dart
if (isWeekDay) {
  print('Bike to work!');
} else {
  print('Go dancing or read a book!');
}
```

**Exception:** When you have an `if` statement with no `else` clause and the whole `if` statement fits on one line, you can omit the braces if you prefer:
异常：当您有一个没有 `else` 子句的 `if` 语句，并且整个 `if` 语句都适合一行时，您可以省略大括号（如果您愿意）：

```dart
if (arg == null) return defaultValue;
```

If the body wraps to the next line, though, use braces:
但是，如果主体换行到下一行，请使用大括号：

```dart
if (overflowChars != other.overflowChars) {
  return overflowChars < other.overflowChars;
}

if (overflowChars != other.overflowChars)
  return overflowChars < other.overflowChars;
```
