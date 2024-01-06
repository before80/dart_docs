+++
title = "异步编程"
date = 2024-01-05T20:29:36+08:00
weight = 30
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文: [https://dart.dev/codelabs/async-await](https://dart.dev/codelabs/async-await)

## Asynchronous programming: futures, async, await 异步编程：futures、async、await

This codelab teaches you how to write asynchronous code using futures and the `async` and `await` keywords. Using embedded DartPad editors, you can test your knowledge by running example code and completing exercises.

​	本代码实验室教您如何使用 futures 和 `async` 和 `await` 关键字编写异步代码。使用嵌入式 DartPad 编辑器，您可以通过运行示例代码和完成练习来测试您的知识。

To get the most out of this codelab, you should have the following:

​	要充分利用此代码实验室，您应具备以下条件：

- Knowledge of [basic Dart syntax](https://dart.dev/language).
- 了解基本的 Dart 语法。
- Some experience writing asynchronous code in another language.
- 具备使用其他语言编写异步代码的一些经验。

This codelab covers the following material:

​	此代码实验室涵盖以下内容：

- How and when to use the `async` and `await` keywords.
- 了解如何以及何时使用 `async` 和 `await` 关键字。
- How using `async` and `await` affects execution order.
- 了解使用 `async` 和 `await` 如何影响执行顺序。
- How to handle errors from an asynchronous call using `try-catch` expressions in `async` functions.
- 了解如何使用 `async` 函数中的 `try-catch` 表达式处理异步调用的错误。

Estimated time to complete this codelab: 40-60 minutes.

​	估计完成此代码实验室所需时间：40-60 分钟。

*info* **Note:** This page uses embedded DartPads to display examples and exercises. If you see empty boxes instead of DartPads, go to the [DartPad troubleshooting page](https://dart.dev/tools/dartpad/troubleshoot).

​	注意：此页面使用嵌入式 DartPad 来显示示例和练习。如果您看到的是空框而不是 DartPad，请转到 DartPad 故障排除页面。

## 异步代码为何重要 Why asynchronous code matters 

Asynchronous operations let your program complete work while waiting for another operation to finish. Here are some common asynchronous operations:

​	异步操作让您的程序在等待另一个操作完成时完成工作。以下是一些常见的异步操作：

- Fetching data over a network.
- 通过网络获取数据。
- Writing to a database.
- 写入数据库。
- Reading data from a file.
- 从文件中读取数据。

Such asynchronous computations usually provide their result as a `Future` or, if the result has multiple parts, as a `Stream`. These computations introduce asynchrony into a program. To accommodate that initial asynchrony, other plain Dart functions also need to become asynchronous.

​	此类异步计算通常会将结果作为 `Future` 提供，或者如果结果包含多个部分，则作为 `Stream` 提供。这些计算会将异步引入程序。为了适应这种初始异步，其他纯 Dart 函数也需要变为异步。

To interact with these asynchronous results, you can use the `async` and `await` keywords. Most asynchronous functions are just async Dart functions that depend, possibly deep down, on an inherently asynchronous computation.

​	要与这些异步结果进行交互，可以使用 `async` 和 `await` 关键字。大多数异步函数只是异步 Dart 函数，它们可能在底层依赖于本质上异步的计算。

### 示例：错误地使用异步函数 Example: Incorrectly using an asynchronous function 

The following example shows the wrong way to use an asynchronous function (`fetchUserOrder()`). Later you’ll fix the example using `async` and `await`. Before running this example, try to spot the issue – what do you think the output will be?

​	以下示例演示了错误使用异步函数 ( `fetchUserOrder()` ) 的方法。稍后，您将使用 `async` 和 `await` 修复此示例。在运行此示例之前，请尝试发现问题 - 您认为输出会是什么？

<iframe src="https://dartpad.dev/embed-dart.html?theme=light&amp;run=dartpad&amp;split=false&amp;ga_id=incorrect_usage" style="box-sizing: border-box; border: 1px solid rgb(204, 204, 204); margin-bottom: 1rem; min-height: 400px; resize: vertical; width: 896px; height: 380px;"></iframe>

Here’s why the example fails to print the value that `fetchUserOrder()` eventually produces:

​	示例无法打印 `fetchUserOrder()` 最终生成的值的原因如下：

- `fetchUserOrder()` is an asynchronous function that, after a delay, provides a string that describes the user’s order: a “Large Latte”.
- `fetchUserOrder()` 是一个异步函数，它在延迟后提供描述用户订单的字符串：“Large Latte”。
- To get the user’s order, `createOrderMessage()` should call `fetchUserOrder()` and wait for it to finish. Because `createOrderMessage()` does *not* wait for `fetchUserOrder()` to finish, `createOrderMessage()` fails to get the string value that `fetchUserOrder()` eventually provides.
- 要获取用户的订单， `createOrderMessage()` 应调用 `fetchUserOrder()` 并等待其完成。由于 `createOrderMessage()` 不等待 `fetchUserOrder()` 完成， `createOrderMessage()` 无法获取 `fetchUserOrder()` 最终提供的字符串值。
- Instead, `createOrderMessage()` gets a representation of pending work to be done: an uncompleted future. You’ll learn more about futures in the next section.
- 相反， `createOrderMessage()` 获取待完成工作的表示形式：未完成的 future。您将在下一部分中了解有关 future 的更多信息。
- Because `createOrderMessage()` fails to get the value describing the user’s order, the example fails to print “Large Latte” to the console, and instead prints “Your order is: Instance of ‘_Future<String>’”.
- 由于 `createOrderMessage()` 无法获取描述用户订单的值，因此该示例无法将“Large Latte”打印到控制台，而是打印“您的订单是：' _Future ' 的实例”。

In the next sections you’ll learn about futures and about working with futures (using `async` and `await`) so that you’ll be able to write the code necessary to make `fetchUserOrder()` print the desired value (“Large Latte”) to the console.

​	在下一部分中，您将了解 future 以及如何使用 future（使用 `async` 和 `await` ），以便能够编写必要的代码，使 `fetchUserOrder()` 将所需值（“Large Latte”）打印到控制台。

**Key terms:
关键术语：**

- **synchronous operation**: A synchronous operation blocks other operations from executing until it completes.
- 同步操作：同步操作会阻止其他操作执行，直到其完成。
- **synchronous function**: A synchronous function only performs synchronous operations.
- 同步函数：同步函数仅执行同步操作。
- **asynchronous operation**: Once initiated, an asynchronous operation allows other operations to execute before it completes.
- 异步操作：一旦启动，异步操作便允许其他操作在完成之前执行。
- **asynchronous function**: An asynchronous function performs at least one asynchronous operation and can also perform *synchronous* operations.
- 异步函数：异步函数至少执行一个异步操作，也可以执行同步操作。

## What is a future? 

A future (lower case “f”) is an instance of the [Future](https://api.dart.dev/stable/dart-async/Future-class.html) (capitalized “F”) class. A future represents the result of an asynchronous operation, and can have two states: uncompleted or completed.

​	未来（小写“f”）是 Future（大写“F”）类的实例。未来表示异步操作的结果，并且可以有两种状态：未完成或已完成。

*info* **Note:** *Uncompleted* is a Dart term referring to the state of a future before it has produced a value.

​	注意：未完成是 Dart 术语，指未来在产生值之前所处的状态。

### 未完成 Uncompleted 

When you call an asynchronous function, it returns an uncompleted future. That future is waiting for the function’s asynchronous operation to finish or to throw an error.

​	当您调用异步函数时，它会返回一个未完成的未来。该未来正在等待函数的异步操作完成或引发错误。

### 已完成 Completed 

If the asynchronous operation succeeds, the future completes with a value. Otherwise, it completes with an error.

​	如果异步操作成功，则未来将以一个值完成。否则，它将以错误完成。

#### 以值完成 Completing with a value 

A future of type `Future<T>` completes with a value of type `T`. For example, a future with type `Future<String>` produces a string value. If a future doesn’t produce a usable value, then the future’s type is `Future<void>`.

​	类型为 `Future<T>` 的未来将以类型为 `T` 的值完成。例如，类型为 `Future<String>` 的未来会产生一个字符串值。如果未来不产生可用值，则未来的类型为 `Future<void>` 。

#### 以错误完成 Completing with an error 

If the asynchronous operation performed by the function fails for any reason, the future completes with an error.

​	如果函数执行的异步操作因任何原因而失败，则未来将以错误完成。

### 示例：介绍 Future Example: Introducing futures 

In the following example, `fetchUserOrder()` returns a future that completes after printing to the console. Because it doesn’t return a usable value, `fetchUserOrder()` has the type `Future<void>`. Before you run the example, try to predict which will print first: “Large Latte” or “Fetching user order…”.

​	在以下示例中， `fetchUserOrder()` 返回一个在打印到控制台后完成的 Future。由于它不返回可用值，因此 `fetchUserOrder()` 的类型为 `Future<void>` 。在运行示例之前，请尝试预测先打印哪一项：“Large Latte”还是“Fetching user order…”。

<iframe src="https://dartpad.dev/embed-dart.html?theme=light&amp;run=dartpad&amp;split=false&amp;ga_id=introducting_futures" style="box-sizing: border-box; border: 1px solid rgb(204, 204, 204); margin-bottom: 1rem; min-height: 400px; resize: vertical; width: 896px; height: 300px;"></iframe>

In the preceding example, even though `fetchUserOrder()` executes before the `print()` call on line 8, the console shows the output from line 8(“Fetching user order…”) before the output from `fetchUserOrder()` (“Large Latte”). This is because `fetchUserOrder()` delays before it prints “Large Latte”.

​	在前面的示例中，即使 `fetchUserOrder()` 在第 8 行执行 `print()` 调用之前，控制台也会先显示第 8 行的输出（“Fetching user order…”），然后再显示 `fetchUserOrder()` 的输出（“Large Latte”）。这是因为 `fetchUserOrder()` 在打印“Large Latte”之前会延迟一段时间。

### 示例：使用错误完成 Example: Completing with an error 

Run the following example to see how a future completes with an error. A bit later you’ll learn how to handle the error.

​	运行以下示例以了解 Future 如何使用错误完成。稍后您将学习如何处理错误。

<iframe src="https://dartpad.dev/embed-dart.html?theme=light&amp;run=dartpad&amp;split=false&amp;ga_id=completing_with_error" style="box-sizing: border-box; border: 1px solid rgb(204, 204, 204); margin-bottom: 1rem; min-height: 400px; resize: vertical; width: 896px; height: 300px;"></iframe>

In this example, `fetchUserOrder()` completes with an error indicating that the user ID is invalid.

​	在此示例中， `fetchUserOrder()` 使用错误完成，表明用户 ID 无效。

You’ve learned about futures and how they complete, but how do you use the results of asynchronous functions? In the next section you’ll learn how to get results with the `async` and `await` keywords.

​	您已经了解了 Future 以及它们如何完成，但如何使用异步函数的结果？在下一部分中，您将学习如何使用 `async` 和 `await` 关键字获取结果。

**Quick review:
快速回顾：**

- A [Future](https://api.dart.dev/stable/dart-async/Future-class.html) instance produces a value of type `T`.
- Future 实例会生成类型为 `T` 的值。
- If a future doesn’t produce a usable value, then the future’s type is `Future<void>`.
- 如果 Future 不生成可用值，则 Future 的类型为 `Future<void>` 。
- A future can be in one of two states: uncompleted or completed.
- 未来可以处于两种状态之一：未完成或已完成。
- When you call a function that returns a future, the function queues up work to be done and returns an uncompleted future.
- 当您调用返回未来的函数时，该函数会将要完成的工作排队，并返回一个未完成的未来。
- When a future’s operation finishes, the future completes with a value or with an error.
- 当未来的操作完成时，未来将以值或错误完成。

**Key terms:
关键术语：**

- **Future**: the Dart [Future](https://api.dart.dev/stable/dart-async/Future-class.html) class.
- Future：Dart Future 类。
- **future**: an instance of the Dart `Future` class.
- future：Dart `Future` 类的实例。

## 使用 future：async 和 await - Working with futures: async and await 

The `async` and `await` keywords provide a declarative way to define asynchronous functions and use their results. Remember these two basic guidelines when using `async` and `await`:

​	`async` 和 `await` 关键字提供了一种声明方式来定义异步函数并使用其结果。使用 `async` 和 `await` 时记住这两个基本准则：

- To define an async function, add `async` before the function body:
- 要定义异步函数，请在函数体前添加 `async` ：
- The `await` keyword works only in `async` functions.
- `await` 关键字仅在 `async` 函数中有效。

Here’s an example that converts `main()` from a synchronous to asynchronous function.

​	这是一个将 `main()` 从同步函数转换为异步函数的示例。

First, add the `async` keyword before the function body:

​	首先，在函数体前添加 `async` 关键字：

```dart
void main() async { ··· }
```

If the function has a declared return type, then update the type to be `Future<T>`, where `T` is the type of the value that the function returns. If the function doesn’t explicitly return a value, then the return type is `Future<void>`:

​	如果函数具有已声明的返回类型，则将类型更新为 `Future<T>` ，其中 `T` 是函数返回的值的类型。如果函数未显式返回一个值，则返回类型为 `Future<void>` ：

```dart
Future<void> main() async { ··· }
```

Now that you have an `async` function, you can use the `await` keyword to wait for a future to complete:

​	现在您拥有一个 `async` 函数，可以使用 `await` 关键字来等待 future 完成：

```dart
print(await createOrderMessage());
```

As the following two examples show, the `async` and `await` keywords result in asynchronous code that looks a lot like synchronous code. The only differences are highlighted in the asynchronous example, which—if your window is wide enough—is to the right of the synchronous example.

​	如下面的两个示例所示， `async` 和 `await` 关键字导致异步代码看起来非常像同步代码。唯一的区别突出显示在异步示例中，如果您的窗口足够宽，则该示例位于同步示例的右侧。

#### 示例：同步函数 Example: synchronous functions 

```dart
String createOrderMessage() {
  var order = fetchUserOrder();
  return 'Your order is: $order';
}

Future<String> fetchUserOrder() =>
    // Imagine that this function is
    // more complex and slow.
    Future.delayed(
      const Duration(seconds: 2),
      () => 'Large Latte',
    );

void main() {
  print('Fetching user order...');
  print(createOrderMessage());
}
Fetching user order...
Your order is: Instance of '_Future<String>'
```

#### 示例：异步函数 Example: asynchronous functions 

```dart
Future<String> createOrderMessage() async {
  var order = await fetchUserOrder();
  return 'Your order is: $order';
}

Future<String> fetchUserOrder() =>
    // Imagine that this function is
    // more complex and slow.
    Future.delayed(
      const Duration(seconds: 2),
      () => 'Large Latte',
    );

Future<void> main() async {
  print('Fetching user order...');
  print(await createOrderMessage());
}
Fetching user order...
Your order is: Large Latte
```

The asynchronous example is different in three ways:

​	异步示例有三个不同之处：

- The return type for `createOrderMessage()` changes from `String` to `Future<String>`.
- `createOrderMessage()` 的返回类型从 `String` 更改为 `Future<String>` 。
- The **`async`** keyword appears before the function bodies for `createOrderMessage()` and `main()`.
- `async` 关键字出现在 `createOrderMessage()` 和 `main()` 的函数体之前。
- The **`await`** keyword appears before calling the asynchronous functions `fetchUserOrder()` and `createOrderMessage()`.
- `await` 关键字出现在调用异步函数 `fetchUserOrder()` 和 `createOrderMessage()` 之前。

**Key terms:
关键术语：**

- **async**: You can use the `async` keyword before a function’s body to mark it as asynchronous.
- async：您可以在函数主体前使用 `async` 关键字将其标记为异步。
- **async function**: An `async` function is a function labeled with the `async` keyword.
- async 函数： `async` 函数是使用 `async` 关键字标记的函数。
- **await**: You can use the `await` keyword to get the completed result of an asynchronous expression. The `await` keyword only works within an `async` function.
- await：您可以使用 `await` 关键字获取异步表达式的完成结果。 `await` 关键字仅在 `async` 函数中有效。

### async 和 await 的执行流程 Execution flow with async and await 

An `async` function runs synchronously until the first `await` keyword. This means that within an `async` function body, all synchronous code before the first `await` keyword executes immediately.

​	`async` 函数在遇到第一个 `await` 关键字之前同步运行。这意味着在 `async` 函数体中，第一个 `await` 关键字之前的所有同步代码都会立即执行。

### Example: Execution within async functions 示例：async 函数中的执行

Run the following example to see how execution proceeds within an `async` function body. What do you think the output will be?

​	运行以下示例以了解如何在 `async` 函数体中执行。您认为输出会是什么？

<iframe src="https://dartpad.dev/embed-dart.html?theme=light&amp;run=dartpad&amp;split=false&amp;ga_id=execution_within_async_function" style="box-sizing: border-box; border: 1px solid rgb(204, 204, 204); margin-bottom: 1rem; min-height: 400px; resize: vertical; width: 896px; height: 530px;"></iframe>

After running the code in the preceding example, try reversing lines 2 and 3:

​	在运行前一个示例中的代码后，尝试颠倒第 2 行和第 3 行：

```dart
var order = await fetchUserOrder();
print('Awaiting user order...');
```

Notice that timing of the output shifts, now that `print('Awaiting user order')` appears after the first `await` keyword in `printOrderMessage()`.

​	请注意，现在 `print('Awaiting user order')` 出现在 `printOrderMessage()` 中的第一个 `await` 关键字之后，输出的时序发生了变化。

### 练习：练习使用 async 和 await - Exercise: Practice using async and await 

The following exercise is a failing unit test that contains partially completed code snippets. Your task is to complete the exercise by writing code to make the tests pass. You don’t need to implement `main()`.

​	以下练习是一个包含部分完成的代码片段的失败单元测试。您的任务是通过编写代码使测试通过来完成练习。您不需要实现 `main()` 。

To simulate asynchronous operations, call the following functions, which are provided for you:

​	要模拟异步操作，请调用为您提供的以下函数：

| Function 函数      | Type signature 类型签名          | Description 说明                                             |
| ------------------ | -------------------------------- | ------------------------------------------------------------ |
| fetchRole()        | `Future<String> fetchRole()`     | Gets a short description of the user’s role. 获取用户角色的简短说明。 |
| fetchLoginAmount() | `Future<int> fetchLoginAmount()` | Gets the number of times a user has logged in. 获取用户登录的次数。 |

#### Part 1: `reportUserRole()` 

Add code to the `reportUserRole()` function so that it does the following:

​	向 `reportUserRole()` 函数添加代码，使其执行以下操作：

- Returns a future that completes with the following string: `"User role: <user role>"`

- 返回一个 future，其中包含以下字符串： `"User role: <user role>"`
  - Note: You must use the actual value returned by `fetchRole()`; copying and pasting the example return value won’t make the test pass.
  - 注意：您必须使用 `fetchRole()` 返回的实际值；复制并粘贴示例返回值不会使测试通过。
  - Example return value: `"User role: tester"`
  - 示例返回值： `"User role: tester"`
  
- Gets the user role by calling the provided function `fetchRole()`.
  
- 通过调用提供的函数 `fetchRole()` 获取用户角色。

#### Part 2: `reportLogins()` 

Implement an `async` function `reportLogins()` so that it does the following:

​	实现 `async` 函数 `reportLogins()` ，使其执行以下操作：

- Returns the string `"Total number of logins: <# of logins>"`.

  

返回字符串 `"Total number of logins: <# of logins>"` 。

  - Note: You must use the actual value returned by `fetchLoginAmount()`; copying and pasting the example return value won’t make the test pass.
  - 注意：您必须使用 `fetchLoginAmount()` 返回的实际值；复制并粘贴示例返回值不会使测试通过。
- Example return value from `reportLogins()`: `"Total number of logins: 57"`
  - 从 `reportLogins()` 返回的示例值： `"Total number of logins: 57"`

- Gets the number of logins by calling the provided function `fetchLoginAmount()`.
  
- 通过调用提供的函数 `fetchLoginAmount()` 获取登录次数。

<iframe src="https://dartpad.dev/embed-dart.html?theme=dark&amp;run=false&amp;split=false&amp;ga_id=practice_using" style="box-sizing: border-box; border: 1px solid rgb(204, 204, 204); margin-bottom: 1rem; min-height: 400px; resize: vertical; width: 896px; height: 380px;"></iframe>

*info* **Note:** If your code passes the tests, you can ignore [info-level messages.](https://dart.dev/tools/analysis#customizing-analysis-rules)

​	注意：如果您的代码通过了测试，则可以忽略信息级消息。

## 处理错误 Handling errors 

To handle errors in an `async` function, use try-catch:

​	要在 `async` 函数中处理错误，请使用 try-catch：

```dart
try {
  print('Awaiting user order...');
  var order = await fetchUserOrder();
} catch (err) {
  print('Caught error: $err');
}
```

Within an `async` function, you can write [try-catch clauses](https://dart.dev/language/error-handling#catch) the same way you would in synchronous code.

​	在 `async` 函数中，您可以像在同步代码中一样编写 try-catch 子句。

### 示例：async 和 await 与 try-catch - Example: async and await with try-catch 

Run the following example to see how to handle an error from an asynchronous function. What do you think the output will be?

​	运行以下示例以了解如何处理异步函数中的错误。您认为输出会是什么？

<iframe src="https://dartpad.dev/embed-dart.html?theme=light&amp;run=dartpad&amp;split=false&amp;ga_id=try_catch" style="box-sizing: border-box; border: 1px solid rgb(204, 204, 204); margin-bottom: 1rem; min-height: 400px; resize: vertical; width: 896px; height: 530px;"></iframe>

### 练习：练习处理错误 Exercise: Practice handling errors 

The following exercise provides practice handling errors with asynchronous code, using the approach described in the previous section. To simulate asynchronous operations, your code will call the following function, which is provided for you:

​	以下练习提供了使用异步代码处理错误的练习，方法如上一节所述。为了模拟异步操作，您的代码将调用以下函数，该函数已为您提供：

| Function 函数      | Type signature 类型签名             | Description 说明                                             |
| ------------------ | ----------------------------------- | ------------------------------------------------------------ |
| fetchNewUsername() | `Future<String> fetchNewUsername()` | Returns the new username that you can use to replace an old one. 返回您可以用来替换旧名称的新用户名。 |

Use `async` and `await` to implement an asynchronous `changeUsername()` function that does the following:

​	使用 `async` 和 `await` 来实现一个异步 `changeUsername()` 函数，该函数执行以下操作：

- Calls the provided asynchronous function `fetchNewUsername()` and returns its result.

  

调用提供的异步函数 `fetchNewUsername()` 并返回其结果。

  - Example return value from `changeUsername()`: `"jane_smith_92"`
  - 来自 `changeUsername()` 的示例返回值： `"jane_smith_92"`

- Catches any error that occurs and returns the string value of the error.

- 
  捕获发生的任何错误并返回错误的字符串值。
  
  - You can use the [toString()](https://api.dart.dev/stable/dart-core/ArgumentError/toString.html) method to stringify both [Exceptions](https://api.dart.dev/stable/dart-core/Exception-class.html) and [Errors.](https://api.dart.dev/stable/dart-core/Error-class.html)
- 您可以使用 toString() 方法将异常和错误都字符串化。

<iframe src="https://dartpad.dev/embed-dart.html?theme=dark&amp;run=false&amp;split=false&amp;ga_id=practice_errors" style="box-sizing: border-box; border: 1px solid rgb(204, 204, 204); margin-bottom: 1rem; min-height: 400px; resize: vertical; width: 896px; height: 380px;"></iframe>

## 练习：将所有内容放在一起 Exercise: Putting it all together 

It’s time to practice what you’ve learned in one final exercise. To simulate asynchronous operations, this exercise provides the asynchronous functions `fetchUsername()` and `logoutUser()`:

​	现在是时候在一个最终练习中练习您学到的内容了。 为了模拟异步操作，此练习提供了异步函数 `fetchUsername()` 和 `logoutUser()` ：

| Function 函数   | Type signature 类型签名          | Description 说明                                             |
| --------------- | -------------------------------- | ------------------------------------------------------------ |
| fetchUsername() | `Future<String> fetchUsername()` | Returns the name associated with the current user. 返回与当前用户关联的名称。 |
| logoutUser()    | `Future<String> logoutUser()`    | Performs logout of current user and returns the username that was logged out. 执行当前用户的注销并返回已注销的用户名。 |

Write the following:

​	编写以下内容：

#### Part 1: `addHello()` 第 1 部分： `addHello()`

- Write a function `addHello()` that takes a single `String` argument.
- 编写一个函数 `addHello()` ，它接受一个 `String` 参数。
- `addHello()` returns its `String` argument preceded by `'Hello '`.
- `addHello()` 返回其 `String` 参数，前面加上 `'Hello '` 。
  Example: `addHello('Jon')` returns `'Hello Jon'`.
- 示例： `addHello('Jon')` 返回 `'Hello Jon'` 。

#### Part 2: `greetUser()` 第 2 部分： `greetUser()`

- Write a function `greetUser()` that takes no arguments.
- 编写一个不带参数的函数 `greetUser()` 。
- To get the username, `greetUser()` calls the provided asynchronous function `fetchUsername()`.
- 要获取用户名， `greetUser()` 调用提供的异步函数 `fetchUsername()` 。
- `greetUser()` creates a greeting for the user by calling `addHello()`, passing it the username, and returning the result.
  `greetUser()` 通过调用 `addHello()` 为用户创建问候语，将用户名传递给它，并返回结果。
- Example: If `fetchUsername()` returns `'Jenny'`, then `greetUser()` returns `'Hello Jenny'`.
  示例：如果 `fetchUsername()` 返回 `'Jenny'` ，则 `greetUser()` 返回 `'Hello Jenny'` 。

#### Part 3: `sayGoodbye()` 第 3 部分： `sayGoodbye()`

- Write a function `sayGoodbye()` that does the following:

- 
  编写一个执行以下操作的函数 `sayGoodbye()` ：

  - Takes no arguments.
  - 不带参数。
  - Catches any errors.
  - 捕获任何错误。
  - Calls the provided asynchronous function `logoutUser()`.
  - 调用提供的异步函数 `logoutUser()` 。

- If `logoutUser()` fails, `sayGoodbye()` returns any string you like.
  
- 如果 `logoutUser()` 失败， `sayGoodbye()` 将返回您喜欢的任何字符串。
  
- If `logoutUser()` succeeds, `sayGoodbye()` returns the string `'<result> Thanks, see you next time'`, where `<result>` is the string value returned by calling `logoutUser()`.
  
- 如果 `logoutUser()` 成功， `sayGoodbye()` 返回字符串 `'<result> Thanks, see you next time'` ，其中 `<result>` 是通过调用 `logoutUser()` 返回的字符串值。

<iframe src="https://dartpad.dev/embed-dart.html?theme=dark&amp;run=false&amp;split=false&amp;ga_id=putting_it_all_together" style="box-sizing: border-box; border: 1px solid rgb(204, 204, 204); margin-bottom: 1rem; min-height: 400px; resize: vertical; width: 896px; height: 380px;"></iframe>

## What’s next? 

Congratulations, you’ve finished the codelab! If you’d like to learn more, here are some suggestions for where to go next:

​	恭喜，您已完成代码实验室！如果您想了解更多信息，这里有一些关于下一步去向的建议：

- Play with [DartPad.](https://dartpad.dev/)
  
- 使用 DartPad 玩耍。
  
- Try another [codelab](https://dart.dev/codelabs).
  
- 尝试另一个代码实验室。
  
- Learn more about futures and asynchrony:

- 
  了解有关期货和异步的更多信息：
  
  - [Streams tutorial](https://dart.dev/tutorials/language/streams): Learn how to work with a sequence of asynchronous events.
  - 流教程：了解如何处理一系列异步事件。
  - [Concurrency in Dart](https://dart.dev/language/concurrency) Understand and learn how to implement concurrency in Dart.
  - Dart 中的并发性了解并学习如何在 Dart 中实现并发性。
  - [Dart videos from Google:](https://www.youtube.com/playlist?list=PLjxrf2q8roU0Net_g1NT5_vOO3s_FR02J) Watch one or more of the videos about asynchronous coding. Or, if you prefer, read the articles that are based on these videos. (Start with [isolates and event loops.](https://medium.com/dartlang/dart-asynchronous-programming-isolates-and-event-loops-bffc3e296a6a))
  - Google 的 Dart 视频：观看有关异步编码的一个或多个视频。或者，如果您愿意，可以阅读基于这些视频的文章。（从隔离和事件循环开始。）
  
- [Get the Dart SDK.](https://dart.dev/get-dart)

- 获取 Dart SDK。
