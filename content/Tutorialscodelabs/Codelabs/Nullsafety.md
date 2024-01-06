+++
title = "空安全"
date = 2024-01-05T20:29:36+08:00
weight = 40
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文: [https://dart.dev/codelabs/null-safety](https://dart.dev/codelabs/null-safety)

## Null safety codelab 空安全代码实验室

This codelab teaches you about Dart’s [null-safe type system](https://dart.dev/null-safety). Dart introduced null safety as an optional setting in Dart 2.12. Dart 3 requires null safety. With null safety, values can’t be `null` unless you say they can be.

​	此代码实验室将向您介绍 Dart 的 null 安全类型系统。Dart 在 Dart 2.12 中将 null 安全性作为可选设置引入。Dart 3 需要 null 安全性。使用 null 安全性，除非您说它们可以为 `null` ，否则值不能为 `null` 。

This codelab covers the following material:

​	此代码实验室涵盖以下内容：

- Nullable and non-nullable types.
- 可空和不可空类型。
- When to add `?` or `!` to indicate nullability or non-nullability.
- 何时添加 `?` 或 `!` 来指示可空性或不可空性。
- Flow analysis and type promotion.
- 流分析和类型提升。
- How and when to use null-aware operators.
- 如何以及何时使用 null 感知运算符。
- How the `late` keyword affects variables and initialization.
- 关键字 `late` 如何影响变量和初始化。

Using embedded DartPad editors, you can test your knowledge by completing and running exercises. To get the most out of this codelab, you should have some knowledge of [basic Dart syntax](https://dart.dev/language).

​	使用嵌入式 DartPad 编辑器，您可以通过完成和运行练习来测试您的知识。为了充分利用此代码实验室，您应该具备一些基本的 Dart 语法知识。

*info* **Note:** This page uses embedded DartPads to display exercises. If you see empty boxes instead of DartPads, go to the [DartPad troubleshooting page](https://dart.dev/tools/dartpad/troubleshoot).

​	注意：此页面使用嵌入式 DartPad 来显示练习。如果您看到的是空框而不是 DartPad，请转到 DartPad 故障排除页面。

## 可空和不可空类型 Nullable and non-nullable types 

With [null safety](https://dart.dev/null-safety#enable-null-safety), all types default to non-nullable. For example, if you have a variable of type `String`, it always contains a string.

​	使用空安全后，所有类型默认为不可空。例如，如果您有一个类型为 `String` 的变量，它始终包含一个字符串。

To allow a variable of type `String` to accept any string or the value `null`, add a question mark (`?`) after the type name. This changes the type of variable to a nullable type. For example, a variable of type `String?` can contain a string, or it can be null.

​	要允许类型为 `String` 的变量接受任何字符串或值 `null` ，请在类型名称后添加一个问号 ( `?` )。这会将变量的类型更改为可空类型。例如，类型为 `String?` 的变量可以包含一个字符串，也可以为 null。

### 练习：不可空类型 Exercise: Non-nullable types 

In the following example, the developer declared variable `a` an `int`. Try changing the value in the assignment to `3` or `145`, but not `null`!
在以下示例中，开发者将变量 `a` 声明为 `int` 。尝试将赋值中的值更改为 `3` 或 `145` ，但不能是 `null` ！

```dart
void main() {
  int a;
  a = null;
  print('a is $a.');
}
```



<iframe src="https://dartpad.dev/embed-dart.html?theme=light&amp;run=dartpad&amp;split=false&amp;ga_id=nonnullable_type" data-gtm-vis-first-on-screen13029053_11="422" data-gtm-vis-total-visible-time13029053_11="100" data-gtm-vis-has-fired13029053_11="1" style="box-sizing: border-box; border: 1px solid rgb(204, 204, 204); margin-bottom: 1rem; min-height: 400px; resize: vertical; width: 896px;"></iframe>

### 练习：可空类型 Exercise: Nullable types 

What if you need a variable that *can* hold a null value? Try changing the type of `a` so that `a` can be either `null` or an `int`:

​	如果需要一个可以保存空值的变量，该怎么办？尝试更改 `a` 的类型，以便 `a` 可以是 `null` 或 `int` ：

```dart
void main() {
  int a;
  a = null;
  print('a is $a.');
}
```



<iframe src="https://dartpad.dev/embed-dart.html?theme=light&amp;run=dartpad&amp;split=false&amp;ga_id=nullable_type" data-gtm-vis-first-on-screen13029053_11="10952" data-gtm-vis-total-visible-time13029053_11="100" data-gtm-vis-has-fired13029053_11="1" style="box-sizing: border-box; border: 1px solid rgb(204, 204, 204); margin-bottom: 1rem; min-height: 400px; resize: vertical; width: 896px;"></iframe>

### 练习：泛型的可空类型参数 Exercise: Nullable type parameters for generics 

Type parameters for generics can also be nullable or non-nullable. Try adding question marks to correct the type declarations of `aNullableListOfStrings` and `aListOfNullableStrings`:

​	泛型的类型参数也可以是可空的或不可空的。尝试添加问号来更正 `aNullableListOfStrings` 和 `aListOfNullableStrings` 的类型声明：



<iframe src="https://dartpad.dev/embed-dart.html?theme=light&amp;run=dartpad&amp;split=false&amp;ga_id=nullable_type_generics" data-gtm-vis-first-on-screen13029053_11="11038" data-gtm-vis-total-visible-time13029053_11="100" data-gtm-vis-has-fired13029053_11="1" style="box-sizing: border-box; border: 1px solid rgb(204, 204, 204); margin-bottom: 1rem; min-height: 400px; resize: vertical; width: 896px;"></iframe>



##  非空断言运算符 (!) - The non-null assertion operator (!)

If you’re sure an expression with a nullable type doesn’t equal `null`, you can use the [non-null assertion operator](https://dart.dev/null-safety/understanding-null-safety#non-null-assertion-operator) (`!`) to make Dart treat it as non-nullable. By adding `!` after the expression, you assert two conditions to Dart about the expression:

​	如果您确定具有可空类型的表达式不等于 `null` ，则可以使用非空断言运算符 ( `!` ) 使 Dart 将其视为不可空。通过在表达式后添加 `!` ，您可以向 Dart 断言有关表达式的两个条件：

1. Its value doesn’t equal `null`
2. 它的值不等于 `null`
3. Dart can assign the value to a non-nullable variable
4. Dart 可以将该值分配给不可空变量

*report_problem* If the expression does equal `null`, **Dart throws an exception at run-time**. This makes the `!` operator *unsafe*. Don’t use it unless you have no doubt the expression can’t equal `null`.

​	如果表达式等于 `null` ，则 Dart 会在运行时引发异常。这使得 `!` 运算符不安全。除非您毫无疑问表达式不会等于 `null` ，否则不要使用它。

### 练习：空断言 Exercise: Null assertion 

In the following code, try adding exclamation points to correct the broken assignments:

​	在以下代码中，尝试添加感叹号来更正错误的赋值：

<iframe src="https://dartpad.dev/embed-dart.html?theme=light&amp;run=dartpad&amp;split=false&amp;ga_id=null_assertion" data-gtm-vis-first-on-screen13029053_11="11138" data-gtm-vis-total-visible-time13029053_11="100" data-gtm-vis-has-fired13029053_11="1" style="box-sizing: border-box; border: 1px solid rgb(204, 204, 204); margin-bottom: 1rem; min-height: 400px; resize: vertical; width: 896px;"></iframe>

## 空感知运算符 Null-aware operators 

If a variable or expression is nullable, you can use [type promotion](https://dart.dev/codelabs/null-safety#type-promotion) to access the type’s members. You can also use null-aware operators to handle nullable values.

​	如果变量或表达式可为 null，则可以使用类型提升来访问该类型的成员。您还可以使用 null 感知运算符来处理可为 null 的值。

Sometimes the flow of the program tells you that the value of an expression cannot be `null`. To force Dart to treat that expression as non-nullable, add the [non-null assertion operator](https://dart.dev/codelabs/null-safety#the-non-null-assertion-operator-) (`!`). If the value does equal `null`, using this operator throws an exception.

​	有时，程序的流程会告诉您表达式的值不可能为 `null` 。要强制 Dart 将该表达式视为非空，请添加非空断言运算符 ( `!` )。如果该值确实等于 `null` ，则使用此运算符会引发异常。

To handle potential `null` values, use the conditional property access operator (`?.`) or null-coalescing operators (`??`) to conditionally access a property or provide a default value if `null` respectively.

​	要处理潜在的 `null` 值，请使用条件属性访问运算符 ( `?.` ) 或 null 合并运算符 ( `??` ) 来有条件地访问属性或分别在 `null` 时提供默认值。

### 练习：条件属性访问 Exercise: Conditional property access 

If you don’t know that an expression with a nullable type equals `null` or not, you can use the conditional member access operator (`?.`). This operator evaluates to `null` if the target expression resolves to `null`. Otherwise, it accesses the property on the non-null target value.

​	如果您不知道具有可为 null 类型的表达式的值是否等于 `null` ，则可以使用条件成员访问运算符 ( `?.` )。如果目标表达式解析为 `null` ，则此运算符求值为 `null` 。否则，它将访问非空目标值上的属性。

```dart
// The following calls the 'action' method only if nullableObject is not null
nullableObject?.action();
```

In the following code, try using conditional property access in the `stringLength` method. This fixes the error and returns the length of the string or `null` if it equals `null`:

​	在以下代码中，尝试在 `stringLength` 方法中使用条件属性访问。这修复了错误，并返回字符串的长度或 `null` （如果它等于 `null` ）：

<iframe src="https://dartpad.dev/embed-dart.html?theme=light&amp;run=dartpad&amp;split=false&amp;ga_id=null" data-gtm-vis-first-on-screen13029053_11="11188" data-gtm-vis-total-visible-time13029053_11="100" data-gtm-vis-has-fired13029053_11="1" style="box-sizing: border-box; border: 1px solid rgb(204, 204, 204); margin-bottom: 1rem; min-height: 400px; resize: vertical; width: 896px;"></iframe>

### 练习：空值合并运算符 Exercise: Null-coalescing operators 

If you want to provide an alternative value when the expression evaluates to `null`, you can specify another expression to evaluate and return instead with the null-coalescing operator (`??`).

​	如果要在表达式评估为 `null` 时提供替代值，可以使用空值合并运算符（`??`）指定另一个要计算并返回的表达式。

```dart
// Both of the following print out 'alternate' if nullableString is null
print(nullableString ?? 'alternate');
print(nullableString != null ? nullableString : 'alternate');
```

You can also use the null-coalescing assignment operator (`??=`) to evaluate and assign an expression result to a variable only if that variable is currently `null`.

​	您还可以使用空值合并赋值运算符（`??=`）仅在变量当前为 null 时计算并将表达式结果分配给变量。

```dart
// Both of the following set nullableString to 'alternate' if it is null
nullableString ??= 'alternate';
nullableString = nullableString != null ? nullableString : 'alternate';
```

In the following code, try using these operators to implement `updateStoredValue` following the logic outlined in its documentation comment:

​	在下面的代码中，请尝试使用这些运算符按照文档注释中概述的逻辑实现 `updateStoredValue`：

<iframe src="https://dartpad.dev/embed-dart.html?theme=light&amp;run=dartpad&amp;split=false&amp;ga_id=null" data-gtm-vis-first-on-screen13029053_11="11820" data-gtm-vis-total-visible-time13029053_11="100" data-gtm-vis-has-fired13029053_11="1" style="box-sizing: border-box; border: 1px solid rgb(204, 204, 204); margin-bottom: 1rem; min-height: 400px; resize: vertical; width: 896px;"></iframe>

## Type promotion 类型提升

Dart’s [flow analysis](https://dart.dev/null-safety/understanding-null-safety#flow-analysis) accounts for nullability. Dart treats nullable variables and fields with no ability to contain null values as non-nullable. We call this behavior [type promotion](https://dart.dev/null-safety/understanding-null-safety#type-promotion-on-null-checks).

​	Dart 的流分析考虑了可空性。Dart 将可空变量和字段（没有包含空值的能力）视为不可空。我们将此行为称为类型提升。

### 练习：明确赋值 Exercise: Definite assignment 

Dart’s type system can track where variables are assigned and read. It can also verify that the developer assigned values to non-nullable variables before any code tries to read from those variables. This process is called [definite assignment](https://dart.dev/null-safety/understanding-null-safety#definite-assignment-analysis).

​	Dart 的类型系统可以跟踪变量的赋值和读取位置。它还可以验证开发人员是否在任何代码尝试从这些变量读取之前向不可空变量赋值。此过程称为明确赋值。

Try uncommenting the `if`-`else` statement in the following code. Watch the analyzer errors disappear:

​	尝试取消注释以下代码中的 `if` - `else` 语句。观察分析器错误消失：

<iframe src="https://dartpad.dev/embed-dart.html?theme=light&amp;run=dartpad&amp;split=false&amp;ga_id=definite_assignment" data-gtm-vis-first-on-screen13029053_11="11922" data-gtm-vis-total-visible-time13029053_11="100" data-gtm-vis-has-fired13029053_11="1" style="box-sizing: border-box; border: 1px solid rgb(204, 204, 204); margin-bottom: 1rem; min-height: 400px; resize: vertical; width: 896px;"></iframe>

### 练习：空值检查 Exercise: Null checking 

In the following code, add an `if` statement to the beginning of `getLength` that returns zero if `str` is `null`:

​	在以下代码中，向 `getLength` 的开头添加一个 `if` 语句，如果 `str` 为 `null` ，则返回零：

<iframe src="https://dartpad.dev/embed-dart.html?theme=light&amp;run=dartpad&amp;split=false&amp;ga_id=null_checking" data-gtm-vis-first-on-screen13029053_11="11955" data-gtm-vis-total-visible-time13029053_11="100" data-gtm-vis-has-fired13029053_11="1" style="box-sizing: border-box; border: 1px solid rgb(204, 204, 204); margin-bottom: 1rem; min-height: 400px; resize: vertical; width: 896px;"></iframe>

### 练习：异常提升 Exercise: Promotion with exceptions 

Promotion works with exceptions as well as return statements. Try a null check that throws an `Exception` instead of returning zero.

​	提升适用于异常和返回语句。尝试一个空值检查，它会引发 `Exception` 而不是返回零。

<iframe src="https://dartpad.dev/embed-dart.html?theme=light&amp;run=dartpad&amp;split=false&amp;ga_id=promotion_exceptions" data-gtm-vis-first-on-screen13029053_11="11990" data-gtm-vis-total-visible-time13029053_11="100" data-gtm-vis-has-fired13029053_11="1" style="box-sizing: border-box; border: 1px solid rgb(204, 204, 204); margin-bottom: 1rem; min-height: 400px; resize: vertical; width: 896px;"></iframe>

## late 关键字 The late keyword 

Sometimes variables—fields in a class, or top-level variables—should be non-nullable, but they can’t be assigned a value immediately. For cases like that, use the [`late` keyword](https://dart.dev/null-safety/understanding-null-safety#late-variables).

​	有时变量（类中的字段或顶级变量）应为不可空，但不能立即为其赋值。对于这种情况，请使用 `late` 关键字。

When you put `late` in front of a variable declaration, that tells Dart the following about the variable:

​	当您在变量声明前放置 `late` 时，这会告诉 Dart 有关变量的以下信息：

- The developer didn’t want to assign it a value yet.
- 开发人员不想为其分配值。
- It will get a value later.
- 它稍后将获得一个值。
- It will have a value *before* being used.
- 在使用之前它将具有一个值。

If you declare a variable `late` and Dart reads the variable before you assigned a value, Dart throws an error.

​	如果您声明了一个变量 `late` ，并且在您分配值之前 Dart 读取了该变量，则 Dart 会引发错误。

### 练习：使用 late - Exercise: Using late 

Try using the `late` keyword to correct the following code. For a little extra fun afterward, try commenting out the line that sets `description`!

​	尝试使用 `late` 关键字来更正以下代码。之后为了增加一点乐趣，尝试注释掉设置 `description` 的行！

<iframe src="https://dartpad.dev/embed-dart.html?theme=light&amp;run=dartpad&amp;split=false&amp;ga_id=late_keyword" data-gtm-vis-first-on-screen13029053_11="12021" data-gtm-vis-total-visible-time13029053_11="100" data-gtm-vis-has-fired13029053_11="1" style="box-sizing: border-box; border: 1px solid rgb(204, 204, 204); margin-bottom: 1rem; min-height: 400px; resize: vertical; width: 896px;"></iframe>

### 练习：延迟循环引用 Exercise: Late circular references 

The `late` keyword helps with tricky patterns like circular references. The following code has two objects that need to maintain non-nullable references to each other. Try using the `late` keyword to fix this code.

​	`late` 关键字有助于解决循环引用等棘手的模式。以下代码有两个对象，它们需要维护彼此的非空引用。尝试使用 `late` 关键字来修复此代码。

You don’t need to remove `final`. You can create [`late final` variables](https://dart.dev/null-safety/understanding-null-safety#late-final-variables): you set their values once, and after that they stay read-only.

​	您无需删除 `final` 。您可以创建 `late final` 变量：您设置它们的值一次，之后它们保持只读。

<iframe src="https://dartpad.dev/embed-dart.html?theme=light&amp;run=dartpad&amp;split=false&amp;ga_id=late_circular" data-gtm-vis-first-on-screen13029053_11="12057" data-gtm-vis-total-visible-time13029053_11="100" data-gtm-vis-has-fired13029053_11="1" style="box-sizing: border-box; border: 1px solid rgb(204, 204, 204); margin-bottom: 1rem; min-height: 400px; resize: vertical; width: 896px;"></iframe>

### 练习：迟到和懒惰 Exercise: Late and lazy 

The `late` keyword can help with another pattern: [lazy initialization](https://dart.dev/null-safety/understanding-null-safety#lazy-initialization) for expensive non-nullable fields. Try the following:

​	关键字可以帮助解决另一个模式：昂贵的非空字段的延迟初始化。尝试以下操作：

1. Run this code without changing it, and note the output.
2. 在不更改代码的情况下运行此代码，并注意输出。
3. Think: What will change if you make `_cache` a `late` field?
4. 思考：如果您将 `_cache` 设为 `late` 字段，会发生什么变化？
5. Make `_cache` a `late` field, and run the code. Was your prediction correct?
6. 将 `_cache` 设为 `late` 字段，然后运行代码。您的预测是否正确？

<iframe src="https://dartpad.dev/embed-dart.html?theme=light&amp;run=dartpad&amp;split=false&amp;ga_id=lazy_late" data-gtm-vis-first-on-screen13029053_11="12072" data-gtm-vis-total-visible-time13029053_11="100" data-gtm-vis-has-fired13029053_11="1" style="box-sizing: border-box; border: 1px solid rgb(204, 204, 204); margin-bottom: 1rem; min-height: 400px; resize: vertical; width: 896px;"></iframe>

*info* **Fun fact:** After you add `late` to the declaration of `_cache`, if you move the `_computeValue` function into the `CachedValueProvider` class, the code still works! Initialization expressions for `late` fields can use instance methods in their initializers.

​	有趣的事实：将 `late` 添加到 `_cache` 的声明后，如果您将 `_computeValue` 函数移至 `CachedValueProvider` 类，代码仍然有效！ `late` 字段的初始化表达式可以在其初始化器中使用实例方法。

## What’s next? 

Congratulations, you’ve finished the codelab! To learn more, check out some suggestions for where to go next:

​	恭喜，您已完成代码实验室！要了解更多信息，请查看以下有关下一步去向的建议：

- Learn more about null safety

- 
  详细了解空安全
  
  - [Overview of null safety](https://dart.dev/null-safety).
  - 空安全的概述。
  - [Deep dive into understanding null safety](https://dart.dev/null-safety/understanding-null-safety).
  - 深入了解空安全。
  
- If you want to improve this codelab, check out [issue #3093](https://github.com/dart-lang/site-www/issues/3093).
  
- 如果您想改进此代码实验室，请查看问题 #3093。
