+++
title = "异步支持"
date = 2024-01-05T20:29:36+08:00
weight = 10
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文: [https://dart.dev/language/async](https://dart.dev/language/async)

## Asynchrony support 异步支持

Dart libraries are full of functions that return [`Future`](https://api.dart.dev/stable/dart-async/Future-class.html) or [`Stream`](https://api.dart.dev/stable/dart-async/Stream-class.html) objects. These functions are *asynchronous*: they return after setting up a possibly time-consuming operation (such as I/O), without waiting for that operation to complete.
Dart 库中充满了返回 `Future` 或 `Stream` 对象的函数。这些函数是异步的：它们在设置可能耗时的操作（例如 I/O）后返回，而无需等待该操作完成。

The `async` and `await` keywords support asynchronous programming, letting you write asynchronous code that looks similar to synchronous code.
关键字 `async` 和 `await` 支持异步编程，让你可以编写看起来类似于同步代码的异步代码。

## 处理 Futures - Handling Futures 

When you need the result of a completed Future, you have two options:

​	当你需要已完成 Future 的结果时，你有两个选择：

- Use `async` and `await`, as described here and in the [asynchronous programming codelab](https://dart.dev/codelabs/async-await).
- 使用 `async` 和 `await` ，如此处和异步编程代码实验室中所述。
- Use the Future API, as described in the [`dart:async` documentation](https://dart.dev/libraries/dart-async#future).
- 使用 Future API，如 `dart:async` 文档中所述。

Code that uses `async` and `await` is asynchronous, but it looks a lot like synchronous code. For example, here’s some code that uses `await` to wait for the result of an asynchronous function:

​	使用 `async` 和 `await` 的代码是异步的，但看起来很像同步代码。例如，以下是一些使用 `await` 等待异步函数结果的代码：

```dart
await lookUpVersion();
```

To use `await`, code must be in an `async` function—a function marked as `async`:

​	要使用 `await` ，代码必须位于 `async` 函数中，即标记为 `async` 的函数：

```dart
Future<void> checkVersion() async {
  var version = await lookUpVersion();
  // Do something with version
}
```

*info* **Note:** Although an `async` function might perform time-consuming operations, it doesn’t wait for those operations. Instead, the `async` function executes only until it encounters its first `await` expression. Then it returns a `Future` object, resuming execution only after the `await` expression completes.

​	注意：尽管 `async` 函数可能会执行耗时的操作，但它不会等待这些操作。相反， `async` 函数仅执行到遇到其第一个 `await` 表达式为止。然后它返回一个 `Future` 对象，仅在 `await` 表达式完成后才恢复执行。

Use `try`, `catch`, and `finally` to handle errors and cleanup in code that uses `await`:

​	使用 `try` 、 `catch` 和 `finally` 来处理使用 `await` 的代码中的错误和清理：

```dart
try {
  version = await lookUpVersion();
} catch (e) {
  // React to inability to look up the version
}
```

You can use `await` multiple times in an `async` function. For example, the following code waits three times for the results of functions:

​	您可以在 `await` 函数中多次使用 `async` 。例如，以下代码等待函数结果三次：

```dart
var entrypoint = await findEntryPoint();
var exitCode = await runExecutable(entrypoint, args);
await flushThenExit(exitCode);
```

In `await *expression*`, the value of `*expression*` is usually a Future; if it isn’t, then the value is automatically wrapped in a Future. This Future object indicates a promise to return an object. The value of `await *expression*` is that returned object. The await expression makes execution pause until that object is available.

​	在 `await *expression*` 中， `*expression*` 的值通常是 Future；如果不是，则该值会自动包装在 Future 中。此 Future 对象表示返回对象的承诺。 `await *expression*` 的值是返回的对象。 await 表达式使执行暂停，直到该对象可用。

**If you get a compile-time error when using `await`, make sure `await` is in an `async` function.** For example, to use `await` in your app’s `main()` function, the body of `main()` must be marked as `async`:

​	如果在使用 `await` 时出现编译时错误，请确保 `await` 位于 `async` 函数中。例如，要在应用的 `main()` 函数中使用 `await` ， `main()` 的主体必须标记为 `async` ：

```dart
void main() async {
  checkVersion();
  print('In main: version is ${await lookUpVersion()}');
}
```

*info* **Note:** The preceding example uses an `async` function (`checkVersion()`) without waiting for a result—a practice that can cause problems if the code assumes that the function has finished executing. To avoid this problem, use the [unawaited_futures linter rule](https://dart.dev/tools/linter-rules/unawaited_futures).
注意：前面的示例使用 `async` 函数 ( `checkVersion()` )，而不等待结果，如果代码假定该函数已完成执行，这种做法可能会导致问题。为避免此问题，请使用 unawaited_futures linter 规则。

For an interactive introduction to using futures, `async`, and `await`, see the [asynchronous programming codelab](https://dart.dev/codelabs/async-await).
有关使用 futures、 `async` 和 `await` 的交互式介绍，请参阅异步编程代码实验室。

## Declaring async functions 声明 async 函数

An `async` function is a function whose body is marked with the `async` modifier.
`async` 函数是其主体用 `async` 修饰符标记的函数。

Adding the `async` keyword to a function makes it return a Future. For example, consider this synchronous function, which returns a String:
为函数添加 `async` 关键字会使其返回一个 Future。例如，考虑这个返回字符串的同步函数：

```dart
String lookUpVersion() => '1.0.0';
```

If you change it to be an `async` function—for example, because a future implementation will be time consuming—the returned value is a Future:
如果您将其更改为 `async` 函数（例如，因为未来的实现将非常耗时），则返回值是一个 Future：

```dart
Future<String> lookUpVersion() async => '1.0.0';
```

Note that the function’s body doesn’t need to use the Future API. Dart creates the Future object if necessary. If your function doesn’t return a useful value, make its return type `Future<void>`.
请注意，函数的主体不需要使用 Future API。Dart 会在必要时创建 Future 对象。如果您的函数不返回有用的值，请将其返回类型设为 `Future<void>` 。

For an interactive introduction to using futures, `async`, and `await`, see the [asynchronous programming codelab](https://dart.dev/codelabs/async-await).
有关使用 future、 `async` 和 `await` 的交互式介绍，请参阅异步编程代码实验室。

## Handling Streams 处理 Stream

When you need to get values from a Stream, you have two options:
当您需要从 Stream 获取值时，您有两个选择：

- Use `async` and an *asynchronous for loop* (`await for`).
  使用 `async` 和异步 for 循环 ( `await for` )。
- Use the Stream API, as described in the [`dart:async` documentation](https://dart.dev/libraries/dart-async#stream).
  使用 Stream API，如 `dart:async` 文档中所述。

*info* **Note:** Before using `await for`, be sure that it makes the code clearer and that you really do want to wait for all of the stream’s results. For example, you usually should **not** use `await for` for UI event listeners, because UI frameworks send endless streams of events.
注意：在使用 `await for` 之前，请确保它使代码更清晰，并且您确实想要等待流的所有结果。例如，您通常不应将 `await for` 用于 UI 事件侦听器，因为 UI 框架会发送无休止的事件流。

An asynchronous for loop has the following form:
异步 for 循环具有以下形式：

```dart
await for (varOrType identifier in expression) {
  // Executes each time the stream emits a value.
}
```

The value of `*expression*` must have type Stream. Execution proceeds as follows:
值 `*expression*` 必须具有 Stream 类型。执行过程如下：

1. Wait until the stream emits a value.
   等待流发出一个值。
2. Execute the body of the for loop, with the variable set to that emitted value.
   执行 for 循环的主体，并将变量设置为发出的值。
3. Repeat 1 and 2 until the stream is closed.
   重复 1 和 2，直到流关闭。

To stop listening to the stream, you can use a `break` or `return` statement, which breaks out of the for loop and unsubscribes from the stream.
要停止侦听流，可以使用 `break` 或 `return` 语句，这会跳出 for 循环并取消订阅流。

**If you get a compile-time error when implementing an asynchronous for loop, make sure the `await for` is in an `async` function.** For example, to use an asynchronous for loop in your app’s `main()` function, the body of `main()` must be marked as `async`:
如果在实现异步 for 循环时遇到编译时错误，请确保 `await for` 位于 `async` 函数中。例如，要在应用的 `main()` 函数中使用异步 for 循环，必须将 `main()` 的主体标记为 `async` ：

```dart
void main() async {
  // ...
  await for (final request in requestServer) {
    handleRequest(request);
  }
  // ...
}
```

For more information about Dart’s asynchronous programming support, check out the [`dart:async`](https://dart.dev/libraries/dart-async) library documentation.
有关 Dart 的异步编程支持的更多信息，请查看 `dart:async` 库文档。
