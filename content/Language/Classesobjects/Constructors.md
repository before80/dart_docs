+++
title = "构造函数"
date = 2024-01-05T20:29:36+08:00
weight = 10
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文: [https://dart.dev/language/constructors](https://dart.dev/language/constructors)

## Constructors 构造函数

Declare a constructor by creating a function with the same name as its class (plus, optionally, an additional identifier as described in [Named constructors](https://dart.dev/language/constructors#named-constructors)).

​	通过创建一个与类同名的函数（另外，还可以选择一个其他标识符，如命名构造函数中所述）来声明构造函数。

Use the most common constructor, the generative constructor, to create a new instance of a class, and [initializing formal parameters](https://dart.dev/language/constructors#initializing-formal-parameters) to instantiate any instance variables, if necessary:

​	使用最常见的构造函数（生成构造函数）来创建类的实例，并在必要时初始化形式参数以实例化任何实例变量：

```dart
class Point {
  double x = 0;
  double y = 0;

  // Generative constructor with initializing formal parameters:
  Point(this.x, this.y);
}
```

The `this` keyword refers to the current instance.

​	关键字 `this` 指的是当前实例。

*info* **Note:** Use `this` only when there is a name conflict. Otherwise, Dart style omits the `this`.

​	注意：仅在存在名称冲突时才使用 `this` 。否则，Dart 样式会省略 `this` 。

## 初始化形式参数 Initializing formal parameters 

Dart has *initializing formal parameters* to simplify the common pattern of assigning a constructor argument to an instance variable. Use `this.propertyName` directly in the constructor declaration, and omit the body.

​	Dart 具有初始化形式参数，可简化将构造函数参数分配给实例变量的常见模式。在构造函数声明中直接使用 `this.propertyName` ，并省略主体。

Initializing parameters also allow you to initialize non-nullable or `final` instance variables, which both must be initialized or provided a default value:

​	初始化参数还允许您初始化不可为空或 `final` 实例变量，这两个变量都必须初始化或提供默认值：

```dart
class Point {
  final double x;
  final double y;

  Point(this.x, this.y);
  // Sets the x and y instance variables
  // before the constructor body runs.
}
```

The variables introduced by the initializing formals are implicitly final and only in scope of the [initializer list](https://dart.dev/language/constructors#initializer-list).

​	初始化形式引入的变量隐式为 final，并且仅在初始化程序列表的范围内。

If you need to perform some logic that cannot be expressed in the initializer list, create a [factory constructor](https://dart.dev/language/constructors#factory-constructors) (or [static method](https://dart.dev/language/classes#static-methods)) with that logic and then pass the computed values to a normal constructor.

​	如果您需要执行无法在初始化程序列表中表达的某些逻辑，请使用该逻辑创建一个工厂构造函数（或静态方法），然后将计算出的值传递给普通构造函数。

## 默认构造函数 Default constructors 

If you don’t declare a constructor, a default constructor is provided for you. The default constructor has no arguments and invokes the no-argument constructor in the superclass.

​	如果您未声明构造函数，则会为您提供一个默认构造函数。默认构造函数没有参数，并且在超类中调用无参数构造函数。

## 构造函数不会被继承 Constructors aren’t inherited 

Subclasses don’t inherit constructors from their superclass. A subclass that declares no constructors has only the default (no argument, no name) constructor.

​	子类不会从其超类继承构造函数。未声明任何构造函数的子类仅具有默认构造函数（无参数，无名称）。

## 命名构造函数 Named constructors 

Use a named constructor to implement multiple constructors for a class or to provide extra clarity:

​	使用命名构造函数为类实现多个构造函数或提供额外的清晰度：

```dart
const double xOrigin = 0;
const double yOrigin = 0;

class Point {
  final double x;
  final double y;

  Point(this.x, this.y);

  // Named constructor
  Point.origin()
      : x = xOrigin,
        y = yOrigin;
}
```

Remember that constructors are not inherited, which means that a superclass’s named constructor is not inherited by a subclass. If you want a subclass to be created with a named constructor defined in the superclass, you must implement that constructor in the subclass.

​	请记住，构造函数不会被继承，这意味着超类的命名构造函数不会被子类继承。如果您希望使用超类中定义的命名构造函数创建子类，则必须在子类中实现该构造函数。

## 调用非默认超类构造函数 Invoking a non-default superclass constructor 

By default, a constructor in a subclass calls the superclass’s unnamed, no-argument constructor. The superclass’s constructor is called at the beginning of the constructor body. If an [initializer list](https://dart.dev/language/constructors#initializer-list) is also being used, it executes before the superclass is called. In summary, the order of execution is as follows:

​	默认情况下，子类中的构造函数会调用超类的未命名无参数构造函数。超类的构造函数在构造函数主体开头被调用。如果还使用了初始化程序列表，则它将在调用超类之前执行。总之，执行顺序如下：

1. initializer list
2. 初始化程序列表
3. superclass’s no-arg constructor
4. 超类的无参数构造函数
5. main class’s no-arg constructor
6. 主类的无参数构造函数

If the superclass doesn’t have an unnamed, no-argument constructor, then you must manually call one of the constructors in the superclass. Specify the superclass constructor after a colon (`:`), just before the constructor body (if any).

​	如果超类没有未命名且无参数的构造函数，则必须手动调用超类中的一个构造函数。在冒号 ( `:` ) 后指定超类构造函数，紧靠在构造函数体（如果有的话）之前。

In the following example, the constructor for the Employee class calls the named constructor for its superclass, Person. Click **Run** to execute the code.

​	在以下示例中，Employee 类的构造函数调用其超类 Person 的命名构造函数。单击运行以执行代码。

<iframe src="https://dartpad.dev/embed-dart.html?theme=light&amp;run=dartpad&amp;split=false&amp;ga_id=non_default_superclass_constructor" style="box-sizing: border-box; border: 1px solid rgb(204, 204, 204); margin-bottom: 1rem; min-height: 400px; resize: vertical; width: 896.007px; height: 450px;"></iframe>

Because the arguments to the superclass constructor are evaluated before invoking the constructor, an argument can be an expression such as a function call:

​	由于在调用构造函数之前会计算超类构造函数的参数，因此参数可以是表达式，例如函数调用：

```dart
class Employee extends Person {
  Employee() : super.fromJson(fetchDefaultData());
  // ···
}
```

*report_problem* **Warning:** Arguments to the superclass constructor don’t have access to `this`. For example, arguments can call static methods but not instance methods.

​	警告：超类构造函数的参数无法访问 `this` 。例如，参数可以调用静态方法，但不能调用实例方法。

### 超参数 Super parameters 

To avoid having to manually pass each parameter into the super invocation of a constructor, you can use super-initializer parameters to forward parameters to the specified or default superclass constructor. This feature can’t be used with redirecting constructors. Super-initializer parameters have similar syntax and semantics to [initializing formal parameters](https://dart.dev/language/constructors#initializing-formal-parameters):

​	为了避免必须手动将每个参数传递到构造函数的超调用中，可以使用超初始化器参数将参数转发到指定的或默认的超类构造函数。此功能不能与重定向构造函数一起使用。超初始化器参数具有与初始化形式参数类似的语法和语义：

```dart
class Vector2d {
  final double x;
  final double y;

  Vector2d(this.x, this.y);
}

class Vector3d extends Vector2d {
  final double z;

  // Forward the x and y parameters to the default super constructor like:
  // Vector3d(final double x, final double y, this.z) : super(x, y);
  Vector3d(super.x, super.y, this.z);
}
```

Super-initializer parameters cannot be positional if the super-constructor invocation already has positional arguments, but they can always be named:

​	如果超构造函数调用已经具有位置参数，则超初始化程序参数不能是位置参数，但它们始终可以命名：

```dart
class Vector2d {
  // ...

  Vector2d.named({required this.x, required this.y});
}

class Vector3d extends Vector2d {
  // ...

  // Forward the y parameter to the named super constructor like:
  // Vector3d.yzPlane({required double y, required this.z})
  //       : super.named(x: 0, y: y);
  Vector3d.yzPlane({required super.y, required this.z}) : super.named(x: 0);
}
```

*merge_type* **Version note:** Using super-initializer parameters requires a [language version](https://dart.dev/guides/language/evolution#language-versioning) of at least 2.17. If you’re using an earlier language version, you must manually pass in all super constructor parameters.

​	版本说明：使用超初始化程序参数需要至少 2.17 的语言版本。如果您使用的是较早的语言版本，则必须手动传入所有超构造函数参数。

## 初始化列表 Initializer list 

Besides invoking a superclass constructor, you can also initialize instance variables before the constructor body runs. Separate initializers with commas.

​	除了调用超类构造函数，您还可以在构造函数主体运行之前初始化实例变量。用逗号分隔各个初始化器。

```dart
// Initializer list sets instance variables before
// the constructor body runs.
Point.fromJson(Map<String, double> json)
    : x = json['x']!,
      y = json['y']! {
  print('In Point.fromJson(): ($x, $y)');
}
```

*report_problem* **Warning:** The right-hand side of an initializer doesn’t have access to `this`.

​	警告：初始化器的右侧无法访问 `this` 。

During development, you can validate inputs by using `assert` in the initializer list.

​	在开发过程中，您可以在初始化器列表中使用 `assert` 来验证输入。

```dart
Point.withAssert(this.x, this.y) : assert(x >= 0) {
  print('In Point.withAssert(): ($x, $y)');
}
```

Initializer lists are handy when setting up final fields. The following example initializes three final fields in an initializer list. Click **Run** to execute the code.

​	在设置 final 字段时，初始化器列表非常方便。以下示例在初始化器列表中初始化三个 final 字段。单击运行以执行代码。

<iframe src="https://dartpad.dev/embed-dart.html?theme=light&amp;run=dartpad&amp;split=false&amp;ga_id=initializer_list" style="box-sizing: border-box; border: 1px solid rgb(204, 204, 204); margin-bottom: 1rem; min-height: 400px; resize: vertical; width: 896.007px; height: 340px;"></iframe>

## 重定向构造函数 Redirecting constructors 

Sometimes a constructor’s only purpose is to redirect to another constructor in the same class. A redirecting constructor’s body is empty, with the constructor call (using `this` instead of the class name) appearing after a colon (:).

​	有时，构造函数的唯一目的是重定向到同一类中的另一个构造函数。重定向构造函数的主体为空，构造函数调用（使用 `this` 而不是类名）出现在冒号 (:) 之后。

```dart
class Point {
  double x, y;

  // The main constructor for this class.
  Point(this.x, this.y);

  // Delegates to the main constructor.
  Point.alongXAxis(double x) : this(x, 0);
}
```

## 常量构造函数 Constant constructors 

If your class produces objects that never change, you can make these objects compile-time constants. To do this, define a `const` constructor and make sure that all instance variables are `final`.

​	如果您的类生成永不更改的对象，则可以将这些对象设为编译时常量。为此，请定义一个 `const` 构造函数，并确保所有实例变量都是 `final` 。

```dart
class ImmutablePoint {
  static const ImmutablePoint origin = ImmutablePoint(0, 0);

  final double x, y;

  const ImmutablePoint(this.x, this.y);
}
```

Constant constructors don’t always create constants. For details, see the section on [using constructors](https://dart.dev/language/classes#using-constructors).

​	常量构造函数并不总是创建常量。有关详细信息，请参阅有关使用构造函数的部分。

## 工厂构造函数 Factory constructors 

Use the `factory` keyword when implementing a constructor that doesn’t always create a new instance of its class. For example, a factory constructor might return an instance from a cache, or it might return an instance of a subtype. Another use case for factory constructors is initializing a final variable using logic that can’t be handled in the initializer list.

​	在实现一个并非总是创建其类的实例的新构造函数时，请使用 `factory` 关键字。例如，工厂构造函数可能会从缓存中返回一个实例，或者它可能会返回一个子类型的实例。工厂构造函数的另一个用例是使用无法在初始化程序列表中处理的逻辑来初始化 final 变量。

*tips_and_updates* **Tip:** Another way to handle late initialization of a final variable is to [use `late final` (carefully!)](https://dart.dev/effective-dart/design#avoid-public-late-final-fields-without-initializers).

​	提示：处理 final 变量的延迟初始化的另一种方法是使用 `late final` （小心使用！）。

In the following example, the `Logger` factory constructor returns objects from a cache, and the `Logger.fromJson` factory constructor initializes a final variable from a JSON object.

​	在以下示例中， `Logger` 工厂构造函数从缓存中返回对象， `Logger.fromJson` 工厂构造函数从 JSON 对象初始化 final 变量。

```dart
class Logger {
  final String name;
  bool mute = false;

  // _cache is library-private, thanks to
  // the _ in front of its name.
  static final Map<String, Logger> _cache = <String, Logger>{};

  factory Logger(String name) {
    return _cache.putIfAbsent(name, () => Logger._internal(name));
  }

  factory Logger.fromJson(Map<String, Object> json) {
    return Logger(json['name'].toString());
  }

  Logger._internal(this.name);

  void log(String msg) {
    if (!mute) print(msg);
  }
}
```

*info* **Note:** Factory constructors have no access to `this`.

​	注意：工厂构造函数无法访问 `this` 。

Invoke a factory constructor just like you would any other constructor:

​	像调用任何其他构造函数一样调用工厂构造函数：

```dart
var logger = Logger('UI');
logger.log('Button clicked');

var logMap = {'name': 'UI'};
var loggerJson = Logger.fromJson(logMap);
```
