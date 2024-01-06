+++
title = "从互联网获取数据"
date = 2024-01-05T20:29:36+08:00
weight = 30
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文: [https://dart.dev/tutorials/server/fetch-data](https://dart.dev/tutorials/server/fetch-data)

## Fetch data from the internet 从互联网获取数据

#### What you'll learn 您将学到什么

- The basics of what HTTP requests and URIs are and what they are used for.
  HTTP 请求和 URI 的基本知识以及它们的使用用途。
- Making HTTP requests using `package:http`.
  使用 `package:http` 发出 HTTP 请求。
- Decoding JSON strings into Dart objects with `dart:convert`.
  使用 `dart:convert` 将 JSON 字符串解码为 Dart 对象。
- Converting JSON objects into class-based structures.
  将 JSON 对象转换为基于类的结构。

Most applications require some form of communication or data retrieval from the internet. Many apps do so through HTTP requests, which are sent from a client to a server to perform a specific action for a resource identified through a [URI](https://wikipedia.org/wiki/Uniform_Resource_Identifier) (Uniform Resource Identifier).
大多数应用程序都需要某种形式的通信或从互联网检索数据。许多应用通过 HTTP 请求来实现，这些请求从客户端发送到服务器，以对通过 URI（统一资源标识符）标识的资源执行特定操作。

Data communicated over HTTP can technically be in any form, but using [JSON](https://www.json.org/) (JavaScript Object Notation) is a popular choice due to its human-readability and language independent nature. The Dart SDK and ecosystem also have extensive support for JSON with multiple options to best meet your app’s requirements.
通过 HTTP 传输的数据在技术上可以采用任何形式，但使用 JSON（JavaScript 对象表示法）是一种流行的选择，因为它具有可读性和语言独立性。Dart SDK 和生态系统还广泛支持 JSON，并提供多种选项来最佳满足您的应用需求。

In this tutorial, you will learn more about HTTP requests, URIs, and JSON. Then you will learn how to use [`package:http`](https://pub.dev/packages/http) as well as Dart’s JSON support in the [`dart:convert`](https://api.dart.dev/stable/dart-convert/dart-convert-library.html) library to fetch, decode, then use JSON-formatted data retrieved from an HTTP server.
在本教程中，您将详细了解 HTTP 请求、URI 和 JSON。然后，您将学习如何使用 `package:http` 以及 `dart:convert` 库中的 Dart JSON 支持来获取、解码，然后使用从 HTTP 服务器检索的 JSON 格式数据。

## Background concepts 背景概念

The following sections provide some extra background and information around the technologies and concepts used in the tutorial to facilitate fetching data from the server. To skip directly to the tutorial content, see [Retrieve the necessary dependencies](https://dart.dev/tutorials/server/fetch-data#retrieve-the-necessary-dependencies).
以下部分提供了一些额外的背景知识和信息，涉及教程中用于促进从服务器获取数据的技术和概念。要直接跳到教程内容，请参阅检索必要的依赖项。

### JSON

JSON (JavaScript Object Notation) is a data-interchange format that has become ubiquitous across application development and client-server communication. It is lightweight but also easy for humans to read and write due to being text based. With JSON, various data types and simple data structures such as lists and maps can be serialized and represented by strings.
JSON（JavaScript 对象表示法）是一种数据交换格式，已在应用程序开发和客户端-服务器通信中无处不在。它很轻巧，但由于基于文本，因此人类也很容易阅读和编写。使用 JSON，可以对各种数据类型和简单数据结构（如列表和映射）进行序列化并由字符串表示。

Most languages have many implementations and parsers have become extremely fast, so you don’t need to worry about interoperability or performance. For more information about the JSON format, see [Introducing JSON](https://www.json.org/). To learn more about working with JSON in Dart, see the [Using JSON](https://dart.dev/guides/json) guide.
大多数语言都有许多实现，并且解析器已经变得非常快，因此您不必担心互操作性或性能。有关 JSON 格式的更多信息，请参阅介绍 JSON。要详细了解如何在 Dart 中使用 JSON，请参阅使用 JSON 指南。

### HTTP requests HTTP 请求

HTTP (Hypertext Transfer Protocol) is a stateless protocol designed for transmitting documents, originally between web clients and web servers. You interacted with the protocol to load this page, as your browser uses an HTTP `GET` request to retrieve the contents of a page from a web server. Since its introduction, use of the HTTP protocol and its various versions have expanded to applications outside the web as well, essentially wherever communication from a client to a server is needed.
HTTP（超文本传输协议）是一种无状态协议，设计用于传输文档，最初用于网络客户端和网络服务器之间。您与该协议交互以加载此页面，因为您的浏览器使用 HTTP `GET` 请求从网络服务器检索页面的内容。自推出以来，HTTP 协议及其各种版本的用途已扩展到网络之外的应用程序，基本上无论何时需要从客户端与服务器通信时。

HTTP requests sent from the client to communicate with the server are composed of multiple components. HTTP libraries, such as `package:http`, allow you to specify the following kinds of communication:
从客户端发送到服务器以进行通信的 HTTP 请求由多个组件组成。HTTP 库（例如 `package:http` ）允许您指定以下类型的通信：

- An HTTP method defining the desired action, such as `GET` to retrieve data or `POST` to submit new data.
  定义所需操作的 HTTP 方法，例如 `GET` 用于检索数据或 `POST` 用于提交新数据。
- The location of the resource through a URI.
  通过 URI 定位资源。
- The version of HTTP being used.
  正在使用的 HTTP 版本。
- Headers that provide extra information to the server.
  向服务器提供额外信息的标头。
- An optional body, so the request can send data to the server, not just retrieve it.
  一个可选的主体，因此请求可以向服务器发送数据，而不仅仅是检索数据。

To learn more about the HTTP protocol, check out [An overview of HTTP](https://developer.mozilla.org/docs/Web/HTTP/Overview) on the mdn web docs.
要详细了解 HTTP 协议，请查看 mdn 网络文档上的 HTTP 概述。

### URIs and URLs URI 和 URL

To make an HTTP request, you need to provide a [URI](https://wikipedia.org/wiki/Uniform_Resource_Identifier) (Uniform Resource Identifier) to the resource. A URI is a character string that uniquely identifies a resource. A URL (Uniform Resource Locator) is a specific kind of URI that also provides the location of the resource. URLs for resources on the web contain three pieces of information. For this current page, the URL is composed of:
要进行 HTTP 请求，您需要向资源提供 URI（统一资源标识符）。URI 是一个唯一标识资源的字符字符串。URL（统一资源定位符）是一种特殊的 URI，它还提供了资源的位置。Web 上资源的 URL 包含三部分信息。对于此当前页面，URL 由以下部分组成：

- The scheme used for determining the protocol used: `https`
  用于确定所用协议的方案： `https`
- The authority or hostname of the server: `dart.dev`
  服务器的权限或主机名： `dart.dev`
- The path to the resource: `/tutorials/server/fetch-data.html`
  资源的路径： `/tutorials/server/fetch-data.html`

There are other optional parameters as well that aren’t used by the current page:
还有其他一些可选参数，当前页面未使用：

- Parameters to customize extra behavior: `?key1=value1&key2=value2`
  自定义额外行为的参数： `?key1=value1&key2=value2`
- An anchor, that isn’t sent to the server, which points to a specific location in the resource: `#uris`
  锚点，不会发送到服务器，它指向资源中的特定位置： `#uris`

To learn more about URLs, see [What is a URL?](https://developer.mozilla.org/docs/Learn/Common_questions/What_is_a_URL) on the mdn web docs.
要详细了解 URL，请参阅 mdn web 文档中的什么是 URL？

## Retrieve the necessary dependencies 检索必要的依赖项

The `package:http` library provides a cross-platform solution for making composable HTTP requests, with optional fine-grained control.
`package:http` 库提供了一个跨平台解决方案，用于发出可组合的 HTTP 请求，并具有可选的细粒度控制。

*info* **Note:** You should avoid directly using `dart:io` or `dart:html` to make HTTP requests. Those libraries are platform-dependent and tied to a single implementation.
注意：您应避免直接使用 `dart:io` 或 `dart:html` 发出 HTTP 请求。这些库是平台相关的，并且与单个实现相关联。

To add a dependency on `package:http`, run the following [`dart pub add`](https://dart.dev/tools/pub/cmd/pub-add) command from the top of your repo:
要添加对 `package:http` 的依赖项，请从存储库的顶部运行以下 `dart pub add` 命令：

```
$ dart pub add http
```

To use `package:http` in your code, import it and optionally [specify a library prefix](https://dart.dev/language/libraries#specifying-a-library-prefix):
要在代码中使用 `package:http` ，请导入它并选择性地指定一个库前缀：

```dart
import 'package:http/http.dart' as http;
```

To learn more specifics about `package:http`, see its [page on the pub.dev site](https://pub.dev/packages/http) and its [API documentation](https://pub.dev/documentation/http).
要详细了解 `package:http` ，请参阅其在 pub.dev 网站上的页面及其 API 文档。

## Build a URL 构建 URL

As previously mentioned, to make an HTTP request, you first need a URL that identifies the resource being requested or endpoint being accessed.
如前所述，要发出 HTTP 请求，首先需要一个 URL 来标识所请求的资源或所访问的端点。

In Dart, URLs are represented through [`Uri`](https://api.dart.dev/dart-core/Uri-class.html) objects. There are many ways to build an `Uri`, but due to its flexibility, parsing a string with `Uri.parse` to create one is a common solution.
在 Dart 中，URL 通过 `Uri` 对象表示。有很多方法可以构建 `Uri` ，但由于其灵活性，使用 `Uri.parse` 解析字符串以创建一个 `Uri` 是一个常见的解决方案。

The following snippet shows two ways to create a `Uri` object pointing to mock JSON-formatted information about `package:http` hosted on this site:
以下代码段展示了两种创建 `Uri` 对象的方法，该对象指向此网站上托管的有关 `package:http` 的模拟 JSON 格式信息：

```dart
// Parse the entire URI, including the scheme
Uri.parse('https://dart.dev/f/packages/http.json');

// Specifically create a URI with the https scheme
Uri.https('dart.dev', '/f/packages/http.json');
```

To learn about other ways of building and interacting with URIs, see the [`URI` documentation](https://dart.dev/libraries/dart-core#uris).
要了解构建和与 URI 交互的其他方法，请参阅 `URI` 文档。

## Make a network request 发出网络请求

If you just need to quickly fetch a string representation of a requested resource, you can use the top-level [`read`](https://pub.dev/documentation/http/latest/http/read.html) function found in `package:http` that returns a `Future<String>` or throws a [`ClientException`](https://pub.dev/documentation/http/latest/http/ClientException-class.html) if the request wasn’t successful. The following example uses `read` to retrieve the mock JSON-formatted information about `package:http` as a string, then prints it out:
如果您只需要快速获取所请求资源的字符串表示形式，则可以使用 `package:http` 中找到的顶级 `read` 函数，该函数返回 `Future<String>` 或在请求不成功时引发 `ClientException` 。以下示例使用 `read` 将有关 `package:http` 的模拟 JSON 格式信息作为字符串检索，然后将其打印出来：

*info* **Note:** Many functions in `package:http`, including `read`, access the network and perform potentially time-consuming operations, therefore they do so asynchronously and return a [`Future`](https://api.dart.dev/stable/dart-async/Future-class.html). If you haven’t encountered futures yet, you can learn about them—as well as the `async` and `await` keywords—in the [asynchronous programming codelab](https://dart.dev/codelabs/async-await).
注意： `package:http` 中的许多函数（包括 `read` ）会访问网络并执行可能耗时的操作，因此它们会异步执行并返回 `Future` 。如果您尚未遇到 futures，可以在异步编程 codelab 中了解它们以及 `async` 和 `await` 关键字。

```dart
void main() async {
  final httpPackageUrl = Uri.https('dart.dev', '/f/packages/http.json');
  final httpPackageInfo = await http.read(httpPackageUrl);
  print(httpPackageInfo);
}
```

This results in the following JSON-formatted output, which can also be seen in your browser at https://dart.dev/f/packages/http.json.
这将生成以下 JSON 格式的输出，您还可以在 https://dart.dev/f/packages/http.json 中的浏览器中看到它。

```
{
  "name": "http",
  "latestVersion": "1.1.2",
  "description": "A composable, multi-platform, Future-based API for HTTP requests.",
  "publisher": "dart.dev",
  "repository": "https://github.com/dart-lang/http"
}
```

Note the structure of the data (in this case a map), as you will need it when decoding the JSON later on.
请注意数据结构（在本例中为映射），因为您稍后在解码 JSON 时会用到它。

If you need other information from the response, such as the [status code](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status) or the [headers](https://developer.mozilla.org/docs/Web/HTTP/Headers), you can instead use the top-level [`get`](https://pub.dev/documentation/http/latest/http/get.html) function that returns a `Future` with a [`Response`](https://pub.dev/documentation/http/latest/http/Response-class.html).
如果您需要响应中的其他信息，例如状态代码或标头，则可以使用返回带有 `Response` 的 `Future` 的顶级 `get` 函数。

The following snippet uses `get` to get the whole response in order to exit early if the request was not successful, which is indicated with a status code of **200**:
以下代码段使用 `get` 获取整个响应，以便在请求不成功时尽早退出，这由状态代码 200 指示：

```dart
void main() async {
  final httpPackageUrl = Uri.https('dart.dev', '/f/packages/http.json');
  final httpPackageResponse = await http.get(httpPackageUrl);
  if (httpPackageResponse.statusCode != 200) {
    print('Failed to retrieve the http package!');
    return;
  }
  print(httpPackageResponse.body);
}
```

There are many other status codes besides **200** and your app might want to handle them differently. To learn more about what different status codes mean, see [HTTP response status codes](https://developer.mozilla.org/docs/Web/HTTP/Status) on the mdn web docs.
除了 200 之外，还有许多其他状态代码，您的应用可能希望以不同的方式处理它们。要详细了解不同状态代码的含义，请参阅 mdn web 文档上的 HTTP 响应状态代码。

Some server requests require more information, such as authentication or user-agent information; in this case you might need to include [HTTP headers](https://developer.mozilla.org/docs/Web/HTTP/Headers). You can specify headers by passing in a `Map<String, String>` of the key-value pairs as the `headers` optional named parameter:
某些服务器请求需要更多信息，例如身份验证或用户代理信息；在这种情况下，您可能需要包含 HTTP 标头。您可以通过传入一个 `Map<String, String>` 的键值对作为 `headers` 可选命名参数来指定标头：

```dart
await http.get(Uri.https('dart.dev', '/f/packages/http.json'),
    headers: {'User-Agent': '<product name>/<product-version>'});
```

### Make multiple requests 发出多个请求

If you’re making multiple requests to the same server, you can instead keep a persistent connection through a [`Client`](https://pub.dev/documentation/http/latest/http/Client-class.html), which has similar methods to the top-level ones. Just make sure to clean up with the [`close`](https://pub.dev/documentation/http/latest/http/Client/close.html) method when done:
如果您向同一服务器发出多个请求，则可以通过 `Client` 保持持久连接，它具有与顶级连接类似的方法。完成时，请务必使用 `close` 方法进行清理：

```dart
void main() async {
  final httpPackageUrl = Uri.https('dart.dev', '/f/packages/http.json');
  final client = http.Client();
  try {
    final httpPackageInfo = await client.read(httpPackageUrl);
    print(httpPackageInfo);
  } finally {
    client.close();
  }
}
```

To enable the client to retry failed requests, import `package:http/retry.dart` and wrap your created `Client` in a [`RetryClient`](https://pub.dev/documentation/http/latest/retry/RetryClient-class.html):
为了使客户端能够重试失败的请求，请导入 `package:http/retry.dart` 并将您创建的 `Client` 包装在 `RetryClient` 中：

```dart
import 'package:http/http.dart' as http;
import 'package:http/retry.dart';

void main() async {
  final httpPackageUrl = Uri.https('dart.dev', '/f/packages/http.json');
  final client = RetryClient(http.Client());
  try {
    final httpPackageInfo = await client.read(httpPackageUrl);
    print(httpPackageInfo);
  } finally {
    client.close();
  }
}
```

The `RetryClient` has a default behavior for how many times to retry and how long to wait between each request, but its behavior can be modified through parameters to the [`RetryClient()`](https://pub.dev/documentation/http/latest/retry/RetryClient/RetryClient.html) or [`RetryClient.withDelays()`](https://pub.dev/documentation/http/latest/retry/RetryClient/RetryClient.withDelays.html) constructors.
`RetryClient` 具有重试次数和每次请求之间等待时间的默认行为，但可以通过 `RetryClient()` 或 `RetryClient.withDelays()` 构造函数的参数来修改其行为。

`package:http` has much more functionality and customization, so make sure to check out its [page on the pub.dev site](https://pub.dev/packages/http) and its [API documentation](https://pub.dev/documentation/http).
`package:http` 具有更多功能和自定义项，因此请务必查看其在 pub.dev 网站上的页面及其 API 文档。

## Decode the retrieved data 解码检索到的数据

While you now have made a network request and retrieved the returned data as string, accessing specific portions of information from a string can be a challenge.
虽然您现在已经发出了网络请求并将返回的数据作为字符串检索，但从字符串中访问特定部分的信息可能具有挑战性。

Since the data is already in a JSON format, you can use Dart’s built-in [`json.decode`](https://api.dart.dev/stable/dart-convert/JsonCodec/decode.html) function in the `dart:convert` library to convert the raw string into a JSON representation using Dart objects. In this case, the JSON data is represented in a map structure and, in JSON, map keys are always strings, so you can cast the result of `json.decode` to a `Map<String, dynamic>`:
由于数据已经采用 JSON 格式，因此可以使用 Dart 的内置 `json.decode` 函数在 `dart:convert` 库中将原始字符串转换为使用 Dart 对象的 JSON 表示形式。在这种情况下，JSON 数据表示为映射结构，并且在 JSON 中，映射键始终为字符串，因此可以将 `json.decode` 的结果强制转换为 `Map<String, dynamic>` ：

```dart
import 'dart:convert';

import 'package:http/http.dart' as http;

void main() async {
  final httpPackageUrl = Uri.https('dart.dev', '/f/packages/http.json');
  final httpPackageInfo = await http.read(httpPackageUrl);
  final httpPackageJson = json.decode(httpPackageInfo) as Map<String, dynamic>;
  print(httpPackageJson);
}
```

### Create a structured class to store the data 创建结构化类来存储数据

To provide the decoded JSON with more structure, making it easier to work with, create a class that can store the retrieved data using specific types depending on the schema of your data.
为解码的 JSON 提供更多结构，使其更易于使用，创建一个类，该类可以使用特定类型存储检索到的数据，具体取决于数据的架构。

The following snippet shows a class-based representation that can store the package information returned from the mock JSON file you requested. This structure assumes all fields except the `repository` are required and provided every time.
以下代码段显示了一个基于类的表示形式，该表示形式可以存储从您请求的模拟 JSON 文件返回的包信息。此结构假定除 `repository` 之外的所有字段都是必需的，并且每次都会提供。

```dart
class PackageInfo {
  final String name;
  final String latestVersion;
  final String description;
  final String publisher;
  final Uri? repository;

  PackageInfo({
    required this.name,
    required this.latestVersion,
    required this.description,
    required this.publisher,
    this.repository,
  });
}
```

### Encode the data into your class 将数据编码到您的类中

Now that you have a class to store your data in, you need to add a mechanism to convert the decoded JSON into a `PackageInfo` object.
现在您有了一个类来存储数据，您需要添加一个机制将解码的 JSON 转换为 `PackageInfo` 对象。

Convert the decoded JSON by manually writing a `fromJson` method matching the earlier JSON format, casting types as necessary and handling the optional `repository` field:
通过手动编写与早期 JSON 格式匹配的 `fromJson` 方法来转换解码的 JSON，根据需要强制转换类型并处理可选的 `repository` 字段：

```dart
class PackageInfo {
  // ···

  factory PackageInfo.fromJson(Map<String, dynamic> json) {
    final repository = json['repository'] as String?;

    return PackageInfo(
      name: json['name'] as String,
      latestVersion: json['latestVersion'] as String,
      description: json['description'] as String,
      publisher: json['publisher'] as String,
      repository: repository != null ? Uri.tryParse(repository) : null,
    );
  }
}
```

A handwritten method, such as in the previous example, is often sufficient for relatively simple JSON structures, but there are more flexible options as well. To learn more about JSON serialization and deserialization, including automatic generation of the conversion logic, see the [Using JSON](https://dart.dev/guides/json) guide.
对于相对简单的 JSON 结构，手写方法（如上例所示）通常就足够了，但还有更灵活的选择。要详细了解 JSON 序列化和反序列化，包括自动生成转换逻辑，请参阅使用 JSON 指南。

### Convert the response to an object of your structured class 将响应转换为结构化类的对象

Now you have a class to store your data and a way to convert the decoded JSON object into an object of that type. Next, you can write a function that pulls everything together:
现在，您有了一个类来存储数据，以及一种将解码后的 JSON 对象转换为该类型对象的方法。接下来，您可以编写一个将所有内容组合在一起的函数：

1. Create your `URI` based off a passed-in package name.
   根据传入的包名称创建 `URI` 。
2. Use `http.get` to retrieve the data for that package.
   使用 `http.get` 检索该包的数据。
3. If the request didn’t succeed, throw an `Exception` or preferably your own custom `Exception` subclass.
   如果请求未成功，请抛出 `Exception` 或您自己的自定义 `Exception` 子类。
4. If the request succeeded, use `json.decode` to decode the response body into a JSON string.
   如果请求成功，请使用 `json.decode` 将响应主体解码为 JSON 字符串。
5. Converted the decoded JSON string into a `PackageInfo` object using the `PackageInfo.fromJson` factory constructor you created.
   使用您创建的 `PackageInfo.fromJson` 工厂构造函数将解码后的 JSON 字符串转换为 `PackageInfo` 对象。

```dart
Future<PackageInfo> getPackage(String packageName) async {
  final packageUrl = Uri.https('dart.dev', '/f/packages/$packageName.json');
  final packageResponse = await http.get(packageUrl);

  // If the request didn't succeed, throw an exception
  if (packageResponse.statusCode != 200) {
    throw PackageRetrievalException(
      packageName: packageName,
      statusCode: packageResponse.statusCode,
    );
  }

  final packageJson = json.decode(packageResponse.body) as Map<String, dynamic>;

  return PackageInfo.fromJson(packageJson);
}

class PackageRetrievalException implements Exception {
  final String packageName;
  final int? statusCode;

  PackageRetrievalException({required this.packageName, this.statusCode});
}
```

## Utilize the converted data 利用转换后的数据

Now that you’ve retrieved data and converted it to a more easily accessible format, you can use it however you’d like. Some possibilities include outputting information to a CLI, or displaying it in a [web](https://dart.dev/web) or [Flutter](https://flutter.dev/) app.
现在您已经检索了数据并将其转换为更易访问的格式，您可以根据需要使用它。一些可能性包括将信息输出到 CLI，或在 Web 或 Flutter 应用中显示信息。

Here is complete, runnable example that requests, decodes, then displays the mock information about the `http` and `path` packages:
这是一个完整的可运行示例，它请求、解码，然后显示有关 `http` 和 `path` 包的模拟信息：

<iframe src="https://dartpad.dev/embed-dart.html?theme=light&amp;run=dartpad&amp;split=false&amp;ga_id=fetch" data-gtm-vis-first-on-screen13029053_11="5321" data-gtm-vis-total-visible-time13029053_11="100" data-gtm-vis-has-fired13029053_11="1" style="box-sizing: border-box; border: 1px solid rgb(204, 204, 204); margin-bottom: 1rem; min-height: 400px; resize: vertical; width: 896.007px; height: 600px;"></iframe>

![Flutter logo](./Fetchdatafromtheinternet_img/64.png) **Flutter note:** For another example that covers fetching then displaying data in Flutter, see the [Fetching data from the internet](https://docs.flutter.dev/cookbook/networking/fetch-data) Flutter recipe.
![Flutter logo](https://dart.dev/assets/img/shared/flutter/icon/64.png) Flutter 注释：有关另一个涵盖在 Flutter 中获取然后显示数据的示例，请参阅从互联网获取数据的 Flutter 配方。

## What next? 接下来做什么？

Now that you have retrieved, parsed, and used data from the internet, consider learning more about [Concurrency in Dart](https://dart.dev/language/concurrency). If your data is large and complex, you can move retrieval and decoding to another [isolate](https://dart.dev/language/concurrency#isolates) as a background worker to prevent your interface from becoming unresponsive.
既然您已经从互联网检索、解析并使用了数据，请考虑进一步了解 Dart 中的并发性。如果您的数据庞大且复杂，您可以将检索和解码移至另一个隔离作为后台工作进程，以防止您的界面变得无响应。
