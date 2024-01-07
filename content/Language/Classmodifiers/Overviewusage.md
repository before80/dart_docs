+++
title = "类修饰符概览和用法"
date = 2024-01-05T20:29:36+08:00
weight = 1
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文: [https://dart.dev/language/class-modifiers](https://dart.dev/language/class-modifiers)

## Class modifiers 类修饰符

*merge_type* **Version note:** Class modifiers, besides `abstract`, require a [language version](https://dart.dev/guides/language/evolution#language-versioning) of at least 3.0.

​	版本说明：除了 `abstract` 之外，类修饰符需要至少 3.0 的语言版本。

Class modifiers control how a class or mixin can be used, both [from within its own library](https://dart.dev/language/class-modifiers#abstract), and from outside of the library where it’s defined.

​	类修饰符控制类或 mixin 的使用方式，既可以从其自身库中使用，也可以从定义它的库外部使用。

Modifier keywords come before a class or mixin declaration. For example, writing `abstract class` defines an abstract class. The full set of modifiers that can appear before a class declaration include:

​	修饰符关键字位于类或 mixin 声明之前。例如，编写 `abstract class` 定义一个抽象类。可以在类声明之前出现的修饰符的完整集合包括：

- `abstract`
- `base`
- `final`
- `interface`
- `sealed`
- [`mixin`](https://dart.dev/language/mixins#class-mixin-or-mixin-class)

Only the `base` modifier can appear before a mixin declaration. The modifiers do not apply to other declarations like `enum`, `typedef`, or `extension`.

​	只有 `base` 修饰符可以出现在 mixin 声明之前。这些修饰符不适用于其他声明，如 `enum` 、 `typedef` 或 `extension` 。

When deciding whether to use class modifiers, consider the intended uses of the class, and what behaviors the class needs to be able to rely on.

​	在决定是否使用类修饰符时，请考虑类的预期用途，以及类需要能够依赖哪些行为。

*info* **Note:** If you maintain a library, read the [Class modifiers for API maintainers](https://dart.dev/language/class-modifiers-for-apis) page for guidance on how to navigate these changes for your libraries.

​	注意：如果您维护一个库，请阅读面向 API 维护者的类修饰符页面，以获取有关如何为您的库调整这些更改的指导。

## 无修饰符 No modifier 

To allow unrestricted permission to construct or subtype from any library, use a `class` or `mixin` declaration without a modifier. By default, you can:

​	要允许从任何库进行构造或子类型化的无限制权限，请使用不带修饰符的 `class` 或 `mixin` 声明。默认情况下，您可以：

- [Construct](https://dart.dev/language/constructors) new instances of a class.
- 构造类的实例。
- [Extend](https://dart.dev/language/extend) a class to create a new subtype.
- 扩展一个类以创建一个新的子类型。
- [Implement](https://dart.dev/language/classes#implicit-interfaces) a class or mixin’s interface.
- 实现一个类或 mixin 的接口。
- [Mix in](https://dart.dev/language/mixins) a mixin or mixin class.
- 混合一个 mixin 或 mixin 类。

## `abstract`

To define a class that doesn’t require a full, concrete implementation of its entire interface, use the `abstract` modifier.

​	要定义一个不需要其整个接口的完整具体实现的类，请使用 `abstract` 修饰符。

Abstract classes cannot be constructed from any library, whether its own or an outside library. Abstract classes often have [abstract methods](https://dart.dev/language/methods#abstract-methods).

​	抽象类无法从任何库（无论是其自身库还是外部库）构建。抽象类通常具有抽象方法。

```dart
// Library a.dart
abstract class Vehicle {
  void moveForward(int meters);
}
// Library b.dart
import 'a.dart';

// Error: Cannot be constructed
Vehicle myVehicle = Vehicle();

// Can be extended
class Car extends Vehicle {
  int passengers = 4;
  // ···
}

// Can be implemented
class MockVehicle implements Vehicle {
  @override
  void moveForward(int meters) {
    // ...
  }
}
```

If you want your abstract class to appear to be instantiable, define a [factory constructor](https://dart.dev/language/constructors#factory-constructors).

​	如果您希望您的抽象类看起来可实例化，请定义一个工厂构造函数。

## `base`

To enforce inheritance of a class or mixin’s implementation, use the `base` modifier. A base class disallows implementation outside of its own library. This guarantees:

​	要强制执行类或 mixin 的实现的继承，请使用 `base` 修饰符。基类不允许在其自身库之外进行实现。这可确保：

- The base class constructor is called whenever an instance of a subtype of the class is created.
- 每当创建类子类型的实例时，都会调用基类构造函数。
- All implemented private members exist in subtypes.
- 所有已实现的私有成员都存在于子类型中。
- A new implemented member in a `base` class does not break subtypes, since all subtypes inherit the new member.
- `base` 类中的新实现成员不会破坏子类型，因为所有子类型都会继承新成员。
  - This is true unless the subtype already declares a member with the same name and an incompatible signature.
  - 除非子类型已经声明了一个具有相同名称和不兼容签名的成员，否则这是正确的。

You must mark any class which implements or extends a base class as `base`, `final`, or `sealed`. This prevents outside libraries from breaking the base class guarantees.

​	您必须将实现或扩展基类的任何类标记为 `base` 、 `final` 或 `sealed` 。这可以防止外部库破坏基类保证。

```dart
// Library a.dart
base class Vehicle {
  void moveForward(int meters) {
    // ...
  }
}
// Library b.dart
import 'a.dart';

// Can be constructed
Vehicle myVehicle = Vehicle();

// Can be extended
base class Car extends Vehicle {
  int passengers = 4;
  // ...
}

// ERROR: Cannot be implemented
base class MockVehicle implements Vehicle {
  @override
  void moveForward() {
    // ...
  }
}
```

## `interface`

To define an interface, use the `interface` modifier. Libraries outside of the interface’s own defining library can implement the interface, but not extend it. This guarantees:

​	要定义接口，请使用 `interface` 修饰符。接口定义库之外的库可以实现该接口，但不能扩展它。这保证了：

- When one of the class’s instance methods calls another instance method on `this`, it will always invoke a known implementation of the method from the same library.
- 当类的实例方法之一在 `this` 上调用另一个实例方法时，它将始终从同一库中调用该方法的已知实现。
- Other libraries can’t override methods that the interface class’s own methods might later call in unexpected ways. This reduces the [fragile base class problem](https://en.wikipedia.org/wiki/Fragile_base_class).
- 其他库无法覆盖接口类自己的方法可能以意外方式调用的方法。这减少了脆弱的基类问题。

```dart
// Library a.dart
interface class Vehicle {
  void moveForward(int meters) {
    // ...
  }
}
// Library b.dart
import 'a.dart';

// Can be constructed
Vehicle myVehicle = Vehicle();

// ERROR: Cannot be inherited
class Car extends Vehicle {
  int passengers = 4;
  // ...
}

// Can be implemented
class MockVehicle implements Vehicle {
  @override
  void moveForward(int meters) {
    // ...
  }
}
```

### `abstract interface`

The most common use for the `interface` modifier is to define a pure interface. [Combine](https://dart.dev/language/class-modifiers#combining-modifiers) the `interface` and [`abstract`](https://dart.dev/language/class-modifiers#abstract) modifiers for an `abstract interface class`.

​	`interface` 修饰符最常见的用途是定义纯接口。将 `interface` 和 `abstract` 修饰符组合用于 `abstract interface class` 。

Like an `interface` class, other libraries can implement, but cannot inherit, a pure interface. Like an `abstract` class, a pure interface can have abstract members.

​	与 `interface` 类一样，其他库可以实现纯接口，但不能继承纯接口。与 `abstract` 类一样，纯接口可以具有抽象成员。

## `final`

To close the type hierarchy, use the `final` modifier. This prevents subtyping from a class outside of the current library. Disallowing both inheritance and implementation prevents subtyping entirely. This guarantees:

​	要关闭类型层次结构，请使用 `final` 修饰符。这可防止从当前库外部的类进行子类型化。禁止继承和实现可完全防止子类型化。这可确保：

- You can safely add incremental changes to the API.
- 您可以安全地对 API 进行增量更改。
- You can call instance methods knowing that they haven’t been overwritten in a third-party subclass.
- 您可以调用实例方法，因为您知道它们尚未在第三方子类中被覆盖。

Final classes can be extended or implemented within the same library. The `final` modifier encompasses the effects of `base`, and therefore any subclasses must also be marked `base`, `final`, or `sealed`.

​	最终类可以在同一个库中进行扩展或实现。 `final` 修饰符包含 `base` 的效果，因此任何子类也必须标记为 `base` 、 `final` 或 `sealed` 。

```dart
// Library a.dart
final class Vehicle {
  void moveForward(int meters) {
    // ...
  }
}
// Library b.dart
import 'a.dart';

// Can be constructed
Vehicle myVehicle = Vehicle();

// ERROR: Cannot be inherited
class Car extends Vehicle {
  int passengers = 4;
  // ...
}

class MockVehicle implements Vehicle {
  // ERROR: Cannot be implemented
  @override
  void moveForward(int meters) {
    // ...
  }
}
```

## `sealed`

To create a known, enumerable set of subtypes, use the `sealed` modifier. This allows you to create a switch over those subtypes that is statically ensured to be [*exhaustive*](https://dart.dev/language/branches#exhaustiveness-checking).

​	要创建已知的、可枚举的子类型集，请使用 `sealed` 修饰符。这允许您对这些子类型创建静态确保详尽无遗的 switch。

The `sealed` modifier prevents a class from being extended or implemented outside its own library. Sealed classes are implicitly [abstract](https://dart.dev/language/class-modifiers#abstract).

​	`sealed` 修饰符可防止类在其自身库外部被扩展或实现。密封类是隐式抽象的。

- They cannot be constructed themselves.
- 它们不能自行构造。
- They can have [factory constructors](https://dart.dev/language/constructors#factory-constructors).
- 它们可以有工厂构造函数。
- They can define constructors for their subclasses to use.
- 它们可以定义供其子类使用的构造函数。

Subclasses of sealed classes are, however, not implicitly abstract.

​	但是，密封类的子类不是隐式抽象的。

The compiler is aware of any possible direct subtypes because they can only exist in the same library. This allows the compiler to alert you when a switch does not exhaustively handle all possible subtypes in its cases:

​	编译器知道任何可能的直接子类型，因为它们只能存在于同一个库中。这允许编译器在 switch 没有在其 case 中详尽处理所有可能的子类型时提醒您：

```dart
sealed class Vehicle {}

class Car extends Vehicle {}

class Truck implements Vehicle {}

class Bicycle extends Vehicle {}

// ERROR: Cannot be instantiated
Vehicle myVehicle = Vehicle();

// Subclasses can be instantiated
Vehicle myCar = Car();

String getVehicleSound(Vehicle vehicle) {
  // ERROR: The switch is missing the Bicycle subtype or a default case.
  return switch (vehicle) {
    Car() => 'vroom',
    Truck() => 'VROOOOMM',
  };
}
```

If you don’t want [exhaustive switching](https://dart.dev/language/branches#exhaustiveness-checking), or want to be able to add subtypes later without breaking the API, use the [`final`](https://dart.dev/language/class-modifiers#final) modifier. For a more in depth comparison, read [`sealed` versus `final`](https://dart.dev/language/class-modifiers-for-apis#sealed-versus-final).

​	如果您不想要详尽的切换，或者想要能够在不破坏 API 的情况下稍后添加子类型，请使用 `final` 修饰符。有关更深入的比较，请阅读 `sealed` 与 `final` 。

## 组合修饰符 Combining modifiers 

You can combine some modifiers for layered restrictions. A class declaration can be, in order:

​	您可以组合一些修饰符以实现分层限制。类声明可以按顺序为：

1. (Optional) `abstract`, describing whether the class can contain abstract members and prevents instantiation.
2. (可选) `abstract` ，描述类是否可以包含抽象成员并防止实例化。
3. (Optional) One of `base`, `interface`, `final` or `sealed`, describing restrictions on other libraries subtyping the class.
4. (可选) `base` 、 `interface` 、 `final` 或 `sealed` 之一，描述对其他库对类进行子类型化的限制。
5. (Optional) `mixin`, describing whether the declaration can be mixed in.
6. (可选) `mixin` ，描述声明是否可以混合使用。
7. The `class` keyword itself.
8. `class` 关键字本身。

You can’t combine some modifiers because they are contradictory, redundant, or otherwise mutually exclusive:

​	您不能组合一些修饰符，因为它们是矛盾的、冗余的或以其他方式相互排斥的：

- `abstract` with `sealed`. A [sealed](https://dart.dev/language/class-modifiers#sealed) class is always implicitly [abstract](https://dart.dev/language/class-modifiers#abstract).
- `abstract` 与 `sealed` 。密封类始终是隐式抽象的。
- `interface`, `final` or `sealed` with `mixin`. These access modifiers prevent [mixing in](https://dart.dev/language/mixins).
- `interface` 、 `final` 或 `sealed` 与 `mixin` 。这些访问修饰符可防止混入。

See the [Class modifiers reference](https://dart.dev/language/modifier-reference) for complete guidance.

​	有关完整指南，请参阅类修饰符参考。
