+++
title = "dart:io"
date = 2024-01-05T20:29:36+08:00
weight = 50
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文: [https://dart.dev/libraries/dart-io](https://dart.dev/libraries/dart-io)

## dart:io

The [dart:io](https://api.dart.dev/stable/dart-io/dart-io-library.html) library provides APIs to deal with files, directories, processes, sockets, WebSockets, and HTTP clients and servers.

​	dart:io 库提供用于处理文件、目录、进程、套接字、WebSocket 以及 HTTP 客户端和服务器的 API。

*error* **Important:** Only non-web [Flutter apps,](https://flutter.dev/) command-line scripts, and servers can import and use `dart:io`, not web apps.

​	重要提示：只有非 Web Flutter 应用、命令行脚本和服务器可以导入和使用 `dart:io` ，Web 应用不行。

In general, the dart:io library implements and promotes an asynchronous API. Synchronous methods can easily block an application, making it difficult to scale. Therefore, most operations return results via Future or Stream objects, a pattern common with modern server platforms such as Node.js.

​	通常， dart:io 库实现并推广异步 API。同步方法很容易阻塞应用程序，使其难以扩展。因此，大多数操作通过 Future 或 Stream 对象返回结果，这是 Node.js 等现代服务器平台常见的模式。

The few synchronous methods in the dart:io library are clearly marked with a Sync suffix on the method name. Synchronous methods aren’t covered here.

​	dart:io 库中的少数几个同步方法在方法名上都清楚地标有 Sync 后缀。此处不介绍同步方法。

To use the dart:io library you must import it:

​	要使用 dart:io 库，您必须导入它：

```dart
import 'dart:io';
```

### 文件和目录 Files and directories 

The I/O library enables command-line apps to read and write files and browse directories. You have two choices for reading the contents of a file: all at once, or streaming. Reading a file all at once requires enough memory to store all the contents of the file. If the file is very large or you want to process it while reading it, you should use a Stream, as described in [Streaming file contents](https://dart.dev/libraries/dart-io#streaming-file-contents).

​	I/O 库使命令行应用能够读取和写入文件以及浏览目录。您有两种选择来读取文件的内容：一次性读取或流式读取。一次性读取文件需要足够的内存来存储文件的所有内容。如果文件非常大，或者您想在读取时处理它，则应使用流，如流式文件内容中所述。

#### 将文件作为文本读取 Reading a file as text 

When reading a text file encoded using UTF-8, you can read the entire file contents with `readAsString()`. When the individual lines are important, you can use `readAsLines()`. In both cases, a Future object is returned that provides the contents of the file as one or more strings.

​	在读取使用 UTF-8 编码的文本文件时，您可以使用 `readAsString()` 读取整个文件内容。当各个行很重要时，您可以使用 `readAsLines()` 。在这两种情况下，都会返回一个 Future 对象，该对象将文件的内容作为一或多个字符串提供。

```dart
void main() async {
  var config = File('config.txt');

  // Put the whole file in a single string.
  var stringContents = await config.readAsString();
  print('The file is ${stringContents.length} characters long.');

  // Put each line of the file into its own string.
  var lines = await config.readAsLines();
  print('The file is ${lines.length} lines long.');
}
```

#### 将文件作为二进制文件读取 Reading a file as binary 

The following code reads an entire file as bytes into a list of ints. The call to `readAsBytes()` returns a Future, which provides the result when it’s available.

​	以下代码将整个文件作为字节读取到一个整数列表中。对 `readAsBytes()` 的调用返回一个 Future，该 Future 在可用时提供结果。

```dart
void main() async {
  var config = File('config.txt');

  var contents = await config.readAsBytes();
  print('The file is ${contents.length} bytes long.');
}
```

#### 处理错误 Handling errors 

To capture errors so they don’t result in uncaught exceptions, you can register a `catchError` handler on the Future, or (in an `async` function) use try-catch:

​	要捕获错误，以免导致未捕获的异常，您可以在 Future 上注册一个 `catchError` 处理程序，或（在 `async` 函数中）使用 try-catch：

```dart
void main() async {
  var config = File('config.txt');
  try {
    var contents = await config.readAsString();
    print(contents);
  } catch (e) {
    print(e);
  }
}
```

#### 流式传输文件内容 Streaming file contents 

Use a Stream to read a file, a little at a time. You can use either the [Stream API](https://dart.dev/libraries/dart-async#stream) or `await for`, part of Dart’s [asynchrony support.](https://dart.dev/language/async)

​	使用 Stream 一次读取少量文件。您可以使用 Stream API 或 `await for` ，它是 Dart 的异步支持的一部分。

```dart
import 'dart:io';
import 'dart:convert';

void main() async {
  var config = File('config.txt');
  Stream<List<int>> inputStream = config.openRead();

  var lines = utf8.decoder.bind(inputStream).transform(const LineSplitter());
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

#### 写入文件内容 Writing file contents 

You can use an [IOSink](https://api.dart.dev/stable/dart-io/IOSink-class.html) to write data to a file. Use the File `openWrite()` method to get an IOSink that you can write to. The default mode, `FileMode.write`, completely overwrites existing data in the file.

​	您可以使用 IOSink 将数据写入文件。使用 File `openWrite()` 方法获取可写入的 IOSink。默认模式 `FileMode.write` 会完全覆盖文件中的现有数据。

```dart
var logFile = File('log.txt');
var sink = logFile.openWrite();
sink.write('FILE ACCESSED ${DateTime.now()}\n');
await sink.flush();
await sink.close();
```

To add to the end of the file, use the optional `mode` parameter to specify `FileMode.append`:

​	要添加到文件末尾，请使用可选的 `mode` 参数指定 `FileMode.append` ：

```dart
var sink = logFile.openWrite(mode: FileMode.append);
```

To write binary data, use `add(List<int> data)`.

​	要写入二进制数据，请使用 `add(List<int> data)` 。

#### Listing files in a directory 列出目录中的文件

Finding all files and subdirectories for a directory is an asynchronous operation. The `list()` method returns a Stream that emits an object when a file or directory is encountered.
查找目录的所有文件和子目录是一个异步操作。 `list()` 方法返回一个 Stream，在遇到文件或目录时发出对象。

```dart
void main() async {
  var dir = Directory('tmp');

  try {
    var dirList = dir.list();
    await for (final FileSystemEntity f in dirList) {
      if (f is File) {
        print('Found file ${f.path}');
      } else if (f is Directory) {
        print('Found dir ${f.path}');
      }
    }
  } catch (e) {
    print(e.toString());
  }
}
```

#### 其他常见功能 Other common functionality 

The File and Directory classes contain other functionality, including but not limited to:

​	File 和 Directory 类包含其他功能，包括但不限于：

- Creating a file or directory: `create()` in File and Directory
- 创建文件或目录： File 和 Directory 中的 `create()`
- Deleting a file or directory: `delete()` in File and Directory
- 删除文件或目录： File 和 Directory 中的 `delete()`
- Getting the length of a file: `length()` in File
- 获取文件长度： File 中的 `length()`
- Getting random access to a file: `open()` in File
- 获取对文件的随机访问： File 中的 `open()`

Refer to the API docs for [File](https://api.dart.dev/stable/dart-io/File-class.html) and [Directory](https://api.dart.dev/stable/dart-io/Directory-class.html) for a full list of methods.

​	有关方法的完整列表，请参阅 File 和 Directory 的 API 文档。

### 客户端和服务器 HTTP clients and servers HTTP 

The dart:io library provides classes that command-line apps can use for accessing HTTP resources, as well as running HTTP servers.

​	dart:io 库提供命令行应用程序可用于访问 HTTP 资源以及运行 HTTP 服务器的类。

#### HTTP 服务器 HTTP server 

The [HttpServer](https://api.dart.dev/stable/dart-io/HttpServer-class.html) class provides the low-level functionality for building web servers. You can match request handlers, set headers, stream data, and more.

​	HttpServer 类提供用于构建 Web 服务器的底层功能。您可以匹配请求处理程序、设置标头、流式传输数据等。

The following sample web server returns simple text information. This server listens on port 8888 and address 127.0.0.1 (localhost), responding to requests for the path `/dart`. For any other path, the response is status code 404 (page not found).

​	以下示例 Web 服务器返回简单的文本信息。此服务器侦听端口 8888 和地址 127.0.0.1 (localhost)，响应对路径 `/dart` 的请求。对于任何其他路径，响应是状态代码 404（未找到页面）。

```dart
void main() async {
  final requests = await HttpServer.bind('localhost', 8888);
  await for (final request in requests) {
    processRequest(request);
  }
}

void processRequest(HttpRequest request) {
  print('Got request for ${request.uri.path}');
  final response = request.response;
  if (request.uri.path == '/dart') {
    response
      ..headers.contentType = ContentType(
        'text',
        'plain',
      )
      ..write('Hello from the server');
  } else {
    response.statusCode = HttpStatus.notFound;
  }
  response.close();
}
```

#### HTTP 客户端 HTTP client 

You should avoid directly using `dart:io` to make HTTP requests. The [HttpClient](https://api.dart.dev/stable/dart-io/HttpClient-class.html) class in `dart:io` is platform-dependent and tied to a single implementation. Instead, use a higher-level library like [`package:http`](https://pub.dev/packages/http).

​	您应该避免直接使用 `dart:io` 来发出 HTTP 请求。 `dart:io` 中的 HttpClient 类与平台相关，并且与单个实现相关。相反，请使用更高级别的库，例如 `package:http` 。

The [Fetch data from the internet](https://dart.dev/tutorials/server/fetch-data) tutorial explains how to make HTTP requests using `package:http`.

​	从互联网获取数据教程解释了如何使用 `package:http` 发出 HTTP 请求。

### 更多信息 More information 

This page showed how to use the major features of the [dart:io](https://api.dart.dev/stable/dart-io/dart-io-library.html) library. Besides the APIs discussed in this section, the dart:io library also provides APIs for [processes,](https://api.dart.dev/stable/dart-io/Process-class.html) [sockets,](https://api.dart.dev/stable/dart-io/Socket-class.html) and [web sockets.](https://api.dart.dev/stable/dart-io/WebSocket-class.html) For more information about server-side and command-line app development, see the [server-side Dart overview.](https://dart.dev/server)

​	此页面展示了如何使用 dart:io 库的主要功能。除了本节中讨论的 API 之外，dart:io 库还提供用于进程、套接字和 Web 套接字的 API。有关服务器端和命令行应用程序开发的更多信息，请参阅服务器端 Dart 概览。
