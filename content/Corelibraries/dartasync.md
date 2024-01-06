+++
title = "dart:async"
date = 2024-01-05T20:29:36+08:00
weight = 20
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文: [https://dart.dev/libraries/dart-async](https://dart.dev/libraries/dart-async)

# dart:async

Asynchronous programming often uses callback functions, but Dart provides alternatives: [Future](https://api.dart.dev/stable/dart-async/Future-class.html) and [Stream](https://api.dart.dev/stable/dart-async/Stream-class.html) objects. A Future is like a promise for a result to be provided sometime in the future. A Stream is a way to get a sequence of values, such as events. Future, Stream, and more are in the dart:async library ([API reference](https://api.dart.dev/stable/dart-async/dart-async-library.html)).
异步编程通常使用回调函数，但 Dart 提供了替代方案：Future 和 Stream 对象。Future 就像一个承诺，将在未来的某个时间提供结果。Stream 是一种获取一系列值（例如事件）的方法。Future、Stream 等位于 dart:async 库中（API 参考）。

*info* **Note:** You don’t always need to use the Future or Stream APIs directly. The Dart language supports asynchronous coding using keywords such as `async` and `await`. See the [asynchronous programming codelab](https://dart.dev/codelabs/async-await) for details.
注意：您并不总是需要直接使用 Future 或 Stream API。Dart 语言支持使用关键字（例如 `async` 和 `await` ）进行异步编码。有关详细信息，请参阅异步编程代码实验室。

The dart:async library works in both web apps and command-line apps. To use it, import dart:async:
dart:async 库可在网络应用和命令行应用中使用。要使用它，请导入 dart:async：

```dart
import 'dart:async';
```

*tips_and_updates* **Tip:** You don’t need to import dart:async to use the Future and Stream APIs, because dart:core exports those classes.
提示：您无需导入 dart:async 即可使用 Future 和 Stream API，因为 dart:core 导出了这些类。

### Future

Future objects appear throughout the Dart libraries, often as the object returned by an asynchronous method. When a future *completes*, its value is ready to use.
Future 对象出现在整个 Dart 库中，通常作为异步方法返回的对象。当 future 完成时，其值即可使用。

#### Using await 使用 await

Before you directly use the Future API, consider using `await` instead. Code that uses `await` expressions can be easier to understand than code that uses the Future API.
在直接使用 Future API 之前，请考虑改用 `await` 。使用 `await` 表达式的代码可能比使用 Future API 的代码更容易理解。

Consider the following function. It uses Future’s `then()` method to execute three asynchronous functions in a row, waiting for each one to complete before executing the next one.
考虑以下函数。它使用 Future 的 `then()` 方法按顺序执行三个异步函数，等待每个函数完成再执行下一个函数。

```dart
void runUsingFuture() {
  // ...
  findEntryPoint().then((entryPoint) {
    return runExecutable(entryPoint, args);
  }).then(flushThenExit);
}
```

The equivalent code with await expressions looks more like synchronous code:
使用 await 表达式的等效代码看起来更像是同步代码：

```dart
Future<void> runUsingAsyncAwait() async {
  // ...
  var entryPoint = await findEntryPoint();
  var exitCode = await runExecutable(entryPoint, args);
  await flushThenExit(exitCode);
}
```

An `async` function can catch exceptions from Futures. For example:
一个 `async` 函数可以捕获来自 Future 的异常。例如：

```dart
var entryPoint = await findEntryPoint();
try {
  var exitCode = await runExecutable(entryPoint, args);
  await flushThenExit(exitCode);
} catch (e) {
  // Handle the error...
}
```

*error* **Important:** Async functions return Futures. If you don’t want your function to return a future, then use a different solution. For example, you might call an `async` function from your function.
重要提示：异步函数返回 Future。如果您不希望函数返回 Future，请使用其他解决方案。例如，您可以从函数中调用 `async` 函数。

For more information on using `await` and related Dart language features, see the [asynchronous programming codelab](https://dart.dev/codelabs/async-await).
有关使用 `await` 和相关 Dart 语言功能的更多信息，请参阅异步编程代码实验室。

#### Basic usage 基本用法

You can use `then()` to schedule code that runs when the future completes. For example, [`Client.read()`](https://pub.dev/documentation/http/latest/http/Client/read.html) returns a Future, since HTTP requests can take a while. Using `then()` lets you run some code when that Future has completed and the promised string value is available:
您可以使用 `then()` 来安排在 future 完成时运行的代码。例如， `Client.read()` 返回 Future，因为 HTTP 请求可能需要一段时间。使用 `then()` 允许您在 Future 完成且已提供承诺的字符串值时运行一些代码：

```dart
httpClient.read(url).then((String result) {
  print(result);
});
```

Use `catchError()` to handle any errors or exceptions that a Future object might throw.
使用 `catchError()` 来处理 Future 对象可能引发的任何错误或异常。

```dart
httpClient.read(url).then((String result) {
  print(result);
}).catchError((e) {
  // Handle or ignore the error.
});
```

The `then().catchError()` pattern is the asynchronous version of `try`-`catch`.
模式是 `try` - `catch` 的异步版本。

*error* **Important:** Be sure to invoke `catchError()` on the result of `then()`—not on the result of the original Future. Otherwise, the `catchError()` can handle errors only from the original Future’s computation, but not from the handler registered by `then()`.
重要提示：务必对 `then()` 的结果（而不是对原始 Future 的结果）调用 `catchError()` 。否则， `catchError()` 只能处理来自原始 Future 计算的错误，而无法处理由 `then()` 注册的处理程序中的错误。

#### Chaining multiple asynchronous methods 链接多个异步方法

The `then()` method returns a Future, providing a useful way to run multiple asynchronous functions in a certain order. If the callback registered with `then()` returns a Future, `then()` returns a Future that will complete with the same result as the Future returned from the callback. If the callback returns a value of any other type, `then()` creates a new Future that completes with the value.
`then()` 方法返回一个 Future，提供了一种以特定顺序运行多个异步函数的有用方法。如果使用 `then()` 注册的回调返回一个 Future， `then()` 返回一个 Future，该 Future 将以与回调返回的 Future 相同的结果完成。如果回调返回任何其他类型的值， `then()` 将创建一个新的 Future，该 Future 将以该值完成。

```dart
Future result = costlyQuery(url);
result
    .then((value) => expensiveWork(value))
    .then((_) => lengthyComputation())
    .then((_) => print('Done!'))
    .catchError((exception) {
  /* Handle exception... */
});
```

In the preceding example, the methods run in the following order:
在前面的示例中，方法按以下顺序运行：

1. `costlyQuery()`
2. `expensiveWork()`
3. `lengthyComputation()`

Here is the same code written using await:
以下是使用 await 编写的相同代码：

```dart
try {
  final value = await costlyQuery(url);
  await expensiveWork(value);
  await lengthyComputation();
  print('Done!');
} catch (e) {
  /* Handle exception... */
}
```

#### Waiting for multiple futures 等待多个 Future

Sometimes your algorithm needs to invoke many asynchronous functions and wait for them all to complete before continuing. Use the [Future.wait()](https://api.dart.dev/stable/dart-async/Future/wait.html) static method to manage multiple Futures and wait for them to complete:
有时，您的算法需要调用许多异步函数，并在继续之前等待它们全部完成。使用 Future.wait() 静态方法来管理多个 Future 并等待它们完成：

```dart
Future<void> deleteLotsOfFiles() async =>  ...
Future<void> copyLotsOfFiles() async =>  ...
Future<void> checksumLotsOfOtherFiles() async =>  ...

await Future.wait([
  deleteLotsOfFiles(),
  copyLotsOfFiles(),
  checksumLotsOfOtherFiles(),
]);
print('Done with all the long steps!');
```

`Future.wait()` returns a future which completes once all the provided futures have completed. It completes either with their results, or with an error if any of the provided futures fail.
`Future.wait()` 返回一个 future，该 future 在所有提供的 future 完成后完成。它会使用结果完成，或者在任何提供的 future 失败时使用错误完成。

#### Handling errors for multiple futures 处理多个 future 的错误

You can also wait for parallel operations on an [iterable](https://api.dart.dev/stable/dart-async/FutureIterable/wait.html) or [record](https://api.dart.dev/stable/dart-async/FutureRecord2/wait.html) of futures.
您还可以等待可迭代对象或 future 记录上的并行操作。

These extensions return a future with the resulting values of all provided futures. Unlike `Future.wait`, they also let you handle errors.
这些扩展返回一个 future，其中包含所有提供的 future 的结果值。与 `Future.wait` 不同，它们还允许您处理错误。

If any future in the collection completes with an error, `wait` completes with a [`ParallelWaitError`](https://api.dart.dev/stable/dart-async/ParallelWaitError-class.html). This allows the caller to handle individual errors and dispose successful results if necessary.
如果集合中的任何 future 以错误完成，则 `wait` 将使用 `ParallelWaitError` 完成。这允许调用者处理各个错误并在必要时处理成功的结果。

When you *don’t* need the result values from each individual future, use `wait` on an *iterable* of futures:
当您不需要来自每个单独 future 的结果值时，请对 future 的可迭代对象使用 `wait` ：

```
void main() async {
  Future<void> delete() async =>  ...
  Future<void> copy() async =>  ...
  Future<void> errorResult() async =>  ...
  
  try {
    // Wait for each future in a list, returns a list of futures:
    var results = await [delete(), copy(), errorResult()].wait;

    } on ParallelWaitError<List<bool?>, List<AsyncError?>> catch (e) {

    print(e.values[0]);    // Prints successful future
    print(e.values[1]);    // Prints successful future
    print(e.values[2]);    // Prints null when the result is an error

    print(e.errors[0]);    // Prints null when the result is successful
    print(e.errors[1]);    // Prints null when the result is successful
    print(e.errors[2]);    // Prints error
  }

}
```

When you *do* need the individual result values from each future, use `wait` on a *record* of futures. This provides the additional benefit that the futures can be of different types:
当您确实需要来自每个 future 的各个结果值时，请对 future 的记录使用 `wait` 。这提供了额外的优势，即 future 可以是不同类型：

```
void main() async {
  Future<int> delete() async =>  ...
  Future<String> copy() async =>  ...
  Future<bool> errorResult() async =>  ...

  try {    
    // Wait for each future in a record, returns a record of futures:
    (int, String, bool) result = await (delete(), copy(), errorResult()).wait;
  
  } on ParallelWaitError<(int?, String?, bool?),
      (AsyncError?, AsyncError?, AsyncError?)> catch (e) {
    // ...
    }

  // Do something with the results:
  var deleteInt  = result.$1;
  var copyString = result.$2;
  var errorBool  = result.$3;
}
```

### Stream 流

Stream objects appear throughout Dart APIs, representing sequences of data. For example, HTML events such as button clicks are delivered using streams. You can also read a file as a stream.
流对象出现在整个 Dart API 中，表示数据序列。例如，使用流传递 HTML 事件（如按钮点击）。您还可以将文件读取为流。

#### Using an asynchronous for loop 使用异步 for 循环

Sometimes you can use an asynchronous for loop (`await for`) instead of using the Stream API.
有时，您可以使用异步 for 循环 ( `await for` ) 代替使用 Stream API。

Consider the following function. It uses Stream’s `listen()` method to subscribe to a list of files, passing in a function literal that searches each file or directory.
考虑以下函数。它使用 Stream 的 `listen()` 方法订阅文件列表，传入一个搜索每个文件或目录的函数字面量。

```dart
void main(List<String> arguments) {
  // ...
  FileSystemEntity.isDirectory(searchPath).then((isDir) {
    if (isDir) {
      final startingDir = Directory(searchPath);
      startingDir.list().listen((entity) {
        if (entity is File) {
          searchFile(entity, searchTerms);
        }
      });
    } else {
      searchFile(File(searchPath), searchTerms);
    }
  });
}
```

The equivalent code with await expressions, including an asynchronous for loop (`await for`), looks more like synchronous code:
带有 await 表达式的等效代码，包括一个异步 for 循环 ( `await for` )，看起来更像同步代码：

```dart
void main(List<String> arguments) async {
  // ...
  if (await FileSystemEntity.isDirectory(searchPath)) {
    final startingDir = Directory(searchPath);
    await for (final entity in startingDir.list()) {
      if (entity is File) {
        searchFile(entity, searchTerms);
      }
    }
  } else {
    searchFile(File(searchPath), searchTerms);
  }
}
```

*error* **Important:** Before using `await for`, make sure that it makes the code clearer and that you really do want to wait for all of the stream’s results. For example, you usually should **not** use `await for` for DOM event listeners, because the DOM sends endless streams of events. If you use `await for` to register two DOM event listeners in a row, then the second kind of event is never handled.
重要提示：在使用 `await for` 之前，请确保它使代码更清晰，并且您确实想要等待流的所有结果。例如，您通常不应将 `await for` 用于 DOM 事件侦听器，因为 DOM 会发送无休止的事件流。如果您使用 `await for` 连续注册两个 DOM 事件侦听器，则永远不会处理第二种事件。

For more information on using `await` and related Dart language features, see the [asynchronous programming codelab](https://dart.dev/codelabs/async-await).
有关使用 `await` 和相关 Dart 语言功能的更多信息，请参阅异步编程 codelab。

#### Listening for stream data 侦听流数据

To get each value as it arrives, either use `await for` or subscribe to the stream using the `listen()` method:
要获取每个值（在值到达时），请使用 `await for` 或使用 `listen()` 方法订阅流：

```dart
// Add an event handler to a button.
submitButton.onClick.listen((e) {
  // When the button is clicked, it runs this code.
  submitData();
});
```

In this example, the `onClick` property is a `Stream` object provided by the submit button.
在此示例中， `onClick` 属性是由提交按钮提供的 `Stream` 对象。

If you care about only one event, you can get it using a property such as `first`, `last`, or `single`. To test the event before handling it, use a method such as `firstWhere()`, `lastWhere()`, or `singleWhere()`.
如果您只关心一个事件，可以使用 `first` 、 `last` 或 `single` 等属性获取它。要在处理事件之前对其进行测试，请使用 `firstWhere()` 、 `lastWhere()` 或 `singleWhere()` 等方法。

If you care about a subset of events, you can use methods such as `skip()`, `skipWhile()`, `take()`, `takeWhile()`, and `where()`.
如果您关心一个事件子集，可以使用 `skip()` 、 `skipWhile()` 、 `take()` 、 `takeWhile()` 和 `where()` 等方法。

#### Transforming stream data 转换流数据

Often, you need to change the format of a stream’s data before you can use it. Use the `transform()` method to produce a stream with a different type of data:
通常，您需要在使用流数据之前更改其格式。使用 `transform()` 方法生成具有不同类型数据的流：

```dart
var lines =
    inputStream.transform(utf8.decoder).transform(const LineSplitter());
```

This example uses two transformers. First it uses utf8.decoder to transform the stream of integers into a stream of strings. Then it uses a LineSplitter to transform the stream of strings into a stream of separate lines. These transformers are from the dart:convert library (see the [dart:convert section](https://dart.dev/libraries/dart-convert)).
此示例使用两个转换器。首先，它使用 utf8.decoder 将整数流转换为字符串流。然后，它使用 LineSplitter 将字符串流转换为单独行的流。这些转换器来自 dart:convert 库（请参阅 dart:convert 部分）。

#### Handling errors and completion 处理错误和完成

How you specify error and completion handling code depends on whether you use an asynchronous for loop (`await for`) or the Stream API.
您指定错误和完成处理代码的方式取决于您使用异步 for 循环 ( `await for` ) 还是 Stream API。

If you use an asynchronous for loop, then use try-catch to handle errors. Code that executes after the stream is closed goes after the asynchronous for loop.
如果您使用异步 for 循环，则使用 try-catch 来处理错误。流关闭后执行的代码位于异步 for 循环之后。

```dart
Future<void> readFileAwaitFor() async {
  var config = File('config.txt');
  Stream<List<int>> inputStream = config.openRead();

  var lines =
      inputStream.transform(utf8.decoder).transform(const LineSplitter());
  try {
    await for (final line in lines) {
      print('Got ${line.length} characters from stream');
    }
    print('file is now closed');
  } catch (e) {
    print(e);
  }
}
```

If you use the Stream API, then handle errors by registering an `onError` listener. Run code after the stream is closed by registering an `onDone` listener.
如果您使用 Stream API，则通过注册 `onError` 侦听器来处理错误。通过注册 `onDone` 侦听器在流关闭后运行代码。

```dart
var config = File('config.txt');
Stream<List<int>> inputStream = config.openRead();

inputStream.transform(utf8.decoder).transform(const LineSplitter()).listen(
    (String line) {
  print('Got ${line.length} characters from stream');
}, onDone: () {
  print('file is now closed');
}, onError: (e) {
  print(e);
});
```

### More information 更多信息

For some examples of using Future and Stream in command-line apps, check out the [dart:io documentation](https://dart.dev/libraries/dart-io). Also see these articles, codelabs, and tutorials:
有关在命令行应用中使用 Future 和 Stream 的一些示例，请查看 dart:io 文档。另请参阅以下文章、代码实验室和教程：

- [Asynchronous programming: futures, async, await
  异步编程：期货、async、await](https://dart.dev/codelabs/async-await)
- [Futures and error handling
  Future 和错误处理](https://dart.dev/guides/libraries/futures-error-handling)
- [Asynchronous programming: streams
  异步编程：流](https://dart.dev/tutorials/language/streams)
- [Creating streams in Dart
  在 Dart 中创建流](https://dart.dev/articles/libraries/creating-streams)
- [Dart asynchronous programming: Isolates and event loops
  Dart 异步编程：隔离和事件循环](https://dart.dev/language/concurrency)
