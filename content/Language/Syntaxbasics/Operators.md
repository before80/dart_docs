+++
title = "运算符"
date = 2024-01-05T20:29:36+08:00
weight = 10
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文: [https://dart.dev/language/operators](https://dart.dev/language/operators)

## Operators 运算符

Dart supports the operators shown in the following table. The table shows Dart’s operator associativity and [operator precedence](https://dart.dev/language/operators#operator-precedence-example) from highest to lowest, which are an **approximation** of Dart’s operator relationships. You can implement many of these [operators as class members](https://dart.dev/language/methods#operators).

​	Dart 支持下表中所示的运算符。该表显示了 Dart 的运算符结合性和运算符优先级，从最高到最低，这是 Dart 的运算符关系的近似值。您可以将其中许多运算符实现为类成员。

| Description 说明                        | Operator 运算符                                              | Associativity 结合性 |
| --------------------------------------- | ------------------------------------------------------------ | -------------------- |
| unary postfix 一元后缀                  | `expr++`  `expr--`  `()`  `[]`  `?[]`  `.`  `?.`  `!`        | None 无              |
| unary prefix 一元前缀                   | `-expr`  `!expr`  `~expr`  `++expr`  `--expr`   `await expr` | None 无              |
| multiplicative 乘法                     | `*`  `/`  `%` `~/`                                           | Left 左              |
| additive 加法                           | `+`  `-`                                                     | Left 左              |
| shift 移位                              | `<<`  `>>`  `>>>`                                            | Left 左              |
| bitwise AND 按位与                      | `&`                                                          | Left 左              |
| bitwise XOR 按位异或                    | `^`                                                          | Left 左              |
| bitwise OR 按位或                       | `\|`                                                         | Left 左              |
| relational and type test 关系和类型测试 | `>=`  `>`  `<=`  `<`  `as`  `is`  `is!`                      | None 无              |
| equality 相等                           | `==`  `!=`                                                   | None 无              |
| logical AND 逻辑与                      | `&&`                                                         | Left 左              |
| logical OR 逻辑或                       | `\|\|`                                                       | Left 左              |
| if null 如果为空                        | `??`                                                         | Left 左              |
| conditional 条件                        | `expr1 ? expr2 : expr3`                                      | Right 右             |
| cascade 级联                            | `..`  `?..`                                                  | Left 左              |
| assignment 作业                         | `=`  `*=`  `/=`  `+=`  `-=`  `&=`  `^=`  *etc.*              | Right 右             |

> **Warning:** The previous table should only be used as a helpful guide. The notion of operator precedence and associativity is an approximation of the truth found in the language grammar. You can find the authoritative behavior of Dart’s operator relationships in the grammar defined in the [Dart language specification](https://dart.dev/guides/language/spec).
>
> ​	警告：前表仅应作为有用的指南。运算符优先级和结合性的概念是对语言语法中发现的真理的近似。您可以在 Dart 语言规范中定义的语法中找到 Dart 的运算符关系的权威行为。
>

When you use operators, you create expressions. Here are some examples of operator expressions:

​	使用运算符时，您会创建表达式。以下是一些运算符表达式的示例：

```dart
a++
a + b
a = b
a == b
c ? a : b
a is T
```

## 运算符优先级示例 Operator precedence example 

In the [operator table](https://dart.dev/language/operators#operators), each operator has higher precedence than the operators in the rows that follow it. For example, the multiplicative operator `%` has higher precedence than (and thus executes before) the equality operator `==`, which has higher precedence than the logical AND operator `&&`. That precedence means that the following two lines of code execute the same way:

​	在运算符表中，每个运算符的优先级都高于其后行的运算符。例如，乘法运算符 `%` 的优先级高于（因此先于）相等运算符 `==` ，后者优先于逻辑 AND 运算符 `&&` 。该优先级意味着以下两行代码以相同方式执行：

```dart
// Parentheses improve readability.
if ((n % i == 0) && (d % i == 0)) ...

// Harder to read, but equivalent.
if (n % i == 0 && d % i == 0) ...
```

> **Warning:** For operators that take two operands, the leftmost operand determines which method is used. For example, if you have a `Vector` object and a `Point` object, then `aVector + aPoint` uses `Vector` addition (`+`).
>
> ​	警告：对于采用两个操作数的运算符，最左边的操作数决定使用哪种方法。例如，如果您有一个 `Vector` 对象和一个 `Point` 对象，则 `aVector + aPoint` 使用 `Vector` 加法（ `+` ）。
>

## 算术运算符 Arithmetic operators 

Dart supports the usual arithmetic operators, as shown in the following table.

​	Dart 支持通常的算术运算符，如下表所示。

| Operator 运算符 | Meaning 含义                                                 |
| --------------- | ------------------------------------------------------------ |
| `+`             | Add                                                          |
| `-`             | Subtract 减                                                  |
| `-expr`         | Unary minus, also known as negation (reverse the sign of the expression) 一元减号，也称为取反（反转表达式的符号） |
| `*`             | Multiply 乘法                                                |
| `/`             | Divide 除法                                                  |
| `~/`            | Divide, returning an integer result 除法，返回一个整数结果   |
| `%`             | Get the remainder of an integer division (modulo) 获取整数除法的余数（模运算） |

Example:

​	示例：

```dart
assert(2 + 3 == 5);
assert(2 - 3 == -1);
assert(2 * 3 == 6);
assert(5 / 2 == 2.5); // Result is a double
assert(5 ~/ 2 == 2); // Result is an int
assert(5 % 2 == 1); // Remainder

assert('5/2 = ${5 ~/ 2} r ${5 % 2}' == '5/2 = 2 r 1');
```

Dart also supports both prefix and postfix increment and decrement operators.

​	Dart 还支持前缀和后缀增量和减量运算符。

| Operator 运算符 | Meaning 含义                                                 |
| --------------- | ------------------------------------------------------------ |
| `++var`         | `var = var + 1` (expression value is `var + 1`)  （表达式值为 `var + 1` ） |
| `var++`         | `var = var + 1` (expression value is `var`)  （表达式值为 `var` ） |
| `--var`         | `var = var - 1` (expression value is `var - 1`)  （表达式值为 `var - 1` ） |
| `var--`         | `var = var - 1` (expression value is `var`)  （表达式值为 `var` ） |

Example:

​	示例：

```dart
int a;
int b;

a = 0;
b = ++a; // Increment a before b gets its value.
assert(a == b); // 1 == 1

a = 0;
b = a++; // Increment a after b gets its value.
assert(a != b); // 1 != 0

a = 0;
b = --a; // Decrement a before b gets its value.
assert(a == b); // -1 == -1

a = 0;
b = a--; // Decrement a after b gets its value.
assert(a != b); // -1 != 0
```

## 相等和关系运算符 Equality and relational operators 

The following table lists the meanings of equality and relational operators.

​	下表列出了相等和关系运算符的含义。

| Operator 运算符 | Meaning 含义                                       |
| --------------- | -------------------------------------------------- |
| `==`            | Equal; see discussion below 相等；请参阅下面的讨论 |
| `!=`            | Not equal 不相等                                   |
| `>`             | Greater than 大于                                  |
| `<`             | Less than 小于                                     |
| `>=`            | Greater than or equal to 大于或等于                |
| `<=`            | Less than or equal to 小于或等于                   |

To test whether two objects x and y represent the same thing, use the `==` operator. (In the rare case where you need to know whether two objects are the exact same object, use the [identical()](https://api.dart.dev/stable/dart-core/identical.html) function instead.) Here’s how the `==` operator works:

​	要测试两个对象 x 和 y 是否表示相同的事物，请使用 `==` 运算符。（在极少数情况下，如果您需要知道两个对象是否是完全相同对象，请改用 identical() 函数。）以下是 `==` 运算符的工作方式：

1. If *x* or *y* is null, return true if both are null, and false if only one is null.
2. 如果 x 或 y 为 null，如果两者都为 null，则返回 true，如果只有一个为 null，则返回 false。
3. Return the result of invoking the `==` method on *x* with the argument *y*. (That’s right, operators such as `==` are methods that are invoked on their first operand. For details, see [Operators](https://dart.dev/language/methods#operators).)
4. 返回使用参数 y 在 x 上调用 `==` 方法的结果。（没错，诸如 `==` 的运算符是在其第一个操作数上调用的方法。有关详细信息，请参阅运算符。）

Here’s an example of using each of the equality and relational operators:

​	以下是如何使用每个相等性和关系运算符的示例：

```dart
assert(2 == 2);
assert(2 != 3);
assert(3 > 2);
assert(2 < 3);
assert(3 >= 3);
assert(2 <= 3);
```

## 类型测试运算符 Type test operators 

The `as`, `is`, and `is!` operators are handy for checking types at runtime.

​	`as` 、 `is` 和 `is!` 运算符非常适合在运行时检查类型。

| Operator 运算符 | Meaning 含义                                                 |
| --------------- | ------------------------------------------------------------ |
| `as`            | Typecast (also used to specify [library prefixes](https://dart.dev/language/libraries#specifying-a-library-prefix)) 类型转换（也用于指定库前缀） |
| `is`            | True if the object has the specified type 如果对象具有指定类型，则为 True |
| `is!`           | True if the object doesn’t have the specified type 如果对象不具有指定类型，则为 True |

The result of `obj is T` is true if `obj` implements the interface specified by `T`. For example, `obj is Object?` is always true.

​	如果 `obj` 实现 `T` 指定的接口，则 `obj is T` 的结果为 true。例如， `obj is Object?` 始终为 true。

Use the `as` operator to cast an object to a particular type if and only if you are sure that the object is of that type. Example:

​	仅当您确定对象属于特定类型时，才使用 `as` 运算符将对象强制转换为特定类型。示例：

```dart
(employee as Person).firstName = 'Bob';
```

If you aren’t sure that the object is of type `T`, then use `is T` to check the type before using the object.

​	如果不确定对象是否属于 `T` 类型，则使用 `is T` 在使用对象之前检查类型。

```dart
if (employee is Person) {
  // Type check
  employee.firstName = 'Bob';
}
```

*info* **Note:** The code isn’t equivalent. If `employee` is null or not a `Person`, the first example throws an exception; the second does nothing.

​	注意：代码并不等效。如果 `employee` 为 null 或不是 `Person` ，第一个示例会引发异常；第二个示例则不执行任何操作。

## 赋值运算符 Assignment operators 

As you’ve already seen, you can assign values using the `=` operator. To assign only if the assigned-to variable is null, use the `??=` operator.

​	如您所见，您可以使用 `=` 运算符分配值。要仅在被分配变量为 null 时分配，请使用 `??=` 运算符。

```dart
// Assign value to a
a = value;
// Assign value to b if b is null; otherwise, b stays the same
b ??= value;
```

Compound assignment operators such as `+=` combine an operation with an assignment.

​	诸如 `+=` 的复合赋值运算符将运算与赋值结合在一起。

| `=`  | `*=`  | `%=`  | `>>>=` | `^=`  |
| ---- | ----- | ----- | ------ | ----- |
| `+=` | `/=`  | `<<=` | `&=`   | `\|=` |
| `-=` | `~/=` | `>>=` |        |       |

Here’s how compound assignment operators work:

​	复合赋值运算符的工作方式如下：

|                                        | Compound assignment 复合赋值 | Equivalent expression 等价表达式 |
| -------------------------------------- | ---------------------------- | -------------------------------- |
| **For an operator op 对于运算符 op：** | `a op= b`                    | `a = a op b`                     |
| **Example: 示例：**                    | `a += b`                     | `a = a + b`                      |

The following example uses assignment and compound assignment operators:

​	以下示例使用赋值和复合赋值运算符：

```dart
var a = 2; // Assign using =
a *= 3; // Assign and multiply: a = a * 3
assert(a == 6);
```

## 逻辑运算符 Logical operators 

You can invert or combine boolean expressions using the logical operators.

​	您可以使用逻辑运算符反转或组合布尔表达式。

| Operator 运算符 | Meaning 含义                                                 |
| --------------- | ------------------------------------------------------------ |
| `!expr`         | inverts the following expression (changes false to true, and vice versa) 反转以下表达式（将 false 更改为 true，反之亦然） |
| `\|\|`          | logical OR 逻辑 OR                                           |
| `&&`            | logical AND 逻辑 AND                                         |

Here’s an example of using the logical operators:

​	以下是使用逻辑运算符的示例：

```dart
if (!done && (col == 0 || col == 3)) {
  // ...Do something...
}
```

## 按位与移位运算符 Bitwise and shift operators 

You can manipulate the individual bits of numbers in Dart. Usually, you’d use these bitwise and shift operators with integers.

​	您可以在 Dart 中操作数字的各个位。通常，您会将这些按位与移位运算符与整数一起使用。

| Operator 运算符 | Meaning 含义                                                 |
| --------------- | ------------------------------------------------------------ |
| `&`             | AND                                                          |
| `\|`            | `OR`                                                         |
| `^`             | XOR                                                          |
| `~expr`         | Unary bitwise complement (0s become 1s; 1s become 0s) 一元按位补码（0 变为 1；1 变为 0） |
| `<<`            | Shift left 左移                                              |
| `>>`            | Shift right 右移                                             |
| `>>>`           | Unsigned shift right 无符号右移                              |

*info* **Note:** The behavior of bitwise operations with large or negative operands might differ between platforms. To learn more, check out [Bitwise operations platform differences](https://dart.dev/guides/language/numbers#bitwise-operations).

​	注意：按位运算与大操作数或负操作数的行为可能因平台而异。要了解更多信息，请查看[按位运算平台差异]({{< ref "/Development/Numberrepresentation#按位运算-bitwise-operations">}})。

Here’s an example of using bitwise and shift operators:

​	以下是使用按位和移位运算符的示例：

```dart
final value = 0x22;
final bitmask = 0x0f;

assert((value & bitmask) == 0x02); // AND
assert((value & ~bitmask) == 0x20); // AND NOT
assert((value | bitmask) == 0x2f); // OR
assert((value ^ bitmask) == 0x2d); // XOR

assert((value << 4) == 0x220); // Shift left
assert((value >> 4) == 0x02); // Shift right

// Shift right example that results in different behavior on web
// because the operand value changes when masked to 32 bits:
assert((-value >> 4) == -0x03);

assert((value >>> 4) == 0x02); // Unsigned shift right
assert((-value >>> 4) > 0); // Unsigned shift right
```

*merge_type* **Version note:** The `>>>` operator (known as *triple-shift* or *unsigned shift*) requires a [language version](https://dart.dev/guides/language/evolution#language-versioning) of at least 2.14.

​	版本说明： `>>>` 运算符（称为三重移位或无符号移位）需要至少 2.14 的语言版本。

## 条件表达式 Conditional expressions 

Dart has two operators that let you concisely evaluate expressions that might otherwise require [if-else](https://dart.dev/language/branches#if) statements:

​	Dart 有两个运算符，可让您简洁地计算可能需要 if-else 语句的表达式：

- `condition ? expr1 : expr2`

  If condition is true, evaluates expr1 (and returns its value); otherwise, evaluates and returns the value of expr2. 

  如果条件为真，则计算 expr1（并返回其值）；否则，计算并返回 expr2 的值。

- `expr1 ?? expr2`

  If expr1 is non-null, returns its value; otherwise, evaluates and returns the value of expr2. 
  
  如果 expr1 为非空，则返回其值；否则，计算并返回 expr2 的值。

When you need to assign a value based on a boolean expression, consider using `?` and `:`.

​	当您需要根据布尔表达式分配值时，请考虑使用 `?` 和 `:` 。

```dart
var visibility = isPublic ? 'public' : 'private';
```

If the boolean expression tests for null, consider using `??`.

​	如果布尔表达式测试空值，请考虑使用 `??` 。

```dart
String playerName(String? name) => name ?? 'Guest';
```

The previous example could have been written at least two other ways, but not as succinctly:

​	前面的示例至少可以用另外两种方式编写，但不如这种方式简洁：

```dart
// Slightly longer version uses ?: operator.
String playerName(String? name) => name != null ? name : 'Guest';

// Very long version uses if-else statement.
String playerName(String? name) {
  if (name != null) {
    return name;
  } else {
    return 'Guest';
  }
}
```

## 级联表示法 Cascade notation 

Cascades (`..`, `?..`) allow you to make a sequence of operations on the same object. In addition to accessing instance members, you can also call instance methods on that same object. This often saves you the step of creating a temporary variable and allows you to write more fluid code.

​	级联 ( `..` , `?..` ) 允许您对同一对象执行一系列操作。除了访问实例成员外，您还可以对同一对象调用实例方法。这通常可以省去创建临时变量的步骤，并允许您编写更流畅的代码。

Consider the following code:

​	考虑以下代码：

```dart
var paint = Paint()
  ..color = Colors.black
  ..strokeCap = StrokeCap.round
  ..strokeWidth = 5.0;
```

The constructor, `Paint()`, returns a `Paint` object. The code that follows the cascade notation operates on this object, ignoring any values that might be returned.

​	构造函数 `Paint()` 返回 `Paint` 对象。级联符号后面的代码在此对象上运行，忽略可能返回的任何值。

The previous example is equivalent to this code:

​	前面的示例等效于以下代码：

```dart
var paint = Paint();
paint.color = Colors.black;
paint.strokeCap = StrokeCap.round;
paint.strokeWidth = 5.0;
```

If the object that the cascade operates on can be null, then use a *null-shorting* cascade (`?..`) for the first operation. Starting with `?..` guarantees that none of the cascade operations are attempted on that null object.

​	如果级联操作的对象可以为 null，则对第一个操作使用 null 缩短级联 ( `?..` )。从 `?..` 开始可确保对该 null 对象不尝试任何级联操作。

```dart
querySelector('#confirm') // Get an object.
  ?..text = 'Confirm' // Use its members.
  ..classes.add('important')
  ..onClick.listen((e) => window.alert('Confirmed!'))
  ..scrollIntoView();
```

*merge_type* **Version note:** The `?..` syntax requires a [language version](https://dart.dev/guides/language/evolution#language-versioning) of at least 2.12.

​	版本说明： `?..` 语法要求语言版本至少为 2.12。

The previous code is equivalent to the following:

​	前面的代码等效于以下内容：

```dart
var button = querySelector('#confirm');
button?.text = 'Confirm';
button?.classes.add('important');
button?.onClick.listen((e) => window.alert('Confirmed!'));
button?.scrollIntoView();
```

You can also nest cascades. For example:

​	您还可以嵌套级联。例如：

```dart
final addressBook = (AddressBookBuilder()
      ..name = 'jenny'
      ..email = 'jenny@example.com'
      ..phone = (PhoneNumberBuilder()
            ..number = '415-555-0100'
            ..label = 'home')
          .build())
    .build();
```

Be careful to construct your cascade on a function that returns an actual object. For example, the following code fails:

​	请务必在返回实际对象的函数上构建级联。例如，以下代码将失败：

```dart
var sb = StringBuffer();
sb.write('foo')
  ..write('bar'); // Error: method 'write' isn't defined for 'void'.
```

The `sb.write()` call returns void, and you can’t construct a cascade on `void`.

​	`sb.write()` 调用返回 void，您无法在 `void` 上构建级联。

*info* **Note:** Strictly speaking, the “double dot” notation for cascades isn’t an operator. It’s just part of the Dart syntax.

​	注意：严格来说，“双点”级联符号不是一个运算符。它只是 Dart 语法的一部分。

## 其他运算符 Other operators 

You’ve seen most of the remaining operators in other examples:

​	您已经在其他示例中看到大多数剩余的运算符：

| Operator 运算符 | Name 名称                                  | Meaning 含义                                                 |
| --------------- | ------------------------------------------ | ------------------------------------------------------------ |
| `()`            | Function application 函数应用              | Represents a function call 表示函数调用                      |
| `[]`            | Subscript access 下标访问                  | Represents a call to the overridable `[]` operator; example: `fooList[1]` passes the int `1` to `fooList` to access the element at index `1` 表示对可重写 `[]` 运算符的调用；示例： `fooList[1]` 将 int `1` 传递给 `fooList` 以访问索引 `1` 处的元素 |
| `?[]`           | Conditional subscript access 条件下标访问  | Like `[]`, but the leftmost operand can be null; example: `fooList?[1]` passes the int `1` to `fooList` to access the element at index `1` unless `fooList` is null (in which case the expression evaluates to null) 与 `[]` 类似，但最左边的操作数可以为 null；示例： `fooList?[1]` 将 int `1` 传递给 `fooList` 以访问索引 `1` 处的元素，除非 `fooList` 为 null（在这种情况下，表达式计算结果为 null） |
| `.`             | Member access 成员访问                     | Refers to a property of an expression; example: `foo.bar` selects property `bar` from expression `foo` 引用表达式的属性；例如： `foo.bar` 从表达式 `foo` 中选择属性 `bar` |
| `?.`            | Conditional member access 条件成员访问     | Like `.`, but the leftmost operand can be null; example: `foo?.bar` selects property `bar` from expression `foo` unless `foo` is null (in which case the value of `foo?.bar` is null) 与 `.` 类似，但最左边的操作数可以为 null；例如： `foo?.bar` 从表达式 `foo` 中选择属性 `bar` ，除非 `foo` 为 null（在这种情况下， `foo?.bar` 的值为 null） |
| `!`             | Non-null assertion operator 非空断言运算符 | Casts an expression to its underlying non-nullable type, throwing a runtime exception if the cast fails; example: `foo!.bar` asserts `foo` is non-null and selects the property `bar`, unless `foo` is null in which case a runtime exception is thrown 将表达式强制转换为其基础非空类型，如果强制转换失败，则会引发运行时异常；例如： `foo!.bar` 断言 `foo` 为非空并选择属性 `bar` ，除非 `foo` 为 null，在这种情况下会引发运行时异常 |

For more information about the `.`, `?.`, and `..` operators, see [Classes](https://dart.dev/language/classes).

​	有关 `.` 、 `?.` 和 `..` 运算符的更多信息，请参阅类。
