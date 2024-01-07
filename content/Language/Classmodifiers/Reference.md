+++
title = "类修饰符参考"
date = 2024-01-05T20:29:36+08:00
weight = 20
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文: [https://dart.dev/language/modifier-reference](https://dart.dev/language/modifier-reference)

## Class modifiers reference 类修饰符参考

This page contains reference information for [class modifiers](https://dart.dev/language/class-modifiers).

​	此页面包含类修饰符的参考信息。

## 有效组合 Valid combinations 

The valid combinations of class modifiers and their resulting capabilities are:

​	类修饰符的有效组合及其产生的功能如下：

| Declaration 声明            | [Construct](https://dart.dev/language/classes#using-constructors)? 构造？ | [Extend](https://dart.dev/language/extend)? 扩展？ | [Implement](https://dart.dev/language/classes#implicit-interfaces)? 实现？ | [Mix in](https://dart.dev/language/mixins)? 混合？ | [Exhaustive](https://dart.dev/language/branches#exhaustiveness-checking)? 穷举？ |      |
| --------------------------- | ------------------------------------------------------------ | -------------------------------------------------- | ------------------------------------------------------------ | -------------------------------------------------- | ------------------------------------------------------------ | ---- |
| `class`                     | **Yes**                                                      | **Yes**                                            | **Yes**                                                      | No                                                 | No                                                           |      |
| `base class`                | **Yes**                                                      | **Yes**                                            | No                                                           | No                                                 | No                                                           |      |
| `interface class`           | **Yes**                                                      | No                                                 | **Yes**                                                      | No                                                 | No                                                           |      |
| `final class`               | **Yes**                                                      | No                                                 | No                                                           | No                                                 | No                                                           |      |
| `sealed class`              | No                                                           | No                                                 | No                                                           | No                                                 | **Yes**                                                      |      |
| `abstract class`            | No                                                           | **Yes**                                            | **Yes**                                                      | No                                                 | No                                                           |      |
| `abstract base class`       | No                                                           | **Yes**                                            | No                                                           | No                                                 | No                                                           |      |
| `abstract interface class`  | No                                                           | No                                                 | **Yes**                                                      | No                                                 | No                                                           |      |
| `abstract final class`      | No                                                           | No                                                 | No                                                           | No                                                 | No                                                           |      |
| `mixin class`               | **Yes**                                                      | **Yes**                                            | **Yes**                                                      | **Yes**                                            | No                                                           |      |
| `base mixin class`          | **Yes**                                                      | **Yes**                                            | No                                                           | **Yes**                                            | No                                                           |      |
| `abstract mixin class`      | No                                                           | **Yes**                                            | **Yes**                                                      | **Yes**                                            | No                                                           |      |
| `abstract base mixin class` | No                                                           | **Yes**                                            | No                                                           | **Yes**                                            | No                                                           |      |
| `mixin`                     | No                                                           | No                                                 | **Yes**                                                      | **Yes**                                            | No                                                           |      |
| `base mixin`                | No                                                           | No                                                 | No                                                           | **Yes**                                            | No                                                           |      |

## 无效组合 Invalid combinations 

Certain [combinations](https://dart.dev/language/class-modifiers#combining-modifiers) of modifiers are not allowed:

​	某些修饰符组合不允许：

| Combination 组合                                             | Reasoning 推理                                               |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| `base`, `interface`, and `final` `base` 、 `interface` 和 `final` | All control the same two capabilities (`extend` and `implement`), so are mutually exclusive. 所有都控制相同的两个功能（ `extend` 和 `implement` ），因此是互斥的。 |
| `sealed` and `abstract`                                      | Neither can be constructed, so are redundant together. 两者都不能构造，因此一起是多余的。 |
| `sealed` with `base`, `interface`, or `final`                | `sealed` types already cannot be mixed in, extended or implemented from another library, so are redundant to combine with the listed modifiers. `sealed` 类型已经无法在另一个库中混合、扩展或实现，因此与列出的修饰符结合是多余的。 |
| `mixin` and `abstract`                                       | Neither can be constructed, so are redundant together. 两者都无法构造，因此一起是多余的。 |
| `mixin` and `interface`, `final`, or `sealed`                | A `mixin` or `mixin class` declaration is intended to be mixed in, which the listed modifiers prevent. A `mixin` 或 `mixin class` 声明旨在混合使用，列出的修饰符会阻止这种混合使用。 |
| `enum` and any modifiers                                     | `enum` declarations cannot be extended, implemented, mixed in, and can always be instantiated, so no modifiers apply to `enum` declarations. `enum` 声明都不能扩展、实现、混合使用，并且始终可以实例化，因此没有修饰符适用于 `enum` 声明。 |
