+++
title = "dart:convert"
date = 2024-01-05T20:29:36+08:00
weight = 40
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文: [https://dart.dev/libraries/dart-convert](https://dart.dev/libraries/dart-convert)

## dart:convert

The dart:convert library ([API reference](https://api.dart.dev/stable/dart-convert/dart-convert-library.html)) has converters for JSON and UTF-8, as well as support for creating additional converters. [JSON](https://www.json.org/) is a simple text format for representing structured objects and collections. [UTF-8](https://en.wikipedia.org/wiki/UTF-8) is a common variable-width encoding that can represent every character in the Unicode character set.

​	dart:convert 库（API 参考）具有 JSON 和 UTF-8 的转换器，以及创建其他转换器的支持。JSON 是一种用于表示结构化对象和集合的简单文本格式。UTF-8 是一种常见的可变宽度编码，可以表示 Unicode 字符集中的每个字符。

To use this library, import dart:convert.

​	要使用此库，请导入 dart:convert。

```dart
import 'dart:convert';
```

### 解码和编码 JSON - Decoding and encoding JSON 

Decode a JSON-encoded string into a Dart object with `jsonDecode()`:

​	使用 `jsonDecode()` 将 JSON 编码的字符串解码为 Dart 对象：

```dart
// NOTE: Be sure to use double quotes ("),
// not single quotes ('), inside the JSON string.
// This string is JSON, not Dart.
var jsonString = '''
  [
    {"score": 40},
    {"score": 80}
  ]
''';

var scores = jsonDecode(jsonString);
assert(scores is List);

var firstScore = scores[0];
assert(firstScore is Map);
assert(firstScore['score'] == 40);
```

Encode a supported Dart object into a JSON-formatted string with `jsonEncode()`:

​	使用 `jsonEncode()` 将受支持的 Dart 对象编码为 JSON 格式的字符串：

```dart
var scores = [
  {'score': 40},
  {'score': 80},
  {'score': 100, 'overtime': true, 'special_guest': null}
];

var jsonText = jsonEncode(scores);
assert(jsonText ==
    '[{"score":40},{"score":80},'
        '{"score":100,"overtime":true,'
        '"special_guest":null}]');
```

Only objects of type int, double, String, bool, null, List, or Map (with string keys) are directly encodable into JSON. List and Map objects are encoded recursively.

​	只有类型为 int、double、String、bool、null、List 或 Map（带字符串键）的对象才能直接编码为 JSON。List 和 Map 对象会递归编码。

You have two options for encoding objects that aren’t directly encodable. The first is to invoke `jsonEncode()` with a second argument: a function that returns an object that is directly encodable. Your second option is to omit the second argument, in which case the encoder calls the object’s `toJson()` method.

​	对于无法直接编码的对象，您有两个编码选项。第一个是使用第二个参数调用 `jsonEncode()` ：一个返回可直接编码的对象的函数。第二个选项是省略第二个参数，在这种情况下，编码器会调用对象的 `toJson()` 方法。

For more examples and links to JSON-related packages, see [Using JSON](https://dart.dev/guides/json).

​	有关更多示例和指向与 JSON 相关的软件包的链接，请参阅使用 JSON。

### 解码和编码 UTF-8 字符 Decoding and encoding UTF-8 characters 

Use `utf8.decode()` to decode UTF8-encoded bytes to a Dart string:

​	使用 `utf8.decode()` 将 UTF8 编码的字节解码为 Dart 字符串：

```dart
List<int> utf8Bytes = [
  0xc3, 0x8e, 0xc3, 0xb1, 0xc5, 0xa3, 0xc3, 0xa9,
  0x72, 0xc3, 0xb1, 0xc3, 0xa5, 0xc5, 0xa3, 0xc3,
  0xae, 0xc3, 0xb6, 0xc3, 0xb1, 0xc3, 0xa5, 0xc4,
  0xbc, 0xc3, 0xae, 0xc5, 0xbe, 0xc3, 0xa5, 0xc5,
  0xa3, 0xc3, 0xae, 0xe1, 0xbb, 0x9d, 0xc3, 0xb1
];

var funnyWord = utf8.decode(utf8Bytes);

assert(funnyWord == 'Îñţérñåţîöñåļîžåţîờñ');
```

To convert a stream of UTF-8 characters into a Dart string, specify `utf8.decoder` to the Stream `transform()` method:

​	要将 UTF-8 字符流转换为 Dart 字符串，请将 `utf8.decoder` 指定给 Stream `transform()` 方法：

```dart
var lines = utf8.decoder.bind(inputStream).transform(const LineSplitter());
try {
  await for (final line in lines) {
    print('Got ${line.length} characters from stream');
  }
  print('file is now closed');
} catch (e) {
  print(e);
}
```

Use `utf8.encode()` to encode a Dart string as a list of UTF8-encoded bytes:

​	使用 `utf8.encode()` 将 Dart 字符串编码为 UTF8 编码字节的列表：

```dart
Uint8List encoded = utf8.encode('Îñţérñåţîöñåļîžåţîờñ');

assert(encoded.length == utf8Bytes.length);
for (int i = 0; i < encoded.length; i++) {
  assert(encoded[i] == utf8Bytes[i]);
}
```

### 其他功能 Other functionality 

The dart:convert library also has converters for ASCII and ISO-8859-1 (Latin1). For details, see the [API reference for the dart:convert library.](https://api.dart.dev/stable/dart-convert/dart-convert-library.html)

​	dart:convert 库还具有 ASCII 和 ISO-8859-1 (Latin1) 的转换器。有关详细信息，请参阅 dart:convert 库的 API 参考。
