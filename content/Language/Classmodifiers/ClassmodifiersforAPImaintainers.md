+++
title = "面向 API 维护者的类修饰符"
date = 2024-01-05T20:29:36+08:00
weight = 10
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文: [https://dart.dev/language/class-modifiers-for-apis](https://dart.dev/language/class-modifiers-for-apis)

## Class modifiers for API maintainers 面向 API 维护者的类修饰符

Dart 3.0 adds a few [new modifiers](https://dart.dev/language/class-modifiers) that you can place on class and [mixin declarations](https://dart.dev/language/mixins). If you are the author of a library package, these modifiers give you more control over what users are allowed to do with the types that your package exports. This can make it easier to evolve your package, and easier to know if a change to your code may break users.

​	Dart 3.0 添加了一些新的修饰符，您可以将它们放在类和 mixin 声明上。如果您是库包的作者，这些修饰符可以让您更好地控制用户可以使用包导出的类型执行的操作。这可以使您更容易地演进包，并且更容易知道对代码的更改是否会破坏用户。

Dart 3.0 also includes a [breaking change](https://dart.dev/resources/dart-3-migration#mixin) around using classes as mixins. This change might not break *your* class, but it could break *users* of your class.

​	Dart 3.0 还包括一个有关将类用作 mixin 的重大更改。此更改可能不会破坏您的类，但可能会破坏您类的用户。

This guide walks you through these changes so you know how to use the new modifiers, and how they affect users of your libraries.

​	本指南将引导您完成这些更改，以便您知道如何使用新修饰符，以及它们如何影响库的用户。

## 类上的 `mixin` 修饰符 The `mixin` modifier on classes 

The most important modifier to be aware of is `mixin`. Language versions prior to Dart 3.0 allow any class to be used as a mixin in another class’s `with` clause, *UNLESS* the class:

​	最需要注意的修饰符是 `mixin` 。Dart 3.0 之前的语言版本允许在另一个类的 `with` 子句中将任何类用作 mixin，除非该类：

- Declares any non-factory constructors.
- 声明任何非工厂构造函数。
- Extends any class other than `Object`.
- 扩展 `Object` 以外的任何类。

This makes it easy to accidentally break someone else’s code, by adding a constructor or `extends` clause to a class without realizing that others are using it in a `with` clause.

​	这使得很容易意外破坏其他人的代码，方法是在类中添加构造函数或 `extends` 子句，而没有意识到其他人正在 `with` 子句中使用它。

Dart 3.0 no longer allows classes to be used as mixins by default. Instead, you must explicitly opt-in to that behavior by declaring a `mixin class`:

​	Dart 3.0 不再允许默认情况下将类用作 mixin。相反，您必须通过声明 `mixin class` :

```
mixin class Both {}

class UseAsMixin with Both {}
class UseAsSuperclass extends Both {}
```

If you update your package to Dart 3.0 and don’t change any of your code, you may not see any errors. But you may inadvertently break users of your package if they were using your classes as mixins.

​	显式选择加入该行为。如果您将软件包更新到 Dart 3.0 并且不更改任何代码，则可能看不到任何错误。但是，如果用户将您的类用作 mixin，则您可能会无意中破坏软件包的用户。

### 将类迁移为 mixin - Migrating classes as mixins 

If the class has a non-factory constructor, an `extends` clause, or a `with` clause, then it already can’t be used as a mixin. Behavior won’t change with Dart 3.0; there’s nothing to worry about and nothing you need to do.

​	如果类具有非工厂构造函数、 `extends` 子句或 `with` 子句，则它已经无法用作 mixin。行为不会因 Dart 3.0 而改变；无需担心，也无需执行任何操作。

In practice, this describes about 90% of existing classes. For the remaining classes that can be used as mixins, you have to decide what you want to support.

​	实际上，这描述了大约 90% 的现有类。对于可以作为 mixin 使用的其余类，您必须决定要支持什么。

Here are a few questions to help decide. The first is pragmatic:

​	以下是一些有助于做出决定的问题。第一个是务实的：

- **Do you want to risk breaking any users?** If the answer is a hard “no”, then place `mixin` before any and all classes that [could be used as a mixin](https://dart.dev/language/class-modifiers-for-apis#the-mixin-modifier-on-classes). This exactly preserves the existing behavior of your API.
- 您是否愿意冒着破坏任何用户的风险？如果答案是坚决的“不”，那么请在任何可以作为 mixin 使用的类之前放置 `mixin` 。这完全保留了 API 的现有行为。

On the other hand, if you want to take this opportunity to rethink the affordances your API offers, then you may want to *not* turn it into a `mixin class`. Consider these two design questions:

​	另一方面，如果您想借此机会重新考虑您的 API 提供的功能，那么您可能不想将其变成 `mixin class` 。考虑以下两个设计问题：

- **Do you want users to be able to construct instances of it directly?** In other words, is the class deliberately not abstract?
- 您是否希望用户能够直接构建它的实例？换句话说，该类是否故意不是抽象的？
- **Do you want people to be able to use the declaration as a mixin?** In other words, do you want them to be able to use it in `with` clauses?
- 您是否希望人们能够将声明用作 mixin？换句话说，您是否希望他们能够在 `with` 子句中使用它？

If the answer to both is “yes”, then make it a mixin class. If the answer to the second is “no”, then just leave it as a class. If the answer to the first is “no” and the second is “yes”, then change it from a class to a mixin declaration.

​	如果这两个问题的答案都是“是”，那么就将其设为 mixin 类。如果第二个问题的答案是“否”，那么就将其保留为类。如果第一个问题的答案是“否”，而第二个问题的答案是“是”，那么就将其从类更改为 mixin 声明。

The last two options, leaving it a class or turning it into a pure mixin, are breaking API changes. You’ll want to bump the major version of your package if you do this.

​	最后两个选项，保留为类或将其变成纯 mixin，都是破坏性 API 更改。如果您这样做，您需要提升软件包的主要版本。

## 其他选择加入修饰符 Other opt-in modifiers 

Handling classes as mixins is the only critical change in Dart 3.0 that affects the API of your package. Once you’ve gotten this far, you can stop if you don’t want to make other changes to what your package allows users to do.

​	将类作为 mixin 处理是 Dart 3.0 中唯一影响软件包 API 的关键更改。一旦您完成此步骤，如果您不想对软件包允许用户执行的操作进行其他更改，则可以停止。

Note that if you do continue and use any of the modifiers described below, it is potentially a breaking change to your package’s API which necessitates a major version increment.

​	请注意，如果您继续使用下面描述的任何修饰符，则可能会破坏软件包的 API，从而需要增加主要版本号。

## 修饰符 `interface` The `interface` modifier 

Dart doesn’t have a separate syntax for declaring pure interfaces. Instead, you declare an abstract class that happens to contain only abstract methods. When a user sees that class in your package’s API, they may not know if it contains code they can reuse by extending the class, or whether it is instead meant to be used as an interface.

​	Dart 没有单独的语法来声明纯接口。相反，您可以声明一个恰好只包含抽象方法的抽象类。当用户在软件包的 API 中看到该类时，他们可能不知道该类是否包含可通过扩展该类来重用的代码，或者该类是否应该用作接口。

You can clarify that by putting the [`interface`](https://dart.dev/language/class-modifiers#interface) modifier on the class. That allows the class to be used in an `implements` clause, but prevents it from being used in `extends`.

​	您可以通过在类上放置 `interface` 修饰符来澄清这一点。这允许在 `implements` 子句中使用该类，但阻止在 `extends` 中使用该类。

Even when the class *does* have non-abstract methods, you may want to prevent users from extending it. Inheritance is one of the most powerful kinds of coupling in software, because it enables code reuse. But that coupling is also [dangerous and fragile](https://en.wikipedia.org/wiki/Fragile_base_class). When inheritance crosses package boundaries, it can be hard to evolve the superclass without breaking subclasses.

​	即使该类确实具有非抽象方法，您可能仍希望阻止用户扩展它。继承是软件中最强大的耦合类型之一，因为它支持代码重用。但这种耦合也很危险且脆弱。当继承跨越软件包边界时，很难在不破坏子类的情况下演化超类。

Marking the class `interface` lets users construct it (unless it’s [also marked `abstract`](https://dart.dev/language/class-modifiers#abstract-interface)) and implement the class’s interface, but prevents them from reusing any of its code.

​	标记类 `interface` 允许用户构建它（除非它也标记为 `abstract` ）并实现类的接口，但阻止他们重用其任何代码。

When a class is marked `interface`, the restriction can be ignored within the library where the class is declared. Inside the library, you’re free to extend it since it’s all your code and presumably you know what you’re doing. The restriction applies to other packages, and even other libraries within your own package.

​	当类标记为 `interface` 时，可以在声明类的库中忽略该限制。在库中，您可以自由地扩展它，因为它是您的所有代码，并且您可能知道自己在做什么。该限制适用于其他软件包，甚至适用于您自己的软件包中的其他库。

## 修饰符 `base` The `base` modifier 

The [`base`](https://dart.dev/language/class-modifiers#base) modifier is somewhat the opposite of `interface`. It allows you to use the class in an `extends` clause, or use a mixin or mixin class in a `with` clause. But, it disallows code outside of the class’s library from using the class or mixin in an `implements` clause.

​	`base` 修饰符与 `interface` 有些相反。它允许您在 `extends` 子句中使用该类，或在 `with` 子句中使用 mixin 或 mixin 类。但是，它不允许类库外部的代码在 `implements` 子句中使用该类或 mixin。

This ensures that every object that is an instance of your class or mixin’s interface inherits your actual implementation. In particular, this means that every instance will include all of the private members your class or mixin declares. This can help prevent runtime errors that might otherwise occur.

​	这可确保作为您的类或 mixin 接口的实例的每个对象都会继承您的实际实现。特别是，这意味着每个实例都将包含您的类或 mixin 声明的所有私有成员。这有助于防止可能发生的运行时错误。

Consider this library:

​	考虑以下库：

```
// a.dart
class A {
  void _privateMethod() {
    print('I inherited from A');
  }
}

void callPrivateMethod(A a) {
  a._privateMethod();
}
```

This code seems fine on its own, but there’s nothing preventing a user from creating another library like this:

​	这段代码本身看起来很好，但没有任何东西可以阻止用户创建另一个这样的库：

```
// b.dart
import 'a.dart';

class B implements A {
  // No implementation of _privateMethod()!
}

main() {
  callPrivateMethod(B()); // Runtime exception!
}
```

Adding the `base` modifier to the class can help prevent these runtime errors. As with `interface`, you can ignore this restriction in the same library where the `base` class or mixin is declared. Then subclasses in the same library will be reminded to implement the private methods. But note that the next section *does* apply:

​	向类添加 `base` 修饰符可以帮助防止这些运行时错误。与 `interface` 一样，您可以在声明 `base` 类或 mixin 的同一库中忽略此限制。然后，同一库中的子类将被提醒实现私有方法。但请注意，下一节确实适用：

### 基类传递性 Base transitivity 

The goal of marking a class `base` is to ensure that every instance of that type concretely inherits from it. To maintain this, the base restriction is “contagious”. Every subtype of a type marked `base` – *direct or indirect* – must also prevent being implemented. That means it must be marked `base` (or `final` or `sealed`, which we’ll get to next).

​	标记类 `base` 的目标是确保该类型的每个实例都具体继承自它。为了保持这一点，基类限制是“具有传染性”。标记为 `base` 的类型的每个子类型（直接或间接）也必须防止被实现。这意味着它必须标记为 `base` （或 `final` 或 `sealed` ，我们接下来会介绍）。

Applying `base` to a type requires some care, then. It affects not just what users can do with your class or mixin, but also the affordances *their* subclasses can offer. Once you’ve put `base` on a type, the whole hierarchy under it is prohibited from being implemented.

​	因此，将 `base` 应用于类型需要谨慎。它不仅影响用户可以使用您的类或 mixin 做什么，还影响其子类可以提供的功能。一旦您在类型上放置了 `base` ，则禁止在其之下的整个层次结构被实现。

That sounds intense, but it’s how most other programming languages have always worked. Most don’t have implicit interfaces at all, so when you declare a class in Java, C#, or other languages, you effectively have the same constraint.

​	听起来很复杂，但这是大多数其他编程语言一直以来的工作方式。大多数语言根本没有隐式接口，因此当你在 Java、C# 或其他语言中声明一个类时，实际上就有相同的约束。

## 修饰符 `final` - The `final` modifier 

If you want all of the restrictions of both `interface` and `base`, you can mark a class or mixin class [`final`](https://dart.dev/language/class-modifiers#final). This prevents anyone outside of your library from creating any kind of subtype of it: no using it in `implements`, `extends`, `with`, or `on` clauses.

​	如果你想要 `interface` 和 `base` 的所有限制，你可以标记一个类或 mixin 类 `final` 。这可以防止库外部的任何人创建它的任何子类型：不能在 `implements` 、 `extends` 、 `with` 或 `on` 子句中使用它。

This is the most restrictive for users of the class. All they can do is construct it (unless it’s marked `abstract`). In return, you have the fewest restrictions as the class maintainer. You can add new methods, turn constructors into factory constructors, etc. without worrying about breaking any downstream users.

​	这是对类用户最具限制性的。他们能做的就是构造它（除非它被标记为 `abstract` ）。作为回报，作为类维护者，你受到的限制最少。你可以添加新方法，将构造函数变成工厂构造函数等，而不用担心破坏任何下游用户。

## 修饰符 `sealed` - The `sealed` modifer 

The last modifier, [`sealed`](https://dart.dev/language/class-modifiers#sealed), is special. It exists primarily to enable [exhaustiveness checking](https://dart.dev/language/branches#exhaustiveness-checking) in pattern matching. If a switch has cases for every direct subtype of a type marked `sealed`, then the compiler knows the switch is exhaustive.

​	最后一个修饰符 `sealed` 是特殊的。它的主要作用是启用模式匹配中的穷举性检查。如果一个 switch 对标记为 `sealed` 的类型的每个直接子类型都有 case，那么编译器就知道这个 switch 是穷举性的。

```
// amigos.dart
sealed class Amigo {}
class Lucky extends Amigo {}
class Dusty extends Amigo {}
class Ned extends Amigo {}

String lastName(Amigo amigo) =>
    switch (amigo) {
      case Lucky _ => 'Day';
      case Dusty _ => 'Bottoms';
      case Ned _   => 'Nederlander';
    }
```

This switch has a case for each of the subtypes of `Amigo`. The compiler knows that every instance of `Amigo` must be an instance of one of those subtypes, so it knows the switch is safely exhaustive and doesn’t require any final default case.

​	此开关为 `Amigo` 的每个子类型提供了一个情况。编译器知道 `Amigo` 的每个实例都必须是其中一个子类型的实例，因此它知道开关是安全穷举的，不需要任何最终的默认情况。

For this to be sound, the compiler enforces two restrictions:

​	为了保证这一点，编译器强制执行两个限制：

1. The sealed class can’t itself be directly constructible. Otherwise, you could have an instance of `Amigo` that isn’t an instance of *any* of the subtypes. So every `sealed` class is implicitly `abstract` too.
2. 密封类本身不能直接构造。否则，您可能会遇到不是任何子类型的 `Amigo` 实例。因此，每个 `sealed` 类隐式地也是 `abstract` 。
3. Every direct subtype of the sealed type must be in the same library where the sealed type is declared. This way, the compiler can find them all. It knows that there aren’t other hidden subtypes floating around that would not match any of the cases.
4. 密封类型的每个直接子类型必须位于声明密封类型的同一个库中。这样，编译器可以找到它们。它知道没有其他隐藏的子类型漂浮在周围，这些子类型与任何情况都不匹配。

The second restriction is similar to `final`. Like `final`, it means that a class marked `sealed` can’t be directly extended, implemented, or mixed in outside of the library where it’s declared. But, unlike `base` and `final`, there is no *transitive* restriction:

​	第二个限制类似于 `final` 。与 `final` 一样，这意味着标记为 `sealed` 的类不能在声明它的库之外直接扩展、实现或混合。但是，与 `base` 和 `final` 不同，没有传递限制：

```
// amigo.dart
sealed class Amigo {}
class Lucky extends Amigo {}
class Dusty extends Amigo {}
class Ned extends Amigo {}

// other.dart

// This is an error:
class Bad extends Amigo {}

// But these are both fine:
class OtherLucky extends Lucky {}
class OtherDusty implements Dusty {}
```

Of course, if you *want* the subtypes of your sealed type to be restricted as well, you can get that by marking them using `interface`, `base`, `final`, or `sealed`.

​	当然，如果您希望密封类型的子类型也受到限制，您可以使用 `interface` 、 `base` 、 `final` 或 `sealed` 标记它们来实现。

### `sealed` 与 `final`

If you have a class that you don’t want users to be able to directly subtype, when should you use `sealed` versus `final`? A couple of simple rules:

​	如果您有一个不想让用户直接进行子类型化的类，那么您应该何时使用 `sealed` 与 `final` ？以下是一些简单的规则：

- If you want users to be able to directly construct instances of the class, then it *can’t* use `sealed` since sealed types are implicitly abstract.
- 如果您希望用户能够直接构造类的实例，那么它不能使用 `sealed` ，因为密封类型是隐式抽象的。
- If the class has no subtypes in your library, then there’s no point in using `sealed` since you get no exhaustiveness checking benefits.
- 如果类在您的库中没有子类型，那么使用 `sealed` 毫无意义，因为您不会获得任何穷举性检查的好处。

Otherwise, if the class does have some subtypes that you define, then `sealed` is likely what you want. If users see that the class has a few subtypes, it’s handy to be able to handle each of them separately as switch cases and have the compiler know that the entire type is covered.

​	否则，如果类确实有一些您定义的子类型，那么 `sealed` 可能就是您想要的。如果用户看到该类有一些子类型，那么能够将它们中的每一个都分别作为 switch 用例来处理并让编译器知道整个类型都已涵盖，这会非常方便。

Using `sealed` does mean that if you later add another subtype to the library, it’s a breaking API change. When a new subtype appears, all of those existing switches become non-exhaustive since they don’t handle the new type. It’s exactly like adding a new value to an enum.

​	使用 `sealed` 确实意味着如果您稍后向库中添加另一个子类型，这将是一个破坏性 API 更改。当出现新的子类型时，所有那些现有的 switch 都会变成非穷举性的，因为它们不处理新类型。这与向枚举添加新值完全一样。

Those non-exhaustive switch compile errors are *useful* to users because they draw the user’s attention to places in their code where they’ll need to handle the new type.

​	这些非详尽的 switch 编译错误对用户很有用，因为它们会引起用户注意他们代码中需要处理新类型的位置。

But it does mean that whenever you add a new subtype, it’s a breaking change. If you want the freedom to add new subtypes in a non-breaking way, then it’s better to mark the supertype using `final` instead of `sealed`. That means that when a user switches on a value of that supertype, even if they have cases for all of the subtypes, the compiler will force them to add another default case. That default case will then be what is executed if you add more subtypes later.

​	但这确实意味着每当您添加一个新子类型时，它就是一个重大更改。如果您希望以非重大更改的方式添加新子类型，那么最好使用 `final` 而不是 `sealed` 来标记超类型。这意味着当用户切换该超类型的值时，即使他们为所有子类型都有 case，编译器也会强制他们添加另一个 default case。如果您稍后添加更多子类型，那么将执行该 default case。

## 总结 Summary

As an API designer, these new modifiers give you control over how users work with your code, and conversely how you are able to evolve your code without breaking theirs.

​	作为 API 设计师，这些新修饰符使您可以控制用户如何使用您的代码，反之亦然，您可以在不破坏用户代码的情况下发展自己的代码。

But these options carry complexity with them: you now have more choices to make as an API designer. Also, since these features are new, we still don’t know what the best practices will be. Every language’s ecosystem is different and has different needs.

​	但这些选项带来了复杂性：您现在作为 API 设计师需要做出更多选择。此外，由于这些功能是新的，我们仍然不知道最佳实践是什么。每种语言的生态系统都不同，并且有不同的需求。

Fortunately, you don’t need to figure it out all at once. We chose the defaults deliberately so that even if you do nothing, your classes mostly have the same affordances they had before 3.0. If you just want to keep your API the way it was, put `mixin` on the classes that already supported that, and you’re done.

​	幸运的是，您不必一次性弄清楚所有内容。我们慎重地选择了默认值，因此即使您什么都不做，您的类也大多具有与 3.0 之前相同的可供性。如果您只想保持 API 原样，请将 `mixin` 放在已经支持它的类上，然后您就完成了。

Over time, as you get a sense of where you want finer control, you can consider applying some of the other modifiers:

​	随着时间的推移，当您了解到想要精细控制的地方时，您可以考虑应用其他一些修饰符：

- Use `interface` to prevent users from reusing your class’s code while allowing them to re-implement its interface.
- 使用 `interface` 来阻止用户重用您的类的代码，同时允许他们重新实现其接口。
- Use `base` to require users to reuse your class’s code and ensure every instance of your class’s type is an instance of that actual class or a subclass.
- 使用 `base` 来要求用户重用您的类的代码，并确保您的类的类型的每个实例都是该实际类或子类的实例。
- Use `final` to completely prevent a class from being extended.
- 使用 `final` 来完全阻止类被扩展。
- Use `sealed` to opt in to exhaustiveness checking on a family of subtypes.
- 使用 `sealed` 来选择对子类型的系列进行详尽性检查。

When you do, increment the major version when publishing your package, since these modifiers all imply restrictions that are breaking changes.

​	执行此操作时，在发布您的软件包时增加主要版本，因为这些修饰符都暗示了限制，即重大更改。
