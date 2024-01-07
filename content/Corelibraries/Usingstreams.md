+++
title = "异步编程：流"
date = 2024-01-05T20:29:36+08:00
weight = 70
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文: [https://dart.dev/tutorials/language/streams](https://dart.dev/tutorials/language/streams)

## Asynchronous programming: Streams 异步编程：流

#### 有什么意义？ What's the point? 

- Streams provide an asynchronous sequence of data.
- 流提供异步数据序列。
- Data sequences include user-generated events and data read from files.
- 数据序列包括用户生成事件和从文件中读取的数据。
- You can process a stream using either **await for** or `listen()` from the Stream API.
- 您可以使用 await for 或 `listen()` from Stream API 来处理流。
- Streams provide a way to respond to errors.
- 流提供了一种响应错误的方法。
- There are two kinds of streams: single subscription or broadcast.
- 有两种流：单一订阅或广播。

Asynchronous programming in Dart is characterized by the [Future](https://api.dart.dev/stable/dart-async/Future-class.html) and [Stream](https://api.dart.dev/stable/dart-async/Stream-class.html) classes.

​	Dart 中的异步编程的特点是 Future 和 Stream 类。

A Future represents a computation that doesn’t complete immediately. Where a normal function returns the result, an asynchronous function returns a Future, which will eventually contain the result. The future will tell you when the result is ready.

​	Future 表示不会立即完成的计算。普通函数返回结果，异步函数返回 Future，最终将包含结果。Future 会告诉您何时结果已准备就绪。

A stream is a sequence of asynchronous events. It is like an asynchronous Iterable—where, instead of getting the next event when you ask for it, the stream tells you that there is an event when it is ready.

​	流是异步事件序列。它就像异步 Iterable，不同之处在于，当您请求下一个事件时不会立即获取该事件，而是当流准备好时会告诉您有事件。

## 接收流事件 Receiving stream events 

Streams can be created in many ways, which is a topic for another article, but they can all be used in the same way: the *asynchronous for loop* (commonly just called **await for**) iterates over the events of a stream like the **for loop** iterates over an [Iterable](https://api.dart.dev/stable/dart-core/Iterable-class.html). For example:

​	可以创建流的方式有很多种，这是另一篇文章的主题，但它们都可以以相同的方式使用：异步 for 循环（通常简称为 await for）会像 for 循环迭代 Iterable 一样迭代流的事件。例如：

```dart
Future<int> sumStream(Stream<int> stream) async {
  var sum = 0;
  await for (final value in stream) {
    sum += value;
  }
  return sum;
}
```

This code simply receives each event of a stream of integer events, adds them up, and returns (a future of) the sum. When the loop body ends, the function is paused until the next event arrives or the stream is done.

​	此代码只是接收整数事件流的每个事件，将它们相加，然后返回（未来）总和。当循环体结束时，函数将暂停，直到下一个事件到达或流完成。

The function is marked with the `async` keyword, which is required when using the **await for** loop.

​	该函数用 `async` 关键字标记，在使用 await for 循环时需要此关键字。

The following example tests the previous code by generating a simple stream of integers using an `async*` function:

​	以下示例通过使用 `async*` 函数生成简单的整数流来测试前面的代码：

*info* **Note:** This page uses embedded DartPads to display runnable examples. If you see empty boxes instead of DartPads, go to the [DartPad troubleshooting page](https://dart.dev/tools/dartpad/troubleshoot).

​	注意：此页面使用嵌入式 DartPad 来显示可运行的示例。如果您看到的是空框而不是 DartPad，请转到 DartPad 故障排除页面。

<iframe src="https://dartpad.dev/embed-dart.html?theme=light&amp;run=dartpad&amp;split=false&amp;ga_id=false" style="box-sizing: border-box; border: 1px solid rgb(204, 204, 204); margin-bottom: 1rem; min-height: 400px; resize: vertical; width: 896.007px;"></iframe>

*info* **Note:** Click **Run** to see the result in the **Console**.

​	注意：点击运行以在控制台中查看结果。

## 错误事件 Error events 

Streams are done when there are no more events in them, and the code receiving the events is notified of this just as it is notified that a new event arrives. When reading events using an **await for** loop, the loops stops when the stream is done.

​	当流中不再有更多事件时，流便完成了，并且接收事件的代码会收到通知，就像收到新事件到达的通知一样。使用 await for 循环读取事件时，当流完成后，循环会停止。

In some cases, an error happens before the stream is done; perhaps the network failed while fetching a file from a remote server, or perhaps the code creating the events has a bug, but someone needs to know about it.

​	在某些情况下，错误会在流完成之前发生；也许是在从远程服务器获取文件时网络出现故障，或者也许是创建事件的代码存在错误，但有人需要知道它。

Streams can also deliver error events like it delivers data events. Most streams will stop after the first error, but it is possible to have streams that deliver more than one error, and streams that deliver more data after an error event. In this document we only discuss streams that deliver at most one error.

​	流还可以像传递数据事件一样传递错误事件。大多数流会在第一个错误后停止，但有可能流会传递多个错误，并且流会在错误事件后传递更多数据。在本文档中，我们只讨论最多传递一个错误的流。

When reading a stream using **await for**, the error is thrown by the loop statement. This ends the loop, as well. You can catch the error using **try-catch**. The following example throws an error when the loop iterator equals 4:

​	使用 await for 读取流时，错误由循环语句引发。这也将结束循环。您可以使用 try-catch 捕获错误。以下示例在循环迭代器等于 4 时引发错误：

<iframe src="https://dartpad.dev/embed-dart.html?theme=light&amp;run=dartpad&amp;split=false&amp;ga_id=false" style="box-sizing: border-box; border: 1px solid rgb(204, 204, 204); margin-bottom: 1rem; min-height: 400px; resize: vertical; width: 896.007px;"></iframe>

*info* **Note:** Click **Run** to see the result in the **Console**.

​	注意：点击运行以在控制台中查看结果。

## 使用流 Working with streams 

The Stream class contains a number of helper methods that can do common operations on a stream for you, similar to the methods on an [Iterable.](https://api.dart.dev/stable/dart-core/Iterable-class.html) For example, you can find the last positive integer in a stream using `lastWhere()` from the Stream API.

​	Stream 类包含许多帮助程序方法，这些方法可以对流执行常见操作，类似于 Iterable 上的方法。例如，您可以使用 Stream API 中的 `lastWhere()` 查找流中的最后一个正整数。

```dart
Future<int> lastPositive(Stream<int> stream) =>
    stream.lastWhere((x) => x >= 0);
```

## 两种流 Two kinds of streams 

There are two kinds of streams.

​	有两种流。

### 单一订阅流 Single subscription streams 

The most common kind of stream contains a sequence of events that are parts of a larger whole. Events need to be delivered in the correct order and without missing any of them. This is the kind of stream you get when you read a file or receive a web request.

​	最常见的流类型包含一系列事件，这些事件是更大整体的一部分。事件需要按正确顺序传递，并且不能丢失任何事件。这是您在读取文件或接收 Web 请求时获得的流类型。

Such a stream can only be listened to once. Listening again later could mean missing out on initial events, and then the rest of the stream makes no sense. When you start listening, the data will be fetched and provided in chunks.

​	此类流只能侦听一次。稍后再次侦听可能会错过初始事件，然后流的其余部分就毫无意义。开始侦听时，将获取数据并分块提供。

### 广播流 Broadcast streams 

The other kind of stream is intended for individual messages that can be handled one at a time. This kind of stream can be used for mouse events in a browser, for example.

​	另一种流适用于可以一次处理一条的各个消息。例如，此类流可用于浏览器中的鼠标事件。

You can start listening to such a stream at any time, and you get the events that are fired while you listen. More than one listener can listen at the same time, and you can listen again later after canceling a previous subscription.

​	您可以随时开始监听这样的流，并且在监听时获取触发的事件。多个侦听器可以同时侦听，并且您可以在取消以前的订阅后稍后再次侦听。

## 处理流的方法 Methods that process a stream 

The following methods on [Stream](https://api.dart.dev/stable/dart-async/Stream-class.html) process the stream and return a result:

​	Stream 上的以下方法处理流并返回结果：

```dart
Future<T> get first;
Future<bool> get isEmpty;
Future<T> get last;
Future<int> get length;
Future<T> get single;
Future<bool> any(bool Function(T element) test);
Future<bool> contains(Object? needle);
Future<E> drain<E>([E? futureValue]);
Future<T> elementAt(int index);
Future<bool> every(bool Function(T element) test);
Future<T> firstWhere(bool Function(T element) test, {T Function()? orElse});
Future<S> fold<S>(S initialValue, S Function(S previous, T element) combine);
Future forEach(void Function(T element) action);
Future<String> join([String separator = '']);
Future<T> lastWhere(bool Function(T element) test, {T Function()? orElse});
Future pipe(StreamConsumer<T> streamConsumer);
Future<T> reduce(T Function(T previous, T element) combine);
Future<T> singleWhere(bool Function(T element) test, {T Function()? orElse});
Future<List<T>> toList();
Future<Set<T>> toSet();
```

All of these functions, except `drain()` and `pipe()`, correspond to a similar function on [Iterable.](https://api.dart.dev/stable/dart-core/Iterable-class.html) Each one can be written easily by using an `async` function with an **await for** loop (or just using one of the other methods). For example, some implementations could be:

​	除了 `drain()` 和 `pipe()` 之外，所有这些函数都对应于 Iterable 上的类似函数。每个函数都可以通过使用带有 await 循环的 `async` 函数轻松编写（或仅使用其他方法之一）。例如，一些实现可以是：

```dart
Future<bool> contains(Object? needle) async {
  await for (final event in this) {
    if (event == needle) return true;
  }
  return false;
}

Future forEach(void Function(T element) action) async {
  await for (final event in this) {
    action(event);
  }
}

Future<List<T>> toList() async {
  final result = <T>[];
  await forEach(result.add);
  return result;
}

Future<String> join([String separator = '']) async =>
    (await toList()).join(separator);
```

(The actual implementations are slightly more complex, but mainly for historical reasons.)

​	（实际实现稍微复杂一些，但主要出于历史原因。）

## 修改流的方法 Methods that modify a stream 

The following methods on Stream return a new stream based on the original stream. Each one waits until someone listens on the new stream before listening on the original.

​	Stream 上的以下方法基于原始流返回一个新流。每个方法都会等到有人侦听新流后才侦听原始流。

```dart
Stream<R> cast<R>();
Stream<S> expand<S>(Iterable<S> Function(T element) convert);
Stream<S> map<S>(S Function(T event) convert);
Stream<T> skip(int count);
Stream<T> skipWhile(bool Function(T element) test);
Stream<T> take(int count);
Stream<T> takeWhile(bool Function(T element) test);
Stream<T> where(bool Function(T event) test);
```

The preceding methods correspond to similar methods on [Iterable](https://api.dart.dev/stable/dart-core/Iterable-class.html) which transform an iterable into another iterable. All of these can be written easily using an `async` function with an **await for** loop.

​	前面的方法对应于 Iterable 上的类似方法，这些方法将一个可迭代对象转换为另一个可迭代对象。所有这些都可以使用带有 await for 循环的 `async` 函数轻松编写。

```dart
Stream<E> asyncExpand<E>(Stream<E>? Function(T event) convert);
Stream<E> asyncMap<E>(FutureOr<E> Function(T event) convert);
Stream<T> distinct([bool Function(T previous, T next)? equals]);
```

The `asyncExpand()` and `asyncMap()` functions are similar to `expand()` and `map()`, but allow their function argument to be an asynchronous function. The `distinct()` function doesn’t exist on `Iterable`, but it could have.

​	`asyncExpand()` 和 `asyncMap()` 函数类似于 `expand()` 和 `map()` ，但允许其函数参数为异步函数。 `distinct()` 函数在 `Iterable` 上不存在，但它可以存在。

```dart
Stream<T> handleError(Function onError, {bool Function(dynamic error)? test});
Stream<T> timeout(Duration timeLimit,
    {void Function(EventSink<T> sink)? onTimeout});
Stream<S> transform<S>(StreamTransformer<T, S> streamTransformer);
```

The final three functions are more special. They involve error handling which an **await for** loop can’t do—the first error reaching the loops will end the loop and its subscription on the stream. There is no recovering from that. The following code shows how to use `handleError()` to remove errors from a stream before using it in an **await for** loop.

​	最后三个函数更特殊。它们涉及错误处理，这是 await for 循环无法做到的——第一个到达循环的错误将结束循环及其对流的订阅。无法从中恢复。以下代码演示如何使用 `handleError()` 从流中删除错误，然后再在 await for 循环中使用它。

```dart
Stream<S> mapLogErrors<S, T>(
  Stream<T> stream,
  S Function(T event) convert,
) async* {
  var streamWithoutErrors = stream.handleError((e) => log(e));
  await for (final event in streamWithoutErrors) {
    yield convert(event);
  }
}
```

### transform() 函数 The transform() function 

The `transform()` function is not just for error handling; it is a more generalized “map” for streams. A normal map requires one value for each incoming event. However, especially for I/O streams, it might take several incoming events to produce an output event. A [StreamTransformer](https://api.dart.dev/stable/dart-async/StreamTransformer-class.html) can work with that. For example, decoders like [Utf8Decoder](https://api.dart.dev/stable/dart-convert/Utf8Decoder-class.html) are transformers. A transformer requires only one function, [bind()](https://api.dart.dev/stable/dart-async/StreamTransformer/bind.html), which can be easily implemented by an `async` function.

​	函数不仅仅用于错误处理；它还是流的更通用的“映射”。普通映射需要每个传入事件一个值。但是，特别是对于 I/O 流，可能需要多个传入事件才能生成一个输出事件。StreamTransformer 可以处理这种情况。例如，Utf8Decoder 等解码器就是转换器。转换器只需要一个函数 bind()，该函数可以很容易地由 `async` 函数实现。

### 读取和解码文件 Reading and decoding a file 

The following code reads a file and runs two transforms over the stream. It first converts the data from UTF8 and then runs it through a [LineSplitter.](https://api.dart.dev/stable/dart-convert/LineSplitter-class.html) All lines are printed, except any that begin with a hashtag, `#`.

​	以下代码读取一个文件并在流上运行两个转换。它首先将数据从 UTF8 转换，然后通过 LineSplitter 运行它。所有行都会打印出来，除了以井号开头的行， `#` 。

```dart
import 'dart:convert';
import 'dart:io';

void main(List<String> args) async {
  var file = File(args[0]);
  var lines = utf8.decoder
      .bind(file.openRead())
      .transform(const LineSplitter());
  await for (final line in lines) {
    if (!line.startsWith('#')) print(line);
  }
}
```

## listen() 方法 The listen() method 

The final method on Stream is `listen()`. This is a “low-level” method—all other stream functions are defined in terms of `listen()`.

​	Stream 上的最后一个方法是 `listen()` 。这是一个“低级”方法——所有其他流函数都是根据 `listen()` 定义的。

```dart
StreamSubscription<T> listen(void Function(T event)? onData,
    {Function? onError, void Function()? onDone, bool? cancelOnError});
```

To create a new `Stream` type, you can just extend the `Stream` class and implement the `listen()` method—all other methods on `Stream` call `listen()` in order to work.

​	要创建一个新的 `Stream` 类型，您只需扩展 `Stream` 类并实现 `listen()` 方法—— `Stream` 上的所有其他方法都会调用 `listen()` 才能工作。

The `listen()` method allows you to start listening on a stream. Until you do so, the stream is an inert object describing what events you want to see. When you listen, a [StreamSubscription](https://api.dart.dev/stable/dart-async/StreamSubscription-class.html) object is returned which represents the active stream producing events. This is similar to how an `Iterable` is just a collection of objects, but the iterator is the one doing the actual iteration.

​	方法允许您开始监听流。在您这样做之前，流是一个描述您想要查看的事件的惰性对象。当您监听时，将返回一个 StreamSubscription 对象，该对象表示产生事件的活动流。这类似于如何 `Iterable` 只是一个对象集合，但迭代器是执行实际迭代的对象。

The stream subscription allows you to pause the subscription, resume it after a pause, and cancel it completely. You can set callbacks to be called for each data event or error event, and when the stream is closed.

​	流订阅允许您暂停订阅，在暂停后恢复订阅，并完全取消订阅。您可以设置回调，以便在每个数据事件或错误事件以及流关闭时调用。

## 其他资源 Other resources 

Read the following documentation for more details on using streams and asynchronous programming in Dart.

​	阅读以下文档以获取有关在 Dart 中使用流和异步编程的更多详细信息。

- [Creating Streams in Dart](https://dart.dev/articles/libraries/creating-streams), an article about creating your own streams
- 在 Dart 中创建流，一篇有关创建您自己的流的文章
- [Futures and Error Handling](https://dart.dev/guides/libraries/futures-error-handling), an article that explains how to handle errors using the Future API
- 期货和错误处理，一篇解释如何使用 Future API 处理错误的文章
- [Asynchrony support](https://dart.dev/language/async), a section in the [language tour](https://dart.dev/language)
- 异步支持，语言之旅中的一节
- [Stream API reference
  流 API 参考](https://api.dart.dev/stable/dart-async/Stream-class.html)
