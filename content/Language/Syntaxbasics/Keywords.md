+++
title = "Keywords"
date = 2024-01-05T20:29:36+08:00
weight = 50
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文: [https://dart.dev/language/keywords](https://dart.dev/language/keywords)

# Keywords 关键字

The following table lists the words that the Dart language treats specially.
下表列出了 Dart 语言特殊对待的单词。

| [abstract](https://dart.dev/language/class-modifiers#abstract) 2 | [else](https://dart.dev/language/branches#if)                | [import](https://dart.dev/language/libraries#using-libraries) 2 | [show](https://dart.dev/language/libraries#importing-only-part-of-a-library) 1 |
| ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| [as](https://dart.dev/language/operators#type-test-operators) 2 | [enum](https://dart.dev/language/enums)                      | [in](https://dart.dev/language/loops#for-loops)              | [static](https://dart.dev/language/classes#class-variables-and-methods) 2 |
| [assert 断言](https://dart.dev/language/error-handling#assert) | [export](https://dart.dev/guides/libraries/create-packages) 2 导出 2 | [interface](https://dart.dev/language/class-modifiers#interface) 2 接口 2 | [super 超级](https://dart.dev/language/extend)               |
| [async](https://dart.dev/language/async) 1 异步 1            | [extends 扩展](https://dart.dev/language/extend)             | [is](https://dart.dev/language/operators#type-test-operators) | [switch 切换](https://dart.dev/language/branches#switch)     |
| [await](https://dart.dev/language/async) 3 等待 3            | [extension](https://dart.dev/language/extension-methods) 2 扩展 2 | [late](https://dart.dev/language/variables#late-variables) 2 延迟 2 | [sync](https://dart.dev/language/functions#generators) 1 同步 1 |
| [base](https://dart.dev/language/class-modifiers#base) 2 基础 2 | [external](https://spec.dart.dev/DartLangSpecDraft.pdf#External Functions) 2 外部 2 | [library](https://dart.dev/language/libraries) 2 库 2        | [this 此](https://dart.dev/language/constructors)            |
| [break 中断](https://dart.dev/language/loops#break-and-continue) | [factory](https://dart.dev/language/constructors#factory-constructors) 2 工厂 2 | [mixin](https://dart.dev/language/mixins) 2 混合 2           | [throw 抛出](https://dart.dev/language/error-handling#throw) |
| [case 案例](https://dart.dev/language/branches#switch)       | [false](https://dart.dev/language/built-in-types#booleans)   | [new](https://dart.dev/language/classes#using-constructors)  | [true](https://dart.dev/language/built-in-types#booleans)    |
| [catch](https://dart.dev/language/error-handling#catch)      | [final (variable) final (变量)](https://dart.dev/language/variables#final-and-const) | [null](https://dart.dev/language/variables#default-value)    | [try](https://dart.dev/language/error-handling#catch)        |
| [class](https://dart.dev/language/classes#instance-variables) | [final (class)](https://dart.dev/language/class-modifiers#final) 2 final (类) 2 | [on](https://dart.dev/language/error-handling#catch) 1       | [typedef](https://dart.dev/language/typedefs) 2              |
| [const](https://dart.dev/language/variables#final-and-const) | [finally](https://dart.dev/language/error-handling#finally)  | [operator](https://dart.dev/language/methods#operators) 2 运算符 2 | [var](https://dart.dev/language/variables)                   |
| [continue](https://dart.dev/language/loops#break-and-continue) | [for](https://dart.dev/language/loops#for-loops)             | [part](https://dart.dev/guides/libraries/create-packages#organizing-a-package) 2 部分 2 | [void](https://dart.dev/language/built-in-types)             |
| [covariant](https://dart.dev/guides/language/sound-problems#the-covariant-keyword) 2 协变 2 | [Function](https://dart.dev/language/functions) 2            | [required](https://dart.dev/language/functions#named-parameters) 2 | [when](https://dart.dev/language/branches#when)              |
| [default](https://dart.dev/language/branches#switch)         | [get](https://dart.dev/language/methods#getters-and-setters) 2 获取 2 | [rethrow 重新抛出](https://dart.dev/language/error-handling#catch) | [while 同时](https://dart.dev/language/loops#while-and-do-while) |
| [deferred](https://dart.dev/language/libraries#lazily-loading-a-library) 2 延迟 2 | [hide](https://dart.dev/language/libraries#importing-only-part-of-a-library) 1 隐藏 1 | [return 返回](https://dart.dev/language/functions#return-values) | [with 带有](https://dart.dev/language/mixins)                |
| [do](https://dart.dev/language/loops#while-and-do-while)     | [if](https://dart.dev/language/branches#if)                  | [sealed](https://dart.dev/language/class-modifiers#sealed) 2 密封 2 | [yield](https://dart.dev/language/functions#generators) 3 产生 3 |
| [dynamic](https://dart.dev/language#important-concepts) 2 动态 2 | [implements](https://dart.dev/language/classes#implicit-interfaces) 2 实现 2 | [set](https://dart.dev/language/methods#getters-and-setters) 2 设置 2 |                                                              |

Avoid using these words as identifiers. However, if necessary, the keywords marked with superscripts can be identifiers:
避免将这些词用作标识符。但是，如果需要，用上标标记的关键字可以是标识符：

- Words with the superscript **1** are **contextual keywords**, which have meaning only in specific places. They’re valid identifiers everywhere.
  带有上标 1 的词是上下文关键字，仅在特定位置有意义。它们是任何地方的有效标识符。
- Words with the superscript **2** are **built-in identifiers**. These keywords are valid identifiers in most places, but they can’t be used as class or type names, or as import prefixes.
  带有上标 2 的词是内置标识符。这些关键字在大多数地方都是有效的标识符，但不能用作类或类型名称，也不能用作导入前缀。
- Words with the superscript **3** are limited reserved words related to [asynchrony support](https://dart.dev/language/async). You can’t use `await` or `yield` as an identifier in any function body marked with `async`, `async*`, or `sync*`.
  带有上标 3 的词是与异步支持相关的有限保留字。您不能在用 `async` 、 `async*` 或 `sync*` 标记的任何函数体中将 `await` 或 `yield` 用作标识符。

All other words in the table are **reserved words**, which can’t be identifiers.
表中的所有其他词都是保留字，不能用作标识符。
