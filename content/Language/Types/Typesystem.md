+++
title = "Dart 类型系统"
date = 2024-01-05T20:29:36+08:00
weight = 50
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文: [https://dart.dev/language/type-system](https://dart.dev/language/type-system)

## The Dart type system  - Dart 类型系统

The Dart language is type safe: it uses a combination of static type checking and [runtime checks](https://dart.dev/language/type-system#runtime-checks) to ensure that a variable’s value always matches the variable’s static type, sometimes referred to as sound typing. Although *types* are mandatory, type *annotations* are optional because of [type inference](https://dart.dev/language/type-system#type-inference).
Dart 语言是类型安全的：它结合使用静态类型检查和运行时检查来确保变量的值始终与变量的静态类型匹配，有时称为健全类型。尽管类型是强制性的，但由于类型推断，类型注释是可选的。

One benefit of static type checking is the ability to find bugs at compile time using Dart’s [static analyzer.](https://dart.dev/tools/analysis)
静态类型检查的一个好处是能够在编译时使用 Dart 的静态分析器查找错误。

You can fix most static analysis errors by adding type annotations to generic classes. The most common generic classes are the collection types `List<T>` and `Map<K,V>`.
您可以通过向泛型类添加类型注释来修复大多数静态分析错误。最常见的泛型类是集合类型 `List<T>` 和 `Map<K,V>` 。

For example, in the following code the `printInts()` function prints an integer list, and `main()` creates a list and passes it to `printInts()`.
例如，在以下代码中， `printInts()` 函数打印一个整数列表， `main()` 创建一个列表并将其传递给 `printInts()` 。

```dart
void printInts(List<int> a) => print(a);

void main() {
  final list = [];
  list.add(1);
  list.add('2');
  printInts(list);
}
```

The preceding code results in a type error on `list` (highlighted above) at the call of `printInts(list)`:
前面的代码导致 `list` 在调用 `printInts(list)` 时出现类型错误（上面突出显示）：

```
error - The argument type 'List<dynamic>' can't be assigned to the parameter type 'List<int>'. - argument_type_not_assignable
```

The error highlights an unsound implicit cast from `List<dynamic>` to `List<int>`. The `list` variable has static type `List<dynamic>`. This is because the initializing declaration `var list = []` doesn’t provide the analyzer with enough information for it to infer a type argument more specific than `dynamic`. The `printInts()` function expects a parameter of type `List<int>`, causing a mismatch of types.
错误突出显示了从 `List<dynamic>` 到 `List<int>` 的不健全的隐式强制转换。 `list` 变量具有静态类型 `List<dynamic>` 。这是因为初始化声明 `var list = []` 没有为分析器提供足够的信息，以便它推断出比 `dynamic` 更具体的类型参数。 `printInts()` 函数期望类型为 `List<int>` 的参数，导致类型不匹配。

When adding a type annotation (`<int>`) on creation of the list (highlighted below) the analyzer complains that a string argument can’t be assigned to an `int` parameter. Removing the quotes in `list.add('2')` results in code that passes static analysis and runs with no errors or warnings.
在创建列表时添加类型注释（ `<int>` ）（如下突出显示）时，分析器会抱怨无法将字符串参数分配给 `int` 参数。删除 `list.add('2')` 中的引号会导致代码通过静态分析并运行，而不会出现任何错误或警告。

```dart
void printInts(List<int> a) => print(a);

void main() {
  final list = <int>[];
  list.add(1);
  list.add(2);
  printInts(list);
}
```

[Try it in DartPad](https://dartpad.dev/25074a51a00c71b4b000f33b688dedd0).
在 DartPad 中试用。

## What is soundness? 健全性是什么？

*Soundness* is about ensuring your program can’t get into certain invalid states. A sound *type system* means you can never get into a state where an expression evaluates to a value that doesn’t match the expression’s static type. For example, if an expression’s static type is `String`, at runtime you are guaranteed to only get a string when you evaluate it.
健全性是指确保你的程序不会进入某些无效状态。健全的类型系统意味着你永远不会进入表达式求值为与表达式的静态类型不匹配的值的状态。例如，如果表达式的静态类型是 `String` ，则在运行时，你保证在求值时只会得到一个字符串。

Dart’s type system, like the type systems in Java and C#, is sound. It enforces that soundness using a combination of static checking (compile-time errors) and runtime checks. For example, assigning a `String` to `int` is a compile-time error. Casting an object to a `String` using `as String` fails with a runtime error if the object isn’t a `String`.
Dart 的类型系统，就像 Java 和 C# 中的类型系统一样，是健全的。它使用静态检查（编译时错误）和运行时检查的组合来强制执行健全性。例如，将 `String` 赋值给 `int` 是一个编译时错误。使用 `as String` 将对象强制转换为 `String` 会在对象不是 `String` 时引发运行时错误。

## The benefits of soundness 健全性的好处

A sound type system has several benefits:
健全的类型系统有几个好处：

- Revealing type-related bugs at compile time.
  在编译时揭示与类型相关的错误。
  A sound type system forces code to be unambiguous about its types, so type-related bugs that might be tricky to find at runtime are revealed at compile time.
  健全的类型系统强制代码对其类型明确无歧义，因此在运行时可能难以发现的与类型相关的错误会在编译时揭示出来。
- More readable code.
  更具可读性的代码。
  Code is easier to read because you can rely on a value actually having the specified type. In sound Dart, types can’t lie.
  代码更容易阅读，因为您可以依赖值实际上具有指定类型。在健全的 Dart 中，类型不会撒谎。
- More maintainable code.
  更易于维护的代码。
  With a sound type system, when you change one piece of code, the type system can warn you about the other pieces of code that just broke.
  使用健全的类型系统，当您更改一段代码时，类型系统可以警告您刚刚中断的其他代码段。
- Better ahead of time (AOT) compilation.
  更好的提前 (AOT) 编译。
  While AOT compilation is possible without types, the generated code is much less efficient.
  虽然无需类型即可进行 AOT 编译，但生成的代码效率低得多。

## Tips for passing static analysis 静态分析通过提示

Most of the rules for static types are easy to understand. Here are some of the less obvious rules:
大多数静态类型规则都很容易理解。以下是一些不太明显的规则：

- Use sound return types when overriding methods.
  重写方法时使用合理的返回类型。
- Use sound parameter types when overriding methods.
  重写方法时使用合理的参数类型。
- Don’t use a dynamic list as a typed list.
  不要将动态列表用作类型化列表。

Let’s see these rules in detail, with examples that use the following type hierarchy:
让我们详细了解这些规则，并使用以下类型层次结构作为示例：

![a hierarchy of animals where the supertype is Animal and the subtypes are Alligator, Cat, and HoneyBadger. Cat has the subtypes of Lion and MaineCoon](./Typesystem_img/type-hierarchy.png)



### Use sound return types when overriding methods 重写方法时使用合理的返回类型

The return type of a method in a subclass must be the same type or a subtype of the return type of the method in the superclass. Consider the getter method in the `Animal` class:
子类中的方法的返回类型必须是超类中方法的返回类型或其子类型。考虑 `Animal` 类中的 getter 方法：

```dart
class Animal {
  void chase(Animal a) { ... }
  Animal get parent => ...
}
```

The `parent` getter method returns an `Animal`. In the `HoneyBadger` subclass, you can replace the getter’s return type with `HoneyBadger` (or any other subtype of `Animal`), but an unrelated type is not allowed.
在 `parent` 子类中，您可以将 getter 的返回类型替换为 `HoneyBadger` （或 `Animal` 的任何其他子类型），但不允许使用不相关的类型。

```dart
class HoneyBadger extends Animal {
  @override
  void chase(Animal a) { ... }

  @override
  HoneyBadger get parent => ...
}
class HoneyBadger extends Animal {
  @override
  void chase(Animal a) { ... }

  @override
  Root get parent => ...
}
```



### Use sound parameter types when overriding methods 重写方法时使用合理的参数类型

The parameter of an overridden method must have either the same type or a supertype of the corresponding parameter in the superclass. Don’t “tighten” the parameter type by replacing the type with a subtype of the original parameter.
重写方法的参数必须具有与超类中相应参数相同或更高级别的类型。不要通过将类型替换为原始参数的子类型来“收紧”参数类型。

*info* **Note:** If you have a valid reason to use a subtype, you can use the [`covariant` keyword](https://dart.dev/guides/language/sound-problems#the-covariant-keyword).
注意：如果您有正当理由使用子类型，则可以使用 `covariant` 关键字。

Consider the `chase(Animal)` method for the `Animal` class:
考虑 `Animal` 类的 `chase(Animal)` 方法：

```dart
class Animal {
  void chase(Animal a) { ... }
  Animal get parent => ...
}
```

The `chase()` method takes an `Animal`. A `HoneyBadger` chases anything. It’s OK to override the `chase()` method to take anything (`Object`).
`chase()` 方法采用 `Animal` 。一个 `HoneyBadger` 追逐任何东西。将 `chase()` 方法重写为采用任何东西（ `Object` ）是可以的。

```dart
class HoneyBadger extends Animal {
  @override
  void chase(Object a) { ... }

  @override
  Animal get parent => ...
}
```

The following code tightens the parameter on the `chase()` method from `Animal` to `Mouse`, a subclass of `Animal`.
以下代码将 `chase()` 方法上的参数从 `Animal` 收紧到 `Mouse` ，它是 `Animal` 的子类。

```dart
class Mouse extends Animal { ... }

class Cat extends Animal {
  @override
  void chase(Mouse a) { ... }
}
```

This code is not type safe because it would then be possible to define a cat and send it after an alligator:
此代码不是类型安全的，因为这样就可以定义一只猫并让它追逐一只鳄鱼：

```dart
Animal a = Cat();
a.chase(Alligator()); // Not type safe or feline safe.
```

### Don’t use a dynamic list as a typed list 不要将动态列表用作类型化列表

A `dynamic` list is good when you want to have a list with different kinds of things in it. However, you can’t use a `dynamic` list as a typed list.
当您想要一个包含不同类型事物的列表时， `dynamic` 列表非常有用。但是，您不能将 `dynamic` 列表用作类型化列表。

This rule also applies to instances of generic types.
此规则也适用于泛型类型的实例。

The following code creates a `dynamic` list of `Dog`, and assigns it to a list of type `Cat`, which generates an error during static analysis.
以下代码创建一个 `dynamic` 列表 `Dog` ，并将其分配给类型为 `Cat` 的列表，这会在静态分析期间生成一个错误。

```dart
void main() {
  List<Cat> foo = <dynamic>[Dog()]; // Error
  List<dynamic> bar = <dynamic>[Dog(), Cat()]; // OK
}
```

## Runtime checks 运行时检查

Runtime checks deal with type safety issues that can’t be detected at compile time.
运行时检查处理无法在编译时检测到的类型安全问题。

For example, the following code throws an exception at runtime because it’s an error to cast a list of dogs to a list of cats:
例如，以下代码在运行时抛出异常，因为将狗列表强制转换为猫列表是一个错误：

```dart
void main() {
  List<Animal> animals = [Dog()];
  List<Cat> cats = animals as List<Cat>;
}
```

## Type inference 类型推断

The analyzer can infer types for fields, methods, local variables, and most generic type arguments. When the analyzer doesn’t have enough information to infer a specific type, it uses the `dynamic` type.
分析器可以推断字段、方法、局部变量和大多数泛型类型参数的类型。当分析器没有足够的信息来推断特定类型时，它会使用 `dynamic` 类型。

Here’s an example of how type inference works with generics. In this example, a variable named `arguments` holds a map that pairs string keys with values of various types.
以下是如何使用泛型进行类型推断的示例。在此示例中，名为 `arguments` 的变量保存了一个将字符串键与各种类型的值配对的地图。

If you explicitly type the variable, you might write this:
如果显式地键入变量，可以这样写：

```dart
Map<String, dynamic> arguments = {'argA': 'hello', 'argB': 42};
```

Alternatively, you can use `var` or `final` and let Dart infer the type:
或者，可以使用 `var` 或 `final` ，让 Dart 推断类型：

```dart
var arguments = {'argA': 'hello', 'argB': 42}; // Map<String, Object>
```

The map literal infers its type from its entries, and then the variable infers its type from the map literal’s type. In this map, the keys are both strings, but the values have different types (`String` and `int`, which have the upper bound `Object`). So the map literal has the type `Map<String, Object>`, and so does the `arguments` variable.
映射字面量从其条目中推断其类型，然后变量从映射字面量的类型中推断其类型。在此映射中，键都是字符串，但值具有不同的类型（ `String` 和 `int` ，它们的上限为 `Object` ）。因此映射字面量的类型为 `Map<String, Object>` ， `arguments` 变量的类型也是如此。

### Field and method inference 字段和方法推断

A field or method that has no specified type and that overrides a field or method from the superclass, inherits the type of the superclass method or field.
没有指定类型且覆盖了超类中的字段或方法的字段或方法，将继承超类方法或字段的类型。

A field that does not have a declared or inherited type but that is declared with an initial value, gets an inferred type based on the initial value.
一个没有声明或继承类型但已用初始值声明的字段，会根据初始值获取一个推断类型。

### Static field inference 静态字段推断

Static fields and variables get their types inferred from their initializer. Note that inference fails if it encounters a cycle (that is, inferring a type for the variable depends on knowing the type of that variable).
静态字段和变量的类型可从其初始化器推断出来。请注意，如果遇到循环（即推断变量的类型取决于知道该变量的类型），则推断将失败。

### Local variable inference 局部变量推断

Local variable types are inferred from their initializer, if any. Subsequent assignments are not taken into account. This may mean that too precise a type may be inferred. If so, you can add a type annotation.
局部变量类型可从其初始化器（如果有）推断出来。后续赋值不会被考虑在内。这可能意味着推断出的类型过于精确。如果是这样，您可以添加类型注释。

```dart
var x = 3; // x is inferred as an int.
x = 4.0;
num y = 3; // A num can be double or int.
y = 4.0;
```

### Type argument inference 类型参数推断

Type arguments to constructor calls and [generic method](https://dart.dev/language/generics#using-generic-methods) invocations are inferred based on a combination of downward information from the context of occurrence, and upward information from the arguments to the constructor or generic method. If inference is not doing what you want or expect, you can always explicitly specify the type arguments.
构造函数调用和泛型方法调用的类型参数是根据发生上下文的向下信息和构造函数或泛型方法的参数的向上信息组合推断出来的。如果推断没有达到您的要求或预期，您可以始终显式指定类型参数。

```dart
// Inferred as if you wrote <int>[].
List<int> listOfInt = [];

// Inferred as if you wrote <double>[3.0].
var listOfDouble = [3.0];

// Inferred as Iterable<int>.
var ints = listOfDouble.map((x) => x.toInt());
```

In the last example, `x` is inferred as `double` using downward information. The return type of the closure is inferred as `int` using upward information. Dart uses this return type as upward information when inferring the `map()` method’s type argument: `<int>`.
在最后一个示例中， `x` 使用向下信息推断为 `double` 。闭包的返回类型使用向上信息推断为 `int` 。Dart 在推断 `map()` 方法的类型参数 `<int>` 时使用此返回类型作为向上信息。

## Substituting types 替换类型

When you override a method, you are replacing something of one type (in the old method) with something that might have a new type (in the new method). Similarly, when you pass an argument to a function, you are replacing something that has one type (a parameter with a declared type) with something that has another type (the actual argument). When can you replace something that has one type with something that has a subtype or a supertype?
当您重写方法时，您正在用可能具有新类型（在新方法中）的东西替换某种类型（在旧方法中）的东西。同样，当您向函数传递参数时，您正在用具有另一种类型（实际参数）的东西替换具有某种类型（具有声明类型参数）的东西。什么时候可以将具有某种类型的东西替换为具有子类型或超类型的东西？

When substituting types, it helps to think in terms of *consumers* and *producers*. A consumer absorbs a type and a producer generates a type.
在替换类型时，可以从消费者和生产者的角度来考虑。消费者吸收类型，而生产者生成类型。

**You can replace a consumer’s type with a supertype and a producer’s type with a subtype.
您可以用超类型替换消费者的类型，用子类型替换生产者的类型。**

Let’s look at examples of simple type assignment and assignment with generic types.
我们来看一些简单类型分配和使用泛型类型的分配的示例。

### Simple type assignment 简单类型分配

When assigning objects to objects, when can you replace a type with a different type? The answer depends on whether the object is a consumer or a producer.
在将对象分配给对象时，什么时候可以将一种类型替换为另一种类型？答案取决于对象是消费者还是生产者。

Consider the following type hierarchy:
考虑以下类型层次结构：

![a hierarchy of animals where the supertype is Animal and the subtypes are Alligator, Cat, and HoneyBadger. Cat has the subtypes of Lion and MaineCoon](./Typesystem_img/type-hierarchy.png)

Consider the following simple assignment where `Cat c` is a *consumer* and `Cat()` is a *producer*:
考虑以下简单分配，其中 `Cat c` 是消费者， `Cat()` 是生产者：

```dart
Cat c = Cat();
```

In a consuming position, it’s safe to replace something that consumes a specific type (`Cat`) with something that consumes anything (`Animal`), so replacing `Cat c` with `Animal c` is allowed, because `Animal` is a supertype of `Cat`.
在消费位置，用可以消费任何内容的东西（ `Animal` ）替换消费特定类型（ `Cat` ）的东西是安全的，因此用 `Animal c` 替换 `Cat c` 是允许的，因为 `Animal` 是 `Cat` 的超类型。

```dart
Animal c = Cat();
```

But replacing `Cat c` with `MaineCoon c` breaks type safety, because the superclass may provide a type of Cat with different behaviors, such as `Lion`:
但是用 `MaineCoon c` 替换 `Cat c` 会破坏类型安全性，因为超类可能会提供具有不同行为的Cat类型，例如 `Lion` ：

```dart
MaineCoon c = Cat();
```

In a producing position, it’s safe to replace something that produces a type (`Cat`) with a more specific type (`MaineCoon`). So, the following is allowed:
在生产位置，用更具体的类型（ `MaineCoon` ）替换产生类型（ `Cat` ）的东西是安全的。因此，允许以下操作：

```dart
Cat c = MaineCoon();
```

### Generic type assignment 泛型类型分配

Are the rules the same for generic types? Yes. Consider the hierarchy of lists of animals—a `List` of `Cat` is a subtype of a `List` of `Animal`, and a supertype of a `List` of `MaineCoon`:
泛型类型的规则是否相同？是的。考虑动物列表的层次结构—— `List` 的 `Cat` 是 `List` 的 `Animal` 的子类型，并且是 `List` 的 `MaineCoon` 的超类型：

![List<Animal> -> List<Cat> -> List<MaineCoon>](./Typesystem_img/type-hierarchy-generics.png)

In the following example, you can assign a `MaineCoon` list to `myCats` because `List<MaineCoon>` is a subtype of `List<Cat>`:
在以下示例中，您可以将 `MaineCoon` 列表分配给 `myCats` ，因为 `List<MaineCoon>` 是 `List<Cat>` 的子类型：

```dart
List<MaineCoon> myMaineCoons = ...
List<Cat> myCats = myMaineCoons;
```

What about going in the other direction? Can you assign an `Animal` list to a `List<Cat>`?
反过来呢？您可以将 `Animal` 列表分配给 `List<Cat>` 吗？

```dart
List<Animal> myAnimals = ...
List<Cat> myCats = myAnimals;
```

This assignment doesn’t pass static analysis because it creates an implicit downcast, which is disallowed from non-`dynamic` types such as `Animal`.
此分配不会通过静态分析，因为它创建了一个隐式向下转换，而 `Animal` 等非 `dynamic` 类型不允许这样做。

To make this type of code pass static analysis, you can use an explicit cast.
要使此类代码通过静态分析，您可以使用显式强制转换。

```dart
List<Animal> myAnimals = ...
List<Cat> myCats = myAnimals as List<Cat>;
```

An explicit cast might still fail at runtime, though, depending on the actual type of the list being cast (`myAnimals`).
不过，显式强制转换仍可能在运行时失败，具体取决于要强制转换的列表的实际类型 ( `myAnimals` )。

### Methods 方法

When overriding a method, the producer and consumer rules still apply. For example:
重写方法时，生产者和消费者规则仍然适用。例如：

![Animal class showing the chase method as the consumer and the parent getter as the producer](./Typesystem_img/consumer-producer-methods.png)

For a consumer (such as the `chase(Animal)` method), you can replace the parameter type with a supertype. For a producer (such as the `parent` getter method), you can replace the return type with a subtype.
对于消费者（例如 `chase(Animal)` 方法），您可以将参数类型替换为超类型。对于生产者（例如 `parent` getter 方法），您可以将返回类型替换为子类型。

For more information, see [Use sound return types when overriding methods](https://dart.dev/language/type-system#use-proper-return-types) and [Use sound parameter types when overriding methods](https://dart.dev/language/type-system#use-proper-param-types).
有关更多信息，请参阅重写方法时使用合理的返回类型和重写方法时使用合理的参数类型。

## Other resources 其他资源

The following resources have further information on sound Dart:
以下资源提供了有关合理的 Dart 的更多信息：

- [Fixing common type problems](https://dart.dev/guides/language/sound-problems) - Errors you may encounter when writing sound Dart code, and how to fix them.
  修复常见类型问题 - 编写健全的 Dart 代码时可能遇到的错误，以及如何修复这些错误。
- [Fixing type promotion failures](https://dart.dev/tools/non-promotion-reasons) - Understand and learn how to fix type promotion errors.
  修复类型提升失败 - 了解并学习如何修复类型提升错误。
- [Sound null safety](https://dart.dev/null-safety) - Learn about writing code with sound null safety.
  健全的空安全 - 了解如何编写具有健全空安全的代码。
- [Customizing static analysis](https://dart.dev/tools/analysis) - How to set up and customize the analyzer and linter using an analysis options file.
  自定义静态分析 - 如何使用分析选项文件设置和自定义分析器和 linter。
