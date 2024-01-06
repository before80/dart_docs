+++
title = "Variables"
date = 2024-01-05T20:29:36+08:00
weight = 1
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文: [https://dart.dev/language/variables](https://dart.dev/language/variables)

# Variables 变量

Here’s an example of creating a variable and initializing it:
下面是一个创建变量并对其进行初始化的示例：

```dart
var name = 'Bob';
```

Variables store references. The variable called `name` contains a reference to a `String` object with a value of “Bob”.
变量存储引用。名为 `name` 的变量包含对值等于“Bob”的 `String` 对象的引用。

The type of the `name` variable is inferred to be `String`, but you can change that type by specifying it. If an object isn’t restricted to a single type, specify the `Object` type (or `dynamic` if necessary).
变量 `name` 的类型推断为 `String` ，但您可以通过指定类型来更改该类型。如果对象不限于单一类型，请指定 `Object` 类型（或必要时指定 `dynamic` ）。

```dart
Object name = 'Bob';
```

Another option is to explicitly declare the type that would be inferred:
另一个选择是显式声明将推断出的类型：

```dart
String name = 'Bob';
```

*info* **Note:** This page follows the [style guide recommendation](https://dart.dev/effective-dart/design#types) of using `var`, rather than type annotations, for local variables.
注意：此页面遵循使用 `var` 而不是类型注释作为局部变量的样式指南建议。

## Null safety 空安全

The Dart language enforces sound null safety.
Dart 语言强制执行健全的空安全性。

Null safety prevents an error that results from unintentional access of variables set to `null`. The error is called a null dereference error. A null dereference error occurs when you access a property or call a method on an expression that evaluates to `null`. An exception to this rule is when `null` supports the property or method, like `toString()` or `hashCode`. With null safety, the Dart compiler detects these potential errors at compile time.
空安全性可防止因意外访问设置为 `null` 的变量而导致的错误。该错误称为空引用错误。当您访问属性或调用对 `null` 求值的表达式上的方法时，就会发生空引用错误。此规则的例外情况是当 `null` 支持该属性或方法时，例如 `toString()` 或 `hashCode` 。通过空安全性，Dart 编译器可在编译时检测到这些潜在错误。

For example, say you want to find the absolute value of an `int` variable `i`. If `i` is `null`, calling `i.abs()` causes a null dereference error. In other languages, trying this could lead to a runtime error, but Dart’s compiler prohibits these actions. Therefore, Dart apps can’t cause runtime errors.
例如，假设您想查找 `int` 变量 `i` 的绝对值。如果 `i` 为 `null` ，则调用 `i.abs()` 会导致空引用错误。在其他语言中，尝试这样做可能会导致运行时错误，但 Dart 的编译器禁止这些操作。因此，Dart 应用不会导致运行时错误。

Null safety introduces three key changes:
空安全引入三个关键更改：

1. When you specify a type for a variable, parameter, or another relevant component, you can control whether the type allows `null`. To enable nullability, you add a `?` to the end of the type declaration.
   为变量、参数或其他相关组件指定类型时，您可以控制该类型是否允许 `null` 。要启用可空性，您可以在类型声明的末尾添加 `?` 。

   ```
   String? name  // Nullable type. Can be `null` or string.
   
   String name   // Non-nullable type. Cannot be `null` but can be string.
   ```

2. You must initialize variables before using them. Nullable variables default to `null`, so they are initialized by default. Dart doesn’t set initial values to non-nullable types. It forces you to set an initial value. Dart doesn’t allow you to observe an uninitialized variable. This prevents you from accessing properties or calling methods where the receiver’s type can be `null` but `null` doesn’t support the method or property used.
   您必须在使用变量之前对其进行初始化。可空变量默认为 `null` ，因此它们默认初始化。Dart 不会为不可空类型设置初始值。它强制您设置初始值。Dart 不允许您观察未初始化的变量。这可防止您访问属性或调用方法，其中接收者的类型可以是 `null` ，但 `null` 不支持所使用的属性或方法。

3. You can’t access properties or call methods on an expression with a nullable type. The same exception applies where it’s a property or method that `null` supports like `hashCode` or `toString()`.
   您无法访问具有可空类型的表达式的属性或调用方法。在 `null` 支持的属性或方法（如 `hashCode` 或 `toString()` ）中也适用相同的异常。

Sound null safety changes potential **runtime errors** into **edit-time** analysis errors. Null safety flags a non-null variable when it has been either:
声音空安全将潜在的运行时错误更改为编辑时分析错误。空安全在以下情况将非空变量标记为非空：

- Not initialized with a non-null value.
  未用非空值初始化。
- Assigned a `null` value.
  分配了 `null` 值。

This check allows you to fix these errors *before* deploying your app.
此检查允许您在部署应用之前修复这些错误。

## Default value 默认值

Uninitialized variables that have a nullable type have an initial value of `null`. Even variables with numeric types are initially null, because numbers—like everything else in Dart—are objects.
具有可空类型的未初始化变量的初始值为 `null` 。即使是数字类型的变量最初也是 null，因为数字（与 Dart 中的其他所有内容一样）都是对象。

```dart
int? lineCount;
assert(lineCount == null);
```

*info* **Note:** Production code ignores the `assert()` call. During development, on the other hand, `assert(*condition*)` throws an exception if *condition* is false. For details, check out [Assert](https://dart.dev/language/error-handling#assert).
注意：生产代码忽略 `assert()` 调用。另一方面，在开发过程中，如果条件为 false， `assert(*condition*)` 会引发异常。有关详细信息，请查看 Assert。

With null safety, you must initialize the values of non-nullable variables before you use them:
使用空安全，您必须在使用非空变量之前初始化其值：

```dart
int lineCount = 0;
```

You don’t have to initialize a local variable where it’s declared, but you do need to assign it a value before it’s used. For example, the following code is valid because Dart can detect that `lineCount` is non-null by the time it’s passed to `print()`:
您不必在声明本地变量时对其进行初始化，但您确实需要在使用它之前为其分配一个值。例如，以下代码有效，因为 Dart 可以检测到在将 `lineCount` 传递给 `print()` 时它不是 null：

```dart
int lineCount;

if (weLikeToCount) {
  lineCount = countLines();
} else {
  lineCount = 0;
}

print(lineCount);
```

Top-level and class variables are lazily initialized; the initialization code runs the first time the variable is used.
顶级变量和类变量是延迟初始化的；初始化代码在变量首次使用时运行。

## Late variables 延迟变量

The `late` modifier has two use cases:
`late` 修饰符有两种用例：

- Declaring a non-nullable variable that’s initialized after its declaration.
  声明一个在声明后初始化的非空变量。
- Lazily initializing a variable.
  延迟初始化变量。

Often Dart’s control flow analysis can detect when a non-nullable variable is set to a non-null value before it’s used, but sometimes analysis fails. Two common cases are top-level variables and instance variables: Dart often can’t determine whether they’re set, so it doesn’t try.
通常，Dart 的控制流分析可以检测到在使用非空变量之前是否已将其设置为非空值，但有时分析会失败。两种常见情况是顶级变量和实例变量：Dart 通常无法确定它们是否已设置，因此不会尝试。

If you’re sure that a variable is set before it’s used, but Dart disagrees, you can fix the error by marking the variable as `late`:
如果您确定在使用变量之前已对其进行设置，但 Dart 不认同，则可以通过将变量标记为 `late` 来修复错误：

```dart
late String description;

void main() {
  description = 'Feijoada!';
  print(description);
}
```

*report_problem* If you fail to initialize a `late` variable, a runtime error occurs when the variable is used.
如果您未能初始化 `late` 变量，则在使用该变量时会发生运行时错误。

When you mark a variable as `late` but initialize it at its declaration, then the initializer runs the first time the variable is used. This lazy initialization is handy in a couple of cases:
当您将变量标记为 `late` 但在声明时对其进行初始化，则初始化程序在第一次使用该变量时运行。这种惰性初始化在以下几种情况下很方便：

- The variable might not be needed, and initializing it is costly.
  可能不需要该变量，并且初始化它很昂贵。
- You’re initializing an instance variable, and its initializer needs access to `this`.
  您正在初始化实例变量，并且其初始化程序需要访问 `this` 。

In the following example, if the `temperature` variable is never used, then the expensive `readThermometer()` function is never called:
在以下示例中，如果从未使用 `temperature` 变量，则永远不会调用昂贵的 `readThermometer()` 函数：

```dart
// This is the program's only call to readThermometer().
late String temperature = readThermometer(); // Lazily initialized.
```

## Final and const Final 和 const

If you never intend to change a variable, use `final` or `const`, either instead of `var` or in addition to a type. A final variable can be set only once; a const variable is a compile-time constant. (Const variables are implicitly final.)
如果您从不打算更改变量，请使用 `final` 或 `const` ，而不是 `var` 或除了类型之外。final 变量只能设置一次；const 变量是编译时常量。（Const 变量隐式为 final。）

*info* **Note:** [Instance variables](https://dart.dev/language/classes#instance-variables) can be `final` but not `const`.
注意：实例变量可以是 `final` 但不能是 `const` 。

Here’s an example of creating and setting a `final` variable:
以下是如何创建和设置 `final` 变量的示例：

```dart
final name = 'Bob'; // Without a type annotation
final String nickname = 'Bobby';
```

You can’t change the value of a `final` variable:
您无法更改 `final` 变量的值：

```dart
name = 'Alice'; // Error: a final variable can only be set once.
```

Use `const` for variables that you want to be **compile-time constants**. If the const variable is at the class level, mark it `static const`. Where you declare the variable, set the value to a compile-time constant such as a number or string literal, a const variable, or the result of an arithmetic operation on constant numbers:
对于您希望作为编译时常量的变量，请使用 `const` 。如果 const 变量位于类级别，请将其标记为 `static const` 。在您声明变量的位置，将值设置为编译时常量，例如数字或字符串文字、const 变量或对常数数字进行算术运算的结果：

```dart
const bar = 1000000; // Unit of pressure (dynes/cm2)
const double atm = 1.01325 * bar; // Standard atmosphere
```

The `const` keyword isn’t just for declaring constant variables. You can also use it to create constant *values*, as well as to declare constructors that *create* constant values. Any variable can have a constant value.
关键字 `const` 不仅用于声明常量变量。您还可以使用它来创建常量值，以及声明创建常量值的构造函数。任何变量都可以具有常量值。

```dart
var foo = const [];
final bar = const [];
const baz = []; // Equivalent to `const []`
```

You can omit `const` from the initializing expression of a `const` declaration, like for `baz` above. For details, see [DON’T use const redundantly](https://dart.dev/effective-dart/usage#dont-use-const-redundantly).
您可以从 `const` 声明的初始化表达式中省略 `const` ，例如上面的 `baz` 。有关详细信息，请参阅不要冗余地使用 const。

You can change the value of a non-final, non-const variable, even if it used to have a `const` value:
您可以更改非 final、非 const 变量的值，即使它曾经具有 `const` 值：

```dart
foo = [1, 2, 3]; // Was const []
```

You can’t change the value of a `const` variable:
您无法更改 `const` 变量的值：

```dart
baz = [42]; // Error: Constant variables can't be assigned a value.
```

You can define constants that use [type checks and casts](https://dart.dev/language/operators#type-test-operators) (`is` and `as`), [collection `if`](https://dart.dev/language/collections#control-flow-operators), and [spread operators](https://dart.dev/language/collections#spread-operators) (`...` and `...?`):
您可以定义使用类型检查和强制转换 ( `is` 和 `as` )、集合 `if` 以及展开运算符 ( `...` 和 `...?` ) 的常量：

```dart
const Object i = 3; // Where i is a const Object with an int value...
const list = [i as int]; // Use a typecast.
const map = {if (i is int) i: 'int'}; // Use is and collection if.
const set = {if (list is List<int>) ...list}; // ...and a spread.
```

*info* **Note:** Although a `final` object cannot be modified, its fields can be changed. In comparison, a `const` object and its fields cannot be changed: they’re *immutable*.
注意：尽管无法修改 `final` 对象，但可以更改其字段。相比之下，无法更改 `const` 对象及其字段：它们是不可变的。

For more information on using `const` to create constant values, see [Lists](https://dart.dev/language/collections#lists), [Maps](https://dart.dev/language/collections#maps), and [Classes](https://dart.dev/language/classes).
有关使用 `const` 创建常量值的更多信息，请参阅列表、映射和类。
