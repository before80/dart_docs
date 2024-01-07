+++
title = "Dart 有效性：设计"
date = 2024-01-05T20:29:36+08:00
weight = 40
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文: [https://dart.dev/effective-dart/design](https://dart.dev/effective-dart/design)

## Effective Dart: Design Dart 有效性：设计

Here are some guidelines for writing consistent, usable APIs for libraries.
以下是一些为库编写一致、可用的 API 的准则。

## 名称 Names 

Naming is an important part of writing readable, maintainable code. The following best practices can help you achieve that goal.

​	命名是编写可读、可维护代码的重要组成部分。以下最佳做法可以帮助您实现该目标。

### DO use terms consistently 确实要一致地使用术语

Use the same name for the same thing, throughout your code. If a precedent already exists outside your API that users are likely to know, follow that precedent.

​	在整个代码中，对相同的事物使用相同的名称。如果在用户可能了解的 API 外部已经存在先例，请遵循该先例。

```
pageCount         // A field.
updatePageCount() // Consistent with pageCount.
toSomething()     // Consistent with Iterable's toList().
asSomething()     // Consistent with List's asMap().
Point             // A familiar concept.
renumberPages()      // Confusingly different from pageCount.
convertToSomething() // Inconsistent with toX() precedent.
wrappedAsSomething() // Inconsistent with asX() precedent.
Cartesian            // Unfamiliar to most users.
```

The goal is to take advantage of what the user already knows. This includes their knowledge of the problem domain itself, the conventions of the core libraries, and other parts of your own API. By building on top of those, you reduce the amount of new knowledge they have to acquire before they can be productive.

​	目标是利用用户已知的信息。这包括他们对问题域本身的了解、核心库的约定以及您自己 API 的其他部分。通过在这些基础上构建，您可以减少他们在能够发挥作用之前必须获取的新知识的数量。

### AVOID abbreviations 避免缩写

Unless the abbreviation is more common than the unabbreviated term, don’t abbreviate. If you do abbreviate, [capitalize it correctly](https://dart.dev/effective-dart/style#identifiers).

​	除非缩写比未缩写术语更常见，否则不要缩写。如果您确实缩写，请正确地使用大写字母。

```
pageCount
buildRectangles
IOStream
HttpRequest
numPages    // "Num" is an abbreviation of "number (of)".
buildRects
InputOutputStream
HypertextTransferProtocolRequest
```

### PREFER putting the most descriptive noun last 优先将最具描述性的名词放在最后

The last word should be the most descriptive of what the thing is. You can prefix it with other words, such as adjectives, to further describe the thing.

​	最后一个词应该是对事物最具描述性的。您可以用其他词（例如形容词）作为前缀，以进一步描述事物。

```
pageCount             // A count (of pages).
ConversionSink        // A sink for doing conversions.
ChunkedConversionSink // A ConversionSink that's chunked.
CssFontFaceRule       // A rule for font faces in CSS.
numPages                  // Not a collection of pages.
CanvasRenderingContext2D  // Not a "2D".
RuleFontFaceCss           // Not a CSS.
```

### CONSIDER making the code read like a sentence 考虑让代码读起来像一个句子

When in doubt about naming, write some code that uses your API, and try to read it like a sentence.

​	在对命名感到疑惑时，编写一些使用您的 API 的代码，并尝试像阅读句子一样阅读它。

```dart
// "If errors is empty..."
if (errors.isEmpty) ...

// "Hey, subscription, cancel!"
subscription.cancel();

// "Get the monsters where the monster has claws."
monsters.where((monster) => monster.hasClaws);
// Telling errors to empty itself, or asking if it is?
if (errors.empty) ...

// Toggle what? To what?
subscription.toggle();

// Filter the monsters with claws *out* or include *only* those?
monsters.filter((monster) => monster.hasClaws);
```

It’s helpful to try out your API and see how it “reads” when used in code, but you can go too far. It’s not helpful to add articles and other parts of speech to force your names to *literally* read like a grammatically correct sentence.

​	尝试您的 API 并查看在代码中使用时它如何“读取”很有帮助，但您可能会做得太过。添加冠词和其他词性以强制您的名称从字面上读起来像一个语法正确的句子是没有帮助的。

```dart
if (theCollectionOfErrors.isEmpty) ...

monsters.producesANewSequenceWhereEach((monster) => monster.hasClaws);
```

### PREFER a noun phrase for a non-boolean property or variable 为非布尔属性或变量选择名词短语

The reader’s focus is on *what* the property is. If the user cares more about *how* a property is determined, then it should probably be a method with a verb phrase name.

​	读者关注的是属性是什么。如果用户更关心属性是如何确定的，那么它可能应该是一个带有动词短语名称的方法。

```
list.length
context.lineWidth
quest.rampagingSwampBeast
list.deleteItems
```

### PREFER a non-imperative verb phrase for a boolean property or variable 为布尔属性或变量选择非命令式动词短语

Boolean names are often used as conditions in control flow, so you want a name that reads well there. Compare:

​	布尔名称通常用作控制流中的条件，因此您需要一个在那里读起来很顺畅的名称。比较：

```
if (window.closeable) ...  // Adjective.
if (window.canClose) ...   // Verb.
```

Good names tend to start with one of a few kinds of verbs:

​	好名称往往以几种动词之一开头：

- a form of “to be”: `isEnabled`, `wasShown`, `willFire`. These are, by far, the most common.
- “to be”的形式： `isEnabled` 、 `wasShown` 、 `willFire` 。这些是迄今为止最常见的。
- an [auxiliary verb](https://en.wikipedia.org/wiki/Auxiliary_verb): `hasElements`, `canClose`, `shouldConsume`, `mustSave`.
- 助动词： `hasElements` 、 `canClose` 、 `shouldConsume` 、 `mustSave` 。
- an active verb: `ignoresInput`, `wroteFile`. These are rare because they are usually ambiguous. `loggedResult` is a bad name because it could mean “whether or not a result was logged” or “the result that was logged”. Likewise, `closingConnection` could be “whether the connection is closing” or “the connection that is closing”. Active verbs are allowed when the name can *only* be read as a predicate.
- 一个主动语态动词： `ignoresInput` 、 `wroteFile` 。这些情况很少见，因为它们通常是模棱两可的。 `loggedResult` 是一个不好的名称，因为它可能表示“是否记录了结果”或“记录的结果”。同样地， `closingConnection` 可能是“连接是否正在关闭”或“正在关闭的连接”。当名称只能被视为谓词时，允许使用主动语态动词。

What separates all these verb phrases from method names is that they are not *imperative*. A boolean name should never sound like a command to tell the object to do something, because accessing a property doesn’t change the object. (If the property *does* modify the object in a meaningful way, it should be a method.)

​	所有这些动词短语与方法名称的区别在于，它们不是命令式的。布尔名称绝不应听起来像一个命令，告诉对象做某事，因为访问属性不会改变对象。（如果属性以有意义的方式修改对象，则它应为方法。）

```
isEmpty
hasElements
canClose
closesWindow
canShowPopup
hasShownPopup
empty         // Adjective or verb?
withElements  // Sounds like it might hold elements.
closeable     // Sounds like an interface.
              // "canClose" reads better as a sentence.
closingWindow // Returns a bool or a window?
showPopup     // Sounds like it shows the popup.
```

### CONSIDER omitting the verb for a named boolean *parameter* 考虑省略命名布尔参数的动词

This refines the previous rule. For named parameters that are boolean, the name is often just as clear without the verb, and the code reads better at the call site.

​	这细化了之前的规则。对于布尔类型的命名参数，名称通常在没有动词的情况下同样清晰，并且在调用站点处代码的可读性更好。

```dart
Isolate.spawn(entryPoint, message, paused: false);
var copy = List.from(elements, growable: true);
var regExp = RegExp(pattern, caseSensitive: false);
```

### PREFER the “positive” name for a boolean property or variable 首选布尔属性或变量的“肯定”名称

Most boolean names have conceptually “positive” and “negative” forms where the former feels like the fundamental concept and the latter is its negation—”open” and “closed”, “enabled” and “disabled”, etc. Often the latter name literally has a prefix that negates the former: “visible” and “*in*-visible”, “connected” and “*dis*-connected”, “zero” and “*non*-zero”.

​	大多数布尔名称在概念上具有“肯定”和“否定”形式，其中前者感觉像是基本概念，而后者是否定——“打开”和“关闭”、“启用”和“禁用”等。通常，后一个名称实际上有一个前缀来否定前一个名称：“可见”和“不可见”、“已连接”和“未连接”、“零”和“非零”。

When choosing which of the two cases that `true` represents—and thus which case the property is named for—prefer the positive or more fundamental one. Boolean members are often nested inside logical expressions, including negation operators. If your property itself reads like a negation, it’s harder for the reader to mentally perform the double negation and understand what the code means.

​	在选择 `true` 表示的两个情况中的哪一个时——因此选择属性的名称——优先选择积极或更基本的情况。布尔成员通常嵌套在逻辑表达式中，包括否定运算符。如果您的属性本身读起来像否定，那么读者就更难在心理上执行双重否定并理解代码的含义。

```dart
if (socket.isConnected && database.hasData) {
  socket.write(database.read());
}
if (!socket.isDisconnected && !database.isEmpty) {
  socket.write(database.read());
}
```

For some properties, there is no obvious positive form. Is a document that has been flushed to disk “saved” or “*un*-changed”? Is a document that *hasn’t* been flushed “*un*-saved” or “changed”? In ambiguous cases, lean towards the choice that is less likely to be negated by users or has the shorter name.

​	对于某些属性，没有明显的肯定形式。已刷新到磁盘的文档是“已保存”还是“未更改”？尚未刷新的文档是“未保存”还是“已更改”？在模棱两可的情况下，倾向于不太可能被用户否定的选择或名称较短的选择。

**Exception:** With some properties, the negative form is what users overwhelmingly need to use. Choosing the positive case would force them to negate the property with `!` everywhere. Instead, it may be better to use the negative case for that property.

​	异常：对于某些属性，用户压倒性地需要使用否定形式。选择肯定情况会迫使他们在任何地方用 `!` 否定该属性。相反，最好对该属性使用否定情况。

### PREFER an imperative verb phrase for a function or method whose main purpose is a side effect 对于主要目的是产生副作用的函数或方法，

Callable members can return a result to the caller and perform other work or side effects. In an imperative language like Dart, members are often called mainly for their side effect: they may change an object’s internal state, produce some output, or talk to the outside world.
PREFER 使用命令式动词短语

Those kinds of members should be named using an imperative verb phrase that clarifies the work the member performs.
可调用成员可以向调用者返回结果并执行其他工作或产生副作用。在像 Dart 这样的命令式语言中，通常主要为了其副作用而调用成员：它们可能会更改对象的内部状态、产生一些输出或与外界通信。

```dart
list.add('element');
queue.removeFirst();
window.refresh();
```

This way, an invocation reads like a command to do that work.

​	这些类型的成员应使用命令式动词短语命名，以阐明成员执行的工作。

### PREFER a noun phrase or non-imperative verb phrase for a function or method if returning a value is its primary purpose 如果函数或方法的主要目的是返回值，则首选名词短语或非命令式动词短语

Other callable members have few side effects but return a useful result to the caller. If the member needs no parameters to do that, it should generally be a getter. But sometimes a logical “property” needs some parameters. For example, `elementAt()` returns a piece of data from a collection, but it needs a parameter to know *which* piece of data to return.

​	其他可调用成员几乎没有副作用，但会向调用者返回一个有用的结果。如果成员不需要任何参数即可执行此操作，则它通常应该是一个 getter。但有时一个逻辑“属性”需要一些参数。例如， `elementAt()` 从集合中返回一个数据块，但它需要一个参数来了解要返回哪个数据块。

This means the member is *syntactically* a method, but *conceptually* it is a property, and should be named as such using a phrase that describes *what* the member returns.

​	这意味着该成员在语法上是一个方法，但在概念上它是一个属性，并且应该使用描述该成员返回内容的短语将其命名为属性。

```dart
var element = list.elementAt(3);
var first = list.firstWhere(test);
var char = string.codeUnitAt(4);
```

This guideline is deliberately softer than the previous one. Sometimes a method has no side effects but is still simpler to name with a verb phrase like `list.take()` or `string.split()`.

​	此准则故意比前一个准则宽松。有时，一个方法没有副作用，但仍然可以使用动词短语（如 `list.take()` 或 `string.split()` ）更简单地命名。

### CONSIDER an imperative verb phrase for a function or method if you want to draw attention to the work it performs 如果要强调函数或方法执行的工作，请考虑使用命令式动词短语

When a member produces a result without any side effects, it should usually be a getter or a method with a noun phrase name describing the result it returns. However, sometimes the work required to produce that result is important. It may be prone to runtime failures, or use heavyweight resources like networking or file I/O. In cases like this, where you want the caller to think about the work the member is doing, give the member a verb phrase name that describes that work.

​	当成员产生结果且没有任何副作用时，它通常应为 getter 或使用名词短语名称描述其返回结果的方法。但是，有时产生该结果所需的工作很重要。它可能容易出现运行时故障，或使用网络或文件 I/O 等重量级资源。在这种情况下，您希望调用方考虑成员正在执行的工作，请为该成员指定描述该工作的动词短语名称。

```dart
var table = database.downloadData();
var packageVersions = packageGraph.solveConstraints();
```

Note, though, that this guideline is softer than the previous two. The work an operation performs is often an implementation detail that isn’t relevant to the caller, and performance and robustness boundaries change over time. Most of the time, name your members based on *what* they do for the caller, not *how* they do it.

​	注意，尽管如此，此准则比前两个准则更宽松。操作执行的工作通常是与调用者无关的实现细节，性能和健壮性边界会随着时间而改变。大多数情况下，根据成员对调用者的作用来命名成员，而不是根据成员的执行方式来命名。

### AVOID starting a method name with `get` 避免以 `get` 开头的方法名称。

In most cases, the method should be a getter with `get` removed from the name. For example, instead of a method named `getBreakfastOrder()`, define a getter named `breakfastOrder`.

​	在大多数情况下，方法应为 getter，且名称中已删除 `get` 。例如，不要定义名为 `getBreakfastOrder()` 的方法，而应定义名为 `breakfastOrder` 的 getter。

Even if the member does need to be a method because it takes arguments or otherwise isn’t a good fit for a getter, you should still avoid `get`. Like the previous guidelines state, either:

​	即使成员确实需要成为方法，因为它带参数或不适合 getter，您仍应避免使用 `get` 。如前述准则所述，请执行以下操作之一：

- Simply drop `get` and [use a noun phrase name](https://dart.dev/effective-dart/design#prefer-a-noun-phrase-or-non-imperative-verb-phrase-for-a-function-or-method-if-returning-a-value-is-its-primary-purpose) like `breakfastOrder()` if the caller mostly cares about the value the method returns.
- 如果调用者主要关心方法返回的值，则只需删除 `get` 并使用名词短语名称，如 `breakfastOrder()` 。
- [Use a verb phrase name](https://dart.dev/effective-dart/design#consider-an-imperative-verb-phrase-for-a-function-or-method-if-you-want-to-draw-attention-to-the-work-it-performs) if the caller cares about the work being done, but pick a verb that more precisely describes the work than `get`, like `create`, `download`, `fetch`, `calculate`, `request`, `aggregate`, etc.
- 如果调用者关心所执行的工作，则使用动词短语名称，但选择比 `get` 更准确地描述该工作的动词，如 `create` 、 `download` 、 `fetch` 、 `calculate` 、 `request` 、 `aggregate` 等。

### PREFER naming a method `to___()` if it copies the object’s state to a new object 如果方法将对象的 state 复制到新对象，则优先命名方法 `to___()`

Linter rule: [use_to_and_as_if_applicable](https://dart.dev/tools/linter-rules/use_to_and_as_if_applicable)

​	Linter 规则：use_to_and_as_if_applicable

A *conversion* method is one that returns a new object containing a copy of almost all of the state of the receiver but usually in some different form or representation. The core libraries have a convention that these methods are named starting with `to` followed by the kind of result.

​	转换方法是一种返回新对象的方法，该对象包含接收器几乎所有状态的副本，但通常采用某种不同的形式或表示形式。核心库有一个约定，这些方法的命名以 `to` 开头，后跟结果的类型。

If you define a conversion method, it’s helpful to follow that convention.

​	如果您定义转换方法，遵循该约定会有所帮助。

```dart
list.toSet();
stackTrace.toString();
dateTime.toLocal();
```

### PREFER naming a method `as___()` if it returns a different representation backed by the original object 如果方法返回由原始对象支持的不同表示形式，则最好将其命名为 `as___()`

Linter rule: [use_to_and_as_if_applicable](https://dart.dev/tools/linter-rules/use_to_and_as_if_applicable)

​	Linter 规则：use_to_and_as_if_applicable

Conversion methods are “snapshots”. The resulting object has its own copy of the original object’s state. There are other conversion-like methods that return *views*—they provide a new object, but that object refers back to the original. Later changes to the original object are reflected in the view.

​	转换方法是“快照”。结果对象具有原始对象状态的副本。还有其他类似转换的方法返回视图——它们提供一个新对象，但该对象引用回原始对象。对原始对象的后续更改会反映在视图中。

The core library convention for you to follow is `as___()`.

​	您要遵循的核心库约定是 `as___()` 。

```dart
var map = table.asMap();
var list = bytes.asFloat32List();
var future = subscription.asFuture();
```

### AVOID describing the parameters in the function’s or method’s name 避免在函数或方法的名称中描述参数

The user will see the argument at the call site, so it usually doesn’t help readability to also refer to it in the name itself.

​	用户将在调用站点看到参数，因此通常不必在名称本身中引用它来提高可读性。

```dart
list.add(element);
map.remove(key);
list.addElement(element)
map.removeKey(key)
```

However, it can be useful to mention a parameter to disambiguate it from other similarly-named methods that take different types:

​	但是，提及参数以将其与采用不同类型的其他同名方法区分开来可能很有用：

```dart
map.containsKey(key);
map.containsValue(value);
```

### DO follow existing mnemonic conventions when naming type parameters 在命名类型参数时遵循现有的助记符约定

Single letter names aren’t exactly illuminating, but almost all generic types use them. Fortunately, they mostly use them in a consistent, mnemonic way. The conventions are:
单个字母名称并不完全明了，但几乎所有泛型类型都使用它们。幸运的是，它们大多以一致的助记符方式使用它们。约定如下：

- `E` for the **element** type in a collection:
  `E` 用于集合中的元素类型：

  ```dart
  class IterableBase<E> {}
  class List<E> {}
  class HashSet<E> {}
  class RedBlackTree<E> {}
  ```

- `K` and `V` for the **key** and **value** types in an associative collection:
  `K` 和 `V` 用于关联集合中的键和值类型：

  ```dart
  class Map<K, V> {}
  class Multimap<K, V> {}
  class MapEntry<K, V> {}
  ```

- `R` for a type used as the **return** type of a function or a class’s methods. This isn’t common, but appears in typedefs sometimes and in classes that implement the visitor pattern:
  `R` 用于用作函数或类方法的返回类型的类型。这并不常见，但有时出现在类型定义中以及实现访问者模式的类中：

  ```dart
  abstract class ExpressionVisitor<R> {
    R visitBinary(BinaryExpression node);
    R visitLiteral(LiteralExpression node);
    R visitUnary(UnaryExpression node);
  }
  ```

- Otherwise, use `T`, `S`, and `U` for generics that have a single type parameter and where the surrounding type makes its meaning obvious. There are multiple letters here to allow nesting without shadowing a surrounding name. For example:
  否则，对具有单个类型参数且周围类型使其含义显而易见的泛型使用 `T` 、 `S` 和 `U` 。这里有多个字母，以便在不隐藏周围名称的情况下进行嵌套。例如：

  ```dart
  class Future<T> {
    Future<S> then<S>(FutureOr<S> onValue(T value)) => ...
  }
  ```

  Here, the generic method `then<S>()` uses `S` to avoid shadowing the `T` on `Future<T>`.
  此处，泛型方法 `then<S>()` 使用 `S` 来避免在 `Future<T>` 上对 `T` 进行阴影处理。

If none of the above cases are a good fit, then either another single-letter mnemonic name or a descriptive name is fine:
如果以上情况都不合适，那么另一个单字母助记符名称或描述性名称都可以：

```dart
class Graph<N, E> {
  final List<N> nodes = [];
  final List<E> edges = [];
}

class Graph<Node, Edge> {
  final List<Node> nodes = [];
  final List<Edge> edges = [];
}
```

In practice, the existing conventions cover most type parameters.
实际上，现有约定涵盖了大多数类型参数。

## 库 Libraries 

A leading underscore character ( `_` ) indicates that a member is private to its library. This is not mere convention, but is built into the language itself.
前导下划线字符 ( `_` ) 表示成员对其库是私有的。这不是简单的约定，而是内置于语言本身。

### PREFER making declarations private 更喜欢将声明设为私有

A public declaration in a library—either top level or in a class—is a signal that other libraries can and should access that member. It is also a commitment on your library’s part to support that and behave properly when it happens.
库中的公共声明（顶级或在类中）是一个信号，表明其他库可以且应该访问该成员。这也是您的库承诺支持该成员并在发生这种情况时表现得当。

If that’s not what you intend, add the little `_` and be happy. Narrow public interfaces are easier for you to maintain and easier for users to learn. As a nice bonus, the analyzer will tell you about unused private declarations so you can delete dead code. It can’t do that if the member is public because it doesn’t know if any code outside of its view is using it.
如果这不是您的本意，请添加小 `_` 并感到高兴。狭窄的公共接口更易于您维护，也更易于用户学习。作为一项不错的奖励，分析器会告诉您未使用的私有声明，以便您可以删除无效代码。如果成员是公共的，它无法做到这一点，因为它不知道其视图之外的任何代码是否正在使用它。

### CONSIDER declaring multiple classes in the same library 考虑在同一个库中声明多个类

Some languages, such as Java, tie the organization of files to the organization of classes—each file may only define a single top level class. Dart does not have that limitation. Libraries are distinct entities separate from classes. It’s perfectly fine for a single library to contain multiple classes, top level variables, and functions if they all logically belong together.
某些语言（例如 Java）将文件的组织与类的组织联系在一起——每个文件只能定义一个顶级类。Dart 没有这种限制。库是独立于类的不同实体。如果它们在逻辑上属于同一类，则一个库包含多个类、顶级变量和函数完全没问题。

Placing multiple classes together in one library can enable some useful patterns. Since privacy in Dart works at the library level, not the class level, this is a way to define “friend” classes like you might in C++. Every class declared in the same library can access each other’s private members, but code outside of that library cannot.
将多个类放在一个库中可以实现一些有用的模式。由于 Dart 中的隐私性在库级别而不是类级别起作用，因此这是一种定义“友元”类的方法，就像您在 C++ 中可能做的那样。在同一个库中声明的每个类都可以访问彼此的私有成员，但该库之外的代码不能访问。

Of course, this guideline doesn’t mean you *should* put all of your classes into a huge monolithic library, just that you are allowed to place more than one class in a single library.
当然，此准则并不意味着您应该将所有类都放入一个巨大的单一库中，只是允许您将多个类放在一个库中。

## Classes 和 mixins

Dart is a “pure” object-oriented language in that all objects are instances of classes. But Dart does not require all code to be defined inside a class—you can define top-level variables, constants, and functions like you can in a procedural or functional language.
Dart 是一种“纯”面向对象语言，因为所有对象都是类的实例。但 Dart 不要求所有代码都在类中定义——您可以像在过程式或函数式语言中一样定义顶级变量、常量和函数。

### AVOID defining a one-member abstract class when a simple function will do 避免定义一个简单的函数就能实现的单成员抽象类

Linter rule: [one_member_abstracts](https://dart.dev/tools/linter-rules/one_member_abstracts)
Linter 规则：one_member_abstracts

Unlike Java, Dart has first-class functions, closures, and a nice light syntax for using them. If all you need is something like a callback, just use a function. If you’re defining a class and it only has a single abstract member with a meaningless name like `call` or `invoke`, there is a good chance you just want a function.
与 Java 不同，Dart 具有头等函数、闭包以及用于使用它们的简洁轻便的语法。如果您只需要回调之类的功能，只需使用函数即可。如果您正在定义一个类，并且它只有一个抽象成员，并且该成员具有无意义的名称，如 `call` 或 `invoke` ，那么很可能您只需要一个函数。

```dart
typedef Predicate<E> = bool Function(E element);
abstract class Predicate<E> {
  bool test(E element);
}
```

### AVOID defining a class that contains only static members 避免定义仅包含静态成员的类

Linter rule: [avoid_classes_with_only_static_members](https://dart.dev/tools/linter-rules/avoid_classes_with_only_static_members)
Linter 规则：avoid_classes_with_only_static_members

In Java and C#, every definition *must* be inside a class, so it’s common to see “classes” that exist only as a place to stuff static members. Other classes are used as namespaces—a way to give a shared prefix to a bunch of members to relate them to each other or avoid a name collision.
在 Java 和 C# 中，每个定义都必须位于类中，因此通常会看到仅作为填充静态成员的地方而存在的“类”。其他类用作命名空间，即为一组成员提供共享前缀，以便将它们相互关联或避免名称冲突。

Dart has top-level functions, variables, and constants, so you don’t *need* a class just to define something. If what you want is a namespace, a library is a better fit. Libraries support import prefixes and show/hide combinators. Those are powerful tools that let the consumer of your code handle name collisions in the way that works best for *them*.
Dart 具有顶级函数、变量和常量，因此您无需类即可定义某些内容。如果您想要的是命名空间，那么库更合适。库支持导入前缀和显示/隐藏组合器。这些都是功能强大的工具，可让代码使用者以最适合他们的方式处理名称冲突。

If a function or variable isn’t logically tied to a class, put it at the top level. If you’re worried about name collisions, give it a more precise name or move it to a separate library that can be imported with a prefix.
如果某个函数或变量与某个类在逻辑上没有关联，请将其放在顶层。如果您担心名称冲突，请为其指定一个更精确的名称，或将其移至可通过前缀导入的单独库中。

```dart
DateTime mostRecent(List<DateTime> dates) {
  return dates.reduce((a, b) => a.isAfter(b) ? a : b);
}

const _favoriteMammal = 'weasel';
class DateUtils {
  static DateTime mostRecent(List<DateTime> dates) {
    return dates.reduce((a, b) => a.isAfter(b) ? a : b);
  }
}

class _Favorites {
  static const mammal = 'weasel';
}
```

In idiomatic Dart, classes define *kinds of objects*. A type that is never instantiated is a code smell.
在习惯用语 Dart 中，类定义对象类型。从不实例化的类型是一种代码缺陷。

However, this isn’t a hard rule. For example, with constants and enum-like types, it may be natural to group them in a class.
但是，这不是一个严格的规则。例如，对于常量和枚举类型，将它们分组到一个类中可能是自然的。

```dart
class Color {
  static const red = '#f00';
  static const green = '#0f0';
  static const blue = '#00f';
  static const black = '#000';
  static const white = '#fff';
}
```

### AVOID extending a class that isn’t intended to be subclassed 避免扩展不打算被子类化的类

If a constructor is changed from a generative constructor to a factory constructor, any subclass constructor calling that constructor will break. Also, if a class changes which of its own methods it invokes on `this`, that may break subclasses that override those methods and expect them to be called at certain points.
如果某个构造函数从生成构造函数更改为工厂构造函数，则调用该构造函数的任何子类构造函数都会中断。此外，如果某个类更改其在 `this` 上调用的自身方法，则可能会中断覆盖这些方法并期望在某些点调用这些方法的子类。

Both of these mean that a class needs to be deliberate about whether or not it wants to allow subclassing. This can be communicated in a doc comment, or by giving the class an obvious name like `IterableBase`. If the author of the class doesn’t do that, it’s best to assume you should *not* extend the class. Otherwise, later changes to it may break your code.
这两者都意味着某个类需要慎重考虑是否允许子类化。这可以通过文档注释来传达，或通过为该类指定一个明显的名称（如 `IterableBase` ）来传达。如果类的作者没有这样做，最好假设您不应扩展该类。否则，以后对它的更改可能会中断您的代码。

### DO document if your class supports being extended 如果你的类支持扩展，请编写文档

This is the corollary to the above rule. If you want to allow subclasses of your class, state that. Suffix the class name with `Base`, or mention it in the class’s doc comment.
这是上述规则的推论。如果你想允许你的类的子类，请声明这一点。用 `Base` 后缀类名，或在类的文档注释中提及它。

### AVOID implementing a class that isn’t intended to be an interface 避免实现不打算作为接口的类

Implicit interfaces are a powerful tool in Dart to avoid having to repeat the contract of a class when it can be trivially inferred from the signatures of an implementation of that contract.
隐式接口是 Dart 中一个强大的工具，可避免在可以从该契约的实现的签名中轻松推断出契约时重复该契约。

But implementing a class’s interface is a very tight coupling to that class. It means virtually *any* change to the class whose interface you are implementing will break your implementation. For example, adding a new member to a class is usually a safe, non-breaking change. But if you are implementing that class’s interface, now your class has a static error because it lacks an implementation of that new method.
但实现类的接口与该类紧密耦合。这意味着对你要实现其接口的类进行几乎任何更改都会破坏你的实现。例如，向类添加新成员通常是安全的、不破坏性的更改。但如果你正在实现该类的接口，现在你的类有一个静态错误，因为它缺少对该新方法的实现。

Library maintainers need the ability to evolve existing classes without breaking users. If you treat every class like it exposes an interface that users are free to implement, then changing those classes becomes very difficult. That difficulty in turn means the libraries you rely on are slower to grow and adapt to new needs.
库维护者需要能够在不破坏用户的情况下发展现有类。如果您将每个类都视为公开用户可以自由实现的接口，那么更改这些类将变得非常困难。这种困难反过来意味着您依赖的库增长和适应新需求的速度会更慢。

To give the authors of the classes you use more leeway, avoid implementing implicit interfaces except for classes that are clearly intended to be implemented. Otherwise, you may introduce a coupling that the author doesn’t intend, and they may break your code without realizing it.
为了给您使用的类的作者更多余地，请避免实现隐式接口，除非这些类明确打算实现。否则，您可能会引入作者无意引入的耦合，并且他们可能会在没有意识到这一点的情况下破坏您的代码。

### DO document if your class supports being used as an interface 如果您的类支持用作接口，请记录

If your class can be used as an interface, mention that in the class’s doc comment.
如果您的类可以用作接口，请在类的文档注释中提及这一点。

### PREFER defining a pure `mixin` or pure `class` to a `mixin class` 优先定义纯 `mixin` 或纯 `class` 而不是 `mixin class`





Linter rule: [prefer_mixin](https://dart.dev/tools/linter-rules/prefer_mixin)
Linter 规则：prefer_mixin

Dart previously (language version [2.12](https://dart.dev/guides/language/evolution#dart-212) to [2.19](https://dart.dev/guides/language/evolution#dart-219)) allowed any class that met certain restrictions (no non-default constructor, no superclass, etc.) to be mixed into other classes. This was confusing because the author of the class might not have intended it to be mixed in.
Dart 之前（语言版本 2.12 到 2.19）允许任何满足某些限制（没有非默认构造函数、没有超类等）的类混入其他类。这很令人困惑，因为该类的作者可能无意将其混入。

Dart 3.0.0 now requires that any type intended to be mixed into other classes, as well as treated as a normal class, must be explicitly declared as such with the `mixin class` declaration.
Dart 3.0.0 现在要求任何旨在混入其他类并被视为普通类的类型都必须使用 `mixin class` 声明明确声明为这种类型。

Types that need to be both a mixin and a class should be a rare case, however. The `mixin class` declaration is mostly meant to help migrate pre-3.0.0 classes being used as mixins to a more explicit declaration. New code should clearly define the behavior and intention of its declarations by using only pure `mixin` or pure `class` declarations, and avoid the ambiguity of mixin classes.
但是，既需要是 mixin 又需要是类的类型应该是一种罕见的情况。 `mixin class` 声明主要用于帮助将用作 mixin 的 3.0.0 之前的类迁移到更明确的声明。新代码应该仅使用纯 `mixin` 或纯 `class` 声明来明确定义其声明的行为和意图，并避免混入类的歧义。

Read [Migrating classes as mixins](https://dart.dev/language/class-modifiers-for-apis#migrating-classes-as-mixins) for more guidance on `mixin` and `mixin class` declarations.
阅读将类作为 mixin 迁移以获取有关 `mixin` 和 `mixin class` 声明的更多指导。

## 构造函数 Constructors 

Dart constructors are created by declaring a function with the same name as the class and, optionally, an additional identifier. The latter are called *named constructors*.
Dart 构造函数是通过声明一个与类同名的函数（以及一个可选的附加标识符）创建的。后者称为命名构造函数。

### CONSIDER making your constructor `const` if the class supports it 如果类支持，请考虑将构造函数设为 `const`

If you have a class where all the fields are final, and the constructor does nothing but initialize them, you can make that constructor `const`. That lets users create instances of your class in places where constants are required—inside other larger constants, switch cases, default parameter values, etc.
如果某个类中的所有字段都是 final，并且构造函数除了初始化这些字段外什么也不做，那么您可以将该构造函数设为 `const` 。这样，用户就可以在需要常量的场合创建您类的实例，例如在其他更大的常量、switch 语句、默认参数值等内部。

If you don’t explicitly make it `const`, they aren’t able to do that.
如果您没有明确地将其设为 `const` ，他们就无法这样做。

Note, however, that a `const` constructor is a commitment in your public API. If you later change the constructor to non-`const`, it will break users that are calling it in constant expressions. If you don’t want to commit to that, don’t make it `const`. In practice, `const` constructors are most useful for simple, immutable value-like types.
但是，请注意， `const` 构造函数是您公开 API 中的承诺。如果您稍后将构造函数更改为非 `const` ，那么在常量表达式中调用该构造函数的用户将会遇到问题。如果您不想做出此承诺，请不要将其设为 `const` 。实际上， `const` 构造函数最适用于简单的、不可变的值类型。

## 成员 Members 

A member belongs to an object and can be either methods or instance variables.
成员属于某个对象，可以是方法或实例变量。

### PREFER making fields and top-level variables `final` 优先使用字段和顶级变量 `final`

Linter rule: [prefer_final_fields](https://dart.dev/tools/linter-rules/prefer_final_fields)
Linter 规则：prefer_final_fields

State that is not *mutable*—that does not change over time—is easier for programmers to reason about. Classes and libraries that minimize the amount of mutable state they work with tend to be easier to maintain. Of course, it is often useful to have mutable data. But, if you don’t need it, your default should be to make fields and top-level variables `final` when you can.
不会发生变化的状态（即不会随时间变化的状态）更便于程序员理解。处理的可变状态越少的类和库往往更容易维护。当然，拥有可变数据通常很有用。但是，如果您不需要，那么您的默认操作应是尽可能使用字段和顶级变量 `final` 。

Sometimes an instance field doesn’t change after it has been initialized, but can’t be initialized until after the instance is constructed. For example, it may need to reference `this` or some other field on the instance. In cases like that, consider making the field `late final`. When you do, you may also be able to [initialize the field at its declaration](https://dart.dev/effective-dart/usage#do-initialize-fields-at-their-declaration-when-possible).
有时，实例字段在初始化后不会发生变化，但在构造实例后才能初始化。例如，它可能需要引用 `this` 或实例上的其他字段。在这种情况，请考虑将字段设为 `late final` 。这样做时，您可能还可以在声明时初始化字段。

### DO use getters for operations that conceptually access properties 确实使用访问属性的概念性操作的 getter

Deciding when a member should be a getter versus a method is a subtle but important part of good API design, hence this very long guideline. Some other language’s cultures shy away from getters. They only use them when the operation is almost exactly like a field—it does a minuscule amount of calculation on state that lives entirely on the object. Anything more complex or heavyweight than that gets `()` after the name to signal “computation goin’ on here!” because a bare name after a `.` means “field”.
决定何时将成员设为 getter 还是方法是良好 API 设计中微妙但重要的一部分，因此有了这份非常长的指南。其他一些语言的文化避开 getter。它们仅在操作几乎完全像字段时使用 getter——它对完全存在于对象中的状态进行微小的计算。比这更复杂或更重量级的任何内容都会在名称后加上 `()` 来表示“正在进行计算！”，因为 `.` 后面的裸名称表示“字段”。

Dart is *not* like that. In Dart, *all* dotted names are member invocations that may do computation. Fields are special—they’re getters whose implementation is provided by the language. In other words, getters are not “particularly slow fields” in Dart; fields are “particularly fast getters”.
Dart 并非如此。在 Dart 中，所有带点的名称都是可能会执行计算的成员调用。字段是特殊的——它们是其实现由语言提供的 getter。换句话说，在 Dart 中，getter 不是“特别慢的字段”；字段是“特别快的 getter”。

Even so, choosing a getter over a method sends an important signal to the caller. The signal, roughly, is that the operation is “field-like”. The operation, at least in principle, *could* be implemented using a field, as far as the caller knows. That implies:
即便如此，选择 getter 而非方法会向调用者发送一个重要的信号。这个信号大致是，该操作“类似字段”。就调用者所知，该操作至少在原则上可以使用字段来实现。这意味着：

- **The operation does not take any arguments and returns a result.
  该操作不接受任何参数并返回结果。**

- **The caller cares mostly about the result.** If you want the caller to worry about *how* the operation produces its result more than they do the result being produced, then give the operation a verb name that describes the work and make it a method.
  调用者最关心的是结果。如果您希望调用者比关心结果的产生过程更关心操作如何产生结果，那么请给操作一个描述工作的动词名称，并使其成为一种方法。

  This does *not* mean the operation has to be particularly fast in order to be a getter. `IterableBase.length` is `O(n)`, and that’s OK. It’s fine for a getter to do significant calculation. But if it does a *surprising* amount of work, you may want to draw their attention to that by making it a method whose name is a verb describing what it does.
  这并不意味着操作必须特别快才能成为一个 getter。 `IterableBase.length` 是 `O(n)` ，这没关系。getter 执行大量计算是可以的。但如果它做了惊人的工作量，您可能希望通过使其成为一个方法（其名称是描述其所做工作的动词）来引起他们的注意。

  ```
  connection.nextIncomingMessage; // Does network I/O.
  expression.normalForm; // Could be exponential to calculate.
  ```

- **The operation does not have user-visible side effects.** Accessing a real field does not alter the object or any other state in the program. It doesn’t produce output, write files, etc. A getter shouldn’t do those things either.
  操作没有用户可见的副作用。访问真实字段不会改变对象或程序中的任何其他状态。它不会产生输出、写入文件等。获取器也不应该做这些事情。

  The “user-visible” part is important. It’s fine for getters to modify hidden state or produce out of band side effects. Getters can lazily calculate and store their result, write to a cache, log stuff, etc. As long as the caller doesn’t *care* about the side effect, it’s probably fine.
  “用户可见”部分很重要。获取器修改隐藏状态或产生带外副作用是可以的。获取器可以延迟计算并存储其结果、写入缓存、记录内容等。只要调用者不在乎副作用，那可能就是好的。

  ```
  stdout.newline; // Produces output.
  list.clear; // Modifies object.
  ```

- **The operation is \*idempotent\*.** “Idempotent” is an odd word that, in this context, basically means that calling the operation multiple times produces the same result each time, unless some state is explicitly modified between those calls. (Obviously, `list.length` produces different results if you add an element to the list between calls.)
  操作是幂等的。“幂等”是一个奇怪的词，在此上下文中，基本上意味着多次调用操作每次都会产生相同的结果，除非在这些调用之间明确修改了一些状态。（显然，如果您在调用之间向列表添加元素， `list.length` 会产生不同的结果。）

  “Same result” here does not mean a getter must literally produce an identical object on successive calls. Requiring that would force many getters to have brittle caching, which negates the whole point of using a getter. It’s common, and perfectly fine, for a getter to return a new future or list each time you call it. The important part is that the future completes to the same value, and the list contains the same elements.
  此处“相同结果”并不意味着一个 getter 必须在连续调用时产生一个完全相同对象。要求这样做会迫使许多 getter 具有脆弱的缓存，这违背了使用 getter 的全部目的。每次调用 getter 时返回一个新的 future 或列表是很常见且完全正常的。重要的是 future 完成到相同的值，并且列表包含相同的元素。

  In other words, the result value should be the same *in the aspects that the caller cares about.*
  换句话说，结果值在调用者关心的方面应该相同。

  ```
  DateTime.now; // New result each time.
  ```

- **The resulting object doesn’t expose all of the original object’s state.** A field exposes only a piece of an object. If your operation returns a result that exposes the original object’s entire state, it’s likely better off as a [`to___()`](https://dart.dev/effective-dart/design#prefer-naming-a-method-to___-if-it-copies-the-objects-state-to-a-new-object) or [`as___()`](https://dart.dev/effective-dart/design#prefer-naming-a-method-as___-if-it-returns-a-different-representation-backed-by-the-original-object) method.
  结果对象不会公开原始对象的所有状态。字段仅公开对象的一部分。如果您的操作返回公开原始对象整个状态的结果，那么它很可能更适合作为 `to___()` 或 `as___()` 方法。

If all of the above describe your operation, it should be a getter. It seems like few members would survive that gauntlet, but surprisingly many do. Many operations just do some computation on some state and most of those can and should be getters.
如果以上所有内容都描述了您的操作，那么它应该是一个 getter。似乎很少有成员能够通过该考验，但令人惊讶的是，许多成员能够通过。许多操作只是对某些状态执行一些计算，其中大多数都可以并且应该成为 getter。

```
rectangle.area;
collection.isEmpty;
button.canShow;
dataSet.minimumValue;
```

### DO use setters for operations that conceptually change properties 对概念上改变属性的操作使用 setter

Linter rule: [use_setters_to_change_properties](https://dart.dev/tools/linter-rules/use_setters_to_change_properties)
Linter 规则：use_setters_to_change_properties

Deciding between a setter versus a method is similar to deciding between a getter versus a method. In both cases, the operation should be “field-like”.
决定使用 setter 还是方法类似于决定使用 getter 还是方法。在这两种情况下，操作都应该是“类似字段的”。

For a setter, “field-like” means:
对于 setter，“类似字段的”意味着：

- **The operation takes a single argument and does not produce a result value.
  操作接受一个参数并且不产生结果值。**
- **The operation changes some state in the object.
  操作更改对象中的某些状态。**
- **The operation is idempotent.** Calling the same setter twice with the same value should do nothing the second time as far as the caller is concerned. Internally, maybe you’ve got some cache invalidation or logging going on. That’s fine. But from the caller’s perspective, it appears that the second call does nothing.
  操作是幂等的。使用相同的值两次调用同一个 setter，就调用者而言，第二次不应执行任何操作。在内部，您可能有一些缓存无效或正在进行日志记录。这很好。但从调用者的角度来看，第二次调用似乎什么也没做。

```
rectangle.width = 3;
button.visible = false;
```

### DON’T define a setter without a corresponding getter 不要定义没有相应 getter 的 setter

Linter rule: [avoid_setters_without_getters](https://dart.dev/tools/linter-rules/avoid_setters_without_getters)
Linter 规则：avoid_setters_without_getters

Users think of getters and setters as visible properties of an object. A “dropbox” property that can be written to but not seen is confusing and confounds their intuition about how properties work. For example, a setter without a getter means you can use `=` to modify it, but not `+=`.
用户将 getter 和 setter 视为对象的可见属性。可以写入但不能看到的“dropbox”属性令人困惑，并且会混淆他们对属性工作方式的直觉。例如，没有 getter 的 setter 意味着您可以使用 `=` 修改它，但不能使用 `+=` 。

This guideline does *not* mean you should add a getter just to permit the setter you want to add. Objects shouldn’t generally expose more state than they need to. If you have some piece of an object’s state that can be modified but not exposed in the same way, use a method instead.
此准则并不意味着您应该添加一个 getter 只是为了允许您想要添加的 setter。对象通常不应该公开比它们需要的更多状态。如果您有一些对象状态可以修改，但不能以相同方式公开，请改用方法。

### AVOID using runtime type tests to fake overloading 避免使用运行时类型测试来伪造重载

It’s common for an API to support similar operations on different types of parameters. To emphasize the similarity, some languages support *overloading*, which lets you define multiple methods that have the same name but different parameter lists. At compile time, the compiler looks at the actual argument types to determine which method to call.
API 通常支持对不同类型的参数执行类似的操作。为了强调相似性，一些语言支持重载，它允许您定义具有相同名称但参数列表不同的多个方法。在编译时，编译器会查看实际参数类型以确定要调用的方法。

Dart doesn’t have overloading. You can define an API that looks like overloading by defining a single method and then using `is` type tests inside the body to look at the runtime types of the arguments and perform the appropriate behavior. However, faking overloading this way turns a *compile time* method selection into a choice that happens at *runtime*.
Dart 没有重载。您可以通过定义一个方法，然后在主体中使用 `is` 类型测试来查看参数的运行时类型并执行适当的行为，来定义看起来像重载的 API。但是，以这种方式伪造重载会将编译时方法选择变成在运行时发生的选项。

If callers usually know which type they have and which specific operation they want, it’s better to define separate methods with different names to let callers select the right operation. This gives better static type checking and faster performance since it avoids any runtime type tests.
如果调用者通常知道他们拥有的类型以及他们想要的特定操作，最好定义具有不同名称的单独方法，以便调用者选择正确的操作。这提供了更好的静态类型检查和更快的性能，因为它避免了任何运行时类型测试。

However, if users might have an object of an unknown type and *want* the API to internally use `is` to pick the right operation, then a single method where the parameter is a supertype of all of the supported types might be reasonable.
但是，如果用户可能有一个未知类型的对象，并且希望 API 在内部使用 `is` 来选择正确的操作，那么一个参数是所有受支持类型的超类型的方法可能是合理的。

### AVOID public `late final` fields without initializers 避免没有初始化程序的 public `late final` 字段

Unlike other `final` fields, a `late final` field without an initializer *does* define a setter. If that field is public, then the setter is public. This is rarely what you want. Fields are usually marked `late` so that they can be initialized *internally* at some point in the instance’s lifetime, often inside the constructor body.
与其他 `final` 字段不同，没有初始化程序的 `late final` 字段确实定义了一个设置器。如果该字段是公共的，那么设置器就是公共的。这很少是你想要的。字段通常标记为 `late` ，以便它们可以在实例生命周期的某个时间点在内部初始化，通常在构造函数体中。

Unless you *do* want users to call the setter, it’s better to pick one of the following solutions:
除非你确实希望用户调用设置器，否则最好选择以下解决方案之一：

- Don’t use `late`.
  不要使用 `late` 。
- Use a factory constructor to compute the `final` field values.
  使用工厂构造函数来计算 `final` 字段值。
- Use `late`, but initialize the `late` field at its declaration.
  使用 `late` ，但在其声明中初始化 `late` 字段。
- Use `late`, but make the `late` field private and define a public getter for it.
  使用 `late` ，但使 `late` 字段为私有，并为其定义一个公共的 getter。

### AVOID returning nullable `Future`, `Stream`, and collection types 避免返回可为 null 的 `Future` 、 `Stream` 和集合类型

When an API returns a container type, it has two ways to indicate the absence of data: It can return an empty container or it can return `null`. Users generally assume and prefer that you use an empty container to indicate “no data”. That way, they have a real object that they can call methods on like `isEmpty`.
当 API 返回容器类型时，它有两种方式来指示没有数据：它可以返回一个空容器，或者可以返回 `null` 。用户通常会假设并希望您使用空容器来指示“没有数据”。这样，他们就有了一个可以调用方法（如 `isEmpty` ）的真实对象。

To indicate that your API has no data to provide, prefer returning an empty collection, a non-nullable future of a nullable type, or a stream that doesn’t emit any values.
为了指示您的 API 没有要提供的数据，最好返回一个空集合、可为 null 类型的一个不可为 null 的 future，或者一个不发出任何值的流。

**Exception:** If returning `null` *means something different* from yielding an empty container, it might make sense to use a nullable type.
例外情况：如果返回 `null` 的含义与生成一个空容器不同，那么使用可为 null 的类型可能是有意义的。

### AVOID returning `this` from methods just to enable a fluent interface 避免从方法返回 `this` ，仅仅是为了启用流畅的界面

Linter rule: [avoid_returning_this](https://dart.dev/tools/linter-rules/avoid_returning_this)
Linter 规则：avoid_returning_this

Method cascades are a better solution for chaining method calls.
方法级联是链接方法调用的更好解决方案。

```dart
var buffer = StringBuffer()
  ..write('one')
  ..write('two')
  ..write('three');
var buffer = StringBuffer()
    .write('one')
    .write('two')
    .write('three');
```

## 类型 Types 

When you write down a type in your program, you constrain the kinds of values that flow into different parts of your code. Types can appear in two kinds of places: *type annotations* on declarations and type arguments to *generic invocations*.
在程序中写下类型时，您会限制流入代码不同部分的值的种类。类型可以出现在两种位置：声明上的类型注释和泛型调用中的类型参数。

Type annotations are what you normally think of when you think of “static types”. You can type annotate a variable, parameter, field, or return type. In the following example, `bool` and `String` are type annotations. They hang off the static declarative structure of the code and aren’t “executed” at runtime.
当您想到“静态类型”时，通常会想到类型注释。您可以对变量、参数、字段或返回类型进行类型注释。在以下示例中， `bool` 和 `String` 是类型注释。它们挂靠在代码的静态声明结构上，并且在运行时不会“执行”。

```dart
bool isEmpty(String parameter) {
  bool result = parameter.isEmpty;
  return result;
}
```

A generic invocation is a collection literal, a call to a generic class’s constructor, or an invocation of a generic method. In the next example, `num` and `int` are type arguments on generic invocations. Even though they are types, they are first-class entities that get reified and passed to the invocation at runtime.
泛型调用是集合文字、对泛型类的构造函数的调用或对泛型方法的调用。在下一个示例中， `num` 和 `int` 是泛型调用上的类型参数。即使它们是类型，它们也是在运行时被具体化并传递给调用的头等实体。

```dart
var lists = <num>[1, 2];
lists.addAll(List<num>.filled(3, 4));
lists.cast<int>();
```

We stress the “generic invocation” part here, because type arguments can *also* appear in type annotations:
我们在此强调“泛型调用”部分，因为类型参数也可以出现在类型注释中：

```dart
List<int> ints = [1, 2];
```

Here, `int` is a type argument, but it appears inside a type annotation, not a generic invocation. You usually don’t need to worry about this distinction, but in a couple of places, we have different guidance for when a type is used in a generic invocation as opposed to a type annotation.
此处， `int` 是一个类型参数，但它出现在类型注释中，而不是泛型调用中。您通常不必担心这种区别，但在某些地方，我们对在泛型调用中使用类型与在类型注释中使用类型的情况有不同的指导。

### 类型推断 Type inference 

Type annotations are optional in Dart. If you omit one, Dart tries to infer a type based on the nearby context. Sometimes it doesn’t have enough information to infer a complete type. When that happens, Dart sometimes reports an error, but usually silently fills in any missing parts with `dynamic`. The implicit `dynamic` leads to code that *looks* inferred and safe, but actually disables type checking completely. The rules below avoid that by requiring types when inference fails.
在 Dart 中，类型注释是可选的。如果您省略一个，Dart 会尝试根据附近的上下文推断一个类型。有时它没有足够的信息来推断一个完整的类型。发生这种情况时，Dart 有时会报告错误，但通常会用 `dynamic` 静默地填充任何缺失的部分。隐式的 `dynamic` 会导致代码看起来像是推断出来的并且是安全的，但实际上完全禁用了类型检查。以下规则通过在推断失败时要求类型来避免这种情况。

The fact that Dart has both type inference and a `dynamic` type leads to some confusion about what it means to say code is “untyped”. Does that mean the code is dynamically typed, or that you didn’t *write* the type? To avoid that confusion, we avoid saying “untyped” and instead use the following terminology:
Dart 既具有类型推断，又具有 `dynamic` 类型，这一事实导致了关于“未类型化”代码的含义的一些混淆。这是否意味着代码是动态类型化的，或者您没有编写类型？为了避免这种混淆，我们避免使用“未类型化”，而使用以下术语：

- If the code is *type annotated*, the type was explicitly written in the code.
  如果代码是类型注释的，则该类型在代码中显式编写。
- If the code is *inferred*, no type annotation was written, and Dart successfully figured out the type on its own. Inference can fail, in which case the guidelines don’t consider that inferred.
  如果代码被推断，没有编写类型注释，并且 Dart 成功地自行弄清楚了类型。推断可能会失败，在这种情况下，指南不认为那是推断的。
- If the code is *dynamic*, then its static type is the special `dynamic` type. Code can be explicitly annotated `dynamic` or it can be inferred.
  如果代码是动态的，那么它的静态类型就是特殊的 `dynamic` 类型。代码可以被显式注释 `dynamic` 或可以被推断。

In other words, whether some code is annotated or inferred is orthogonal to whether it is `dynamic` or some other type.
换句话说，某些代码是注释还是推断与它是 `dynamic` 还是其他类型无关。

Inference is a powerful tool to spare you the effort of writing and reading types that are obvious or uninteresting. It keeps the reader’s attention focused on the behavior of the code itself. Explicit types are also a key part of robust, maintainable code. They define the static shape of an API and create boundaries to document and enforce what kinds of values are allowed to reach different parts of the program.
推断是一个强大的工具，可以省去您编写和阅读明显或无趣类型的精力。它使读者注意力集中在代码本身的行为上。显式类型也是健壮、可维护代码的关键部分。它们定义了 API 的静态形状，并创建了边界来记录和强制允许哪些类型的变量到达程序的不同部分。

Of course, inference isn’t magic. Sometimes inference succeeds and selects a type, but it’s not the type you want. The common case is inferring an overly precise type from a variable’s initializer when you intend to assign values of other types to the variable later. In those cases, you have to write the type explicitly.
当然，推断不是魔术。有时推断会成功并选择一个类型，但它不是您想要的类型。常见的情况是从变量的初始化程序中推断出过于精确的类型，而您打算稍后将其他类型的变量分配给该变量。在这些情况下，您必须显式编写类型。

The guidelines here strike the best balance we’ve found between brevity and control, flexibility and safety. There are specific guidelines to cover all the various cases, but the rough summary is:
这里的准则在我们发现的简洁性和控制性、灵活性和安全性之间取得了最佳平衡。有针对所有各种情况的具体准则，但粗略的总结是：

- Do annotate when inference doesn’t have enough context, even when `dynamic` is the type you want.
  当推理没有足够的上下文时，即使 `dynamic` 是您想要的类型，也要进行注释。
- Don’t annotate locals and generic invocations unless you need to.
  除非需要，否则不要注释本地变量和泛型调用。
- Prefer annotating top-level variables and fields unless the initializer makes the type obvious.
  除非初始化程序使类型显而易见，否则最好注释顶级变量和字段。

### DO type annotate variables without initializers 对没有初始化程序的变量进行类型注释

Linter rule: [prefer_typing_uninitialized_variables](https://dart.dev/tools/linter-rules/prefer_typing_uninitialized_variables)
Linter 规则：prefer_typing_uninitialized_variables

The type of a variable—top-level, local, static field, or instance field—can often be inferred from its initializer. However, if there is no initializer, inference fails.
变量的类型（顶级、局部、静态字段或实例字段）通常可以从其初始化程序中推断出来。但是，如果没有初始化程序，则推断将失败。

```dart
List<AstNode> parameters;
if (node is Constructor) {
  parameters = node.signature;
} else if (node is Method) {
  parameters = node.parameters;
}
var parameters;
if (node is Constructor) {
  parameters = node.signature;
} else if (node is Method) {
  parameters = node.parameters;
}
```

### DO type annotate fields and top-level variables if the type isn’t obvious 如果类型不明显，请对字段和顶级变量进行类型注释

Linter rule: [type_annotate_public_apis](https://dart.dev/tools/linter-rules/type_annotate_public_apis)
Linter 规则：type_annotate_public_apis

Type annotations are important documentation for how a library should be used. They form boundaries between regions of a program to isolate the source of a type error. Consider:
类型注释是有关如何使用库的重要文档。它们在程序的区域之间形成边界，以隔离类型错误的源。考虑：

```dart
install(id, destination) => ...
```

Here, it’s unclear what `id` is. A string? And what is `destination`? A string or a `File` object? Is this method synchronous or asynchronous? This is clearer:
此处，不清楚 `id` 是什么。一个字符串？ `destination` 是什么？一个字符串还是一个 `File` 对象？此方法是同步的还是异步的？这更清楚：

```dart
Future<bool> install(PackageId id, String destination) => ...
```

In some cases, though, the type is so obvious that writing it is pointless:
然而，在某些情况下，类型是如此明显，以至于编写它毫无意义：

```dart
const screenWidth = 640; // Inferred as int.
```

“Obvious” isn’t precisely defined, but these are all good candidates:
“明显”没有被精确定义，但这些都是不错的候选者：

- Literals.
  字面量。
- Constructor invocations.
  构造函数调用。
- References to other constants that are explicitly typed.
  对显式类型化的其他常量的引用。
- Simple expressions on numbers and strings.
  对数字和字符串的简单表达式。
- Factory methods like `int.parse()`, `Future.wait()`, etc. that readers are expected to be familiar with.
  读者应该熟悉的工厂方法，如 `int.parse()` 、 `Future.wait()` 等。

If you think the initializer expression—whatever it is—is sufficiently clear, then you may omit the annotation. But if you think annotating helps make the code clearer, then add one.
如果您认为初始化器表达式（无论是什么）足够清晰，那么您可以省略注释。但如果您认为添加注释有助于使代码更清晰，那么就添加一个。

When in doubt, add a type annotation. Even when a type is obvious, you may still wish to explicitly annotate. If the inferred type relies on values or declarations from other libraries, you may want to type annotate *your* declaration so that a change to that other library doesn’t silently change the type of your own API without you realizing.
如有疑问，请添加类型注释。即使类型很明显，您可能仍然希望显式添加注释。如果推断出的类型依赖于其他库中的值或声明，您可能希望对声明添加类型注释，以便对该其他库的更改不会在您不知情的情况下默默更改您自己的 API 的类型。

This rule applies to both public and private declarations. Just as type annotations on APIs help *users* of your code, types on private members help *maintainers*.
此规则适用于公共声明和私有声明。就像 API 上的类型注释可以帮助您的代码的用户一样，私有成员上的类型可以帮助维护人员。

### DON’T redundantly type annotate initialized local variables 不要对已初始化的局部变量重复添加类型注释

Linter rule: [omit_local_variable_types](https://dart.dev/tools/linter-rules/omit_local_variable_types)
Linter 规则：omit_local_variable_types

Local variables, especially in modern code where functions tend to be small, have very little scope. Omitting the type focuses the reader’s attention on the more important *name* of the variable and its initialized value.
局部变量，尤其是在函数往往很小的现代代码中，作用域非常小。省略类型会使读者将注意力集中在变量的更重要的名称及其初始化值上。

```dart
List<List<Ingredient>> possibleDesserts(Set<Ingredient> pantry) {
  var desserts = <List<Ingredient>>[];
  for (final recipe in cookbook) {
    if (pantry.containsAll(recipe)) {
      desserts.add(recipe);
    }
  }

  return desserts;
}
List<List<Ingredient>> possibleDesserts(Set<Ingredient> pantry) {
  List<List<Ingredient>> desserts = <List<Ingredient>>[];
  for (final List<Ingredient> recipe in cookbook) {
    if (pantry.containsAll(recipe)) {
      desserts.add(recipe);
    }
  }

  return desserts;
}
```

Sometimes the inferred type is not the type you want the variable to have. For example, you may intend to assign values of other types later. In that case, annotate the variable with the type you want.
有时，推断出的类型不是您希望变量具有的类型。例如，您可能打算稍后分配其他类型的变量。在这种情况下，请使用所需的类型注释变量。

```dart
Widget build(BuildContext context) {
  Widget result = Text('You won!');
  if (applyPadding) {
    result = Padding(padding: EdgeInsets.all(8.0), child: result);
  }
  return result;
}
```

### DO annotate return types on function declarations 在函数声明中注释返回类型

Dart doesn’t generally infer the return type of a function declaration from its body, unlike some other languages. That means you should write a type annotation for the return type yourself.
与某些其他语言不同，Dart 通常不会从函数主体的函数声明中推断出返回类型。这意味着您应该自己为返回类型编写类型注释。

```dart
String makeGreeting(String who) {
  return 'Hello, $who!';
}
makeGreeting(String who) {
  return 'Hello, $who!';
}
```

Note that this guideline only applies to *named* function declarations: top-level functions, methods, and local functions. Anonymous function expressions infer a return type from their body. In fact, the syntax doesn’t even allow a return type annotation.
请注意，此准则仅适用于命名函数声明：顶级函数、方法和局部函数。匿名函数表达式从其主体推断出返回类型。事实上，语法甚至不允许返回类型注释。

### DO annotate parameter types on function declarations 在函数声明中注释参数类型

A function’s parameter list determines its boundary to the outside world. Annotating parameter types makes that boundary well defined. Note that even though default parameter values look like variable initializers, Dart doesn’t infer an optional parameter’s type from its default value.
函数的参数列表决定了它与外部世界的边界。注释参数类型使该边界定义明确。请注意，即使默认参数值看起来像变量初始化器，Dart 也不会从其默认值推断可选参数的类型。

```dart
void sayRepeatedly(String message, {int count = 2}) {
  for (var i = 0; i < count; i++) {
    print(message);
  }
}
void sayRepeatedly(message, {count = 2}) {
  for (var i = 0; i < count; i++) {
    print(message);
  }
}
```

**Exception:** Function expressions and initializing formals have different type annotation conventions, as described in the next two guidelines.
例外：函数表达式和初始化形式具有不同的类型注释约定，如接下来的两个准则中所述。

### DON’T annotate inferred parameter types on function expressions 不要在函数表达式上注释推断出的参数类型

Linter rule: [avoid_types_on_closure_parameters](https://dart.dev/tools/linter-rules/avoid_types_on_closure_parameters)
Linter 规则：avoid_types_on_closure_parameters

Anonymous functions are almost always immediately passed to a method taking a callback of some type. When a function expression is created in a typed context, Dart tries to infer the function’s parameter types based on the expected type. For example, when you pass a function expression to `Iterable.map()`, your function’s parameter type is inferred based on the type of callback that `map()` expects:
匿名函数几乎总是立即传递给采用某种类型回调的方法。在类型化上下文中创建函数表达式时，Dart 会尝试根据预期的类型推断函数的参数类型。例如，当您将函数表达式传递给 `Iterable.map()` 时，您的函数的参数类型会根据 `map()` 预期的回调类型进行推断：

```dart
var names = people.map((person) => person.name);
var names = people.map((Person person) => person.name);
```

If the language is able to infer the type you want for a parameter in a function expression, then don’t annotate. In rare cases, the surrounding context isn’t precise enough to provide a type for one or more of the function’s parameters. In those cases, you may need to annotate. (If the function isn’t used immediately, it’s usually better to [make it a named declaration](https://dart.dev/effective-dart/usage#do-use-a-function-declaration-to-bind-a-function-to-a-name).)
如果语言能够推断出函数表达式中某个参数的类型，那么不要添加注释。在极少数情况下，周围的上下文不够精确，无法为一个或多个函数参数提供类型。在这些情况下，您可能需要添加注释。（如果函数没有立即使用，通常最好将其设为命名声明。）

### DON’T type annotate initializing formals 不要对初始化形式参数添加类型注释

Linter rule: [type_init_formals](https://dart.dev/tools/linter-rules/type_init_formals)
Linter 规则：type_init_formals

If a constructor parameter is using `this.` to initialize a field, or `super.` to forward a super parameter, then the type of the parameter is inferred to have the same type as the field or super-constructor parameter respectively.
如果构造函数参数使用 `this.` 初始化字段，或使用 `super.` 转发超类参数，则推断出该参数的类型与字段或超类构造函数参数的类型相同。

```dart
class Point {
  double x, y;
  Point(this.x, this.y);
}

class MyWidget extends StatelessWidget {
  MyWidget({super.key});
}
class Point {
  double x, y;
  Point(double this.x, double this.y);
}

class MyWidget extends StatelessWidget {
  MyWidget({Key? super.key});
}
```

### DO write type arguments on generic invocations that aren’t inferred 请在无法推断的泛型调用中编写类型参数

Dart is pretty smart about inferring type arguments in generic invocations. It looks at the expected type where the expression occurs and the types of values being passed to the invocation. However, sometimes those aren’t enough to fully determine a type argument. In that case, write the entire type argument list explicitly.
Dart 在推断泛型调用中的类型参数方面非常智能。它会查看表达式出现处的预期类型以及传递给调用的值的类型。但是，有时这些信息不足以完全确定类型参数。在这种情况下，请显式编写整个类型参数列表。

```dart
var playerScores = <String, int>{};
final events = StreamController<Event>();
var playerScores = {};
final events = StreamController();
```

Sometimes the invocation occurs as the initializer to a variable declaration. If the variable is *not* local, then instead of writing the type argument list on the invocation itself, you may put a type annotation on the declaration:
有时，调用作为变量声明的初始化程序发生。如果变量不是局部变量，那么您可以将类型注释放在声明中，而不是在调用本身上写类型参数列表：

```dart
class Downloader {
  final Completer<String> response = Completer();
}
class Downloader {
  final response = Completer();
}
```

Annotating the variable also addresses this guideline because now the type arguments *are* inferred.
对变量进行注释也满足了此准则，因为现在可以推断出类型参数。

### DON’T write type arguments on generic invocations that are inferred 不要在推断出的泛型调用上编写类型参数

This is the converse of the previous rule. If an invocation’s type argument list *is* correctly inferred with the types you want, then omit the types and let Dart do the work for you.
这是上一条规则的逆向。如果调用类型参数列表正确推断出您想要的类型，那么省略类型并让 Dart 为您完成工作。

```dart
class Downloader {
  final Completer<String> response = Completer();
}
class Downloader {
  final Completer<String> response = Completer<String>();
}
```

Here, the type annotation on the field provides a surrounding context to infer the type argument of constructor call in the initializer.
此处，字段上的类型注释提供了一个周围上下文，以推断初始化程序中构造函数调用的类型参数。

```dart
var items = Future.value([1, 2, 3]);
var items = Future<List<int>>.value(<int>[1, 2, 3]);
```

Here, the types of the collection and instance can be inferred bottom-up from their elements and arguments.
此处，可以从集合和实例的元素和参数自下而上推断出它们的类型。

### AVOID writing incomplete generic types 避免编写不完整的泛型类型

The goal of writing a type annotation or type argument is to pin down a complete type. However, if you write the name of a generic type but omit its type arguments, you haven’t fully specified the type. In Java, these are called “raw types”. For example:
编写类型注释或类型参数的目标是确定一个完整类型。但是，如果您编写泛型类型的名称但省略其类型参数，则尚未完全指定该类型。在 Java 中，这些称为“原始类型”。例如：

```dart
List numbers = [1, 2, 3];
var completer = Completer<Map>();
```

Here, `numbers` has a type annotation, but the annotation doesn’t provide a type argument to the generic `List`. Likewise, the `Map` type argument to `Completer` isn’t fully specified. In cases like this, Dart will *not* try to “fill in” the rest of the type for you using the surrounding context. Instead, it silently fills in any missing type arguments with `dynamic` (or the bound if the class has one). That’s rarely what you want.
在此处， `numbers` 有一个类型注释，但注释未向泛型 `List` 提供类型参数。同样， `Map` 类型参数到 `Completer` 也未完全指定。在类似这种情况下，Dart 不会尝试使用周围的上下文来“填充”您其余的类型。相反，它会使用 `dynamic` （或类有界限时使用该界限）静默填充任何缺失的类型参数。这很少是您想要的。

Instead, if you’re writing a generic type either in a type annotation or as a type argument inside some invocation, make sure to write a complete type:
相反，如果您在类型注释中或在某些调用中作为类型参数编写泛型类型，请务必编写一个完整的类型：

```dart
List<num> numbers = [1, 2, 3];
var completer = Completer<Map<String, int>>();
```

### DO annotate with `dynamic` instead of letting inference fail 用 `dynamic` 注释，而不是让推断失败

When inference doesn’t fill in a type, it usually defaults to `dynamic`. If `dynamic` is the type you want, this is technically the most terse way to get it. However, it’s not the most *clear* way. A casual reader of your code who sees that an annotation is missing has no way of knowing if you intended it to be `dynamic`, expected inference to fill in some other type, or simply forgot to write the annotation.
当推断未填充类型时，它通常默认为 `dynamic` 。如果 `dynamic` 是您想要的类型，从技术上讲，这是获得它的最简洁方法。但是，这不是最清晰的方法。如果一位普通的读者看到您的代码中缺少注释，他们无法知道您是否打算将其作为 `dynamic` ，期望推断填充其他类型，或者只是忘记编写注释。

When `dynamic` is the type you want, write that explicitly to make your intent clear and highlight that this code has less static safety.
当 `dynamic` 是您想要的类型时，请明确地编写它以明确您的意图并突出显示此代码的静态安全性较低。

```dart
dynamic mergeJson(dynamic original, dynamic changes) => ...
mergeJson(original, changes) => ...
```

Note that it’s OK to omit the type when Dart *successfully* infers `dynamic`.
请注意，当 Dart 成功推断出 `dynamic` 时，可以省略类型。

```dart
Map<String, dynamic> readJson() => ...

void printUsers() {
  var json = readJson();
  var users = json['users'];
  print(users);
}
```

Here, Dart infers `Map<String, dynamic>` for `json` and then from that infers `dynamic` for `users`. It’s fine to leave `users` without a type annotation. The distinction is a little subtle. It’s OK to allow inference to *propagate* `dynamic` through your code from a `dynamic` type annotation somewhere else, but you don’t want it to inject a `dynamic` type annotation in a place where your code did not specify one.
在此，Dart 为 `json` 推断出 `Map<String, dynamic>` ，然后由此为 `users` 推断出 `dynamic` 。可以不为 `users` 添加类型注释。这种区别有点微妙。允许推断从某个 `dynamic` 类型注释处传播到代码中的 `dynamic` 是可以的，但您不希望它在代码未指定类型注释的位置注入 `dynamic` 类型注释。

*info* Before Dart 2, this guideline stated the exact opposite: *don’t* annotate with `dynamic` when it is implicit. With the new stronger type system and type inference, users now expect Dart to behave like an inferred statically-typed language. With that mental model, it is an unpleasant surprise to discover that a region of code has silently lost all of the safety and performance of static types.
在 Dart 2 之前，此准则说明了完全相反的情况：不要在隐式时使用 `dynamic` 进行注释。借助新的更强大的类型系统和类型推断，用户现在希望 Dart 的行为像推断的静态类型语言一样。有了这种思维模型，发现一段代码区域已悄无声息地失去了所有静态类型的安全性和性能，这令人惊讶。

**Exception**: Type annotations on unused parameters (`_`) can be omitted.
例外：可以省略未使用的参数（ `_` ）上的类型注释。

### PREFER signatures in function type annotations 在函数类型注释中首选签名

The identifier `Function` by itself without any return type or parameter signature refers to the special [Function](https://api.dart.dev/stable/dart-core/Function-class.html) type. This type is only marginally more useful than using `dynamic`. If you’re going to annotate, prefer a full function type that includes the parameters and return type of the function.
标识符 `Function` 本身没有任何返回类型或参数签名，它指的是特殊的函数类型。此类型仅比使用 `dynamic` 稍微有用一些。如果您要添加注释，请使用包含函数的参数和返回类型的完整函数类型。

```dart
bool isValid(String value, bool Function(String) test) => ...
bool isValid(String value, Function test) => ...
```

**Exception:** Sometimes, you want a type that represents the union of multiple different function types. For example, you may accept a function that takes one parameter or a function that takes two. Since we don’t have union types, there’s no way to precisely type that and you’d normally have to use `dynamic`. `Function` is at least a little more helpful than that:
例外：有时，您需要一个表示多个不同函数类型的并集的类型。例如，您可以接受一个采用一个参数的函数或一个采用两个参数的函数。由于我们没有并集类型，因此无法精确地键入它，您通常必须使用 `dynamic` 。 `Function` 至少比这更有用一些：

```dart
void handleError(void Function() operation, Function errorHandler) {
  try {
    operation();
  } catch (err, stack) {
    if (errorHandler is Function(Object)) {
      errorHandler(err);
    } else if (errorHandler is Function(Object, StackTrace)) {
      errorHandler(err, stack);
    } else {
      throw ArgumentError('errorHandler has wrong signature.');
    }
  }
}
```

### DON’T specify a return type for a setter 不要为 setter 指定返回类型

Linter rule: [avoid_return_types_on_setters](https://dart.dev/tools/linter-rules/avoid_return_types_on_setters)
Linter 规则：avoid_return_types_on_setters

Setters always return `void` in Dart. Writing the word is pointless.
在 Dart 中，setter 始终返回 `void` 。编写单词毫无意义。

```dart
void set foo(Foo value) { ... }
set foo(Foo value) { ... }
```

### DON’T use the legacy typedef syntax 不要使用旧的 typedef 语法

Linter rule: [prefer_generic_function_type_aliases](https://dart.dev/tools/linter-rules/prefer_generic_function_type_aliases)
Linter 规则：prefer_generic_function_type_aliases

Dart has two notations for defining a named typedef for a function type. The original syntax looks like:
Dart 有两种表示法可用于为函数类型定义命名 typedef。原始语法如下所示：

```dart
typedef int Comparison<T>(T a, T b);
```

That syntax has a couple of problems:
该语法存在几个问题：

- There is no way to assign a name to a *generic* function type. In the above example, the typedef itself is generic. If you reference `Comparison` in your code, without a type argument, you implicitly get the function type `int Function(dynamic, dynamic)`, *not* `int Function<T>(T, T)`. This doesn’t come up in practice often, but it matters in certain corner cases.
  无法为泛型函数类型分配名称。在上面的示例中，typedef 本身是泛型的。如果您在代码中引用 `Comparison` ，而没有类型参数，则隐式获取函数类型 `int Function(dynamic, dynamic)` ，而不是 `int Function<T>(T, T)` 。这在实践中并不常见，但在某些特殊情况下很重要。

- A single identifier in a parameter is interpreted as the parameter’s *name*, not its *type*. Given:
  参数中的单个标识符被解释为参数的名称，而不是其类型。给定：

  ```dart
  typedef bool TestNumber(num);
  ```

  Most users expect this to be a function type that takes a `num` and returns `bool`. It is actually a function type that takes *any* object (`dynamic`) and returns `bool`. The parameter’s *name* (which isn’t used for anything except documentation in the typedef) is “num”. This has been a long-standing source of errors in Dart.
  大多数用户希望这是一个接受 `num` 并返回 `bool` 的函数类型。它实际上是一个接受任何对象 ( `dynamic` ) 并返回 `bool` 的函数类型。参数的名称（除了在 typedef 中用于文档之外，不用于任何其他用途）是“num”。这长期以来一直是 Dart 中错误的根源。

The new syntax looks like this:
新语法如下所示：

```dart
typedef Comparison<T> = int Function(T, T);
```

If you want to include a parameter’s name, you can do that too:
如果您想包含参数的名称，也可以这样做：

```dart
typedef Comparison<T> = int Function(T a, T b);
```

The new syntax can express anything the old syntax could express and more, and lacks the error-prone misfeature where a single identifier is treated as the parameter’s name instead of its type. The same function type syntax after the `=` in the typedef is also allowed anywhere a type annotation may appear, giving us a single consistent way to write function types anywhere in a program.
新的语法可以表达旧语法可以表达的任何内容，并且更多，并且没有错误多发的错误功能，其中单个标识符被视为参数的名称而不是其类型。typedef 中 `=` 之后的相同函数类型语法也允许在任何可能出现类型注释的位置，这为我们在程序中的任何位置编写函数类型提供了一种单一的、一致的方式。

The old typedef syntax is still supported to avoid breaking existing code, but it’s deprecated.
旧的 typedef 语法仍然受支持，以避免破坏现有代码，但它已被弃用。

### PREFER inline function types over typedefs 首选内联函数类型而不是 typedef

Linter rule: [avoid_private_typedef_functions](https://dart.dev/tools/linter-rules/avoid_private_typedef_functions)
Linter 规则：avoid_private_typedef_functions

In Dart, if you want to use a function type for a field, variable, or generic type argument, you can define a typedef for the function type. However, Dart supports an inline function type syntax that can be used anywhere a type annotation is allowed:
在 Dart 中，如果您想对字段、变量或泛型类型参数使用函数类型，则可以为函数类型定义 typedef。但是，Dart 支持内联函数类型语法，可以在允许类型注释的任何位置使用：

```dart
class FilteredObservable {
  final bool Function(Event) _predicate;
  final List<void Function(Event)> _observers;

  FilteredObservable(this._predicate, this._observers);

  void Function(Event)? notify(Event event) {
    if (!_predicate(event)) return null;

    void Function(Event)? last;
    for (final observer in _observers) {
      observer(event);
      last = observer;
    }

    return last;
  }
}
```

It may still be worth defining a typedef if the function type is particularly long or frequently used. But in most cases, users want to see what the function type actually is right where it’s used, and the function type syntax gives them that clarity.
如果函数类型特别长或经常使用，那么定义 typedef 可能仍然值得。但在大多数情况下，用户希望在使用函数类型的位置看到函数类型的实际内容，而函数类型语法为他们提供了这种清晰性。

### PREFER using function type syntax for parameters 首选对参数使用函数类型语法

Linter rule: [use_function_type_syntax_for_parameters](https://dart.dev/tools/linter-rules/use_function_type_syntax_for_parameters)
Linter 规则：use_function_type_syntax_for_parameters

Dart has a special syntax when defining a parameter whose type is a function. Sort of like in C, you surround the parameter’s name with the function’s return type and parameter signature:
在定义类型为函数的参数时，Dart 具有特殊的语法。有点像在 C 中，您用函数的返回类型和参数签名将参数的名称括起来：

```dart
Iterable<T> where(bool predicate(T element)) => ...
```

Before Dart added function type syntax, this was the only way to give a parameter a function type without defining a typedef. Now that Dart has a general notation for function types, you can use it for function-typed parameters as well:
在 Dart 添加函数类型语法之前，这是在不定义 typedef 的情况下为参数指定函数类型的唯一方法。现在 Dart 具有函数类型的通用表示法，您也可以将其用于函数类型参数：

```dart
Iterable<T> where(bool Function(T) predicate) => ...
```

The new syntax is a little more verbose, but is consistent with other locations where you must use the new syntax.
新语法有点冗长，但与必须使用新语法的其他位置一致。

### AVOID using `dynamic` unless you want to disable static checking 除非您想禁用静态检查，否则避免使用 `dynamic`

Some operations work with any possible object. For example, a `log()` method could take any object and call `toString()` on it. Two types in Dart permit all values: `Object?` and `dynamic`. However, they convey different things. If you simply want to state that you allow all objects, use `Object?`. If you want to allow all objects *except* `null`, then use `Object`.
某些操作适用于任何可能的对象。例如， `log()` 方法可以获取任何对象并在其上调用 `toString()` 。Dart 中的两种类型允许所有值： `Object?` 和 `dynamic` 。但是，它们传达了不同的含义。如果您只是想说明允许所有对象，请使用 `Object?` 。如果您想允许除 `null` 之外所有对象，请使用 `Object` 。

The type `dynamic` not only accepts all objects, but it also permits all *operations*. Any member access on a value of type `dynamic` is allowed at compile time, but may fail and throw an exception at runtime. If you want exactly that risky but flexible dynamic dispatch, then `dynamic` is the right type to use.
类型 `dynamic` 不仅接受所有对象，还允许所有操作。在编译时允许对类型 `dynamic` 的值进行任何成员访问，但可能在运行时失败并引发异常。如果您想要这种有风险但灵活的动态分派，那么 `dynamic` 是要使用的正确类型。

Otherwise, prefer using `Object?` or `Object`. Rely on `is` checks and type promotion to ensure that the value’s runtime type supports the member you want to access before you access it.
否则，最好使用 `Object?` 或 `Object` 。依靠 `is` 检查和类型提升来确保在访问值之前，该值运行时类型支持您想要访问的成员。

```dart
/// Returns a Boolean representation for [arg], which must
/// be a String or bool.
bool convertToBool(Object arg) {
  if (arg is bool) return arg;
  if (arg is String) return arg.toLowerCase() == 'true';
  throw ArgumentError('Cannot convert $arg to a bool.');
}
```

The main exception to this rule is when working with existing APIs that use `dynamic`, especially inside a generic type. For example, JSON objects have type `Map<String, dynamic>` and your code will need to accept that same type. Even so, when using a value from one of these APIs, it’s often a good idea to cast it to a more precise type before accessing members.
此规则的主要例外情况是使用 `dynamic` 的现有 API，尤其是在泛型类型内部。例如，JSON 对象具有类型 `Map<String, dynamic>` ，您的代码需要接受相同的类型。即便如此，在使用这些 API 之一的值时，在访问成员之前将其强制转换为更精确的类型通常是一个好主意。

### DO use `Future<void>` as the return type of asynchronous members that do not produce values 将 `Future<void>` 用作不产生值的异步成员的返回类型

When you have a synchronous function that doesn’t return a value, you use `void` as the return type. The asynchronous equivalent for a method that doesn’t produce a value, but that the caller might need to await, is `Future<void>`.
当您具有不返回值的同步函数时，可以使用 `void` 作为返回类型。对于不产生值但调用者可能需要等待的方法，异步等效项是 `Future<void>` 。

You may see code that uses `Future` or `Future<Null>` instead because older versions of Dart didn’t allow `void` as a type argument. Now that it does, you should use it. Doing so more directly matches how you’d type a similar synchronous function, and gives you better error-checking for callers and in the body of the function.
您可能会看到使用 `Future` 或 `Future<Null>` 的代码，因为旧版本的 Dart 不允许 `void` 作为类型参数。现在它允许了，您应该使用它。这样做更直接地匹配您键入类似同步函数的方式，并为您提供更好的错误检查，以便调用者和在函数体中使用。

For asynchronous functions that do not return a useful value and where no callers need to await the asynchronous work or handle an asynchronous failure, use a return type of `void`.
对于不返回值且没有调用者需要等待异步工作或处理异步故障的异步函数，请使用 `void` 作为返回类型。

### AVOID using `FutureOr<T>` as a return type 避免使用 `FutureOr<T>` 作为返回类型

If a method accepts a `FutureOr<int>`, it is [generous in what it accepts](https://en.wikipedia.org/wiki/Robustness_principle). Users can call the method with either an `int` or a `Future<int>`, so they don’t need to wrap an `int` in `Future` that you are going to unwrap anyway.
如果方法接受 `FutureOr<int>` ，则它接受的内容非常宽泛。用户可以使用 `int` 或 `Future<int>` 调用该方法，因此他们无需将 `int` 包装在您要解包的 `Future` 中。

If you *return* a `FutureOr<int>`, users need to check whether get back an `int` or a `Future<int>` before they can do anything useful. (Or they’ll just `await` the value, effectively always treating it as a `Future`.) Just return a `Future<int>`, it’s cleaner. It’s easier for users to understand that a function is either always asynchronous or always synchronous, but a function that can be either is hard to use correctly.
如果您返回 `FutureOr<int>` ，则用户需要检查是否返回 `int` 或 `Future<int>` ，然后才能执行任何有用的操作。（或者他们只会 `await` 该值，实际上始终将其视为 `Future` 。）只需返回 `Future<int>` ，这样更简洁。用户更容易理解函数始终是异步的还是始终是同步的，但既可以是异步的也可以是同步的函数很难正确使用。

```dart
Future<int> triple(FutureOr<int> value) async => (await value) * 3;
FutureOr<int> triple(FutureOr<int> value) {
  if (value is int) return value * 3;
  return value.then((v) => v * 3);
}
```

The more precise formulation of this guideline is to *only use `FutureOr<T>` in [contravariant](https://en.wikipedia.org/wiki/Covariance_and_contravariance_(computer_science)) positions.* Parameters are contravariant and return types are covariant. In nested function types, this gets flipped—if you have a parameter whose type is itself a function, then the callback’s return type is now in contravariant position and the callback’s parameters are covariant. This means it’s OK for a *callback’s* type to return `FutureOr<T>`:
该准则的更精确表述是仅在协变位置中使用 `FutureOr<T>` 。参数是协变的，返回类型是逆变的。在嵌套函数类型中，这一点被颠倒了——如果有一个参数的类型本身是一个函数，那么回调的返回类型现在处于协变位置，而回调的参数是逆变的。这意味着回调的类型返回 `FutureOr<T>` 是可以的：

```dart
Stream<S> asyncMap<T, S>(
    Iterable<T> iterable, FutureOr<S> Function(T) callback) async* {
  for (final element in iterable) {
    yield await callback(element);
  }
}
```

## 参数 Parameters 

In Dart, optional parameters can be either positional or named, but not both.
在 Dart 中，可选参数可以是位置参数或命名参数，但不能同时是两者。

### AVOID positional boolean parameters 避免使用位置布尔参数

Linter rule: [avoid_positional_boolean_parameters](https://dart.dev/tools/linter-rules/avoid_positional_boolean_parameters)
Linter 规则：avoid_positional_boolean_parameters

Unlike other types, booleans are usually used in literal form. Values like numbers are usually wrapped in named constants, but we typically pass around `true` and `false` directly. That can make call sites unreadable if it isn’t clear what the boolean represents:
与其他类型不同，布尔值通常以文字形式使用。数字等值通常用命名常量包装，但我们通常直接传递 `true` 和 `false` 。如果布尔值表示什么不清楚，这可能会使调用站点难以理解：

```
new Task(true);
new Task(false);
new ListBox(false, true, true);
new Button(false);
```

Instead, prefer using named arguments, named constructors, or named constants to clarify what the call is doing.
相反，最好使用命名参数、命名构造函数或命名常量来阐明调用正在做什么。

```dart
Task.oneShot();
Task.repeating();
ListBox(scroll: true, showScrollbars: true);
Button(ButtonState.enabled);
```

Note that this doesn’t apply to setters, where the name makes it clear what the value represents:
请注意，这不适用于 setter，因为名称清楚地说明了该值表示什么：

```
listBox.canScroll = true;
button.isEnabled = false;
```

### AVOID optional positional parameters if the user may want to omit earlier parameters 如果用户可能想要省略前面的参数，请避免使用可选位置参数

Optional positional parameters should have a logical progression such that earlier parameters are passed more often than later ones. Users should almost never need to explicitly pass a “hole” to omit an earlier positional argument to pass later one. You’re better off using named arguments for that.
可选位置参数应具有逻辑顺序，以便前面的参数比后面的参数更常传递。用户几乎不需要显式传递“空位”来省略前面的位置参数以传递后面的参数。最好为此使用命名参数。

```dart
String.fromCharCodes(Iterable<int> charCodes, [int start = 0, int? end]);

DateTime(int year,
    [int month = 1,
    int day = 1,
    int hour = 0,
    int minute = 0,
    int second = 0,
    int millisecond = 0,
    int microsecond = 0]);

Duration(
    {int days = 0,
    int hours = 0,
    int minutes = 0,
    int seconds = 0,
    int milliseconds = 0,
    int microseconds = 0});
```

### AVOID mandatory parameters that accept a special “no argument” value 避免接受特殊“无参数”值的强制参数

If the user is logically omitting a parameter, prefer letting them actually omit it by making the parameter optional instead of forcing them to pass `null`, an empty string, or some other special value that means “did not pass”.

​	如果用户在逻辑上省略了一个参数，最好让他们实际省略它，方法是使参数可选，而不是强迫他们传递 `null` 、空字符串或其他表示“未传递”的特殊值。

Omitting the parameter is more terse and helps prevent bugs where a sentinel value like `null` is accidentally passed when the user thought they were providing a real value.

​	省略参数更加简洁，并有助于防止错误，例如在用户认为他们提供了真实值时意外传递了哨兵值 `null` 。

```dart
var rest = string.substring(start);
var rest = string.substring(start, null);
```

### DO use inclusive start and exclusive end parameters to accept a range 确实使用包含性开始和排他性结束参数来接受范围

If you are defining a method or function that lets a user select a range of elements or items from some integer-indexed sequence, take a start index, which refers to the first item and a (likely optional) end index which is one greater than the index of the last item.
如果您正在定义一个方法或函数，允许用户从某个整数索引序列中选择一系列元素或项目，请采用一个开始索引，它指的是第一个项目，以及一个（可能可选的）结束索引，它比最后一个项目的索引大 1。

This is consistent with core libraries that do the same thing.
这与执行相同操作的核心库是一致的。

```dart
[0, 1, 2, 3].sublist(1, 3) // [1, 2]
'abcd'.substring(1, 3) // 'bc'
```

It’s particularly important to be consistent here because these parameters are usually unnamed. If your API takes a length instead of an end point, the difference won’t be visible at all at the call site.
在这里保持一致性尤其重要，因为这些参数通常没有命名。如果您的 API 采用长度而不是结束点，那么在调用站点根本看不到差异。

## 相等 Equality 

Implementing custom equality behavior for a class can be tricky. Users have deep intuition about how equality works that your objects need to match, and collection types like hash tables have subtle contracts that they expect elements to follow.
为类实现自定义相等性行为可能很棘手。用户对相等性如何发挥作用有深刻的直觉，您的对象需要与之匹配，并且哈希表等集合类型具有微妙的契约，它们期望元素遵循这些契约。

### DO override `hashCode` if you override `==` 如果您重写 `==` ，

Linter rule: [hash_and_equals](https://dart.dev/tools/linter-rules/hash_and_equals)
DO 重写

The default hash code implementation provides an *identity* hash—two objects generally only have the same hash code if they are the exact same object. Likewise, the default behavior for `==` is identity.
Linter 规则：hash_and_equals

If you are overriding `==`, it implies you may have different objects that are considered “equal” by your class. **Any two objects that are equal must have the same hash code.** Otherwise, maps and other hash-based collections will fail to recognize that the two objects are equivalent.
默认哈希代码实现提供标识哈希——两个对象通常只有在它们是完全相同对象时才具有相同的哈希代码。同样， `==` 的默认行为是标识。

### DO make your `==` operator obey the mathematical rules of equality 如果您重写 `==` ，这意味着您可能具有被您的类视为“相等”的不同对象。任何两个相等的对象都必须具有相同的哈希代码。否则，映射和其他基于哈希的集合将无法识别这两个对象是等效的。

An equivalence relation should be:
DO 使您的 运算符遵守相等性的数学规则

- **Reflexive**: `a == a` should always return `true`.
  反身性： `a == a` 应始终返回 `true` 。
- **Symmetric**: `a == b` should return the same thing as `b == a`.
  对称： `a == b` 应返回与 `b == a` 相同的内容。
- **Transitive**: If `a == b` and `b == c` both return `true`, then `a == c` should too.
  传递性：如果 `a == b` 和 `b == c` 都返回 `true` ，则 `a == c` 也应该返回 `true` 。

Users and code that uses `==` expect all of these laws to be followed. If your class can’t obey these rules, then `==` isn’t the right name for the operation you’re trying to express.
用户和使用 `==` 的代码期望遵循所有这些规则。如果您的类无法遵守这些规则，那么 `==` 不是您尝试表达的操作的正确名称。

### AVOID defining custom equality for mutable classes 避免为可变类定义自定义相等性

Linter rule: [avoid_equals_and_hash_code_on_mutable_classes](https://dart.dev/tools/linter-rules/avoid_equals_and_hash_code_on_mutable_classes)
Linter 规则：avoid_equals_and_hash_code_on_mutable_classes

When you define `==`, you also have to define `hashCode`. Both of those should take into account the object’s fields. If those fields *change* then that implies the object’s hash code can change.
定义 `==` 时，您还必须定义 `hashCode` 。这两个都应考虑对象字段。如果这些字段发生更改，则意味着对象的哈希码可能会更改。

Most hash-based collections don’t anticipate that—they assume an object’s hash code will be the same forever and may behave unpredictably if that isn’t true.
大多数基于哈希的集合不会预料到这一点——它们假设对象的哈希码将永远保持不变，如果事实并非如此，则可能会表现出不可预测的行为。

### DON’T make the parameter to `==` nullable 不要使 `==` 的参数可为 null

Linter rule: [avoid_null_checks_in_equality_operators](https://dart.dev/tools/linter-rules/avoid_null_checks_in_equality_operators)
Linter 规则：avoid_null_checks_in_equality_operators

The language specifies that `null` is equal only to itself, and that the `==` method is called only if the right-hand side is not `null`.
该语言规定 `null` 仅等于它本身，并且仅在右侧不是 `null` 时才调用 `==` 方法。

```dart
class Person {
  final String name;
  // ···

  bool operator ==(Object other) => other is Person && name == other.name;
}


class Person {
  final String name;
  // ···

  bool operator ==(Object? other) =>
      other != null && other is Person && name == other.name;
}
```
