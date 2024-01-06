+++
title = "有效的 Dart：文档"
date = 2024-01-05T20:29:36+08:00
weight = 20
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文: [https://dart.dev/effective-dart/documentation](https://dart.dev/effective-dart/documentation)

## Effective Dart: Documentation 有效的 Dart：文档

It’s easy to think your code is obvious today without realizing how much you rely on context already in your head. People new to your code, and even your forgetful future self won’t have that context. A concise, accurate comment only takes a few seconds to write but can save one of those people hours of time.
很容易认为你的代码今天很明显，而没有意识到你已经有多少依赖于你头脑中的上下文了。不熟悉你的代码的人，甚至是健忘的未来的你，都没有那个上下文。一个简洁、准确的注释只需几秒钟即可写出，但可以为这些人节省数小时的时间。

We all know code should be self-documenting and not all comments are helpful. But the reality is that most of us don’t write as many comments as we should. It’s like exercise: you technically *can* do too much, but it’s a lot more likely that you’re doing too little. Try to step it up.
我们都知道代码应该是自注释的，并非所有注释都有用。但现实情况是，我们大多数人写的注释并不像我们应该写的那么多。这就像锻炼：从技术上讲，你可以做得太多，但更有可能的是你做得太少了。尝试加强它。

## Comments 注释

The following tips apply to comments that you don’t want included in the generated documentation.
以下提示适用于你不想包含在生成的文档中的注释。

### DO format comments like sentences DO 格式化注释，使其像句子一样

```dart
// Not if anything comes before it.
if (_chunks.isNotEmpty) return false;
```

Capitalize the first word unless it’s a case-sensitive identifier. End it with a period (or “!” or “?”, I suppose). This is true for all comments: doc comments, inline stuff, even TODOs. Even if it’s a sentence fragment.
首字母大写，除非它是区分大小写的标识符。以句号（或“！”或“？”，我想）结尾。这适用于所有注释：文档注释、内联内容，甚至是 TODO。即使它是一个句子片段。

### DON’T use block comments for documentation 不要使用块注释进行文档

```dart
void greet(String name) {
  // Assume we have a valid name.
  print('Hi, $name!');
}
void greet(String name) {
  /* Assume we have a valid name. */
  print('Hi, $name!');
}
```

You can use a block comment (`/* ... */`) to temporarily comment out a section of code, but all other comments should use `//`.
您可以使用块注释 ( `/* ... */` ) 暂时注释掉一段代码，但所有其他注释都应使用 `//` 。

## Doc comments 文档注释

Doc comments are especially handy because [`dart doc`](https://dart.dev/tools/dart-doc) parses them and generates [beautiful doc pages](https://api.dart.dev/stable) from them. A doc comment is any comment that appears before a declaration and uses the special `///` syntax that `dart doc` looks for.
文档注释特别方便，因为 `dart doc` 会分析它们并从中生成漂亮的文档页面。文档注释是出现在声明之前并使用 `dart doc` 查找的特殊 `///` 语法的任何注释。

### DO use `///` doc comments to document members and types DO 使用 `///` 文档注释来记录成员和类型

Linter rule: [slash_for_doc_comments](https://dart.dev/tools/linter-rules/slash_for_doc_comments)
Linter 规则：slash_for_doc_comments

Using a doc comment instead of a regular comment enables [`dart doc`](https://dart.dev/tools/dart-doc) to find it and generate documentation for it.
使用文档注释而不是常规注释可使 `dart doc` 找到它并为其生成文档。

```dart
/// The number of characters in this chunk when unsplit.
int get length => ...
// The number of characters in this chunk when unsplit.
int get length => ...
```

For historical reasons, `dart doc` supports two syntaxes of doc comments: `///` (“C# style”) and `/** ... */` (“JavaDoc style”). We prefer `///` because it’s more compact. `/**` and `*/` add two content-free lines to a multiline doc comment. The `///` syntax is also easier to read in some situations, such as when a doc comment contains a bulleted list that uses `*` to mark list items.
出于历史原因， `dart doc` 支持两种文档注释语法： `///` （“C# 样式”）和 `/** ... */` （“JavaDoc 样式”）。我们更喜欢 `///` ，因为它更紧凑。 `/**` 和 `*/` 会向多行文档注释添加两行无内容的行。 `///` 语法在某些情况下也更容易阅读，例如当文档注释包含使用 `*` 标记列表项的项目符号列表时。

If you stumble onto code that still uses the JavaDoc style, consider cleaning it up.
如果您偶然发现仍然使用 JavaDoc 样式的代码，请考虑对其进行清理。

### PREFER writing doc comments for public APIs 更喜欢为公共 API 编写文档注释

Linter rules: [package_api_docs](https://dart.dev/tools/linter-rules/package_api_docs), [public_member_api_docs](https://dart.dev/tools/linter-rules/public_member_api_docs)
Linter 规则：package_api_docs、public_member_api_docs

You don’t have to document every single library, top-level variable, type, and member, but you should document most of them.
您不必记录每个库、顶级变量、类型和成员，但您应该记录其中大部分内容。

### CONSIDER writing a library-level doc comment 考虑编写库级文档注释

Unlike languages like Java where the class is the only unit of program organization, in Dart, a library is itself an entity that users work with directly, import, and think about. That makes the `library` directive a great place for documentation that introduces the reader to the main concepts and functionality provided within. Consider including:
与 Java 等仅将类作为程序组织单位的语言不同，在 Dart 中，库本身就是用户直接使用、导入和考虑的实体。这使得 `library` 指令成为向读者介绍其中提供的核心概念和功能的文档的绝佳位置。考虑包括：

- A single-sentence summary of what the library is for.
  库的用途的单句摘要。
- Explanations of terminology used throughout the library.
  贯穿整个库的术语说明。
- A couple of complete code samples that walk through using the API.
  几个使用 API 的完整代码示例。
- Links to the most important or most commonly used classes and functions.
  指向最重要的或最常用的类和函数的链接。
- Links to external references on the domain the library is concerned with.
  指向库涉及的域的外部引用的链接。

To document a library, place a doc comment before the `library` directive and any annotations that might be attached at the start of the file.
要记录库，请在 `library` 指令之前放置文档注释，以及可能附加在文件开头的任何注释。

```dart
/// A really great test library.
@TestOn('browser')
library;
```

### CONSIDER writing doc comments for private APIs 考虑为私有 API 编写文档注释

Doc comments aren’t just for external consumers of your library’s public API. They can also be helpful for understanding private members that are called from other parts of the library.
文档注释不仅仅适用于库的公共 API 的外部使用者。它们还有助于理解从库的其他部分调用的私有成员。

### DO start doc comments with a single-sentence summary 使用单句摘要开始文档注释

Start your doc comment with a brief, user-centric description ending with a period. A sentence fragment is often sufficient. Provide just enough context for the reader to orient themselves and decide if they should keep reading or look elsewhere for the solution to their problem.
以简短、以用户为中心且以句号结尾的描述开始文档注释。句子片段通常就足够了。仅提供足够的上下文，以便读者定位自己并决定他们是否应该继续阅读或在其他地方寻找解决其问题的方案。

```dart
/// Deletes the file at [path] from the file system.
void delete(String path) {
  ...
}
/// Depending on the state of the file system and the user's permissions,
/// certain operations may or may not be possible. If there is no file at
/// [path] or it can't be accessed, this function throws either [IOError]
/// or [PermissionError], respectively. Otherwise, this deletes the file.
void delete(String path) {
  ...
}
```

### DO separate the first sentence of a doc comment into its own paragraph 将文档注释的第一句话分成单独的段落

Add a blank line after the first sentence to split it out into its own paragraph. If more than a single sentence of explanation is useful, put the rest in later paragraphs.
在第一句话后添加一个空行，将其拆分为自己的段落。如果多于一个解释句有用，则将其余部分放在后面的段落中。

This helps you write a tight first sentence that summarizes the documentation. Also, tools like `dart doc` use the first paragraph as a short summary in places like lists of classes and members.
这有助于您编写一个紧凑的第一句话，总结文档。此外， `dart doc` 等工具在类和成员列表等位置使用第一段作为简短摘要。

```dart
/// Deletes the file at [path].
///
/// Throws an [IOError] if the file could not be found. Throws a
/// [PermissionError] if the file is present but could not be deleted.
void delete(String path) {
  ...
}
/// Deletes the file at [path]. Throws an [IOError] if the file could not
/// be found. Throws a [PermissionError] if the file is present but could
/// not be deleted.
void delete(String path) {
  ...
}
```

### AVOID redundancy with the surrounding context 避免与周围上下文重复

The reader of a class’s doc comment can clearly see the name of the class, what interfaces it implements, etc. When reading docs for a member, the signature is right there, and the enclosing class is obvious. None of that needs to be spelled out in the doc comment. Instead, focus on explaining what the reader *doesn’t* already know.
类的文档注释的读者可以清楚地看到类的名称、它实现的接口等。在阅读成员的文档时，签名就在那里，封闭类是显而易见的。所有这些都不需要在文档注释中写出来。相反，重点解释读者还不了解的内容。

```dart
class RadioButtonWidget extends Widget {
  /// Sets the tooltip to [lines], which should have been word wrapped using
  /// the current font.
  void tooltip(List<String> lines) {
    ...
  }
}
class RadioButtonWidget extends Widget {
  /// Sets the tooltip for this radio button widget to the list of strings in
  /// [lines].
  void tooltip(List<String> lines) {
    ...
  }
}
```

If you really don’t have anything interesting to say that can’t be inferred from the declaration itself, then omit the doc comment. It’s better to say nothing than waste a reader’s time telling them something they already know.
如果您确实没有任何有趣的内容可说，无法从声明本身推断出来，那么就省略文档注释。什么都不说总比浪费读者的时间告诉他们他们已经知道的事情要好。

### PREFER starting function or method comments with third-person verbs 首选以第三人称动词开头函数或方法注释

The doc comment should focus on what the code *does*.
文档注释应重点关注代码的作用。

```dart
/// Returns `true` if every element satisfies the [predicate].
bool all(bool predicate(T element)) => ...

/// Starts the stopwatch if not already running.
void start() {
  ...
}
```

### PREFER starting a non-boolean variable or property comment with a noun phrase 首选以名词短语开头非布尔变量或属性注释

The doc comment should stress what the property *is*. This is true even for getters which may do calculation or other work. What the caller cares about is the *result* of that work, not the work itself.
文档注释应强调属性是什么。即使对于可能进行计算或其他工作的 getter 也是如此。调用者关心的是该工作的结果，而不是工作本身。

```dart
/// The current day of the week, where `0` is Sunday.
int weekday;

/// The number of checked buttons on the page.
int get checkedCount => ...
```

### PREFER starting a boolean variable or property comment with “Whether” followed by a noun or gerund phrase 最好以“Whether”开头，后跟名词或动名词短语，来开始布尔变量或属性注释

The doc comment should clarify the states this variable represents. This is true even for getters which may do calculation or other work. What the caller cares about is the *result* of that work, not the work itself.
文档注释应阐明此变量表示的状态。即使对于可能进行计算或其他工作的 getter 也是如此。调用者关心的是该工作的结果，而不是工作本身。

```dart
/// Whether the modal is currently displayed to the user.
bool isVisible;

/// Whether the modal should confirm the user's intent on navigation.
bool get shouldConfirm => ...

/// Whether resizing the current browser window will also resize the modal.
bool get canResize => ...
```

*info* **Note:** This guideline intentionally doesn’t include using “Whether or not”. In many cases, usage of “or not” with “whether” is superfluous and can be omitted, especially when used in this context.
注意：此准则故意不包括使用“Whether or not”。在许多情况下，“whether”和“or not”的使用是多余的，可以省略，尤其是在此上下文中使用时。

### DON’T write documentation for both the getter and setter of a property 不要为属性的 getter 和 setter 都编写文档

If a property has both a getter and a setter, then create a doc comment for only one of them. `dart doc` treats the getter and setter like a single field, and if both the getter and the setter have doc comments, then `dart doc` discards the setter’s doc comment.
如果某个属性同时具有 getter 和 setter，则只为其中一个创建文档注释。 `dart doc` 将 getter 和 setter 视为单个字段，如果 getter 和 setter 都具有文档注释，则 `dart doc` 会丢弃 setter 的文档注释。

```dart
/// The pH level of the water in the pool.
///
/// Ranges from 0-14, representing acidic to basic, with 7 being neutral.
int get phLevel => ...
set phLevel(int level) => ...
/// The depth of the water in the pool, in meters.
int get waterDepth => ...

/// Updates the water depth to a total of [meters] in height.
set waterDepth(int meters) => ...
```

### PREFER starting library or type comments with noun phrases 最好以名词短语开头库或类型注释

Doc comments for classes are often the most important documentation in your program. They describe the type’s invariants, establish the terminology it uses, and provide context to the other doc comments for the class’s members. A little extra effort here can make all of the other members simpler to document.
类的文档注释通常是程序中最重要的文档。它们描述类型的变量，建立它使用的术语，并为类的成员的其他文档注释提供上下文。在这里多花点功夫可以使所有其他成员更容易记录。

```dart
/// A chunk of non-breaking output text terminated by a hard or soft newline.
///
/// ...
class Chunk { ... }
```

### CONSIDER including code samples in doc comments 考虑在文档注释中包含代码示例

```dart
/// Returns the lesser of two numbers.
///
/// ```dart
/// min(5, 3) == 3
/// ```
num min(num a, num b) => ...
```

`

Humans are great at generalizing from examples, so even a single code sample makes an API easier to learn.
人类擅长从示例中进行概括，因此即使是单个代码示例也使 API 更容易学习。

### DO use square brackets in doc comments to refer to in-scope identifiers 在文档注释中使用方括号来引用作用域内的标识符

Linter rule: [comment_references](https://dart.dev/tools/linter-rules/comment_references)
Linter 规则：comment_references

If you surround things like variable, method, or type names in square brackets, then `dart doc` looks up the name and links to the relevant API docs. Parentheses are optional, but can make it clearer when you’re referring to a method or constructor.
如果您将变量、方法或类型名称等内容用方括号括起来，那么 `dart doc` 会查找名称并链接到相关的 API 文档。括号是可选的，但当您引用方法或构造函数时，可以使它更清晰。

```dart
/// Throws a [StateError] if ...
/// similar to [anotherMethod()], but ...
```

To link to a member of a specific class, use the class name and member name, separated by a dot:
要链接到特定类的成员，请使用类名和成员名，中间用点分隔：

```dart
/// Similar to [Duration.inDays], but handles fractional days.
```

The dot syntax can also be used to refer to named constructors. For the unnamed constructor, use `.new` after the class name:
点语法还可用于引用命名构造函数。对于未命名的构造函数，请在类名后使用 `.new` ：

```dart
/// To create a point, call [Point.new] or use [Point.polar] to ...
```

### DO use prose to explain parameters, return values, and exceptions 使用散文解释参数、返回值和异常

Other languages use verbose tags and sections to describe what the parameters and returns of a method are.
其他语言使用冗长的标记和部分来描述方法的参数和返回值是什么。

```dart
/// Defines a flag with the given name and abbreviation.
///
/// @param name The name of the flag.
/// @param abbr The abbreviation for the flag.
/// @returns The new flag.
/// @throws ArgumentError If there is already an option with
///     the given name or abbreviation.
Flag addFlag(String name, String abbr) => ...
```

The convention in Dart is to integrate that into the description of the method and highlight parameters using square brackets.
Dart 中的惯例是将这些内容集成到方法的描述中，并使用方括号突出显示参数。

```dart
/// Defines a flag.
///
/// Throws an [ArgumentError] if there is already an option named [name] or
/// there is already an option using abbreviation [abbr]. Returns the new flag.
Flag addFlag(String name, String abbr) => ...
```

### DO put doc comments before metadata annotations 在元数据注释之前放置文档注释

```dart
/// A button that can be flipped on and off.
@Component(selector: 'toggle')
class ToggleComponent {}
@Component(selector: 'toggle')
/// A button that can be flipped on and off.
class ToggleComponent {}
```

## Markdown

You are allowed to use most [markdown](https://daringfireball.net/projects/markdown/) formatting in your doc comments and `dart doc` will process it accordingly using the [markdown package.](https://pub.dev/packages/markdown)
您可以在文档注释中使用大多数 markdown 格式， `dart doc` 将使用 markdown 包对其进行相应处理。

There are tons of guides out there already to introduce you to Markdown. Its universal popularity is why we chose it. Here’s just a quick example to give you a flavor of what’s supported:
已经有很多指南向您介绍 Markdown。我们选择它是因为它具有普遍的流行性。这里有一个快速示例，让您了解支持的内容：

```dart
/// This is a paragraph of regular text.
///
/// This sentence has *two* _emphasized_ words (italics) and **two**
/// __strong__ ones (bold).
///
/// A blank line creates a separate paragraph. It has some `inline code`
/// delimited using backticks.
///
/// * Unordered lists.
/// * Look like ASCII bullet lists.
/// * You can also use `-` or `+`.
///
/// 1. Numbered lists.
/// 2. Are, well, numbered.
/// 1. But the values don't matter.
///
///     * You can nest lists too.
///     * They must be indented at least 4 spaces.
///     * (Well, 5 including the space after `///`.)
///
/// Code blocks are fenced in triple backticks:
///
/// ```dart
/// this.code
///     .will
///     .retain(its, formatting);
/// ```
///
/// The code language (for syntax highlighting) defaults to Dart. You can
/// specify it by putting the name of the language after the opening backticks:
///
/// ```html
/// <h1>HTML is magical!</h1>
/// ```
///
/// Links can be:
///
/// * https://www.just-a-bare-url.com
/// * [with the URL inline](https://google.com)
/// * [or separated out][ref link]
///
/// [ref link]: https://google.com
///
/// # A Header
///
/// ## A subheader
///
/// ### A subsubheader
///
/// #### If you need this many levels of headers, you're doing it wrong
```

`

### AVOID using markdown excessively 避免过度使用 markdown

When in doubt, format less. Formatting exists to illuminate your content, not replace it. Words are what matter.
如有疑问，减少格式化。格式化的目的是阐明您的内容，而不是替换它。文字才是最重要的。

### AVOID using HTML for formatting 避免使用 HTML 进行格式化

It *may* be useful to use it in rare cases for things like tables, but in almost all cases, if it’s too complex to express in Markdown, you’re better off not expressing it.
在极少数情况下，它可能对表格等内容有用，但在几乎所有情况下，如果在 Markdown 中表达过于复杂，那么最好不要表达。

### PREFER backtick fences for code blocks 首选反引号围栏作为代码块

Markdown has two ways to indicate a block of code: indenting the code four spaces on each line, or surrounding it in a pair of triple-backtick “fence” lines. The former syntax is brittle when used inside things like Markdown lists where indentation is already meaningful or when the code block itself contains indented code.
Markdown 有两种方法来表示代码块：在每行缩进四个空格，或用一对三重反引号“围栏”行将其括起来。当在缩进已经具有意义的 Markdown 列表中使用时，或当代码块本身包含缩进代码时，前一种语法很脆弱。

The backtick syntax avoids those indentation woes, lets you indicate the code’s language, and is consistent with using backticks for inline code.
反引号语法避免了这些缩进问题，允许您指示代码的语言，并且与将反引号用于内联代码一致。

```
/// You can use [CodeBlockExample] like this:
///
/// ```dart
/// var example = CodeBlockExample();
/// print(example.isItGreat); // "Yes."
/// ```
/// You can use [CodeBlockExample] like this:
///
///     var example = CodeBlockExample();
///     print(example.isItGreat); // "Yes."
```

## Writing 写作

We think of ourselves as programmers, but most of the characters in a source file are intended primarily for humans to read. English is the language we code in to modify the brains of our coworkers. As for any programming language, it’s worth putting effort into improving your proficiency.
我们认为自己是程序员，但源文件中大多数字符主要供人类阅读。英语是我们用来修改同事大脑的编码语言。对于任何编程语言来说，都值得努力提高您的熟练程度。

This section lists a few guidelines for our docs. You can learn more about best practices for technical writing, in general, from articles such as [Technical writing style](https://en.wikiversity.org/wiki/Technical_writing_style).
本节列出了我们文档的一些准则。您通常可以从技术写作风格等文章中了解有关技术写作最佳实践的更多信息。

### PREFER brevity 偏好简洁

Be clear and precise, but also terse.
清晰准确，但也要简洁。

### AVOID abbreviations and acronyms unless they are obvious 避免使用缩写和首字母缩略词，除非它们很明显

Many people don’t know what “i.e.”, “e.g.” and “et al.” mean. That acronym that you’re sure everyone in your field knows may not be as widely known as you think.
许多人不知道“i.e.”、“e.g.”和“et al.”的意思。你确信你们领域中的每个人都知道的缩写可能没有你想象的那么广为人知。

### PREFER using “this” instead of “the” to refer to a member’s instance 首选使用“this”而不是“the”来引用成员的实例

When documenting a member for a class, you often need to refer back to the object the member is being called on. Using “the” can be ambiguous.
在为类的成员编写文档时，通常需要引用成员被调用的对象。使用“the”可能会引起歧义。

```dart
class Box {
  /// The value this wraps.
  Object? _value;

  /// True if this box contains a value.
  bool get hasValue => _value != null;
}
```
