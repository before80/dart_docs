+++
title = "循环"
date = 2024-01-05T20:29:36+08:00
weight = 1
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文: [https://dart.dev/language/loops](https://dart.dev/language/loops)

## Loops 循环

This page shows how you can control the flow of your Dart code using loops and supporting statements:

​	此页面展示了如何使用循环和支持语句控制 Dart 代码的流程：

- `for` loops
- `for` 循环
- `while` and `do while` loops
- `while` 和 `do while` 循环
- `break` and `continue`
- `break` 和 `continue`

You can also manipulate control flow in Dart using:

​	您还可以使用以下方式在 Dart 中控制控制流：

- [Branching](https://dart.dev/language/branches), like `if` and `switch`
- 分支，例如 `if` 和 `switch`
- [Exceptions](https://dart.dev/language/error-handling), like `try`, `catch`, and `throw`
- 异常，如 `try` 、 `catch` 和 `throw`

## For 循环

You can iterate with the standard `for` loop. For example:

​	您可以使用标准 `for` 循环进行迭代。例如：

```dart
var message = StringBuffer('Dart is fun');
for (var i = 0; i < 5; i++) {
  message.write('!');
}
```

Closures inside of Dart’s `for` loops capture the *value* of the index. This avoids a common pitfall found in JavaScript. For example, consider:

​	Dart 的 `for` 循环内的闭包捕获索引的值。这避免了 JavaScript 中常见的陷阱。例如，考虑：

```dart
var callbacks = [];
for (var i = 0; i < 2; i++) {
  callbacks.add(() => print(i));
}

for (final c in callbacks) {
  c();
}
```

The output is `0` and then `1`, as expected. In contrast, the example would print `2` and then `2` in JavaScript.

​	输出为 `0` 然后是 `1` ，正如预期的那样。相比之下，该示例将在 JavaScript 中打印 `2` 然后是 `2` 。

Sometimes you might not need to know the current iteration counter when iterating over an [`Iterable`](https://api.dart.dev/stable/dart-core/Iterable-class.html) type, like `List` or `Set`. In that case, use the `for-in` loop for cleaner code:

​	有时，您可能不需要在迭代 `Iterable` 类型（如 `List` 或 `Set` ）时知道当前迭代计数器。在这种情况下，使用 `for-in` 循环以获得更简洁的代码：

```dart
for (final candidate in candidates) {
  candidate.interview();
}
```

To process the values obtained from the iterable, you can also use a [pattern](https://dart.dev/language/patterns) in a `for-in` loop:

​	要处理从可迭代对象获取的值，您还可以在 `for-in` 循环中使用模式：

```dart
for (final Candidate(:name, :yearsExperience) in candidates) {
  print('$name has $yearsExperience of experience.');
}
```

*tips_and_updates* **Tip:** To practice using `for-in`, follow the [Iterable collections codelab](https://dart.dev/codelabs/iterables).

​	提示：要练习使用 `for-in` ，请按照 Iterable 集合 codelab 进行操作。

Iterable classes also have a [forEach()](https://api.dart.dev/stable/dart-core/Iterable/forEach.html) method as another option:

​	Iterable 类还具有 forEach() 方法作为另一个选项：

```dart
var collection = [1, 2, 3];
collection.forEach(print); // 1 2 3
```

## while 和 do-while

A `while` loop evaluates the condition before the loop:

​	`while` 循环在循环之前计算条件：

```dart
while (!isDone()) {
  doSomething();
}
```

A `do`-`while` loop evaluates the condition *after* the loop:

​	A `do` - `while` 循环在循环后评估条件：

```dart
do {
  printLine();
} while (!atEndOfPage());
```

## Break 和 continue

Use `break` to stop looping:

​	使用 `break` 停止循环：

```dart
while (true) {
  if (shutDownRequested()) break;
  processIncomingRequests();
}
```

Use `continue` to skip to the next loop iteration:

​	使用 `continue` 跳至下一个循环迭代：

```dart
for (int i = 0; i < candidates.length; i++) {
  var candidate = candidates[i];
  if (candidate.yearsExperience < 5) {
    continue;
  }
  candidate.interview();
}
```

If you’re using an [`Iterable`](https://api.dart.dev/stable/dart-core/Iterable-class.html) such as a list or set, how you write the previous example might differ:

​	如果您正在使用 `Iterable` （例如列表或集合），则编写前一个示例的方式可能有所不同：

```dart
candidates
    .where((c) => c.yearsExperience >= 5)
    .forEach((c) => c.interview());
```
