+++
title = "Callable objects"
date = 2024-01-05T20:29:36+08:00
weight = 70
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文: [https://dart.dev/language/callable-objects](https://dart.dev/language/callable-objects)

# Callable objects 可调用对象

To allow an instance of your Dart class to be called like a function, implement the `call()` method.
要允许您的 Dart 类的实例像函数一样被调用，请实现 `call()` 方法。

The `call()` method allows an instance of any class that defines it to emulate a function. This method supports the same functionality as normal [functions](https://dart.dev/language/functions) such as parameters and return types.
`call()` 方法允许定义它的任何类的实例模拟函数。此方法支持与普通函数相同的功能，例如参数和返回类型。

In the following example, the `WannabeFunction` class defines a `call()` function that takes three strings and concatenates them, separating each with a space, and appending an exclamation. Click **Run** to execute the code.
在以下示例中， `WannabeFunction` 类定义了一个 `call()` 函数，该函数采用三个字符串并将它们连接起来，用空格分隔，并追加一个感叹号。单击运行以执行代码。

```dart
class WannabeFunction {
  String call(String a, String b, String c) => '$a $b $c!';
}

var wf = WannabeFunction();
var out = wf('Hi', 'there,', 'gang');

void main() => print(out);
```



<iframe src="https://dartpad.dev/embed-dart.html?theme=light&amp;run=dartpad&amp;split=false&amp;ga_id=callable_objects" data-gtm-vis-recent-on-screen13029053_11="248" data-gtm-vis-first-on-screen13029053_11="248" data-gtm-vis-total-visible-time13029053_11="100" data-gtm-vis-has-fired13029053_11="1" style="box-sizing: border-box; border: 1px solid rgb(204, 204, 204); margin-bottom: 1rem; min-height: 400px; resize: vertical; width: 896.007px; height: 350px;"></iframe>
