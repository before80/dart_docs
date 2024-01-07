+++
title = "Mixins"
date = 2024-01-05T20:29:36+08:00
weight = 40
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文: [https://dart.dev/language/mixins](https://dart.dev/language/mixins)

## Mixins

Mixins are a way of defining code that can be reused in multiple class hierarchies. They are intended to provide member implementations en masse.

​	Mixins 是一种可以在多个类层次结构中重用的代码定义方式。它们旨在批量提供成员实现。

To use a mixin, use the `with` keyword followed by one or more mixin names. The following example shows two classes that use mixins:

​	要使用 mixin，请使用 `with` 关键字，后跟一个或多个 mixin 名称。以下示例显示了使用 mixin 的两个类：

```dart
class Musician extends Performer with Musical {
  // ···
}

class Maestro extends Person with Musical, Aggressive, Demented {
  Maestro(String maestroName) {
    name = maestroName;
    canConduct = true;
  }
}
```

To define a mixin, use the `mixin` declaration. In the rare case where you need to define both a mixin *and* a class, you can use the [`mixin class` declaration](https://dart.dev/language/mixins#class-mixin-or-mixin-class).

​	要定义 mixin，请使用 `mixin` 声明。在极少数情况下，您需要同时定义 mixin 和类，可以使用 `mixin class` 声明。

Mixins and mixin classes cannot have an `extends` clause, and must not declare any generative constructors.

​	Mixins 和 mixin 类不能有 `extends` 子句，并且不得声明任何生成构造函数。

For example:

​	例如：

```dart
mixin Musical {
  bool canPlayPiano = false;
  bool canCompose = false;
  bool canConduct = false;

  void entertainMe() {
    if (canPlayPiano) {
      print('Playing piano');
    } else if (canConduct) {
      print('Waving hands');
    } else {
      print('Humming to self');
    }
  }
}
```

Sometimes you might want to restrict the types that can use a mixin. For example, the mixin might depend on being able to invoke a method that the mixin doesn’t define. As the following example shows, you can restrict a mixin’s use by using the `on` keyword to specify the required superclass:

​	有时您可能希望限制可以使用 mixin 的类型。例如，mixin 可能依赖于调用 mixin 未定义的方法。如下例所示，您可以使用 `on` 关键字指定所需的超类来限制 mixin 的使用：

```dart
class Musician {
  // ...
}
mixin MusicalPerformer on Musician {
  // ...
}
class SingerDancer extends Musician with MusicalPerformer {
  // ...
}
```

In the preceding code, only classes that extend or implement the `Musician` class can use the mixin `MusicalPerformer`. Because `SingerDancer` extends `Musician`, `SingerDancer` can mix in `MusicalPerformer`.

​	在前面的代码中，只有扩展或实现 `Musician` 类的类才能使用 mixin `MusicalPerformer` 。因为 `SingerDancer` 扩展 `Musician` ，所以 `SingerDancer` 可以混入 `MusicalPerformer` 。

## `class`, `mixin`, 还是 `mixin class`? 

*merge_type* **Version note:** The `mixin class` declaration requires a [language version](https://dart.dev/guides/language/evolution#language-versioning) of at least 3.0.

​	版本说明： `mixin class` 声明至少需要3.0的语言版本。

A `mixin` declaration defines a mixin. A `class` declaration defines a [class](https://dart.dev/language/classes). A `mixin class` declaration defines a class that is usable as both a regular class and a mixin, with the same name and the same type.

​	`mixin` 声明定义了一个混合类。 `class` 声明定义了一个类。 `mixin class` 声明定义了一个类，该类可用作常规类和混合类，具有相同的名称和相同的类型。

Any restrictions that apply to classes or mixins also apply to mixin classes:

​	适用于类或混合类的任何限制也适用于混合类：

- Mixins can’t have `extends` or `with` clauses, so neither can a `mixin class`.
- 混合类不能有 `extends` 或 `with` 子句，因此 `mixin class` 也不能有。
- Classes can’t have an `on` clause, so neither can a `mixin class`.
- 类不能有 `on` 子句，因此 `mixin class` 也不能有。

### `abstract mixin class`

You can achieve similar behavior to the `on` directive for a mixin class. Make the mixin class `abstract` and define the abstract methods its behavior depends on:

​	您可以实现与混合类的 `on` 指令类似的行为。 使混合类 `abstract` 并定义其行为所依赖的抽象方法：

```
abstract mixin class Musician {
  // No 'on' clause, but an abstract method that other types must define if 
  // they want to use (mix in or extend) Musician: 
  void playInstrument(String instrumentName);

  void playPiano() {
    playInstrument('Piano');
  }
  void playFlute() {
    playInstrument('Flute');
  }
}

class Virtuoso with Musician { // Use Musician as a mixin
  void playInstrument(String instrumentName) {
    print('Plays the $instrumentName beautifully');
  }  
} 

class Novice extends Musician { // Use Musician as a class
  void playInstrument(String instrumentName) {
    print('Plays the $instrumentName poorly');
  }  
} 
```

By declaring the `Musician` mixin as abstract, you force any type that uses it to define the abstract method upon which its behavior depends.

​	通过将 `Musician` 混合类声明为抽象类，您可以强制使用它的任何类型定义其行为所依赖的抽象方法。

This is similar to how the `on` directive ensures a mixin has access to any interfaces it depends on by specifying the superclass of that interface.

​	这类似于 `on` 指令如何通过指定该接口的超类来确保混合类可以访问其依赖的任何接口。
