+++
title = "FAQ"
date = 2024-01-05T20:29:36+08:00
weight = 40
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文: [https://dart.dev/null-safety/faq](https://dart.dev/null-safety/faq)

## Null safety: Frequently asked questions 空安全性：常见问题解答

This page collects some common questions we’ve heard about [null safety](https://dart.dev/null-safety) based on the experience of migrating Google internal code.

​	此页面收集了一些我们根据迁移 Google 内部代码的经验听到的有关空安全的常见问题。

## 对于已迁移代码的用户，我应该注意哪些运行时更改？ What runtime changes should I be aware of for users of migrated code? 

Most of the effects of migration do not immediately affect users of migrated code:

​	迁移的大多数影响不会立即影响已迁移代码的用户：

- Static null safety checks for users first apply when they migrate their code.
- 用户在迁移代码时首先应用静态空安全检查。
- Full null safety checks happen when all the code is migrated and sound mode is turned on.
- 在迁移所有代码并打开健全模式时，会执行完整的空安全检查。

Two exceptions to be aware of are:

​	需要注意的两个例外是：

- The `!` operator is a runtime null check in all modes, for all users. So, when migrating, ensure that you only add `!` where it’s an error for a `null` to flow to that location, even if the calling code has not migrated yet.
- 对于所有用户， `!` 运算符在所有模式中都是运行时空检查。因此，在迁移时，请确保仅在 `null` 流向该位置时出错的情况下添加 `!` ，即使调用代码尚未迁移。
- Runtime checks associated with the `late` keyword apply in all modes, for all users. Only mark a field `late` if you are sure it is always initialized before it is used.
- 与 `late` 关键字关联的运行时检查适用于所有模式，适用于所有用户。仅当您确定某个字段在使用前始终已初始化时，才标记该字段 `late` 。

## 如果某个值仅在测试中为 `null` 怎么办？ What if a value is only `null` in tests? 

If a value is only ever `null` in tests, the code can be improved by marking it non-nullable and making the tests pass non-null values.

​	如果某个值仅在测试中为 `null` ，则可以通过将其标记为非空并使测试通过非空值来改进代码。

## 如何将 `@required` 与新的 `required` 关键字进行比较？ How does `@required` compare to the new `required` keyword? 

The `@required` annotation marks named arguments that must be passed; if not, the analyzer reports a hint.

​	`@required` 注释标记必须传递的命名参数；如果没有，分析器会报告一个提示。

With null safety, a named argument with a non-nullable type must either have a default or be marked with the new `required` keyword. Otherwise, it wouldn’t make sense for it to be non-nullable, because it would default to `null` when not passed.

​	使用空安全性，具有非空类型的命名参数必须具有默认值或用新的 `required` 关键字标记。否则，它成为非空就没有意义，因为在未传递时，它会默认为 `null` 。

When null safe code is called from legacy code the `required` keyword is treated exactly like the `@required` annotation: failure to supply the argument will cause an analyzer hint.

​	当从旧版代码调用 null 安全代码时， `required` 关键字的处理方式与 `@required` 注释完全相同：未提供参数将导致分析器提示。

When null safe code is called from null safe code, failing to supply a `required` argument is an error.

​	当从 null 安全代码调用 null 安全代码时，未提供 `required` 参数是一个错误。

What does this mean for migration? Be careful if adding `required` where there was no `@required` before. Any callers not passing the newly-required argument will no longer compile. Instead, you could add a default or make the argument type nullable.

​	这对迁移意味着什么？在以前没有 `@required` 的地方添加 `required` 时要小心。任何不传递新要求的参数的调用方都将无法再编译。相反，您可以添加一个默认值或使参数类型可为 null。

## 我应该如何迁移应该为 `final` 但不是的非空字段？ How should I migrate non-nullable fields that should be `final`, but aren’t? 

Some computations can be moved to the static initializer. Instead of:

​	可以将一些计算移至静态初始化程序。您可以执行以下操作，而不是执行以下操作：

```
// Initialized without values
ListQueue _context;
Float32List _buffer;
dynamic _readObject;

Vec2D(Map<String, dynamic> object) {
  _buffer = Float32List.fromList([0.0, 0.0]);
  _readObject = object['container'];
  _context = ListQueue<dynamic>();
}
```

you can do:

```
// Initialized with values
final ListQueue _context = ListQueue<dynamic>();
final Float32List _buffer = Float32List.fromList([0.0, 0.0]);
final dynamic _readObject;

Vec2D(Map<String, dynamic> object) : _readObject = object['container'];
```

However, if a field is initialized by doing computation in the constructor, then it can’t be `final`. With null safety, you’ll find this also makes it harder for it to be non-nullable; if it’s initialized too late, then it’s `null` until it’s initialized, and must be nullable. Fortunately, you have options:

​	但是，如果通过在构造函数中执行计算来初始化字段，则该字段不能为 。使用 null 安全后，您会发现这也使得字段更难为非空；如果字段初始化得太晚，则在初始化之前该字段为 ，并且必须可为 null。幸运的是，您有以下选择：

- Turn the constructor into a factory, then make it delegate to an actual constructor that initializes all the fields directly. A common name for such a private constructor is just an underscore: `_`. Then, the field can be `final` and non-nullable. This refactoring can be done *before* the migration to null safety.
- 将构造函数变成一个工厂，然后让它委托给一个实际的构造函数，该构造函数直接初始化所有字段。这种私有构造函数的常见名称只是一个下划线： `_` 。然后，该字段可以是 `final` 且不可为空。这种重构可以在迁移到空安全之前完成。
- Or, mark the field `late final`. This enforces that it’s initialized exactly once. It must be initialized before it can be read.
- 或者，标记字段 `late final` 。这强制要求它只初始化一次。必须在读取之前对其进行初始化。

## 我应该如何迁移 `built_value` 类？ How should I migrate a `built_value` class? 

Getters that were annotated `@nullable` should instead have nullable types; then remove all `@nullable` annotations. For example:

注释为 `@nullable` 的 getter 应该具有可空类型；然后删除所有 `@nullable` 注释。例如：

```
@nullable
int get count;
```

becomes

变为

```
int? get count; //  Variable initialized with ?
```

Getters that were *not* marked `@nullable` should *not* have nullable types, even if the migration tool suggests them. Add `!` hints as needed then rerun the analysis.

​	未标记为 `@nullable` 的 getter 不应具有可空类型，即使迁移工具建议使用它们。根据需要添加 `!` 提示，然后重新运行分析。

## 我应该如何迁移可以返回 `null` 的工厂？ How should I migrate a factory that can return `null`? 

*Prefer factories that do not return null.* We have seen code that meant to throw an exception due to invalid input but instead ended up returning null.

​	更喜欢不返回 null 的工厂。我们已经看到由于无效输入而打算抛出异常的代码，但最终返回 null。

Instead of:

而不是：

```
  factory StreamReader(dynamic data) {
    StreamReader reader;
    if (data is ByteData) {
      reader = BlockReader(data);
    } else if (data is Map) {
      reader = JSONBlockReader(data);
    }
    return reader;
  }
```

Do:

```
  factory StreamReader(dynamic data) {
    if (data is ByteData) {
      // Move the readIndex forward for the binary reader.
      return BlockReader(data);
    } else if (data is Map) {
      return JSONBlockReader(data);
    } else {
      throw ArgumentError('Unexpected type for data');
    }
  }
```

If the intent of the factory was indeed to return null, then you can turn it into a static method so it is allowed to return `null`.

​	如果工厂的目的是确实返回 null，那么您可以将其变成一个静态方法，以便允许它返回 `null` 。

## 如何迁移现在显示为不必要的 `assert(x != null)` ？ How should I migrate an `assert(x != null)` that now shows as unnecessary? 

The assert will be unnecessary when everything is fully migrated, but for now it *is* needed if you actually want to keep the check. Options:

​	当所有内容完全迁移后，断言将变得不必要，但如果您实际上想要保留检查，则现在需要它。选项：

- Decide that the assert is not really necessary, and remove it. This is a change in behavior when asserts are enabled.
- 决定断言确实不必要，并将其删除。当启用断言时，这是行为的改变。
- Decide that the assert can be checked always, and turn it into `ArgumentError.checkNotNull`. This is a change in behavior when asserts are not enabled.
- 决定始终可以检查断言，并将其变成 `ArgumentError.checkNotNull` 。当未启用断言时，这是行为的改变。
- Keep the behavior exactly as is: add `// ignore: unnecessary_null_comparison` to bypass the warning.
- 保持行为完全不变：添加 `// ignore: unnecessary_null_comparison` 以绕过警告。

## 如何迁移现在显示为不必要的运行时空值检查？ How should I migrate a runtime null check that now shows as unnecessary? 

The compiler flags an explicit runtime null check as an unnecessary comparison if you make `arg` non-nullable.

​	如果您使 `arg` 为非空，则编译器会将显式运行时空值检查标记为不必要的比较。

```
if (arg == null) throw ArgumentError(...)`
```

You must include this check if the program is a mixed-version one. Until everything is fully migrated and the code switches to running with sound null safety, `arg` might be set to `null`.

​	如果程序是混合版本程序，则必须包含此检查。在所有内容完全迁移并且代码切换为使用健全的空值安全性运行之前， `arg` 可能会设置为 `null` 。

The simplest way to preserve behavior is change the check into [`ArgumentError.checkNotNull`](https://api.dart.dev/stable/dart-core/ArgumentError/checkNotNull.html).

​	保留行为的最简单方法是将检查更改为 `ArgumentError.checkNotNull` 。

The same applies to some runtime type checks. If `arg` has static type `String`, then `if (arg is! String)` is actually checking whether `arg` is `null`. It might look like migrating to null safety means `arg` can never be `null`, but it could be `null` in unsound null safety. So, to preserve behavior, the null check should remain.

​	对于某些运行时类型检查也适用。如果 `arg` 具有静态类型 `String` ，则 `if (arg is! String)` 实际上是检查 `arg` 是否为 `null` 。看起来迁移到空安全意味着 `arg` 永远不会是 `null` ，但它在不健全的空安全中可能是 `null` 。因此，为了保持行为，空检查应该保留。

## 方法 `Iterable.firstWhere` 不再接受 `orElse: () => null` 。 The `Iterable.firstWhere` method no longer accepts `orElse: () => null`. 

Import `package:collection` and use the extension method `firstWhereOrNull` instead of `firstWhere`.

​	导入 `package:collection` 并使用扩展方法 `firstWhereOrNull` 而不是 `firstWhere` 。

## 如何处理具有设置器的属性？ How do I deal with attributes that have setters? 

Unlike the `late final` suggestion above, these attributes cannot be marked as final. Often, settable attributes also do not have initial values since they are expected to be set sometime later.

​	与上述 `late final` 建议不同，这些属性不能标记为 final。通常，可设置属性也没有初始值，因为它们预计稍后会设置。

In such cases, you have two options:

​	在这种情况下，您有两个选择：

- Set it to an initial value. Often times, the omission of an initial value is by mistake rather than deliberate.
  
- 将其设置为初始值。通常，省略初始值是错误而不是故意的。
  
- If you are *sure* that the attribute needs to be set before accessed, mark it as `late`.
  
- 如果您确定需要在访问属性之前设置该属性，请将其标记为 `late` 。
  
  WARNING: The `late` keyword adds a runtime check. If any user calls `get` before `set` they’ll get an error at runtime.
  
  ​	警告： `late` 关键字会添加运行时检查。如果任何用户在 `set` 之前调用 `get` ，他们将在运行时收到错误。

## 如何发出 Map 的返回值不可为空的信号？ How do I signal that the return value from a Map is non-nullable? 

The [lookup operator](https://api.dart.dev/stable/dart-core/Map/operator_get.html) on Map (`[]`) by default returns a nullable type. There’s no way to signal to the language that the value is guaranteed to be there.

​	Map（ `[]` ）上的查找运算符默认返回一个可空类型。没有办法向语言发出该值保证存在于那里的信号。

In this case, you should use the bang operator (`!`) to cast the value back to V:

​	在这种情况下，您应该使用感叹号运算符（ `!` ）将值强制转换回 V：

```
return blockTypes[key]!;
```

Which will throw if the map returns null. If you want explicit handling for that case:

​	如果映射返回 null，则会引发异常。如果您想对该情况进行显式处理：

```
var result = blockTypes[key];
if (result != null) return result;
// Handle the null case here, e.g. throw with explanation.
```

## 为什么我的 List/Map 上的泛型类型可为 null？ Why is the generic type on my List/Map nullable? 

It is typically a code smell to end up with nullable code like this:

​	通常，以这种可为 null 的代码结束是一种代码缺陷：

```
List<Foo?> fooList; // fooList can contain null values
```

This implies `fooList` might contain null values. This might happen if you are initializing the list with length and filling it in via a loop.

​	这意味着 `fooList` 可能包含 null 值。如果使用长度初始化列表并通过循环填充列表，则可能会发生这种情况。

If you are simply initializing the list with the same value, you should instead use the [`filled`](https://api.dart.dev/stable/dart-core/List/List.filled.html) constructor.

​	如果您只是使用相同的值初始化列表，则应改用 `filled` 构造函数。

```
_jellyCounts = List<int?>(jellyMax + 1);
for (var i = 0; i <= jellyMax; i++) {
  _jellyCounts[i] = 0; // List initialized with the same value
}
_jellyCounts = List<int>.filled(jellyMax + 1, 0); // List initialized with filled constructor
```

If you are setting the elements of the list via an index, or you are populating each element of the list with a distinct value, you should instead use the list literal syntax to build the list.

​	如果您通过索引设置列表的元素，或者使用不同的值填充列表的每个元素，则应改用列表文字语法来构建列表。

```
_jellyPoints = List<Vec2D?>(jellyMax + 1);
for (var i = 0; i <= jellyMax; i++) {
  _jellyPoints[i] = Vec2D(); // Each list element is a distinct Vec2D
}
_jellyPoints = [
  for (var i = 0; i <= jellyMax; i++)
    Vec2D() // Each list element is a distinct Vec2D
];
```

To generate a fixed-length list, use the [`List.generate`](https://api.dart.dev/stable/dart-core/List/List.generate.html) constructor with the `growable` parameter set to `false`:

​	要生成固定长度的列表，请使用 `List.generate` 构造函数，其中 `growable` 参数设置为 `false` ：

```
_jellyPoints = List.generate(jellyMax, (_) => Vec2D(), growable: false);
```

## 默认列表构造函数怎么了？ What happened to the default List constructor? 

You may encounter this error:

​	您可能会遇到此错误：

```nocode
The default 'List' constructor isn't available when null safety is enabled. #default_list_constructor
```

The default list constructor fills the list with `null`, which is a problem.

​	默认列表构造函数使用 `null` 填充列表，这是一个问题。

Change it to `List.filled(length, default)` instead.
改为 `List.filled(length, default)` 。

## 我正在使用 `package:ffi` ，并且在迁移时收到 `Dart_CObject_kUnsupported` 失败。发生了什么？ I’m using `package:ffi` and get a failure with `Dart_CObject_kUnsupported` when I migrate. What happened? 

Lists sent via ffi can only be `List<dynamic>`, not `List<Object>` or `List<Object?>`. If you didn’t change a list type explicitly in your migration, a type might still have changed because of changes to type inference that happen when you enable null safety.

​	通过 ffi 发送的列表只能是 `List<dynamic>` ，而不是 `List<Object>` 或 `List<Object?>` 。如果您在迁移中未明确更改列表类型，则类型仍可能因启用空安全时发生的类型推断更改而发生更改。

The fix is to explicitly create such lists as `List<dynamic>`.

​	修复方法是将此类列表明确创建为 `List<dynamic>` 。

## 为什么迁移工具会向我的代码添加注释？ Why does the migration tool add comments to my code? 

The migration tool adds `/* == false */` or `/* == true */` comments when it sees conditions that will always be false or true while running in sound mode. Comments like these might indicate that the automatic migration is incorrect and needs human intervention. For example:

​	当迁移工具在健全模式下运行时看到始终为假或为真的条件时，它会添加 `/* == false */` 或 `/* == true */` 注释。此类注释可能表明自动迁移不正确，需要人工干预。例如：

```
if (registry.viewFactory(viewDescriptor.id) == null /* == false */)
```

In these cases, the migration tool can’t distinguish defensive-coding situations and situations where a null value is really expected. So the tool tells you what it knows (“it looks like this condition will always be false!”) and lets you decide what to do.

​	在这些情况下，迁移工具无法区分防御性编码情况和确实预期空值的情况。因此，该工具会告诉您它知道的内容（“看起来此条件始终为假！”），并让您决定要做什么。

##  我应该了解有关编译为 JavaScript 和空安全性的哪些知识？ What should I know about compiling to JavaScript and null safety?

Null safety brings many benefits like reduced code size and improved app performance. Such benefits surface more when compiled to native targets like Flutter and AOT. Previous work on the production web compiler had introduced optimizations similar to what null safety later introduced. This may make resulting gains to production web apps seem less than their native targets.

​	空安全性带来了许多好处，例如减少代码大小和提高应用性能。在编译成原生目标（如 Flutter 和 AOT）时，此类好处会更加明显。之前对生产 Web 编译器的研究引入了与空安全性后来引入的类似优化。这可能会使生成的生产 Web 应用的收益看起来低于其原生目标。

A few notes that are worth highlighting:

​	值得强调的几点说明：

- The production JavaScript compiler generates `!` null assertions. You might not notice them when comparing the output of the compiler before and after adding null assertions. That’s because the compiler already generated null checks in programs that weren’t null safe.
  
- 生产 JavaScript 编译器会生成 `!` null 断言。在比较添加 null 断言前后编译器的输出时，您可能不会注意到它们。这是因为编译器已经在非 null 安全的程序中生成了 null 检查。
  
- The compiler generates these null assertions regardless of the soundness of null safety or optimization level. In fact, the compiler doesn’t remove `!` when using `-O3` or `--omit-implicit-checks`.
  
- 无论 null 安全性或优化级别的健全性如何，编译器都会生成这些 null 断言。事实上，编译器在使用 `-O3` 或 `--omit-implicit-checks` 时不会删除 `!` 。
  
- The production JavaScript compiler might remove unnecessary null checks. This happens because the optimizations that the production web compiler made prior to null safety removed those checks when it knew the value was not null.

- 生产 JavaScript 编译器可能会删除不必要的 null 检查。这是因为生产 Web 编译器在 null 安全之前所做的优化在知道该值不为 null 时删除了那些检查。

- By default, the compiler would generate parameter subtype checks. These runtime checks ensure covariant virtual calls have appropriate arguments. The compiler skips these checks with the `--omit-implicit-checks` option. Using this option can generate apps with unexpected behavior if the code includes invalid types. To avoid any surprises, continue provide strong test coverage for your code. In particular, the compiler optimizes code based on the fact that inputs should comply with the type declaration. If the code provides arguments of an invalid type, those optimizations would be wrong and the program could misbehave. This was true for inconsistent types before, and is true with inconsistent nullabilities now with sound null-safety.
  
- 默认情况下，编译器会生成参数子类型检查。这些运行时检查确保协变虚拟调用具有适当的参数。编译器使用 `--omit-implicit-checks` 选项跳过这些检查。如果代码包含无效类型，使用此选项可能会生成行为意外的应用。为了避免任何意外，请继续为您的代码提供强大的测试覆盖率。特别是，编译器根据输入应符合类型声明这一事实优化代码。如果代码提供无效类型的参数，这些优化将是错误的，程序可能会出现错误。以前对于不一致的类型来说是这样，现在对于具有健全空安全的不一致的空值来说也是如此。
  
- You may notice that the development JavaScript compiler and the Dart VM have special error messages for null checks, but to keep applications small, the production JavaScript compiler does not.
  
- 您可能会注意到，开发 JavaScript 编译器和 Dart VM 对空检查有特殊的错误消息，但为了保持应用程序的精简，生产 JavaScript 编译器没有。
  
- You may see errors indicating that `.toString` is not found on `null`. This is not a bug. The compiler has always encoded some null checks in this way. That is, the compiler represents some null checks compactly by making an unguarded access of a property of the receiver. So instead of `if (a == null) throw`, it generates `a.toString`. The `toString` method is defined in JavaScript Object and is a fast way to verify that an object is not null.
  
- 您可能会看到错误，指出在 `null` 上找不到 `.toString` 。这不是一个错误。编译器始终以这种方式对一些空检查进行编码。也就是说，编译器通过对接收器的属性进行无保护访问来紧凑地表示一些空检查。因此，它不是生成 `if (a == null) throw` ，而是生成 `a.toString` 。方法 `toString` 在 JavaScript 对象中定义，是验证对象不为空的快速方法。
  
  If the very first action after a null check is an action that crashes when the value is null, the compiler can remove the null check and let the action cause the error.
  
  ​	如果空检查后的第一个操作是在值为 null 时崩溃的操作，则编译器可以删除空检查并让操作导致错误。

  For example, a Dart expression `print(a!.foo());` could turn directly into:
  
  ​	例如，Dart 表达式 `print(a!.foo());` 可以直接转换为：
  
  ```
    P.print(a.foo$0());
  ```
  
  This is because the call `a.foo$()` will crash if `a` is null. If the compiler inlines `foo`, it will preserve the null check. So for example, if `foo` was `int foo() => 1;` the compiler might generate:

  ​	这是因为如果 `a` 为 null，则调用 `a.foo$()` 会崩溃。如果编译器内联 `foo` ，它将保留空检查。因此，例如，如果 `foo` 为 `int foo() => 1;` ，则编译器可能会生成：
  
  ```
    a.toString;
    P.print(1);
  ```
  
  If the inlined method first accessed a field on the receiver, like `int foo() => this.x + 1;`, then the production compiler can remove the redundant `a.toString` null check, as non-inlined calls, and generate:
  
  ​	如果内联方法首先访问接收器上的字段，如 `int foo() => this.x + 1;` ，则生成编译器可以删除冗余的 `a.toString` 空检查，作为非内联调用，并生成：
  
  ```
    P.print(a.x + 1);
  ```

## 资源 Resources 

- [DartPad with Null Safety](https://dartpad.dev/)
- 带空安全性的 DartPad
- [Sound null safety](https://dart.dev/null-safety)
- 完善的空安全性
