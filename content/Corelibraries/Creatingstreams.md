+++
title = "在 Dart 中创建流"
date = 2024-01-05T20:29:36+08:00
weight = 80
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文: [https://dart.dev/articles/libraries/creating-streams](https://dart.dev/articles/libraries/creating-streams)

## Creating streams in Dart 在 Dart 中创建流

*Written by Lasse Nielsen
作者 Lasse Nielsen
April 2013 (updated May 2021)
2013 年 4 月（2021 年 5 月更新）*

The dart:async library contains two types that are important for many Dart APIs: [Stream](https://api.dart.dev/stable/dart-async/Stream-class.html) and [Future.](https://api.dart.dev/stable/dart-async/Future-class.html) Where a Future represents the result of a single computation, a stream is a *sequence* of results. You listen on a stream to get notified of the results (both data and errors) and of the stream shutting down. You can also pause while listening or stop listening to the stream before it is complete.
dart:async 库包含两种对许多 Dart API 很重要的类型：Stream 和 Future。Future 表示单个计算的结果，而流是结果的序列。您监听流以获取结果（数据和错误）以及流关闭的通知。您还可以在监听时暂停或在流完成之前停止监听流。

But this article is not about *using* streams. It’s about creating your own streams. You can create streams in a few ways:
但本文不是关于使用流。而是关于创建您自己的流。您可以通过以下几种方式创建流：

- Transforming existing streams.
  转换现有流。
- Creating a stream from scratch by using an `async*` function.
  通过使用 `async*` 函数从头开始创建流。
- Creating a stream by using a `StreamController`.
  通过使用 `StreamController` 创建流。

This article shows the code for each approach and gives tips to help you implement your stream correctly.
本文展示了每种方法的代码，并提供帮助您正确实现流的提示。

For help on using streams, see [Asynchronous Programming: Streams](https://dart.dev/tutorials/language/streams).
有关使用流的帮助，请参阅异步编程：流。

## Transforming an existing stream 转换现有流

The common case for creating streams is that you already have a stream, and you want to create a new stream based on the original stream’s events. For example you might have a stream of bytes that you want to convert to a stream of strings by UTF-8 decoding the input. The most general approach is to create a new stream that waits for events on the original stream and then outputs new events. Example:
创建流的常见情况是您已有一个流，并且您想要基于原始流的事件创建一个新流。例如，您可能有一个字节流，您想通过 UTF-8 解码输入将其转换为字符串流。最通用的方法是创建一个新流，等待原始流上的事件，然后输出新事件。示例：

```dart
/// Splits a stream of consecutive strings into lines.
///
/// The input string is provided in smaller chunks through
/// the `source` stream.
Stream<String> lines(Stream<String> source) async* {
  // Stores any partial line from the previous chunk.
  var partial = '';
  // Wait until a new chunk is available, then process it.
  await for (final chunk in source) {
    var lines = chunk.split('\n');
    lines[0] = partial + lines[0]; // Prepend partial line.
    partial = lines.removeLast(); // Remove new partial line.
    for (final line in lines) {
      yield line; // Add lines to output stream.
    }
  }
  // Add final partial line to output stream, if any.
  if (partial.isNotEmpty) yield partial;
}
```

For many common transformations, you can use `Stream`-supplied transforming methods such as `map()`, `where()`, `expand()`, and `take()`.
对于许多常见转换，您可以使用 `Stream` 提供的转换方法，例如 `map()` 、 `where()` 、 `expand()` 和 `take()` 。

For example, assume you have a stream, `counterStream`, that emits an increasing counter every second. Here’s how it might be implemented:
例如，假设您有一个流 `counterStream` ，它每秒发出一个递增计数器。以下是如何实现它的方法：

```dart
var counterStream =
    Stream<int>.periodic(const Duration(seconds: 1), (x) => x).take(15);
```

To quickly see the events, you can use code like this:
要快速查看事件，您可以使用如下代码：

```dart
counterStream.forEach(print); // Print an integer every second, 15 times.
```

To transform the stream events, you can invoke a transforming method such as `map()` on the stream before listening to it. The method returns a new stream.
要转换流事件，您可以在侦听流之前调用流上的转换方法，例如 `map()` 。该方法返回一个新流。

```dart
// Double the integer in each event.
var doubleCounterStream = counterStream.map((int x) => x * 2);
doubleCounterStream.forEach(print);
```

Instead of `map()`, you could use any other transforming method, such as the following:
您可以使用任何其他转换方法来代替 `map()` ，例如以下方法：

```dart
.where((int x) => x.isEven) // Retain only even integer events.
.expand((var x) => [x, x]) // Duplicate each event.
.take(5) // Stop after the first five events.
```

Often, a transforming method is all you need. However, if you need even more control over the transformation, you can specify a [StreamTransformer](https://api.dart.dev/stable/dart-async/StreamTransformer-class.html) with `Stream`’s `transform()` method. The platform libraries provide stream transformers for many common tasks. For example, the following code uses the `utf8.decoder` and `LineSplitter` transformers provided by the dart:convert library.
通常，您只需要一个转换方法。但是，如果您需要对转换有更多控制，则可以使用 `Stream` 的 `transform()` 方法指定一个 StreamTransformer。平台库为许多常见任务提供了流转换器。例如，以下代码使用了 dart:convert 库提供的 `utf8.decoder` 和 `LineSplitter` 转换器。

```dart
Stream<List<int>> content = File('someFile.txt').openRead();
List<String> lines = await content
    .transform(utf8.decoder)
    .transform(const LineSplitter())
    .toList();
```

## Creating a stream from scratch 从头开始创建流

One way to create a new stream is with an asynchronous generator (`async*`) function. The stream is created when the function is called, and the function’s body starts running when the stream is listened to. When the function returns, the stream closes. Until the function returns, it can emit events on the stream by using `yield` or `yield*` statements.
创建新流的一种方法是使用异步生成器 ( `async*` ) 函数。当调用该函数时创建流，当流被监听时，函数的主体开始运行。当函数返回时，流关闭。在函数返回之前，它可以通过使用 `yield` 或 `yield*` 语句在流上发出事件。

Here’s a primitive example that emits numbers at regular intervals:
这是一个以规则间隔发出数字的原始示例：

```dart
Stream<int> timedCounter(Duration interval, [int? maxCount]) async* {
  int i = 0;
  while (true) {
    await Future.delayed(interval);
    yield i++;
    if (i == maxCount) break;
  }
}
```

This function returns a `Stream`. When that stream is listened to, the body starts running. It repeatedly delays for the requested interval and then yields the next number. If the `maxCount` parameter is omitted, there is no stop condition on the loop, so the stream outputs increasingly larger numbers forever - or until the listener cancels its subscription.
此函数返回一个 `Stream` 。当监听该流时，主体开始运行。它重复延迟请求的间隔，然后产生下一个数字。如果省略 `maxCount` 参数，则循环没有停止条件，因此流会永远输出越来越大的数字 - 或直到侦听器取消其订阅。

When the listener cancels (by invoking `cancel()` on the `StreamSubscription` object returned by the `listen()` method), then the next time the body reaches a `yield` statement, the `yield` instead acts as a `return` statement. Any enclosing `finally` block is executed, and the function exits. If the function attempts to yield a value before exiting, that fails and acts as a return.
当侦听器取消（通过调用 `listen()` 方法返回的 `StreamSubscription` 对象上的 `cancel()` ）时，下次主体到达 `yield` 语句时， `yield` 反而充当 `return` 语句。执行任何封闭的 `finally` 块，函数退出。如果函数尝试在退出前生成一个值，则该操作失败并充当返回。

When the function finally exits, the future returned by the `cancel()` method completes. If the function exits with an error, the future completes with that error; otherwise, it completes with `null`.
当函数最终退出时， `cancel()` 方法返回的 future 完成。如果函数因错误而退出，则 future 将完成该错误；否则，它将完成 `null` 。

Another, more useful example is a function that converts a sequence of futures to a stream:
另一个更有用的示例是一个将 future 序列转换为流的函数：

```dart
Stream<T> streamFromFutures<T>(Iterable<Future<T>> futures) async* {
  for (final future in futures) {
    var result = await future;
    yield result;
  }
}
```

This function asks the `futures` iterable for a new future, waits for that future, emits the resulting value, and then loops. If a future completes with an error, then the stream completes with that error.
此函数向 `futures` 可迭代对象请求一个新的 future，等待该 future，发出结果值，然后循环。如果 future 因错误而完成，则流将完成该错误。

It’s rare to have an `async*` function building a stream from nothing. It needs to get its data from somewhere, and most often that somewhere is another stream. In some cases, like the sequence of futures above, the data comes from other asynchronous event sources. In many cases, however, an `async*` function is too simplistic to easily handle multiple data sources. That’s where the `StreamController` class comes in.
很少有 `async*` 函数从无构建流。它需要从某个地方获取数据，而大多数情况下，该地方是另一个流。在某些情况下，例如上面的期货序列，数据来自其他异步事件源。然而，在许多情况下， `async*` 函数过于简单，无法轻松处理多个数据源。这就是 `StreamController` 类发挥作用的地方。

## Using a StreamController 使用 StreamController

If the events of your stream comes from different parts of your program, and not just from a stream or futures that can traversed by an `async` function, then use a [StreamController](https://api.dart.dev/stable/dart-async/StreamController-class.html) to create and populate the stream.
如果流的事件来自程序的不同部分，而不仅仅来自流或可由 `async` 函数遍历的 future，那么使用 StreamController 来创建和填充流。

A `StreamController` gives you a new stream and a way to add events to the stream at any point, and from anywhere. The stream has all the logic necessary to handle listeners and pausing. You return the stream and keep the controller to yourself.
`StreamController` 为您提供一个新流和一种在任何点和任何位置向流中添加事件的方法。该流具有处理侦听器和暂停所需的所有逻辑。您返回流并将控制器保留给自己。

The following example (from [stream_controller_bad.dart](https://github.com/dart-lang/site-www/blob/main/examples/misc/lib/articles/creating-streams/stream_controller_bad.dart)) shows a basic, though flawed, usage of `StreamController` to implement the `timedCounter()` function from the previous examples. This code creates a stream to return, and then feeds data into it based on timer events, which are neither futures nor stream events.
以下示例（来自 stream_controller_bad.dart）展示了 `StreamController` 的基本用法（尽管有缺陷），以实现先前示例中的 `timedCounter()` 函数。此代码创建一个要返回的流，然后根据计时器事件（既不是 future 也不是流事件）将数据馈送到该流。

```dart
// NOTE: This implementation is FLAWED!
// It starts before it has subscribers, and it doesn't implement pause.
Stream<int> timedCounter(Duration interval, [int? maxCount]) {
  var controller = StreamController<int>();
  int counter = 0;
  void tick(Timer timer) {
    counter++;
    controller.add(counter); // Ask stream to send counter values as event.
    if (maxCount != null && counter >= maxCount) {
      timer.cancel();
      controller.close(); // Ask stream to shut down and tell listeners.
    }
  }

  Timer.periodic(interval, tick); // BAD: Starts before it has subscribers.
  return controller.stream;
}
```

As before, you can use the stream returned by `timedCounter()` like this:
与之前一样，您可以像这样使用 `timedCounter()` 返回的流：

```dart
var counterStream = timedCounter(const Duration(seconds: 1), 15);
counterStream.listen(print); // Print an integer every second, 15 times.
```

This implementation of `timedCounter()` has a couple of problems:
此 `timedCounter()` 实现存在几个问题：

- It starts producing events before it has subscribers.
  它在有订阅者之前就开始生成事件。
- It keeps producing events even if the subscriber requests a pause.
  即使订阅者请求暂停，它也会继续生成事件。

As the next sections show, you can fix both of these problems by specifying callbacks such as `onListen` and `onPause` when creating the `StreamController`.
正如下一节所示，您可以在创建 `StreamController` 时指定回调（例如 `onListen` 和 `onPause` ）来修复这两个问题。

### Waiting for a subscription 等待订阅

As a rule, streams should wait for subscribers before starting their work. An `async*` function does this automatically, but when using a `StreamController`, you are in full control and can add events even when you shouldn’t. When a stream has no subscriber, its `StreamController` buffers events, which can lead to a memory leak if the stream never gets a subscriber.
通常，流应等待订阅者开始工作。 `async*` 函数会自动执行此操作，但使用 `StreamController` 时，您可以完全控制，即使您不应该添加事件也可以添加事件。当流没有订阅者时，其 `StreamController` 缓冲事件，如果流永远没有订阅者，则可能导致内存泄漏。

Try changing the code that uses the stream to the following:
尝试将使用流的代码更改为以下内容：

```dart
void listenAfterDelay() async {
  var counterStream = timedCounter(const Duration(seconds: 1), 15);
  await Future.delayed(const Duration(seconds: 5));

  // After 5 seconds, add a listener.
  await for (final n in counterStream) {
    print(n); // Print an integer every second, 15 times.
  }
}
```

When this code runs, nothing is printed for the first 5 seconds, although the stream is doing work. Then the listener is added, and the first 5 or so events are printed all at once, since they were buffered by the `StreamController`.
当此代码运行时，尽管流正在工作，但在前 5 秒内不会打印任何内容。然后添加侦听器，并且前 5 个左右的事件会一次性打印出来，因为它们由 `StreamController` 缓冲。

To be notified of subscriptions, specify an `onListen` argument when you create the `StreamController`. The `onListen` callback is called when the stream gets its first subscriber. If you specify an `onCancel` callback, it’s called when the controller loses its last subscriber. In the preceding example, `Timer.periodic()` should move to an `onListen` handler, as shown in the next section.
要接收订阅通知，请在创建 `StreamController` 时指定 `onListen` 参数。当流获取其第一个订阅者时，将调用 `onListen` 回调。如果您指定 `onCancel` 回调，则在控制器失去其最后一个订阅者时会调用该回调。在前面的示例中， `Timer.periodic()` 应移至 `onListen` 处理程序，如下一部分所示。

### Honoring the pause state 尊重暂停状态

Avoid producing events when the listener has requested a pause. An `async*` function automatically pauses at a `yield` statement while the stream subscription is paused. A `StreamController`, on the other hand, buffers events during the pause. If the code providing the events doesn’t respect the pause, the size of the buffer can grow indefinitely. Also, if the listener stops listening soon after pausing, then the work spent creating the buffer is wasted.
当侦听器请求暂停时，避免生成事件。当流订阅暂停时， `async*` 函数会自动在 `yield` 语句处暂停。另一方面， `StreamController` 会在暂停期间缓冲事件。如果提供事件的代码不遵守暂停，则缓冲区的大小可能会无限增长。此外，如果侦听器在暂停后不久停止侦听，那么用于创建缓冲区的工作就浪费了。

To see what happens without pause support, try changing the code that uses the stream to the following:
要了解在没有暂停支持的情况下会发生什么，请尝试将使用流的代码更改为以下内容：

```dart
void listenWithPause() {
  var counterStream = timedCounter(const Duration(seconds: 1), 15);
  late StreamSubscription<int> subscription;

  subscription = counterStream.listen((int counter) {
    print(counter); // Print an integer every second.
    if (counter == 5) {
      // After 5 ticks, pause for five seconds, then resume.
      subscription.pause(Future.delayed(const Duration(seconds: 5)));
    }
  });
}
```

When the five seconds of pause are up, the events fired during that time are all received at once. That happens because the stream’s source doesn’t honor pauses and keeps adding events to the stream. So the stream buffers the events, and it then empties its buffer when the stream becomes unpaused.
五秒的暂停时间结束后，在此期间触发的事件将全部一次性收到。这是因为流的源不遵守暂停，并继续向流中添加事件。因此，流会缓冲事件，然后在流取消暂停时清空其缓冲区。

The following version of `timedCounter()` (from [stream_controller.dart](https://github.com/dart-lang/site-www/blob/main/examples/misc/lib/articles/creating-streams/stream_controller.dart)) implements pause by using the `onListen`, `onPause`, `onResume`, and `onCancel` callbacks on the `StreamController`.
以下版本的 `timedCounter()` （来自 stream_controller.dart）通过使用 `StreamController` 上的 `onListen` 、 `onPause` 、 `onResume` 和 `onCancel` 回调来实现暂停。

```dart
Stream<int> timedCounter(Duration interval, [int? maxCount]) {
  late StreamController<int> controller;
  Timer? timer;
  int counter = 0;

  void tick(_) {
    counter++;
    controller.add(counter); // Ask stream to send counter values as event.
    if (counter == maxCount) {
      timer?.cancel();
      controller.close(); // Ask stream to shut down and tell listeners.
    }
  }

  void startTimer() {
    timer = Timer.periodic(interval, tick);
  }

  void stopTimer() {
    timer?.cancel();
    timer = null;
  }

  controller = StreamController<int>(
      onListen: startTimer,
      onPause: stopTimer,
      onResume: startTimer,
      onCancel: stopTimer);

  return controller.stream;
}
```

Run this code with the `listenWithPause()` function above. You’ll see that it stops counting while paused, and it resumes nicely afterwards.
使用上面的 `listenWithPause()` 函数运行此代码。您会看到它在暂停时停止计数，然后在之后很好地恢复。

You must use all of the listeners—`onListen`, `onCancel`, `onPause`, and `onResume`—to be notified of changes in pause state. The reason is that if the subscription and pause states both change at the same time, only the `onListen` or `onCancel` callback is called.
您必须使用所有侦听器—— `onListen` 、 `onCancel` 、 `onPause` 和 `onResume` ——才能收到暂停状态的更改通知。原因是，如果订阅和暂停状态同时更改，则只会调用 `onListen` 或 `onCancel` 回调。

## Final hints 最后的提示

When creating a stream without using an async* function, keep these tips in mind:
在不使用 async* 函数创建流时，请记住以下提示：

- Be careful when using a synchronous controller—for example, one created using `StreamController(sync: true)`. When you send an event on an unpaused synchronous controller (for example, using the `add()`, `addError()`, or `close()` methods defined by [EventSink](https://api.dart.dev/stable/dart-async/EventSink-class.html)), the event is sent immediately to all listeners on the stream. `Stream` listeners must never be called until the code that added the listener has fully returned, and using a synchronous controller at the wrong time can break this promise and cause good code to fail. Avoid using synchronous controllers.
  小心使用同步控制器——例如，使用 `StreamController(sync: true)` 创建的控制器。当您在未暂停的同步控制器上发送事件时（例如，使用 EventSink 定义的 `add()` 、 `addError()` 或 `close()` 方法），该事件会立即发送给流上的所有侦听器。在添加侦听器的代码完全返回之前，绝不能调用 `Stream` 侦听器，在错误的时间使用同步控制器可能会破坏此承诺并导致良好的代码失败。避免使用同步控制器。

- If you use `StreamController`, the `onListen` callback is called before the `listen` call returns the `StreamSubscription`. Don’t let the `onListen` callback depend on the subscription already existing. For example, in the following code, an `onListen` event fires (and `handler` is called) before the `subscription` variable has a valid value.
  如果您使用 `StreamController` ，则在 `listen` 调用返回 `StreamSubscription` 之前调用 `onListen` 回调。不要让 `onListen` 回调依赖于已存在的订阅。例如，在以下代码中，在 `subscription` 变量具有有效值之前，会触发 `onListen` 事件（并调用 `handler` ）。

  ```dart
  subscription = stream.listen(handler);
  ```

- The `onListen`, `onPause`, `onResume`, and `onCancel` callbacks defined by `StreamController` are called by the stream when the stream’s listener state changes, but never during the firing of an event or during the call of another state change handler. In those cases, the state change callback is delayed until the previous callback is complete.
  当流的侦听器状态发生更改时，流会调用 `StreamController` 定义的 `onListen` 、 `onPause` 、 `onResume` 和 `onCancel` 回调，但绝不会在事件触发期间或在调用另一个状态更改处理程序期间调用。在这些情况下，状态更改回调会延迟，直到上一个回调完成。

- Don’t try to implement the `Stream` interface yourself. It’s easy to get the interaction between events, callbacks, and adding and removing listeners subtly wrong. Always use an existing stream, possibly from a `StreamController`, to implement the `listen` call of a new stream.
  不要尝试自己实现 `Stream` 接口。很容易错误地理解事件、回调以及添加和删除侦听器之间的交互。始终使用现有流（可能来自 `StreamController` ）来实现新流的 `listen` 调用。

- Although it’s possible to create classes that extend `Stream` with more functionality by extending the `Stream` class and implementing the `listen` method and the extra functionality on top, that is generally not recommended because it introduces a new type that users have to consider. Instead of a class that *is* a `Stream` (and more), you can often make a class that *has* a `Stream` (and more).
  虽然可以通过扩展 `Stream` 类并实现 `listen` 方法以及顶部的额外功能来创建扩展 `Stream` 并具有更多功能的类，但通常不建议这样做，因为它引入了一种用户必须考虑的新类型。您可以经常创建一个具有 `Stream` （以及更多）的类，而不是一个 `Stream` （以及更多）的类。
