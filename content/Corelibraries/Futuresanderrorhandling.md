+++
title = "Futures 和错误处理"
date = 2024-01-05T20:29:36+08:00
weight = 90
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文: [https://dart.dev/guides/libraries/futures-error-handling](https://dart.dev/guides/libraries/futures-error-handling)

## Futures and error handling - Futures 和错误处理

The Dart language has native [asynchrony support](https://dart.dev/language/async), making asynchronous Dart code much easier to read and write. However, some code—especially older code—might still use [Future methods](https://api.dart.dev/stable/dart-async/Future-class.html) such as `then()`, `catchError()`, and `whenComplete()`.

​	Dart 语言具有原生异步支持，使异步 Dart 代码更易于阅读和编写。但是，某些代码（尤其是较旧的代码）可能仍使用 Future 方法，例如 `then()` 、 `catchError()` 和 `whenComplete()` 。

This page can help you avoid some common pitfalls when using those Future methods.

​	此页面可帮助您在使用这些 Future 方法时避免一些常见陷阱。

*report_problem* **Warning:** You don’t need this page if your code uses the language’s asynchrony support: `async`, `await`, and error handling using try-catch. For more information, see the [asynchronous programming codelab](https://dart.dev/codelabs/async-await).

​	警告：如果您的代码使用该语言的异步支持，则不需要此页面： `async` 、 `await` 和使用 try-catch 进行错误处理。有关更多信息，请参阅异步编程代码实验室。

## Future API 和回调 The Future API and callbacks 

Functions that use the Future API register callbacks that handle the value (or the error) that completes a Future. For example:

​	使用 Future API 的函数注册处理完成 Future 的值（或错误）的回调。例如：

```dart
myFunc().then(processValue).catchError(handleError);
```

The registered callbacks fire based on the following rules: `then()`’s callback fires if it is invoked on a Future that completes with a value; `catchError()`’s callback fires if it is invoked on a Future that completes with an error.

​	注册的回调根据以下规则触发： `then()` 的回调在使用值完成的 Future 上调用时触发； `catchError()` 的回调在使用错误完成的 Future 上调用时触发。

In the example above, if `myFunc()`’s Future completes with a value, `then()`’s callback fires. If no new error is produced within `then()`, `catchError()`’s callback does not fire. On the other hand, if `myFunc()` completes with an error, `then()`’s callback does not fire, and `catchError()`’s callback does.

​	在上面的示例中，如果 `myFunc()` 的 Future 使用值完成，则 `then()` 的回调触发。如果在 `then()` 内未产生新错误，则 `catchError()` 的回调不会触发。另一方面，如果 `myFunc()` 使用错误完成，则 `then()` 的回调不会触发，而 `catchError()` 的回调会触发。

## 结合 catchError() 使用 then() 的示例 Examples of using then() with catchError() 

Chained `then()` and `catchError()` invocations are a common pattern when dealing with Futures, and can be thought of as the rough equivalent of try-catch blocks.

​	在处理 Future 时，链接的 `then()` 和 `catchError()` 调用是一种常见模式，可以被认为是 try-catch 块的粗略等价物。

The next few sections give examples of this pattern.

​	接下来的几节提供此模式的示例。

### catchError() 作为综合错误处理程序 catchError() as a comprehensive error handler 

The following example deals with throwing an exception from within `then()`’s callback and demonstrates `catchError()`’s versatility as an error handler:

​	以下示例涉及从 `then()` 的回调中引发异常，并演示 `catchError()` 作为错误处理程序的多功能性：

```dart
myFunc().then((value) {
  doSomethingWith(value);
  ...
  throw Exception('Some arbitrary error');
}).catchError(handleError);
```

If `myFunc()`’s Future completes with a value, `then()`’s callback fires. If code within `then()`’s callback throws (as it does in the example above), `then()`’s Future completes with an error. That error is handled by `catchError()`.

​	如果 `myFunc()` 的 Future 完成并带有值，则 `then()` 的回调触发。如果 `then()` 的回调中的代码引发异常（如上面的示例所示），则 `then()` 的 Future 完成并带有错误。此错误由 `catchError()` 处理。

If `myFunc()`’s Future completes with an error, `then()`’s Future completes with that error. The error is also handled by `catchError()`.

​	如果 `myFunc()` 的 Future 完成并带有错误，则 `then()` 的 Future 完成并带有该错误。此错误也由 `catchError()` 处理。

Regardless of whether the error originated within `myFunc()` or within `then()`, `catchError()` successfully handles it.

​	无论错误源自 `myFunc()` 还是源自 `then()` ， `catchError()` 都能成功处理它。

### then() 中的错误处理 - Error handling within then() 

For more granular error handling, you can register a second (`onError`) callback within `then()` to handle Futures completed with errors. Here is `then()`’s signature:

​	为了进行更细粒度的错误处理，您可以在 `then()` 中注册第二个（ `onError` ）回调来处理完成并带有错误的 Future。以下是 `then()` 的签名：

```dart
Future<R> then<R>(FutureOr<R> Function(T value) onValue, {Function? onError});
```

Register the optional onError callback only if you want to differentiate between an error forwarded *to* `then()`, and an error generated *within* `then()`:

​	仅当您想区分转发给 `then()` 的错误和在 `then()` 中生成的错误时，才注册可选的 onError 回调：

```dart
asyncErrorFunction().then(successCallback, onError: (e) {
  handleError(e); // Original error.
  anotherAsyncErrorFunction(); // Oops, new error.
}).catchError(handleError); // Error from within then() handled.
```

In the example above, `asyncErrorFunction()`’s Future’s error is handled with the `onError` callback; `anotherAsyncErrorFunction()` causes `then()`’s Future to complete with an error; this error is handled by `catchError()`.

​	在上面的示例中， `asyncErrorFunction()` 的 Future 的错误由 `onError` 回调处理； `anotherAsyncErrorFunction()` 导致 `then()` 的 Future 完成并带有错误；此错误由 `catchError()` 处理。

In general, implementing two different error handling strategies is not recommended: register a second callback only if there is a compelling reason to catch the error within `then()`.

​	通常，不建议实现两种不同的错误处理策略：仅在有充分理由在 `then()` 中捕获错误时才注册第二个回调。

### 长链中间的错误 Errors in the middle of a long chain 

It is common to have a succession of `then()` calls, and catch errors generated from any part of the chain using `catchError()`:

​	通常会有连续的 `then()` 调用，并使用 `catchError()` 捕获从链的任何部分生成的错误：

```dart
Future<String> one() => Future.value('from one');
Future<String> two() => Future.error('error from two');
Future<String> three() => Future.value('from three');
Future<String> four() => Future.value('from four');

void main() {
  one() // Future completes with "from one".
      .then((_) => two()) // Future completes with two()'s error.
      .then((_) => three()) // Future completes with two()'s error.
      .then((_) => four()) // Future completes with two()'s error.
      .then((value) => value.length) // Future completes with two()'s error.
      .catchError((e) {
    print('Got error: $e'); // Finally, callback fires.
    return 42; // Future completes with 42.
  }).then((value) {
    print('The value is $value');
  });
}

// Output of this program:
//   Got error: error from two
//   The value is 42
```

In the code above, `one()`’s Future completes with a value, but `two()`’s Future completes with an error. When `then()` is invoked on a Future that completes with an error, `then()`’s callback does not fire. Instead, `then()`’s Future completes with the error of its receiver. In our example, this means that after `two()` is called, the Future returned by every subsequent `then()`completes with `two()`’s error. That error is finally handled within `catchError()`.

​	在上面的代码中， `one()` 的 Future 使用值完成，但 `two()` 的 Future 使用错误完成。当对使用错误完成的 Future 调用 `then()` 时， `then()` 的回调不会触发。相反， `then()` 的 Future 使用其接收器的错误完成。在我们的示例中，这意味着在调用 `two()` 之后，每个后续 `then()` 返回的 Future 都使用 `two()` 的错误完成。该错误最终在 `catchError()` 中得到处理。

### 处理特定错误 Handling specific errors 

What if we want to catch a specific error? Or catch more than one error?

​	如果我们想捕获特定错误呢？或者捕获多个错误呢？

`catchError()` takes an optional named argument, `test`, that allows us to query the kind of error thrown.

​	`catchError()` 接受一个可选的命名参数 `test` ，它允许我们查询抛出的错误类型。

```dart
Future<T> catchError(Function onError, {bool Function(Object error)? test});
```

Consider `handleAuthResponse(params)`, a function that authenticates a user based on the params provided, and redirects the user to an appropriate URL. Given the complex workflow, `handleAuthResponse()` could generate various errors and exceptions, and you should handle them differently. Here’s how you can use `test` to do that:

​	考虑 `handleAuthResponse(params)` ，一个根据提供的参数对用户进行身份验证并将其重定向到适当 URL 的函数。鉴于复杂的工作流， `handleAuthResponse()` 可能会生成各种错误和异常，您应该以不同的方式处理它们。以下是如何使用 `test` 来做到这一点：

```dart
void main() {
  handleAuthResponse(const {'username': 'dash', 'age': 3})
      .then((_) => ...)
      .catchError(handleFormatException, test: (e) => e is FormatException)
      .catchError(handleAuthorizationException,
          test: (e) => e is AuthorizationException);
}
```

## 使用 whenComplete() 进行异步 try-catch-finally - Async try-catch-finally using whenComplete() 

If `then().catchError()` mirrors a try-catch, `whenComplete()` is the equivalent of ‘finally’. The callback registered within `whenComplete()` is called when `whenComplete()`’s receiver completes, whether it does so with a value or with an error:

​	如果 `then().catchError()` 类似于 try-catch，那么 `whenComplete()` 等同于“finally”。当 `whenComplete()` 的接收器完成时，无论它是否带有值或错误，都会调用在 `whenComplete()` 中注册的回调：

```dart
final server = connectToServer();
server
    .post(myUrl, fields: const {'name': 'Dash', 'profession': 'mascot'})
    .then(handleResponse)
    .catchError(handleError)
    .whenComplete(server.close);
```

We want to call `server.close` regardless of whether `server.post()` produces a valid response, or an error. We ensure this happens by placing it inside `whenComplete()`.

​	我们希望调用 `server.close` ，无论 `server.post()` 是产生有效响应还是错误。我们通过将其放在 `whenComplete()` 中来确保这一点。

### 完成由 whenComplete() 返回的 Future - Completing the Future returned by whenComplete() 

If no error is emitted from within `whenComplete()`, its Future completes the same way as the Future that `whenComplete()` is invoked on. This is easiest to understand through examples.

​	如果 `whenComplete()` 中没有发出错误，那么它的 Future 将以与调用 `whenComplete()` 的 Future 相同的方式完成。通过示例最容易理解这一点。

In the code below, `then()`’s Future completes with an error, so `whenComplete()`’s Future also completes with that error.

​	在下面的代码中， `then()` 的 Future 以错误完成，因此 `whenComplete()` 的 Future 也以该错误完成。

```dart
void main() {
  asyncErrorFunction()
      // Future completes with an error:
      .then((_) => print("Won't reach here"))
      // Future completes with the same error:
      .whenComplete(() => print('Reaches here'))
      // Future completes with the same error:
      .then((_) => print("Won't reach here"))
      // Error is handled here:
      .catchError(handleError);
}
```

In the code below, `then()`’s Future completes with an error, which is now handled by `catchError()`. Because `catchError()`’s Future completes with `someObject`, `whenComplete()`’s Future completes with that same object.

​	在下面的代码中， `then()` 的 Future 以错误完成，现在由 `catchError()` 处理。因为 `catchError()` 的 Future 以 `someObject` 完成，所以 `whenComplete()` 的 Future 以相同的对象完成。

```dart
void main() {
  asyncErrorFunction()
      // Future completes with an error:
      .then((_) => ...)
      .catchError((e) {
    handleError(e);
    printErrorMessage();
    return someObject; // Future completes with someObject
  }).whenComplete(() => print('Done!')); // Future completes with someObject
}
```

### 源自 whenComplete() 的错误 Errors originating within whenComplete() 

If `whenComplete()`’s callback throws an error, then `whenComplete()`’s Future completes with that error:

​	如果 `whenComplete()` 的回调抛出错误，则 `whenComplete()` 的 Future 将以该错误完成：

```dart
void main() {
  asyncErrorFunction()
      // Future completes with a value:
      .catchError(handleError)
      // Future completes with an error:
      .whenComplete(() => throw Exception('New error'))
      // Error is handled:
      .catchError(handleError);
}
```

## 潜在问题：未能尽早注册错误处理程序 Potential problem: failing to register error handlers early 

It is crucial that error handlers are installed before a Future completes: this avoids scenarios where a Future completes with an error, the error handler is not yet attached, and the error accidentally propagates. Consider this code:

​	至关重要的是，在 Future 完成之前安装错误处理程序：这避免了 Future 以错误完成、错误处理程序尚未附加且错误意外传播的情况。考虑以下代码：

```dart
void main() {
  Future<Object> future = asyncErrorFunction();

  // BAD: Too late to handle asyncErrorFunction() exception.
  Future.delayed(const Duration(milliseconds: 500), () {
    future.then(...).catchError(...);
  });
}
```

In the code above, `catchError()` is not registered until half a second after `asyncErrorFunction()` is called, and the error goes unhandled.

​	在上面的代码中， `catchError()` 直到调用 `asyncErrorFunction()` 后半秒才注册，并且错误未得到处理。

The problem goes away if `asyncErrorFunction()` is called within the `Future.delayed()` callback:

​	如果在 `Future.delayed()` 回调中调用 `asyncErrorFunction()` ，则问题就会消失：

```dart
void main() {
  Future.delayed(const Duration(milliseconds: 500), () {
    asyncErrorFunction()
        .then(...)
        .catchError(...); // We get here.
  });
}
```

## 潜在问题：意外混合同步和异步错误 Potential problem: accidentally mixing synchronous and asynchronous errors 

Functions that return Futures should almost always emit their errors in the future. Since we do not want the caller of such functions to have to implement multiple error-handling scenarios, we want to prevent any synchronous errors from leaking out. Consider this code:

​	返回 Future 的函数几乎总是应该在 future 中发出错误。由于我们不希望此类函数的调用者必须实现多个错误处理方案，因此我们希望防止任何同步错误泄漏。考虑以下代码：

```dart
Future<int> parseAndRead(Map<String, dynamic> data) {
  final filename = obtainFilename(data); // Could throw.
  final file = File(filename);
  return file.readAsString().then((contents) {
    return parseFileData(contents); // Could throw.
  });
}
```

Two functions in that code could potentially throw synchronously: `obtainFilename()` and `parseFileData()`. Because `parseFileData()` executes inside a `then()` callback, its error does not leak out of the function. Instead, `then()`’s Future completes with `parseFileData()`’s error, the error eventually completes `parseAndRead()`’s Future, and the error can be successfully handled by `catchError()`.

​	该代码中的两个函数可能会同步引发： `obtainFilename()` 和 `parseFileData()` 。因为 `parseFileData()` 在 `then()` 回调中执行，所以它的错误不会从函数中泄漏。相反， `then()` 的 Future 使用 `parseFileData()` 的错误完成，该错误最终完成 `parseAndRead()` 的 Future，并且 `catchError()` 可以成功处理该错误。

But `obtainFilename()` is not called within a `then()` callback; if *it* throws, a synchronous error propagates:

​	但是 `obtainFilename()` 并未在 `then()` 回调中调用；如果它引发，则同步错误会传播：

```dart
void main() {
  parseAndRead(data).catchError((e) {
    print('Inside catchError');
    print(e);
    return -1;
  });
}

// Program Output:
//   Unhandled exception:
//   <error from obtainFilename>
//   ...
```

Because using `catchError()` does not capture the error, a client of `parseAndRead()` would implement a separate error-handling strategy for this error.

​	由于使用 `catchError()` 不会捕获错误，因此 `parseAndRead()` 的客户端将为此错误实现单独的错误处理策略。

### 解决方案：使用 Future.sync() 包装您的代码 Solution: Using Future.sync() to wrap your code 

A common pattern for ensuring that no synchronous error is accidentally thrown from a function is to wrap the function body inside a new `Future.sync()` callback:

​	确保不会从函数意外引发任何同步错误的常见模式是将函数主体包装在新 `Future.sync()` 回调中：

```dart
Future<int> parseAndRead(Map<String, dynamic> data) {
  return Future.sync(() {
    final filename = obtainFilename(data); // Could throw.
    final file = File(filename);
    return file.readAsString().then((contents) {
      return parseFileData(contents); // Could throw.
    });
  });
}
```

If the callback returns a non-Future value, `Future.sync()`’s Future completes with that value. If the callback throws (as it does in the example above), the Future completes with an error. If the callback itself returns a Future, the value or the error of that Future completes `Future.sync()`’s Future.

​	如果回调返回一个非 Future 值， `Future.sync()` 的 Future 将使用该值完成。如果回调抛出异常（如上面的示例所示），Future 将使用错误完成。如果回调本身返回一个 Future，该 Future 的值或错误将完成 `Future.sync()` 的 Future。

With code wrapped within `Future.sync()`, `catchError()` can handle all errors:

​	使用 `Future.sync()` 包装的代码， `catchError()` 可以处理所有错误：

```dart
void main() {
  parseAndRead(data).catchError((e) {
    print('Inside catchError');
    print(e);
    return -1;
  });
}

// Program Output:
//   Inside catchError
//   <error from obtainFilename>
```

`Future.sync()` makes your code resilient against uncaught exceptions. If your function has a lot of code packed into it, chances are that you could be doing something dangerous without realizing it:

​	`Future.sync()` 使您的代码能够抵御未捕获的异常。如果您的函数包含大量代码，您很可能在不知情的情况下做一些危险的事情：

```dart
Future fragileFunc() {
  return Future.sync(() {
    final x = someFunc(); // Unexpectedly throws in some rare cases.
    var y = 10 / x; // x should not equal 0.
    ...
  });
}
```

`Future.sync()` not only allows you to handle errors you know might occur, but also prevents errors from *accidentally* leaking out of your function.

​	`Future.sync()` 不仅允许您处理您知道可能发生的错误，而且还可以防止错误意外泄漏出您的函数。

## 更多信息 More information 

See the [Future API reference](https://api.dart.dev/stable/dart-async/Future-class.html) for more information on Futures.

​	请参阅 Future API 参考以获取有关 Future 的更多信息。
