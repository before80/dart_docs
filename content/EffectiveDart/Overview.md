+++
title = "有效的 Dart：概览"
date = 2024-01-05T20:29:36+08:00
weight = 1
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文: [https://dart.dev/effective-dart](https://dart.dev/effective-dart)

## Effective Dart 有效的 Dart

Over the past several years, we’ve written a ton of Dart code and learned a lot about what works well and what doesn’t. We’re sharing this with you so you can write consistent, robust, fast code too. There are two overarching themes:

​	在过去的几年中，我们编写了大量的 Dart 代码，并了解到很多关于哪些有效哪些无效的内容。我们与您分享这些内容，以便您也可以编写一致、健壮、快速的代码。有两个总体主题：

1. **Be consistent.** When it comes to things like formatting, and casing, arguments about which is better are subjective and impossible to resolve. What we do know is that being *consistent* is objectively helpful.

2. 保持一致。当涉及到诸如格式化和大小写等内容时，关于哪种更好的争论是主观的，并且不可能解决。我们所知道的是，保持一致在客观上是有帮助的。

   If two pieces of code look different it should be because they *are* different in some meaningful way. When a bit of code stands out and catches your eye, it should do so for a useful reason.

   如果两段代码看起来不同，那应该是因为它们在某些有意义的方式上不同。当一段代码脱颖而出并引起你的注意时，它应该出于有用的原因。

3. **Be brief.** Dart was designed to be familiar, so it inherits many of the same statements and expressions as C, Java, JavaScript and other languages. But we created Dart because there is a lot of room to improve on what those languages offer. We added a bunch of features, from string interpolation to initializing formals, to help you express your intent more simply and easily.

4. 简洁。Dart 被设计为易于理解，因此它继承了许多与 C、Java、JavaScript 和其他语言相同的语句和表达式。但我们创建 Dart 是因为有很大的空间可以改进这些语言提供的功能。我们添加了许多功能，从字符串插值到初始化形式参数，以帮助你更简单、更轻松地表达你的意图。

   If there are multiple ways to say something, you should generally pick the most concise one. This is not to say you should [code golf](https://en.wikipedia.org/wiki/Code_golf) yourself into cramming a whole program into a single line. The goal is code that is *economical*, not *dense*.

   如果有多种方法来说某件事，你通常应该选择最简洁的一种。这并不是说你应该将自己编程高尔夫化，将整个程序塞进一行代码中。目标是经济的代码，而不是密集的代码。

## 指南 The guides 

We split the guidelines into a few separate pages for easy digestion:

​	我们将指南分成几个单独的页面，以便于理解：

- **[Style Guide](https://dart.dev/effective-dart/style)** – This defines the rules for laying out and organizing code, or at least the parts that [`dart format`](https://dart.dev/tools/dart-format) doesn’t handle for you. The style guide also specifies how identifiers are formatted: `camelCase`, `using_underscores`, etc.
- 样式指南 - 这定义了布局和组织代码的规则，或者至少是 `dart format` 不会为你处理的部分。样式指南还指定了标识符的格式： `camelCase` 、 `using_underscores` 等。
- **[Documentation Guide](https://dart.dev/effective-dart/documentation)** – This tells you everything you need to know about what goes inside comments. Both doc comments and regular, run-of-the-mill code comments.
- 文档指南 – 告诉您需要了解的有关注释内容的所有信息。包括文档注释和常规的、普通的代码注释。
- **[Usage Guide](https://dart.dev/effective-dart/usage)** – This teaches you how to make the best use of language features to implement behavior. If it’s in a statement or expression, it’s covered here.
- 用法指南 – 教您如何充分利用语言特性来实现行为。如果它在语句或表达式中，则在此处介绍。
- **[Design Guide](https://dart.dev/effective-dart/design)** – This is the softest guide, but the one with the widest scope. It covers what we’ve learned about designing consistent, usable APIs for libraries. If it’s in a type signature or declaration, this goes over it.
- 设计指南 – 这是最软的指南，但范围最广。它涵盖了我们关于为库设计一致、可用的 API 所学到的知识。如果它在类型签名或声明中，则会对此进行介绍。

For links to all the guidelines, see the [summary](https://dart.dev/effective-dart#summary-of-all-rules).

​	有关所有指南的链接，请参阅摘要。

## 如何阅读指南 How to read the guides 

Each guide is broken into a few sections. Sections contain a list of guidelines. Each guideline starts with one of these words:

​	每个指南分为几个部分。部分包含指南列表。每条指南都以以下单词之一开头：

- **DO** guidelines describe practices that should always be followed. There will almost never be a valid reason to stray from them.
- DO 指南描述应始终遵循的做法。几乎永远没有理由偏离它们。
- **DON’T** guidelines are the converse: things that are almost never a good idea. Hopefully, we don’t have as many of these as other languages do because we have less historical baggage.
- DON’T 指南与此相反：几乎永远不是好主意的事情。希望我们没有像其他语言那样多，因为我们的历史包袱较少。
- **PREFER** guidelines are practices that you *should* follow. However, there may be circumstances where it makes sense to do otherwise. Just make sure you understand the full implications of ignoring the guideline when you do.
- PREFER 指南是您应该遵循的做法。但是，在某些情况下，这样做是有意义的。当您这样做时，请确保您了解忽略该指南的全部含义。
- **AVOID** guidelines are the dual to “prefer”: stuff you shouldn’t do but where there may be good reasons to on rare occasions.
- AVOID 指南是“prefer”的双重指南：您不应该做的事情，但在某些情况下可能有充分的理由去做。
- **CONSIDER** guidelines are practices that you might or might not want to follow, depending on circumstances, precedents, and your own preference.
- CONSIDER 指南是您可能希望或可能不希望遵循的做法，具体取决于情况、先例和您自己的偏好。

Some guidelines describe an **exception** where the rule does *not* apply. When listed, the exceptions may not be exhaustive—you might still need to use your judgement on other cases.

​	某些指南描述了规则不适用的例外情况。列出时，例外情况可能不是详尽无遗的——您可能仍需要对其他情况进行判断。

This sounds like the police are going to beat down your door if you don’t have your laces tied correctly. Things aren’t that bad. Most of the guidelines here are common sense and we’re all reasonable people. The goal, as always, is nice, readable and maintainable code.

​	这听起来像是警察会因为你的鞋带系得不正确而破门而入。事情并没有那么糟糕。这里的大多数指南都是常识，我们都是讲道理的人。一如既往，目标是编写美观、可读且可维护的代码。

The Dart analyzer provides a linter to help you write good, consistent code that follows these and other guidelines. If one or more [linter rules](https://dart.dev/tools/linter-rules) exist that can help you follow a guideline then the guideline links to those rules. The links use the following format:

​	Dart 分析器提供了一个 linter 来帮助您编写遵循这些和其他指南的良好、一致的代码。如果存在一个或多个可以帮助您遵循指南的 linter 规则，那么该指南会链接到这些规则。这些链接使用以下格式：

Linter rule: [unnecessary_getters_setters](https://dart.dev/tools/linter-rules/unnecessary_getters_setters)

​	Linter 规则：不必要的 getter 和 setter

To learn how to use the linter, see [Enabling linter rules](https://dart.dev/tools/analysis#enabling-linter-rules) and the list of [linter rules](https://dart.dev/tools/linter-rules).

​	要了解如何使用 linter，请参阅启用 linter 规则和 linter 规则列表。

## 词汇表 Glossary 

To keep the guidelines brief, we use a few shorthand terms to refer to different Dart constructs.

​	为了使指南简短，我们使用一些简写术语来指代不同的 Dart 构造。

- A **library member** is a top-level field, getter, setter, or function. Basically, anything at the top level that isn’t a type.
- 库成员是顶级字段、getter、setter 或函数。基本上，任何不是类型的顶级内容。
- A **class member** is a constructor, field, getter, setter, function, or operator declared inside a class. Class members can be instance or static, abstract or concrete.
- 类成员是在类中声明的构造函数、字段、getter、setter、函数或运算符。类成员可以是实例或静态的，可以是抽象的或具体的。
- A **member** is either a library member or a class member.
- 成员是库成员或类成员。
- A **variable**, when used generally, refers to top-level variables, parameters, and local variables. It doesn’t include static or instance fields.
- 变量在一般情况下是指顶级变量、参数和局部变量。它不包括静态字段或实例字段。
- A **type** is any named type declaration: a class, typedef, or enum.
- 类型是任何命名的类型声明：类、typedef 或枚举。
- A **property** is a top-level variable, getter (inside a class or at the top level, instance or static), setter (same), or field (instance or static). Roughly any “field-like” named construct.
- 属性是顶级变量、getter（在类中或顶级，实例或静态）、setter（相同）或字段（实例或静态）。大致上是任何“类似字段”的命名构造。

## 所有规则的摘要 Summary of all rules 

### 样式 Style 

**标识符 Identifiers**

- [DO name types using `UpperCamelCase`.
  使用 `UpperCamelCase` 命名类型。](https://dart.dev/effective-dart/style#do-name-types-using-uppercamelcase)
- [DO name extensions using `UpperCamelCase`.
  使用 `UpperCamelCase` 命名扩展。](https://dart.dev/effective-dart/style#do-name-extensions-using-uppercamelcase)
- [DO name packages, directories, and source files using `lowercase_with_underscores`.
  使用 `lowercase_with_underscores` 命名包、目录和源文件。](https://dart.dev/effective-dart/style#do-name-packages-and-file-system-entities-using-lowercase-with-underscores)
- [DO name import prefixes using `lowercase_with_underscores`.
  使用 `lowercase_with_underscores` 命名导入前缀。](https://dart.dev/effective-dart/style#do-name-import-prefixes-using-lowercase_with_underscores)
- [DO name other identifiers using `lowerCamelCase`.
  使用 `lowerCamelCase` 命名其他标识符。](https://dart.dev/effective-dart/style#do-name-other-identifiers-using-lowercamelcase)
- [PREFER using `lowerCamelCase` for constant names.
  首选使用 `lowerCamelCase` 命名常量。](https://dart.dev/effective-dart/style#prefer-using-lowercamelcase-for-constant-names)
- [DO capitalize acronyms and abbreviations longer than two letters like words.
  将超过两个字母的缩写词和首字母缩略词像单词一样大写。](https://dart.dev/effective-dart/style#do-capitalize-acronyms-and-abbreviations-longer-than-two-letters-like-words)
- [PREFER using `_`, `__`, etc. for unused callback parameters.
  优先使用 `_` 、 `__` 等作为未使用的回调参数。](https://dart.dev/effective-dart/style#prefer-using-_-__-etc-for-unused-callback-parameters)
- [DON’T use a leading underscore for identifiers that aren’t private.
  不要对非私有标识符使用前导下划线。](https://dart.dev/effective-dart/style#dont-use-a-leading-underscore-for-identifiers-that-arent-private)
- [DON’T use prefix letters.
  不要使用前缀字母。](https://dart.dev/effective-dart/style#dont-use-prefix-letters)
- [DON’T explicitly name libraries.
  不要显式命名库。](https://dart.dev/effective-dart/style#dont-explicitly-name-libraries)

**排序 Ordering**

- [DO place `dart:` imports before other imports.
  在其他导入之前放置 `dart:` 导入。](https://dart.dev/effective-dart/style#do-place-dart-imports-before-other-imports)
- [DO place `package:` imports before relative imports.
  在相对导入之前放置 `package:` 导入。](https://dart.dev/effective-dart/style#do-place-package-imports-before-relative-imports)
- [DO specify exports in a separate section after all imports.
  在所有导入之后，在单独的部分中指定导出。](https://dart.dev/effective-dart/style#do-specify-exports-in-a-separate-section-after-all-imports)
- [DO sort sections alphabetically.
  按字母顺序对部分进行排序。](https://dart.dev/effective-dart/style#do-sort-sections-alphabetically)

**格式化 Formatting**

- [DO format your code using `dart format`.
  使用 `dart format` 格式化您的代码。](https://dart.dev/effective-dart/style#do-format-your-code-using-dart-format)
- [CONSIDER changing your code to make it more formatter-friendly.
  考虑更改代码以使其更易于格式化。](https://dart.dev/effective-dart/style#consider-changing-your-code-to-make-it-more-formatter-friendly)
- [AVOID lines longer than 80 characters.
  避免使用长度超过 80 个字符的行。](https://dart.dev/effective-dart/style#avoid-lines-longer-than-80-characters)
- [DO use curly braces for all flow control statements.
  对所有流程控制语句使用大括号。](https://dart.dev/effective-dart/style#do-use-curly-braces-for-all-flow-control-structures)

### Documentation 文档

**注释 Comments**

- [DO format comments like sentences.
  将注释格式化为句子。](https://dart.dev/effective-dart/documentation#do-format-comments-like-sentences)
- [DON’T use block comments for documentation.
  不要将块注释用于文档。](https://dart.dev/effective-dart/documentation#dont-use-block-comments-for-documentation)

**文档注释 Doc comments**

- [DO use `///` doc comments to document members and types.
  使用 `///` 文档注释来记录成员和类型。](https://dart.dev/effective-dart/documentation#do-use--doc-comments-to-document-members-and-types)
- [PREFER writing doc comments for public APIs.
  优先为公共 API 编写文档注释。](https://dart.dev/effective-dart/documentation#prefer-writing-doc-comments-for-public-apis)
- [CONSIDER writing a library-level doc comment.
  考虑编写库级文档注释。](https://dart.dev/effective-dart/documentation#consider-writing-a-library-level-doc-comment)
- [CONSIDER writing doc comments for private APIs.
  考虑为私有 API 编写文档注释。](https://dart.dev/effective-dart/documentation#consider-writing-doc-comments-for-private-apis)
- [DO start doc comments with a single-sentence summary.
  使用单句摘要开始文档注释。](https://dart.dev/effective-dart/documentation#do-start-doc-comments-with-a-single-sentence-summary)
- [DO separate the first sentence of a doc comment into its own paragraph.
  将文档注释的第一句话单独分段。](https://dart.dev/effective-dart/documentation#do-separate-the-first-sentence-of-a-doc-comment-into-its-own-paragraph)
- [AVOID redundancy with the surrounding context.
  避免与周围的上下文重复。](https://dart.dev/effective-dart/documentation#avoid-redundancy-with-the-surrounding-context)
- [PREFER starting function or method comments with third-person verbs.
  最好使用第三人称动词开始函数或方法注释。](https://dart.dev/effective-dart/documentation#prefer-starting-function-or-method-comments-with-third-person-verbs)
- [PREFER starting a non-boolean variable or property comment with a noun phrase.
  最好使用名词短语开始非布尔变量或属性注释。](https://dart.dev/effective-dart/documentation#prefer-starting-a-non-boolean-variable-or-property-comment-with-a-noun-phrase)
- [PREFER starting a boolean variable or property comment with "Whether" followed by a noun or gerund phrase.
  最好使用“Whether”后跟名词或动名词短语开始布尔变量或属性注释。](https://dart.dev/effective-dart/documentation#prefer-starting-a-boolean-variable-or-property-comment-with-whether-followed-by-a-noun-or-gerund-phrase)
- [DON’T write documentation for both the getter and setter of a property.
  不要为属性的 getter 和 setter 同时编写文档。](https://dart.dev/effective-dart/documentation#dont-write-documentation-for-both-the-getter-and-setter-of-a-property)
- [PREFER starting library or type comments with noun phrases.
  最好使用名词短语开始库或类型注释。](https://dart.dev/effective-dart/documentation#prefer-starting-library-or-type-comments-with-noun-phrases)
- [CONSIDER including code samples in doc comments.
  考虑在文档注释中包含代码示例。](https://dart.dev/effective-dart/documentation#consider-including-code-samples-in-doc-comments)
- [DO use square brackets in doc comments to refer to in-scope identifiers.
  在文档注释中使用方括号来引用作用域内的标识符。](https://dart.dev/effective-dart/documentation#do-use-square-brackets-in-doc-comments-to-refer-to-in-scope-identifiers)
- [DO use prose to explain parameters, return values, and exceptions.
  使用散文来解释参数、返回值和异常。](https://dart.dev/effective-dart/documentation#do-use-prose-to-explain-parameters-return-values-and-exceptions)
- [DO put doc comments before metadata annotations.
  将文档注释放在元数据注释之前。](https://dart.dev/effective-dart/documentation#do-put-doc-comments-before-metadata-annotations)

**Markdown**

- [AVOID using markdown excessively.
  避免过度使用 Markdown。](https://dart.dev/effective-dart/documentation#avoid-using-markdown-excessively)
- [AVOID using HTML for formatting.
  避免使用 HTML 进行格式化。](https://dart.dev/effective-dart/documentation#avoid-using-html-for-formatting)
- [PREFER backtick fences for code blocks.
  优先使用反引号围栏作为代码块。](https://dart.dev/effective-dart/documentation#prefer-backtick-fences-for-code-blocks)

**写作 Writing**

- [PREFER brevity.
  优先考虑简洁性。](https://dart.dev/effective-dart/documentation#prefer-brevity)
- [AVOID abbreviations and acronyms unless they are obvious.
  避免使用缩写和首字母缩写词，除非它们很明显。](https://dart.dev/effective-dart/documentation#avoid-abbreviations-and-acronyms-unless-they-are-obvious)
- [PREFER using "this" instead of "the" to refer to a member’s instance.
  优先使用“this”而不是“the”来引用成员的实例。](https://dart.dev/effective-dart/documentation#prefer-using-this-instead-of-the-to-refer-to-a-members-instance)

### Usage 用法

**库 Libraries**

- [DO use strings in `part of` directives.
  在 `part of` 指令中使用字符串。](https://dart.dev/effective-dart/usage#do-use-strings-in-part-of-directives)
- [DON’T import libraries that are inside the `src` directory of another package.
  不要导入另一个包的 `src` 目录中的库。](https://dart.dev/effective-dart/usage#dont-import-libraries-that-are-inside-the-src-directory-of-another-package)
- [DON’T allow an import path to reach into or out of `lib`.
  不要允许导入路径进入或离开 `lib` 。](https://dart.dev/effective-dart/usage#dont-allow-an-import-path-to-reach-into-or-out-of-lib)
- [PREFER relative import paths.
  优先使用相对导入路径。](https://dart.dev/effective-dart/usage#prefer-relative-import-paths)

**Null**

- [DON’T explicitly initialize variables to `null`.
  不要将变量显式初始化为 `null` 。](https://dart.dev/effective-dart/usage#dont-explicitly-initialize-variables-to-null)
- [DON’T use an explicit default value of `null`.
  不要使用显式默认值 `null` 。](https://dart.dev/effective-dart/usage#dont-use-an-explicit-default-value-of-null)
- [DON’T use `true` or `false` in equality operations.
  不要在相等运算中使用 `true` 或 `false` 。](https://dart.dev/effective-dart/usage#dont-use-true-or-false-in-equality-operations)
- [AVOID `late` variables if you need to check whether they are initialized.
  如果需要检查 `late` 变量是否已初始化，请避免使用它们。](https://dart.dev/effective-dart/usage#avoid-late-variables-if-you-need-to-check-whether-they-are-initialized)
- [CONSIDER assigning a nullable field to a local variable to enable type promotion.
  考虑将可空字段分配给局部变量以启用类型提升。](https://dart.dev/effective-dart/usage#consider-assigning-a-nullable-field-to-a-local-variable-to-enable-type-promotion)

**字符串 Strings**

- [DO use adjacent strings to concatenate string literals.
  确实使用相邻字符串连接字符串文字。](https://dart.dev/effective-dart/usage#do-use-adjacent-strings-to-concatenate-string-literals)
- [PREFER using interpolation to compose strings and values.
  更喜欢使用插值来组合字符串和值。](https://dart.dev/effective-dart/usage#prefer-using-interpolation-to-compose-strings-and-values)
- [AVOID using curly braces in interpolation when not needed.
  避免在不必要时在插值中使用花括号。](https://dart.dev/effective-dart/usage#avoid-using-curly-braces-in-interpolation-when-not-needed)

**集合 Collections**

- [DO use collection literals when possible.
  尽可能使用集合文字。](https://dart.dev/effective-dart/usage#do-use-collection-literals-when-possible)
- [DON’T use `.length` to see if a collection is empty.
  不要使用 `.length` 来查看集合是否为空。](https://dart.dev/effective-dart/usage#dont-use-length-to-see-if-a-collection-is-empty)
- [AVOID using `Iterable.forEach()` with a function literal.
  避免将 `Iterable.forEach()` 与函数字面量一起使用。](https://dart.dev/effective-dart/usage#avoid-using-iterableforeach-with-a-function-literal)
- [DON’T use `List.from()` unless you intend to change the type of the result.
  除非您打算更改结果的类型，否则不要使用 `List.from()` 。](https://dart.dev/effective-dart/usage#dont-use-listfrom-unless-you-intend-to-change-the-type-of-the-result)
- [DO use `whereType()` to filter a collection by type.
  使用 `whereType()` 按类型过滤集合。](https://dart.dev/effective-dart/usage#do-use-wheretype-to-filter-a-collection-by-type)
- [DON’T use `cast()` when a nearby operation will do.
  当附近的操作可以执行时，不要使用 `cast()` 。](https://dart.dev/effective-dart/usage#dont-use-cast-when-a-nearby-operation-will-do)
- [AVOID using `cast()`.
  避免使用 `cast()` 。](https://dart.dev/effective-dart/usage#avoid-using-cast)

**函数 Functions**

- [DO use a function declaration to bind a function to a name.
  使用函数声明将函数绑定到名称。](https://dart.dev/effective-dart/usage#do-use-a-function-declaration-to-bind-a-function-to-a-name)
- [DON’T create a lambda when a tear-off will do.
  当撕纸可以执行时，不要创建 lambda。](https://dart.dev/effective-dart/usage#dont-create-a-lambda-when-a-tear-off-will-do)

**变量 Variables**

- [DO follow a consistent rule for `var` and `final` on local variables.
  对局部变量遵循一致的 `var` 和 `final` 规则。](https://dart.dev/effective-dart/usage#do-follow-a-consistent-rule-for-var-and-final-on-local-variables)
- [AVOID storing what you can calculate.
  避免存储您可以计算的内容。](https://dart.dev/effective-dart/usage#avoid-storing-what-you-can-calculate)

**成员 Members**

- [DON’T wrap a field in a getter and setter unnecessarily.
  不要不必要地用 getter 和 setter 包装字段。](https://dart.dev/effective-dart/usage#dont-wrap-a-field-in-a-getter-and-setter-unnecessarily)
- [PREFER using a `final` field to make a read-only property.
  更喜欢使用 `final` 字段来创建只读属性。](https://dart.dev/effective-dart/usage#prefer-using-a-final-field-to-make-a-read-only-property)
- [CONSIDER using `=>` for simple members.
  考虑对简单成员使用 `=>` 。](https://dart.dev/effective-dart/usage#consider-using--for-simple-members)
- [DON’T use `this.` except to redirect to a named constructor or to avoid shadowing.
  不要使用 `this.` ，除非要重定向到命名构造函数或避免遮蔽。](https://dart.dev/effective-dart/usage#dont-use-this-when-not-needed-to-avoid-shadowing)
- [DO initialize fields at their declaration when possible.
  尽可能在声明时初始化字段。](https://dart.dev/effective-dart/usage#do-initialize-fields-at-their-declaration-when-possible)

**构造函数 Constructors**

- [DO use initializing formals when possible.
  尽可能使用初始化形式。](https://dart.dev/effective-dart/usage#do-use-initializing-formals-when-possible)
- [DON’T use `late` when a constructor initializer list will do.
  当构造函数初始化程序列表可以完成时，不要使用 `late` 。](https://dart.dev/effective-dart/usage#dont-use-late-when-a-constructor-initializer-list-will-do)
- [DO use `;` instead of `{}` for empty constructor bodies.
  对于空构造函数体，使用 `;` 代替 `{}` 。](https://dart.dev/effective-dart/usage#do-use--instead-of--for-empty-constructor-bodies)
- [DON’T use `new`.
  不要使用 `new` 。](https://dart.dev/effective-dart/usage#dont-use-new)
- [DON’T use `const` redundantly.
  不要冗余地使用 `const` 。](https://dart.dev/effective-dart/usage#dont-use-const-redundantly)

**错误处理Error handling**

- [AVOID catches without `on` clauses.
  避免没有 `on` 子句的 catch 语句。](https://dart.dev/effective-dart/usage#avoid-catches-without-on-clauses)
- [DON’T discard errors from catches without `on` clauses.
  不要丢弃没有 `on` 子句的 catch 语句中的错误。](https://dart.dev/effective-dart/usage#dont-discard-errors-from-catches-without-on-clauses)
- [DO throw objects that implement `Error` only for programmatic errors.
  仅对编程错误抛出实现 `Error` 的对象。](https://dart.dev/effective-dart/usage#do-throw-objects-that-implement-error-only-for-programmatic-errors)
- [DON’T explicitly catch `Error` or types that implement it.
  不要显式捕获 `Error` 或实现它的类型。](https://dart.dev/effective-dart/usage#dont-explicitly-catch-error-or-types-that-implement-it)
- [DO use `rethrow` to rethrow a caught exception.
  使用 `rethrow` 重新抛出捕获的异常。](https://dart.dev/effective-dart/usage#do-use-rethrow-to-rethrow-a-caught-exception)

**异步 Asynchrony**

- [PREFER async/await over using raw futures.
  优先使用 async/await 而非原始 future。](https://dart.dev/effective-dart/usage#prefer-asyncawait-over-using-raw-futures)
- [DON’T use `async` when it has no useful effect.
  当 `async` 没有有用的效果时，不要使用它。](https://dart.dev/effective-dart/usage#dont-use-async-when-it-has-no-useful-effect)
- [CONSIDER using higher-order methods to transform a stream.
  考虑使用高阶方法来转换流。](https://dart.dev/effective-dart/usage#consider-using-higher-order-methods-to-transform-a-stream)
- [AVOID using Completer directly.
  避免直接使用 Completer。](https://dart.dev/effective-dart/usage#avoid-using-completer-directly)
- [DO test for `Future` when disambiguating a `FutureOr` whose type argument could be `Object`.
  在对类型参数可能为 `Object` 的 `FutureOr` 进行消歧义时，对 `Future` 进行测试。](https://dart.dev/effective-dart/usage#do-test-for-futuret-when-disambiguating-a-futureort-whose-type-argument-could-be-object)

### Design 设计

**名称 Names**

- [DO use terms consistently.
  一致地使用术语。](https://dart.dev/effective-dart/design#do-use-terms-consistently)
- [AVOID abbreviations.
  避免缩写。](https://dart.dev/effective-dart/design#avoid-abbreviations)
- [PREFER putting the most descriptive noun last.
  优先将最具描述性的名词放在最后。](https://dart.dev/effective-dart/design#prefer-putting-the-most-descriptive-noun-last)
- [CONSIDER making the code read like a sentence.
  考虑让代码读起来像一个句子。](https://dart.dev/effective-dart/design#consider-making-the-code-read-like-a-sentence)
- [PREFER a noun phrase for a non-boolean property or variable.
  对于非布尔属性或变量，首选名词短语。](https://dart.dev/effective-dart/design#prefer-a-noun-phrase-for-a-non-boolean-property-or-variable)
- [PREFER a non-imperative verb phrase for a boolean property or variable.
  对于布尔属性或变量，优先使用非命令式动词短语。](https://dart.dev/effective-dart/design#prefer-a-non-imperative-verb-phrase-for-a-boolean-property-or-variable)
- [CONSIDER omitting the verb for a named boolean *parameter*.
  考虑省略命名布尔参数的动词。](https://dart.dev/effective-dart/design#consider-omitting-the-verb-for-a-named-boolean-parameter)
- [PREFER the "positive" name for a boolean property or variable.
  对于布尔属性或变量，优先使用“肯定”名称。](https://dart.dev/effective-dart/design#prefer-the-positive-name-for-a-boolean-property-or-variable)
- [PREFER an imperative verb phrase for a function or method whose main purpose is a side effect.
  对于主要目的是产生副作用的函数或方法，优先使用命令式动词短语。](https://dart.dev/effective-dart/design#prefer-an-imperative-verb-phrase-for-a-function-or-method-whose-main-purpose-is-a-side-effect)
- [PREFER a noun phrase or non-imperative verb phrase for a function or method if returning a value is its primary purpose.
  如果函数或方法的主要目的是返回值，则优先使用名词短语或非命令式动词短语。](https://dart.dev/effective-dart/design#prefer-a-noun-phrase-or-non-imperative-verb-phrase-for-a-function-or-method-if-returning-a-value-is-its-primary-purpose)
- [CONSIDER an imperative verb phrase for a function or method if you want to draw attention to the work it performs.
  如果要强调函数或方法执行的工作，请考虑使用命令式动词短语。](https://dart.dev/effective-dart/design#consider-an-imperative-verb-phrase-for-a-function-or-method-if-you-want-to-draw-attention-to-the-work-it-performs)
- [AVOID starting a method name with `get`.
  避免以 `get` 开头命名方法。](https://dart.dev/effective-dart/design#avoid-starting-a-method-name-with-get)
- [PREFER naming a method `to___()` if it copies the object’s state to a new object.
  如果方法将对象的 state 复制到新对象，则优先将方法命名为 `to___()` 。](https://dart.dev/effective-dart/design#prefer-naming-a-method-to___-if-it-copies-the-objects-state-to-a-new-object)
- [PREFER naming a method `as___()` if it returns a different representation backed by the original object.
  如果方法返回由原始对象支持的不同表示形式，则优先将方法命名为 `as___()` 。](https://dart.dev/effective-dart/design#prefer-naming-a-method-as___-if-it-returns-a-different-representation-backed-by-the-original-object)
- [AVOID describing the parameters in the function’s or method’s name.
  避免在函数或方法的名称中描述参数。](https://dart.dev/effective-dart/design#avoid-describing-the-parameters-in-the-functions-or-methods-name)
- [DO follow existing mnemonic conventions when naming type parameters.
  命名类型参数时，请遵循现有的助记符约定。](https://dart.dev/effective-dart/design#do-follow-existing-mnemonic-conventions-when-naming-type-parameters)

**库 Libraries**

- [PREFER making declarations private.
  优先将声明设为私有。](https://dart.dev/effective-dart/design#prefer-making-declarations-private)
- [CONSIDER declaring multiple classes in the same library.
  考虑在同一个库中声明多个类。](https://dart.dev/effective-dart/design#consider-declaring-multiple-classes-in-the-same-library)

**类和 mixin - Classes and mixins**

- [AVOID defining a one-member abstract class when a simple function will do.
  当一个简单的函数可以实现时，请避免定义一个只有一个成员的抽象类。](https://dart.dev/effective-dart/design#avoid-defining-a-one-member-abstract-class-when-a-simple-function-will-do)
- [AVOID defining a class that contains only static members.
  避免定义一个仅包含静态成员的类。](https://dart.dev/effective-dart/design#avoid-defining-a-class-that-contains-only-static-members)
- [AVOID extending a class that isn’t intended to be subclassed.
  避免扩展一个不打算被子类化的类。](https://dart.dev/effective-dart/design#avoid-extending-a-class-that-isnt-intended-to-be-subclassed)
- [DO document if your class supports being extended.
  如果您的类支持扩展，请记录下来。](https://dart.dev/effective-dart/design#do-document-if-your-class-supports-being-extended)
- [AVOID implementing a class that isn’t intended to be an interface.
  避免实现一个不打算作为接口的类。](https://dart.dev/effective-dart/design#avoid-implementing-a-class-that-isnt-intended-to-be-an-interface)
- [DO document if your class supports being used as an interface.
  如果您的类支持用作接口，则 DO 文档。](https://dart.dev/effective-dart/design#do-document-if-your-class-supports-being-used-as-an-interface)
- [PREFER defining a pure `mixin` or pure `class` to a `mixin class`.
  优先定义纯 `mixin` 或纯 `class` 而非 `mixin class` 。](https://dart.dev/effective-dart/design#prefer-defining-a-pure-mixin-or-pure-class-to-a-mixin-class)

**构造函数 Constructors**

- [CONSIDER making your constructor `const` if the class supports it.
  如果类支持，请考虑将构造函数设为 `const` 。](https://dart.dev/effective-dart/design#consider-making-your-constructor-const-if-the-class-supports-it)

**成员 Members**

- [PREFER making fields and top-level variables `final`.
  最好将字段和顶级变量设为 `final` 。](https://dart.dev/effective-dart/design#prefer-making-fields-and-top-level-variables-final)
- [DO use getters for operations that conceptually access properties.
  对概念上访问属性的操作使用 getter。](https://dart.dev/effective-dart/design#do-use-getters-for-operations-that-conceptually-access-properties)
- [DO use setters for operations that conceptually change properties.
  对概念上更改属性的操作使用 setter。](https://dart.dev/effective-dart/design#do-use-setters-for-operations-that-conceptually-change-properties)
- [DON’T define a setter without a corresponding getter.
  不要定义没有对应 getter 的 setter。](https://dart.dev/effective-dart/design#dont-define-a-setter-without-a-corresponding-getter)
- [AVOID using runtime type tests to fake overloading.
  避免使用运行时类型测试来伪造重载。](https://dart.dev/effective-dart/design#avoid-using-runtime-type-tests-to-fake-overloading)
- [AVOID public `late final` fields without initializers.
  避免使用没有初始化程序的 public `late final` 字段。](https://dart.dev/effective-dart/design#avoid-public-late-final-fields-without-initializers)
- [AVOID returning nullable `Future`, `Stream`, and collection types.
  避免返回可为 null 的 `Future` 、 `Stream` 和集合类型。](https://dart.dev/effective-dart/design#avoid-returning-nullable-future-stream-and-collection-types)
- [AVOID returning `this` from methods just to enable a fluent interface.
  避免仅仅为了启用流畅的接口而从方法中返回 `this` 。](https://dart.dev/effective-dart/design#avoid-returning-this-from-methods-just-to-enable-a-fluent-interface)

**类型 Types**

- [DO type annotate variables without initializers.
  确实会为没有初始化程序的变量添加类型注释。](https://dart.dev/effective-dart/design#do-type-annotate-variables-without-initializers)
- [DO type annotate fields and top-level variables if the type isn’t obvious.
  如果类型不明显，确实会为字段和顶级变量添加类型注释。](https://dart.dev/effective-dart/design#do-type-annotate-fields-and-top-level-variables-if-the-type-isnt-obvious)
- [DON’T redundantly type annotate initialized local variables.
  不要对已初始化的局部变量重复添加类型注释。](https://dart.dev/effective-dart/design#dont-redundantly-type-annotate-initialized-local-variables)
- [DO annotate return types on function declarations.
  确实要在函数声明中注释返回类型。](https://dart.dev/effective-dart/design#do-annotate-return-types-on-function-declarations)
- [DO annotate parameter types on function declarations.
  确实要在函数声明中注释参数类型。](https://dart.dev/effective-dart/design#do-annotate-parameter-types-on-function-declarations)
- [DON’T annotate inferred parameter types on function expressions.
  不要在函数表达式中注释推断出的参数类型。](https://dart.dev/effective-dart/design#dont-annotate-inferred-parameter-types-on-function-expressions)
- [DON’T type annotate initializing formals.
  不要为初始化形式参数添加类型注释。](https://dart.dev/effective-dart/design#dont-type-annotate-initializing-formals)
- [DO write type arguments on generic invocations that aren’t inferred.
  确实要在未推断出的泛型调用中编写类型参数。](https://dart.dev/effective-dart/design#do-write-type-arguments-on-generic-invocations-that-arent-inferred)
- [DON’T write type arguments on generic invocations that are inferred.
  不要在推断出的泛型调用中编写类型参数。](https://dart.dev/effective-dart/design#dont-write-type-arguments-on-generic-invocations-that-are-inferred)
- [AVOID writing incomplete generic types.
  避免编写不完整的泛型类型。](https://dart.dev/effective-dart/design#avoid-writing-incomplete-generic-types)
- [DO annotate with `dynamic` instead of letting inference fail.
  使用 `dynamic` 进行注释，而不是让推断失败。](https://dart.dev/effective-dart/design#do-annotate-with-dynamic-instead-of-letting-inference-fail)
- [PREFER signatures in function type annotations.
  在函数类型注释中首选签名。](https://dart.dev/effective-dart/design#prefer-signatures-in-function-type-annotations)
- [DON’T specify a return type for a setter.
  不要为 setter 指定返回类型。](https://dart.dev/effective-dart/design#dont-specify-a-return-type-for-a-setter)
- [DON’T use the legacy typedef syntax.
  不要使用旧的 typedef 语法。](https://dart.dev/effective-dart/design#dont-use-the-legacy-typedef-syntax)
- [PREFER inline function types over typedefs.
  首选内联函数类型而不是 typedef。](https://dart.dev/effective-dart/design#prefer-inline-function-types-over-typedefs)
- [PREFER using function type syntax for parameters.
  首选使用函数类型语法作为参数。](https://dart.dev/effective-dart/design#prefer-using-function-type-syntax-for-parameters)
- [AVOID using `dynamic` unless you want to disable static checking.
  避免使用 `dynamic` ，除非您想禁用静态检查。](https://dart.dev/effective-dart/design#avoid-using-dynamic-unless-you-want-to-disable-static-checking)
- [DO use `Future` as the return type of asynchronous members that do not produce values.
  将 `Future` 用作不产生值的异步成员的返回类型。](https://dart.dev/effective-dart/design#do-use-futurevoid-as-the-return-type-of-asynchronous-members-that-do-not-produce-values)
- [AVOID using `FutureOr` as a return type.
  避免使用 `FutureOr` 作为返回类型。](https://dart.dev/effective-dart/design#avoid-using-futureort-as-a-return-type)

**参数 Parameters**

- [AVOID positional boolean parameters.
  避免使用位置布尔参数。](https://dart.dev/effective-dart/design#avoid-positional-boolean-parameters)
- [AVOID optional positional parameters if the user may want to omit earlier parameters.
  如果用户可能想要省略前面的参数，请避免使用可选位置参数。](https://dart.dev/effective-dart/design#avoid-optional-positional-parameters-if-the-user-may-want-to-omit-earlier-parameters)
- [AVOID mandatory parameters that accept a special "no argument" value.
  避免使用接受特殊“无参数”值的强制参数。](https://dart.dev/effective-dart/design#avoid-mandatory-parameters-that-accept-a-special-no-argument-value)
- [DO use inclusive start and exclusive end parameters to accept a range.
  使用包含性开始和排他性结束参数来接受范围。](https://dart.dev/effective-dart/design#do-use-inclusive-start-and-exclusive-end-parameters-to-accept-a-range)

**相等 Equality**

- [DO override `hashCode` if you override `==`.
  如果覆盖 `==` ，请覆盖 `hashCode` 。](https://dart.dev/effective-dart/design#do-override-hashcode-if-you-override-)
- [DO make your `==` operator obey the mathematical rules of equality.
  使 `==` 运算符遵守数学相等规则。](https://dart.dev/effective-dart/design#do-make-your--operator-obey-the-mathematical-rules-of-equality)
- [AVOID defining custom equality for mutable classes.
  避免为可变类定义自定义相等性。](https://dart.dev/effective-dart/design#avoid-defining-custom-equality-for-mutable-classes)
- [DON’T make the parameter to `==` nullable.
  不要使 `==` 的参数可为空。](https://dart.dev/effective-dart/design#dont-make-the-parameter-to--nullable)
