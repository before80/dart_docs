+++
title = "Error handling"
date = 2024-01-05T20:29:36+08:00
weight = 60
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文: [https://dart.dev/language/error-handling](https://dart.dev/language/error-handling)

# Error handling 错误处理

## Exceptions 异常

Your Dart code can throw and catch exceptions. Exceptions are errors indicating that something unexpected happened. If the exception isn’t caught, the [isolate](https://dart.dev/language/concurrency#isolates) that raised the exception is suspended, and typically the isolate and its program are terminated.
Dart 代码可以抛出和捕获异常。异常是指示发生意外情况的错误。如果未捕获异常，则引发异常的隔离将被挂起，并且通常会终止隔离及其程序。

In contrast to Java, all of Dart’s exceptions are unchecked exceptions. Methods don’t declare which exceptions they might throw, and you aren’t required to catch any exceptions.
与 Java 不同，Dart 的所有异常都是未经检查的异常。方法不会声明它们可能抛出的异常，并且您不需要捕获任何异常。

Dart provides [`Exception`](https://api.dart.dev/stable/dart-core/Exception-class.html) and [`Error`](https://api.dart.dev/stable/dart-core/Error-class.html) types, as well as numerous predefined subtypes. You can, of course, define your own exceptions. However, Dart programs can throw any non-null object—not just Exception and Error objects—as an exception.
Dart 提供 `Exception` 和 `Error` 类型，以及许多预定义的子类型。当然，您可以定义自己的异常。但是，Dart 程序可以抛出任何非空对象（不仅仅是 Exception 和 Error 对象）作为异常。

### Throw 抛出

Here’s an example of throwing, or *raising*, an exception:
以下是如何抛出或引发异常的示例：

```dart
throw FormatException('Expected at least 1 section');
```

You can also throw arbitrary objects:
您还可以抛出任意对象：

```dart
throw 'Out of llamas!';
```

*info* **Note:** Production-quality code usually throws types that implement [`Error`](https://api.dart.dev/stable/dart-core/Error-class.html) or [`Exception`](https://api.dart.dev/stable/dart-core/Exception-class.html).
注意：生产质量的代码通常会抛出实现 `Error` 或 `Exception` 的类型。

Because throwing an exception is an expression, you can throw exceptions in => statements, as well as anywhere else that allows expressions:
因为抛出异常是一种表达式，所以可以在 => 语句中抛出异常，也可以在允许表达式的任何其他位置抛出异常：

```dart
void distanceTo(Point other) => throw UnimplementedError();
```

### Catch

Catching, or capturing, an exception stops the exception from propagating (unless you rethrow the exception). Catching an exception gives you a chance to handle it:
捕获异常可阻止异常传播（除非您重新抛出异常）。捕获异常可让您有机会处理它：

```dart
try {
  breedMoreLlamas();
} on OutOfLlamasException {
  buyMoreLlamas();
}
```

To handle code that can throw more than one type of exception, you can specify multiple catch clauses. The first catch clause that matches the thrown object’s type handles the exception. If the catch clause does not specify a type, that clause can handle any type of thrown object:
要处理可以抛出多种类型异常的代码，您可以指定多个 catch 子句。第一个与抛出对象的类型匹配的 catch 子句将处理异常。如果 catch 子句未指定类型，则该子句可以处理任何类型的抛出对象：

```dart
try {
  breedMoreLlamas();
} on OutOfLlamasException {
  // A specific exception
  buyMoreLlamas();
} on Exception catch (e) {
  // Anything else that is an exception
  print('Unknown exception: $e');
} catch (e) {
  // No specified type, handles all
  print('Something really unknown: $e');
}
```

As the preceding code shows, you can use either `on` or `catch` or both. Use `on` when you need to specify the exception type. Use `catch` when your exception handler needs the exception object.
如前述代码所示，您可以使用 `on` 或 `catch` 或两者兼用。当您需要指定异常类型时，请使用 `on` 。当您的异常处理程序需要异常对象时，请使用 `catch` 。

You can specify one or two parameters to `catch()`. The first is the exception that was thrown, and the second is the stack trace (a [`StackTrace`](https://api.dart.dev/stable/dart-core/StackTrace-class.html) object).
您可以为 `catch()` 指定一个或两个参数。第一个是抛出的异常，第二个是堆栈跟踪（ `StackTrace` 对象）。

```dart
try {
  // ···
} on Exception catch (e) {
  print('Exception details:\n $e');
} catch (e, s) {
  print('Exception details:\n $e');
  print('Stack trace:\n $s');
}
```

To partially handle an exception, while allowing it to propagate, use the `rethrow` keyword.
要部分处理异常，同时允许它传播，请使用 `rethrow` 关键字。

```dart
void misbehave() {
  try {
    dynamic foo = true;
    print(foo++); // Runtime error
  } catch (e) {
    print('misbehave() partially handled ${e.runtimeType}.');
    rethrow; // Allow callers to see the exception.
  }
}

void main() {
  try {
    misbehave();
  } catch (e) {
    print('main() finished handling ${e.runtimeType}.');
  }
}
```

### Finally 最后

To ensure that some code runs whether or not an exception is thrown, use a `finally` clause. If no `catch` clause matches the exception, the exception is propagated after the `finally` clause runs:
要确保某些代码无论是否引发异常都会运行，请使用 `finally` 子句。如果没有任何 `catch` 子句与异常匹配，则在 `finally` 子句运行后传播异常：

```dart
try {
  breedMoreLlamas();
} finally {
  // Always clean up, even if an exception is thrown.
  cleanLlamaStalls();
}
```

The `finally` clause runs after any matching `catch` clauses:
在任何匹配的 `catch` 子句之后运行 `finally` 子句：

```dart
try {
  breedMoreLlamas();
} catch (e) {
  print('Error: $e'); // Handle the exception first.
} finally {
  cleanLlamaStalls(); // Then clean up.
}
```

To learn more, check out the [core library exception docs](https://dart.dev/libraries/dart-core#exceptions).
要了解更多信息，请查看核心库异常文档。

## Assert 断言

During development, use an assert statement— `assert(<condition>, <optionalMessage>);` —to disrupt normal execution if a boolean condition is false.
在开发过程中，如果布尔条件为假，请使用断言语句 — `assert(<condition>, <optionalMessage>);` — 来中断正常执行。

```dart
// Make sure the variable has a non-null value.
assert(text != null);

// Make sure the value is less than 100.
assert(number < 100);

// Make sure this is an https URL.
assert(urlString.startsWith('https'));
```

To attach a message to an assertion, add a string as the second argument to `assert` (optionally with a [trailing comma](https://dart.dev/language/collections#trailing-comma)):
要将消息附加到断言，请将字符串作为第二个参数添加到 `assert` （可选地带尾随逗号）：

```dart
assert(urlString.startsWith('https'),
    'URL ($urlString) should start with "https".');
```

The first argument to `assert` can be any expression that resolves to a boolean value. If the expression’s value is true, the assertion succeeds and execution continues. If it’s false, the assertion fails and an exception (an [`AssertionError`](https://api.dart.dev/stable/dart-core/AssertionError-class.html)) is thrown.
`assert` 的第一个参数可以是解析为布尔值的任何表达式。如果表达式的值为真，则断言成功且执行继续。如果为假，则断言失败并引发异常（ `AssertionError` ）。

When exactly do assertions work? That depends on the tools and framework you’re using:
断言究竟在什么时候起作用？这取决于您使用的工具和框架：

- Flutter enables assertions in [debug mode.](https://docs.flutter.dev/testing/debugging#debug-mode-assertions)
  Flutter 在调试模式下启用断言。
- Development-only tools such as [`webdev serve`](https://dart.dev/tools/webdev#serve) typically enable assertions by default.
  诸如 `webdev serve` 之类的仅限开发工具通常默认启用断言。
- Some tools, such as [`dart run`](https://dart.dev/tools/dart-run) and [`dart compile js`](https://dart.dev/tools/dart-compile#js) support assertions through a command-line flag: `--enable-asserts`.
  某些工具（例如 `dart run` 和 `dart compile js` ）通过命令行标志支持断言： `--enable-asserts` 。

In production code, assertions are ignored, and the arguments to `assert` aren’t evaluated.
在生产代码中，断言被忽略，并且不会计算 `assert` 的参数。
