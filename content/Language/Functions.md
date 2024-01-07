+++
title = "函数"
date = 2024-01-05T20:29:36+08:00
weight = 40
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文: [https://dart.dev/language/functions](https://dart.dev/language/functions)

## Functions 函数

Dart is a true object-oriented language, so even functions are objects and have a type, [Function.](https://api.dart.dev/stable/dart-core/Function-class.html) This means that functions can be assigned to variables or passed as arguments to other functions. You can also call an instance of a Dart class as if it were a function. For details, see [Callable objects](https://dart.dev/language/callable-objects).

​	Dart 是一种真正的面向对象语言，因此即使是函数也是对象，并且具有类型 Function。这意味着可以将函数分配给变量或作为参数传递给其他函数。您还可以像调用函数一样调用 Dart 类的实例。有关详细信息，请参阅可调用对象。

Here’s an example of implementing a function:

​	下面是实现函数的示例：

```dart
bool isNoble(int atomicNumber) {
  return _nobleGases[atomicNumber] != null;
}
```

Although Effective Dart recommends [type annotations for public APIs](https://dart.dev/effective-dart/design#do-type-annotate-fields-and-top-level-variables-if-the-type-isnt-obvious), the function still works if you omit the types:

​	虽然 Effective Dart 建议为公共 API 使用类型注释，但如果省略类型，函数仍然有效：

```dart
isNoble(atomicNumber) {
  return _nobleGases[atomicNumber] != null;
}
```

For functions that contain just one expression, you can use a shorthand syntax:

​	对于仅包含一个表达式的函数，可以使用简写语法：

```dart
bool isNoble(int atomicNumber) => _nobleGases[atomicNumber] != null;
```

The `=> expr` syntax is a shorthand for `{ return expr; }`. The `=>` notation is sometimes referred to as *arrow* syntax.

​	`=> expr` 语法是 `{ return expr; }` 的简写。 `=>` 符号有时称为箭头语法。

*info* **Note:** Only an *expression*—not a *statement*—can appear between the arrow (=>) and the semicolon (;). For example, you can’t put an [if statement](https://dart.dev/language/branches#if) there, but you can use a [conditional expression](https://dart.dev/language/operators#conditional-expressions).

​	注意：箭头 (=>) 和分号 (;) 之间只能出现表达式，不能出现语句。例如，您不能在那里放置 if 语句，但可以使用条件表达式。

## 参数 Parameters 

A function can have any number of *required positional* parameters. These can be followed either by *named* parameters or by *optional positional* parameters (but not both).

​	函数可以具有任意数量的必需位置参数。这些参数后面可以跟命名参数或可选位置参数（但不能同时跟两者）。

*info* **Note:** Some APIs—notably [Flutter](https://flutter.dev/) widget constructors—use only named parameters, even for parameters that are mandatory. See the next section for details.

​	注意：一些 API（尤其是 Flutter 小部件构造函数）仅使用命名参数，即使对于强制性参数也是如此。有关详细信息，请参阅下一部分。

You can use [trailing commas](https://dart.dev/language/collections#lists) when you pass arguments to a function or when you define function parameters.

​	在向函数传递参数或定义函数参数时，可以使用尾随逗号。

### 命名参数 Named parameters 

Named parameters are optional unless they’re explicitly marked as `required`.

​	命名参数是可选的，除非它们被明确标记为 `required` 。

When defining a function, use `{*param1*, *param2*, …}` to specify named parameters. If you don’t provide a default value or mark a named parameter as `required`, their types must be nullable as their default value will be `null`:

​	在定义函数时，使用 `{*param1*, *param2*, …}` 来指定命名参数。如果您未提供默认值或将命名参数标记为 `required` ，则它们的类型必须可为 null，因为它们的默认值将为 `null` ：

```dart
/// Sets the [bold] and [hidden] flags ...
void enableFlags({bool? bold, bool? hidden}) {...}
```

When calling a function, you can specify named arguments using `*paramName*: *value*`. For example:

​	在调用函数时，可以使用 `*paramName*: *value*` 指定命名参数。例如：

```dart
enableFlags(bold: true, hidden: false);
```

To define a default value for a named parameter besides `null`, use `=` to specify a default value. The specified value must be a compile-time constant. For example:

​	要为命名参数定义默认值（除了 `null` ），请使用 `=` 来指定默认值。指定的值必须是编译时常量。例如：

```dart
/// Sets the [bold] and [hidden] flags ...
void enableFlags({bool bold = false, bool hidden = false}) {...}

// bold will be true; hidden will be false.
enableFlags(bold: true);
```

If you instead want a named parameter to be mandatory, requiring callers to provide a value for the parameter, annotate them with `required`:

​	如果您希望命名参数是强制性的，要求调用者为参数提供值，请使用 `required` 对它们进行注释：

```dart
const Scrollbar({super.key, required Widget child});
```

If someone tries to create a `Scrollbar` without specifying the `child` argument, then the analyzer reports an issue.

​	如果有人尝试创建一个 `Scrollbar` 而未指定 `child` 参数，则分析器会报告一个问题。

*info* **Note:** A parameter marked as `required` can still be nullable:

​	注意：标记为 `required` 的参数仍然可以为 null：

```dart
const Scrollbar({super.key, required Widget? child});
```

You might want to place positional arguments first, but Dart doesn’t require it. Dart allows named arguments to be placed anywhere in the argument list when it suits your API:

​	您可能希望首先放置位置参数，但 Dart 不要求这样做。当它适合您的 API 时，Dart 允许将命名参数放在参数列表中的任何位置：

```dart
repeat(times: 2, () {
  ...
});
```

### 可选位置参数 Optional positional parameters 

Wrapping a set of function parameters in `[]` marks them as optional positional parameters. If you don’t provide a default value, their types must be nullable as their default value will be `null`:

​	用 `[]` 包装一组函数参数会将它们标记为可选位置参数。如果您不提供默认值，则它们的类型必须可为 null，因为它们的默认值将为 `null` ：

```dart
String say(String from, String msg, [String? device]) {
  var result = '$from says $msg';
  if (device != null) {
    result = '$result with a $device';
  }
  return result;
}
```

Here’s an example of calling this function without the optional parameter:

​	以下是如何在不使用可选参数的情况下调用此函数的示例：

```dart
assert(say('Bob', 'Howdy') == 'Bob says Howdy');
```

And here’s an example of calling this function with the third parameter:

​	以下是如何使用第三个参数调用此函数的示例：

```dart
assert(say('Bob', 'Howdy', 'smoke signal') ==
    'Bob says Howdy with a smoke signal');
```

To define a default value for an optional positional parameter besides `null`, use `=` to specify a default value. The specified value must be a compile-time constant. For example:

​	要为 `null` 之外的可选位置参数定义默认值，请使用 `=` 指定默认值。指定的值必须是编译时常量。例如：

```dart
String say(String from, String msg, [String device = 'carrier pigeon']) {
  var result = '$from says $msg with a $device';
  return result;
}

assert(say('Bob', 'Howdy') == 'Bob says Howdy with a carrier pigeon');
```

## main() 函数 The main() function 

Every app must have a top-level `main()` function, which serves as the entrypoint to the app. The `main()` function returns `void` and has an optional `List<String>` parameter for arguments.

​	每个应用都必须有一个顶级 `main()` 函数，该函数充当应用的入口点。 `main()` 函数返回 `void` ，并有一个用于参数的可选 `List<String>` 参数。

Here’s a simple `main()` function:

​	以下是一个简单的 `main()` 函数：

```dart
void main() {
  print('Hello, World!');
}
```

Here’s an example of the `main()` function for a command-line app that takes arguments:

​	以下是一个用于获取参数的命令行应用的 `main()` 函数示例：

```dart
// Run the app like this: dart args.dart 1 test
void main(List<String> arguments) {
  print(arguments);

  assert(arguments.length == 2);
  assert(int.parse(arguments[0]) == 1);
  assert(arguments[1] == 'test');
}
```

You can use the [args library](https://pub.dev/packages/args) to define and parse command-line arguments.

​	您可以使用 args 库来定义和解析命令行参数。

## 函数作为一等对象 Functions as first-class objects 

You can pass a function as a parameter to another function. For example:

​	您可以将一个函数作为参数传递给另一个函数。例如：

```dart
void printElement(int element) {
  print(element);
}

var list = [1, 2, 3];

// Pass printElement as a parameter.
list.forEach(printElement);
```

You can also assign a function to a variable, such as:

​	您还可以将函数分配给变量，例如：

```dart
var loudify = (msg) => '!!! ${msg.toUpperCase()} !!!';
assert(loudify('hello') == '!!! HELLO !!!');
```

This example uses an anonymous function. More about those in the next section.

​	此示例使用匿名函数。有关这些内容的更多信息，请参阅下一部分。

## 匿名函数 Anonymous functions 

Most functions are named, such as `main()` or `printElement()`. You can also create a nameless function called an *anonymous function*, or sometimes a *lambda* or *closure*. You might assign an anonymous function to a variable so that, for example, you can add or remove it from a collection.

​	大多数函数都有名称，例如 `main()` 或 `printElement()` 。您还可以创建一个无名函数，称为匿名函数，有时也称为 lambda 或闭包。您可以将匿名函数分配给变量，以便例如，您可以将其添加到集合中或从中删除它。

An anonymous function looks similar to a named function—zero or more parameters, separated by commas and optional type annotations, between parentheses.

​	匿名函数看起来类似于命名函数——零个或多个参数，用逗号分隔，括号中带有可选的类型注释。

The code block that follows contains the function’s body:

​	紧随其后的代码块包含函数的主体：

```
([[*Type*] *param1*[, …]]) { *codeBlock*;};
```

The following example defines an anonymous function with an untyped parameter, `item`, and passes it to the `map` function. The function, invoked for each item in the list, converts each string to uppercase. Then in the anonymous function passed to `forEach`, each converted string is printed out alongside its length.

​	以下示例定义了一个具有未类型化参数 `item` 的匿名函数，并将其传递给 `map` 函数。该函数针对列表中的每个项目调用，将每个字符串转换为大写。然后在传递给 `forEach` 的匿名函数中，每个转换后的字符串都会与其长度一起打印出来。

```dart
const list = ['apples', 'bananas', 'oranges'];
list.map((item) {
  return item.toUpperCase();
}).forEach((item) {
  print('$item: ${item.length}');
});
```

Click **Run** to execute the code.

​	点击运行以执行代码。

<iframe src="https://dartpad.dev/embed-dart.html?theme=light&amp;run=dartpad&amp;split=false&amp;ga_id=anonymous_functions" data-gtm-vis-first-on-screen13029053_11="16941" data-gtm-vis-total-visible-time13029053_11="100" data-gtm-vis-has-fired13029053_11="1" style="box-sizing: border-box; border: 1px solid rgb(204, 204, 204); margin-bottom: 1rem; min-height: 400px; resize: vertical; width: 896.007px; height: 400px;"></iframe>

If the function contains only a single expression or return statement, you can shorten it using arrow notation. Paste the following line into DartPad and click **Run** to verify that it is functionally equivalent.

​	如果函数只包含一个表达式或返回语句，则可以使用箭头符号缩短它。将以下行粘贴到 DartPad 中，然后单击运行以验证它在功能上是等效的。

```dart
list
    .map((item) => item.toUpperCase())
    .forEach((item) => print('$item: ${item.length}'));
```

## 词法作用域 Lexical scope 

Dart is a lexically scoped language, which means that the scope of variables is determined statically, simply by the layout of the code. You can “follow the curly braces outwards” to see if a variable is in scope.

​	Dart 是一种词法作用域语言，这意味着变量的作用域是静态确定的，仅仅通过代码的布局。您可以“向外追踪大括号”以查看变量是否在作用域中。

Here is an example of nested functions with variables at each scope level:

​	下面是具有每个作用域级别变量的嵌套函数示例：

```dart
bool topLevel = true;

void main() {
  var insideMain = true;

  void myFunction() {
    var insideFunction = true;

    void nestedFunction() {
      var insideNestedFunction = true;

      assert(topLevel);
      assert(insideMain);
      assert(insideFunction);
      assert(insideNestedFunction);
    }
  }
}
```

Notice how `nestedFunction()` can use variables from every level, all the way up to the top level.

​	请注意 `nestedFunction()` 如何可以使用每个级别的变量，一直到最顶层。

## 词法闭包 Lexical closures 

A *closure* is a function object that has access to variables in its lexical scope, even when the function is used outside of its original scope.

​	闭包是一个函数对象，可以访问其词法作用域中的变量，即使该函数在原始作用域之外使用。

Functions can close over variables defined in surrounding scopes. In the following example, `makeAdder()` captures the variable `addBy`. Wherever the returned function goes, it remembers `addBy`.

​	函数可以封闭在周围作用域中定义的变量上。在以下示例中， `makeAdder()` 捕获变量 `addBy` 。无论返回的函数到哪里，它都会记住 `addBy` 。

```dart
/// Returns a function that adds [addBy] to the
/// function's argument.
Function makeAdder(int addBy) {
  return (int i) => addBy + i;
}

void main() {
  // Create a function that adds 2.
  var add2 = makeAdder(2);

  // Create a function that adds 4.
  var add4 = makeAdder(4);

  assert(add2(3) == 5);
  assert(add4(3) == 7);
}
```

## 测试函数的相等性 Testing functions for equality 

Here’s an example of testing top-level functions, static methods, and instance methods for equality:

​	以下是一个测试顶级函数、静态方法和实例方法相等性的示例：

```dart
void foo() {} // A top-level function

class A {
  static void bar() {} // A static method
  void baz() {} // An instance method
}

void main() {
  Function x;

  // Comparing top-level functions.
  x = foo;
  assert(foo == x);

  // Comparing static methods.
  x = A.bar;
  assert(A.bar == x);

  // Comparing instance methods.
  var v = A(); // Instance #1 of A
  var w = A(); // Instance #2 of A
  var y = w;
  x = w.baz;

  // These closures refer to the same instance (#2),
  // so they're equal.
  assert(y.baz == x);

  // These closures refer to different instances,
  // so they're unequal.
  assert(v.baz != w.baz);
}
```

## 返回值 Return values 

All functions return a value. If no return value is specified, the statement `return null;` is implicitly appended to the function body.

​	所有函数都会返回值。如果未指定返回值，则语句 `return null;` 会隐式追加到函数体。

```dart
foo() {}

assert(foo() == null);
```

To return multiple values in a function, aggregate the values in a [record](https://dart.dev/language/records#multiple-returns).

​	要在函数中返回多个值，请将值聚合到记录中。

```
(String, int) foo() {
  return ('something', 42);
}
```

## 生成器 Generators 

When you need to lazily produce a sequence of values, consider using a *generator function*. Dart has built-in support for two kinds of generator functions:

​	当您需要惰性生成值序列时，请考虑使用生成器函数。Dart 内置支持两种生成器函数：

- **Synchronous** generator: Returns an [`Iterable`](https://api.dart.dev/stable/dart-core/Iterable-class.html) object.
- 同步生成器：返回 `Iterable` 对象。
- **Asynchronous** generator: Returns a [`Stream`](https://api.dart.dev/stable/dart-async/Stream-class.html) object.
- 异步生成器：返回 `Stream` 对象。

To implement a **synchronous** generator function, mark the function body as `sync`, and use `yield` statements to deliver values:

​	要实现同步生成器函数，请将函数体标记为 `sync` ，并使用 `yield` 语句传递值：

```dart
Iterable<int> naturalsTo(int n) sync {
  int k = 0;
  while (k < n) yield k++;
}
```

To implement an **asynchronous** generator function, mark the function body as `async`, and use `yield` statements to deliver values:

​	要实现异步生成器函数，请将函数体标记为 `async` ，并使用 `yield` 语句传递值：

```dart
Stream<int> asynchronousNaturalsTo(int n) async {
  int k = 0;
  while (k < n) yield k++;
}
```

If your generator is recursive, you can improve its performance by using `yield`:

​	如果生成器是递归的，则可以使用 `yield` 来提高其性能：

```dart
Iterable<int> naturalsDownFrom(int n) sync {
  if (n > 0) {
    yield n;
    yield naturalsDownFrom(n - 1);
  }
}
```
