---
layout: post
title: " Kotlin  Apprentice Kotlin 学徒"
author: iosdevlog
date: 2020-12-08 09:24:39 +0800
description: ""
cover-img: /assets/images/Android/Kotlin/Kotlin_Apprentice.png
category: 
tags: [ Kotlin , android, book]
---

学习 Kotlin 编程！ *Kotlin  Apprentice* 是一本专为 Kotlin （适用于 Android 开发的现代语言）的初学者而设计的书。

By Eli Ganim, Irina Galata, Cosmin Pupăză, Ellen Shapiro, Matt Galloway and Ben Morrow

Version | Platform | Language | Editor
---|---|---|---
Second Edition | Android 10 | Kotlin 1.3 | IDEA

## 这本书是给谁看的 [^1]

对于 Kotlin 的完整初学者。无需事先编程经验！  
这是一本面向新的现代 Kotlin 语言的初学者的书。

本书涵盖的概念

* Kotlin 开发环境
* 数字和字符串
* 做出决定
* 函数 和 Lambda
*  **集合** 类型
* 建立自己的类型
* 函数编程
* 协程
* Kotlin 平台和脚本
* Kotlin /原生和多平台


本书中的所有内容都在干净，现代的开发环境中进行，这意味着您可以专注于使用 Kotlin 语言进行编程的核心功能，而不必陷入构建应用程序的许多细节中。

这是 《Android学徒》[^2] 的姊妹书，《Android学徒》 侧重于为 Android 开发应用程序，而 Kotlin 学徒侧重于 Kotlin 语言基础。

在你开始之前

本节告诉您开始之前需要了解的一些知识，例如对硬件和软件的需求，在何处可以找到本书的项目文件等等。

## Section I: Kotlin Basics 第一节： Kotlin 基础

本节中的各章将向您介绍 Kotlin 编程的基础知识。从计算机工作原理到语言结构，您将涵盖足够多的语言，以便能够处理数据并组织代码的行为。

本节开始一些基础让你开始，那么你有机会做的事与你新学到的数据类型。最后，您将越过一直使用的具有具体值的变量和常量，移到更棘手的数据类型上。

这些基础知识将助您一臂之力，在不知不觉中，您将为以后的更高级主题做好准备。让我们开始吧！

### Your Kotlin Development Environment 您的 Kotlin 开发环境

通过本书，可以快速入门 Kotlin ，以及如何使用 `Intellij IDEA` [^3] 处理项目。

```kotlin
fun main() {
  println("Hello, Kotlin!")
}
```

![computer](/assets/images/Android/kotlin/computer.png)

#### 要点

* ntelliJ IDEA 是 JetBrains 的集成开发环境，它是 Kotlin 语言的创建者，您可以在其中编写和运行 Kotlin 代码。
* IntelliJ IDEA 社区版是用于书中项目的免费版本。
* Kotlin 代码在许多平台上运行，其中最突出的是 Java 虚拟机，它将用于本书的多数内容。
* 要使用 IntelliJ IDEA 构建 Kotlin 项目，您需要安装 Java 开发工具包（版本 8）或以上。
* IntelliJ IDEA 应用窗口由多个面板组成，其中最相关的是 "项目" 面板、编辑器面板和"运行"面板。
* 可以通过从 IntelliJ IDEA 菜单中选择 "文件 + 打开" 菜单并选择相应项目的根文件夹来打开书籍入门、最终项目和挑战项目。

### Expressions, Variables & Constants 表达式，变量和常量

就是这样，您对编程世界的狂热介绍！您将从计算机和编程的概述开始，然后花时间处理代码注释，算术运算，常量和变量。

#### pseudo-code 伪代码

```
Step 1. Load photo from hard drive.
Step 2. Resize photo to 400 pixels wide by 300 pixels high.
Step 3. Apply sepia filter to photo.
Step 4. Print photo.
```

#### Code comments 注释

单行注释

```kotlin
// This is a comment. It is not executed.
```

多行注释

```kotlin
/* This is also a comment. 
   Over many..
   many...
   many lines. */
```

嵌套注释

```kotlin
/* This is a comment. 

 /* And inside it
 is
 another comment.
 */

 Back to the first.
 */
```

#### Printing out 打印输出

```kotlin
println("Hello, Kotlin Apprentice reader!")
```

#### Shift operations 移位运算

* Shift left: shl
* Shift right: shr

#### Naming data 命名数据

Constants 常量

```kotlin
val number: Int = 10
```

Variables 变量

```kotlin
var variableNumber: Int = 42
variableNumber = 0
variableNumber = 1_000_000
```

*camel case* 骆驼命名风格

* 以小写字母开头。
* 如果名称由多个单词组成，请将它们连接在一起，并每隔一个单词以大写字母开头。
* 如果这些单词中有一个是缩写，请用相同的大小写下整个缩写（例如 `sourceURL` 和 `urlDescription`）

### Types & Operations 类型与操作

您将学习有关处理不同类型的信息，包括允许您表示文本的字符串。您将学习有关在类型之间进行转换的知识，还将介绍类型推断，这使您作为程序员的生活变得更加简单。

#### Pairs and Triples 元组

Pair 二元元组

```kotlin
val coordinates: Pair<Int, Int> = Pair(2, 3)
val coordinatesInferred = Pair(2, 3)
val coordinatesWithTo = 2 to 3
// Inferred to be of type Pair<Double, Double>
val coordinatesDoubles = Pair(2.1, 3.5)
// Inferred to be of type Pair<Double, Int>
val coordinatesMixed = Pair(2.1, 3)
val x1 = coordinates.first
val y1 = coordinates.second
// x and y both inferred to be of type Int
val (x, y) = coordinates
```

Triples 三元元组

```kotlin
val coordinates3D = Triple(2, 3, 1)
val (x3, y3, z3) = coordinates3D

val coordinates3D = Triple(2, 3, 1)
val x3 = coordinates3D.first
val y3 = coordinates3D.second
val z3 = coordinates3D.third

val (x4, y4, _) = coordinates3D
```

#### Number types 数字类型

![Int](/assets/images/Android/kotlin/Int.png)

![Double](/assets/images/Android/kotlin/Double.png)

#### Any, Unit, and Nothing Types

* Any: 任意类型，不包括 `nullable`，类似 Java 中的 `Object`
* Unit: 类似 `void`  
    ```kotlin
    fun add(): Unit {
    val result = 2 + 2
    println(result)
    }
    ```
* Nothing: 是一种有助于声明函数不仅不会返回任何东西，而且永远不会完成的类型。  
    ```kotlin
    fun doNothingForever(): Nothing {
        while(true) {
            
        }
    }
    ```
#### 要点

* 类型转换允许您将一种类型的值转换为另一种类型。
* 当使用混合类型的运算符（例如基本算术运算符（`+，-，*，/`））时，Kotlin 会为您转换类型。
* 通过类型推断，您可以在 Kotlin 已经知道类型的情况下忽略它。
* Unicode 是将字符映射到数字的标准。
* Unicode 中的单个映射称为 `code point` 代码点。
* 字符数据类型存储单个字符。 String 数据类型存储字符或字符串的集合。
* 您可以使用加法运算符组合字符串。
* 您可以使用字符串模板就地构建字符串。
* 您可以使用 `Pair` 二元组和 `Triple` 三元组将数据分组为单个数据类型。
* 有许多类型的数字类型具有不同的存储和精度功能。
* `Any` 是所有非空类型之母，`Unit` 有点像 Java 中的 void，`Nothing` 就是什么也没有。

### Basic Control Flow 基本控制流程

您将学习如何使用语法来控制流程，从而在程序中制定决策并重复执行任务。您还将了解布尔值（代表真值和假值），以及如何使用它们比较数据。

#### The if expression

```kotlin
if (2 > 1) {
  println("Yes, 2 is greater than 1.")
}
```

`if-else`

```kotlin
val animal = "Fox"

if (animal == "Cat" || animal == "Dog") {
  println("Animal is a house pet.")
} else {
  println("Animal is not a house pet.")
}
```

```kotlin
val a = 5
val b = 10

val min: Int
if (a < b) {
  min = a
} else {
  min = b
}

val max: Int
if (a > b) {
  max = a
} else {
  max = b
}
```

简化成一行

```kotlin
val a = 5
val b = 10

val min = if (a < b) a else b
val max = if (a > b) a else b
```

`else-if`

```kotlin
val hourOfDay = 12

val timeOfDay = if (hourOfDay < 6) {
  "Early morning"
} else if (hourOfDay < 12) {
  "Morning"
} else if (hourOfDay < 17) {
  "Afternoon"
} else if (hourOfDay < 20) {
  "Evening"
} else if (hourOfDay < 24) {
  "Late evening"
} else {
  "INVALID HOUR!"
}
println(timeOfDay)
```

#### Loops 循环

`While loops`

```kotlin
while (<CONDITION>) {
  <LOOP CODE>
}
```

死循环

```kotlin
while (true) {
}
```

`Repeat-while loops`

```kotlin
do {
  <LOOP CODE>
} while (<CONDITION>)
```

示例

```kotlin
sum = 1

do {
  sum = sum + (sum + 1)
} while (sum < 1000)
```

`Breaking out of a loop`

```kotlin
sum = 1

while (true) {
  sum = sum + (sum + 1)
  if (sum >= 1000) {
    break
  }
}
```

### 要点

* 您使用布尔数据类型 `Boolean` 表示 `true` 和 `false`。
* 全部返回布尔值的比较运算符为：  
Equal: `==`  
Not equal: `!=`  
Less than: `<`  
Greater than: `>`  
Less than or equal: `<=`  
Greater than or equal: `>=`
* 您可以将布尔逻辑与 `&&` 和 `||` 结合使用以组合比较条件。
* 您可以使用 `if` 表达式根据条件做出简单的决定，然后返回一个值。
* 您可以在表达式中使用 `else` 和 `else-if` 来将决策扩展到单个条件之外的 `if`。
* 您可以使用单行 `if-else` 表达式使代码更加清晰简洁。
* **短路** 确保仅评估布尔表达式的最少必需部分。
* 变量和常量属于一定范围，超出范围您将无法使用它们。范围从其父级继承可见变量和常量。
* `while` 循环使您可以多次执行特定任务，直到满足条件为止。
* `break` 语句使您摆脱循环。

### Advanced Control Flow 高级控制流程

继续讲代码不是直线运行的主题，您将学到另一个称为 `for` 循环的循环。您还将了解何时在 Kotlin 中特别有用的表达式。

#### Ranges

#### closed range

```kotlin
// (0, 1, 2, 3, 4, 5)
val closedRange = 0..5
```

#### half-open range

```kotlin
// (0, 1, 2, 3, 4)
val halfOpenRange = 0 until 5
```

#### downTo

```kotlin
// (5, 4, 3, 2, 1, 0)
val decreasingRange = 5 downTo 0
```

#### For loops

```kotlin
for (<CONSTANT> in <RANGE>) {
  <LOOP CODE>
}
```

example

```kotlin
val count = 10

var sum = 0
for (i in 1..count) {
  sum += i
}
```

#### Labeled statements 标签语句

```kotlin
sum = 0
rowLoop@ for (row in 0 until 8) {
  columnLoop@ for (column in 0 until 8) {
    if (row == column) {
      continue@rowLoop
    }
    sum += row * column
  }
}
```

#### when expressions 表达式

```kotlin
val number = 10

when (number) {
  0 -> println("Zero")
  else -> println("Non-zero")
}
```

#### Returning values 返回值

```kotlin
val numberName = when (number) {
  2 -> "two"
  4 -> "four"
  6 -> "six"
  8 -> "eight"
  10 -> "ten"
  else -> {
    println("Unknown number")
    "Unknown"
  }
}

println(numberName) // > ten
```

#### Advanced when expressions 高级 when 表达

```kotlin
timeOfDay = when (hourOfDay) {
  in 0..5 -> "Early morning"
  in 6..11 -> "Morning"
  in 12..16 -> "Afternoon"
  in 17..19 -> "Evening"
  in 20..23 -> "Late evening"
  else -> "INVALID HOUR!"
}
```

`if`

```kotlin
when {
  number % 2 == 0 -> println("Even")
  else -> println("Odd")
}
```

## 要点

* 您可以使用范围来创建数字序列，然后递增以从一个值移动到另一个值。
* 封闭范围包括起始值和结束值。
* 半开范围包括起始值，并在终止值之前停止。
* `for` 循环允许您遍历一个范围。
* 在 `continue` 语句让你完成一个循环的当前迭代，并开始下一次迭代。
* 带 **标签** 的语句使您可以在外部循环上使用 `break` 和 `continue`。
* 您可以使用 `when` 表达式来确定要运行的代码，具体取决于变量或常量的值。
* 一个的更强的 `when` 表达来自 **正则匹配**：利用复杂规则匹配值。

### Functions 函数

函数是在 Kotlin 中用于构造代码的基本构建块。您将学习如何定义将代码分组为可重用单元的函数。

#### Function basics 函数基础

```kotlin
fun printMyName() {
  println("My name is Joe Howard.")
}

printMyName()
```

#### Function parameters 函数参数

```kotlin
fun printMultipleOfFive(value: Int) {
  println("$value * 5 = ${value * 5}")
}
printMultipleOfFive(10)
```

`named arguments` 命名参数

```kotlin
fun printMultipleOf(multiplier: Int, andValue: Int) {
  println("$multiplier * $andValue = ${multiplier * andValue}")
}
printMultipleOf(4, 2)

printMultipleOf(multiplier = 4, andValue = 2)
```

`default value` 默认值

```kotlin
fun printMultipleOf(multiplier: Int, value: Int = 1) {
  println("$multiplier * $value = ${multiplier * value}")
}

printMultipleOf(4)
```

`Return values` 返回值

```kotlin
fun multiply(number: Int, multiplier: Int): Int {
  return number * multiplier
}
```

省略 `{ }`

```kotlin
fun multiplyInferred(number: Int, multiplier: Int) = number * multiplier
```

`Parameters as values` 参数默认不可修改

```kotlin
fun incrementAndPrint(value: Int) {
  value += 1
  print(value)
}

// Val cannot be reassigned
```

declaring a new variable

```kotlin
fun incrementAndPrint(value: Int): Int {
  val newValue = value + 1
  println(newValue)
  return newValue
}
```

#### Overloading 重载

```kotlin
fun getValue(value: Int): Int {
  return value + 1
}

fun getValue(value: String): String {
  return "The value is $value"
}
```

#### Functions as variables 函数做变量

```kotlin
fun add(a: Int, b: Int): Int {
  return a + b
}
```

#### method reference operator `::` 方法引用运算符

```kotlin
var function = ::add
function(4, 2)
```

example

```kotlin
fun printResult(function: (Int, Int) -> Int, a: Int, b: Int) {
  val result = function(a, b)
  print(result)
}
printResult(::add, 4, 2)
```

#### The land of no return 无需返回的地方

```kotlin
fun infiniteLoop(): Nothing {
  while (true) {

  }
}
```

它很有用，因为编译器知道函数永远不会返回，它可以在生成代码来调用函数时进行某些优化。

#### Writing good functions 写好的函数

#### 要点

* 使用函数定义任务，无需多次编写代码即可执行多次任务。
* 函数可以包含零个或多个参数，并可以选择返回值。
* 为了在调用站点中清楚起见，您可以在调用函数时使用命名参数。
* 指定默认函数值可以使这些函数更易于使用，并减少必须编写的代码量。
* 函数可以具有相同的名称，具有不同的参数，这称为 **重载**。
* 您可以将函数分配给变量并将其传递给其他函数。
* 函数可以具有特殊的无返回类型 `Nothing`，以通知 Kotlin 此函数永远不会退出。
* 努力创建具有清晰命名的函数，并且具有一个具有可重复输入和输出的工作。

### Nullability 可空性

许多编程语言都遭受空值的“十亿美元错误”。您将了解 Kotlin 如何保护您免受可怕的空指针异常的影响。

#### Introducing null 介绍

#### Sentinel values 哨兵值

```kotlin
var errorCode = 0
```

`null` 表示值缺失

#### Introducing nullable types 可空类型

```kotlin
var errorCode: Int?

errorCode = 100
errorCode = null
```

#### Checking for null

```kotlin
var result: Int? = 30
println(result)

println(result + 1) // ERROR
// Operator call corresponds to a dot-qualified call 'result.plus(1)' which is not allowed on a nullable receiver 'result'.
```

#### Not-null assertion operator 非空断言操作符 `!!`

```kotlin
var authorName: String? = "Joe Howard"
var authorAge: Int? = 24

val ageAfterBirthday = authorAge!! + 1
println("After their next birthday, author will be $ageAfterBirthday")

// After their next birthday, author will be 25
```

fail

```kotlin
authorAge = null
println("After two birthdays, author will be ${authorAge!! + 2}")
// Exception in thread "main" kotlin.KotlinNullPointerException
```

#### Smart casts 智能转换

```kotlin
var nonNullableAuthor: String
var nullableAuthor: String?

if (authorName != null) {
  nonNullableAuthor = authorName
} else {
  nullableAuthor = authorName
}
```

#### Safe calls 安全调用

```kotlin
var nameLength = authorName?.length
println("Author's name has length $nameLength.")
// > Author's name has length 10.
```

chain

```kotlin
val nameLengthPlus5 = authorName?.length?.plus(5)
println("Author's name length plus 5 is $nameLengthPlus5.")
// > Author's name length plus 5 is 15.
```

#### The let() function

```kotlin
authorName?.let {
  nonNullableAuthor = authorName
}
```

#### Elvis operator 简洁三目运算符

```kotlin
var nullableInt: Int? = 10
var mustHaveResult = nullableInt ?: 0
```

等于以下

```kotlin
var nullableInt: Int? = 10
var mustHaveResult = if (nullableInt != null) nullableInt else 0
```

#### 要点

* null 表示缺少值。
* 非空变量和常量必须始终具有非空值。
* 可用变量和常量就像包含值或空（空）的框。
* 若要在可 null 中处理值，通常必须首先检查该值是否为 null。
* 使用 nullable 值最安全的方法是使用安全呼叫或 Elvis 运算符。仅在适当的时候使用非空断言，因为它们可能会生成运行时错误。

## Section II: Collections & Lambdas 第二节： **集合** 和 Lambda

到目前为止，您几乎已经看到单个元素形式的数据。尽管成对和三元组可以包含多个数据，但是您只能将两个或三个项目组合在一起。在本节中，您将了解 Kotlin 中的 **集合** 类型。 **集合** 是灵活的“容器”，可让您将任意数量的值存储在一起。

本节中的各章将为您提供组织数据和代码所需的基础，以便您可以开始在 Kotlin 中进行更复杂的编程！

### Arrays & Lists 数组和列表

数组和列表是您将在 Kotlin 中遇到的最常见的 **集合** 类型。它们都像常规变量和常量一样键入，并在单个数据结构中存储多个值。了解如何遍历数组和列表以及如何修改其内容。

### Maps & Sets **映射** 和 **集合** 

当您想通过标识符查找值时， **映射** 很有用。例如，本书的目录将章节名称 **映射** 到其页码，从而可以轻松跳至您要阅读的章节。对于数组，只能按其索引获取值，该索引必须是整数，并且所有索引必须是顺序的。在 **映射** 中，键可以是任何类型，并且通常没有特定的顺序。您还将了解 **集合** ，该 **集合** 使您可以在 **集合** 中存储唯一值。

### Lambdas

上一章教了您有关函数的知识。但是 Kotlin 还有另一种可用于将代码分解为可重用块的构造：`lambda`。它们有很多用途，在处理列表或 **映射** 之类的 **集合** 时特别有用。

## Section III: Building Your Own Types 第三节：建立自己的类型

您可以通过将变量和函数组合到新的类型定义中来创建自己的类型。例如，整数和双精度可能不足以满足您的目的，并且可能需要创建一种类型来存储复数。或者可能在三个独立变量中存储名字、中间名和姓氏变得难以管理，所以您决定创建一个 `FullName` 类型。

创建新类型时，请给它起一个名字。因此，这些自定义类型称为命名类型。命名类型是用于对现实概念建模的强大工具。您可以将相关的概念，属性和方法封装到一个统一的模型中。

自定义类型使您可以利用到目前为止已学习的基本构建块来构建大型和复杂的事物。现在是时候将您的 Kotlin 学徒提升到一个新的水平！

### class 类

在本章中，您将熟悉被称为类型的类。类是面向对象编程的基石之一，这是一种类型同时具有数据和行为的编程风格。在类中，数据采用 `属性` 的形式，并且行为使用称为 `方法` 的 `函数` 来实现。

### Objects 对象

Kotlin 有一个特殊的关键字对象，可以很容易地在项目中遵循Singleton模式，还可以用于创建特定于类而不是其实例的属性。您还可以使用关键字创建匿名类。

### Properties 属性

在本章中，您将了解有关 Kotlin 属性的更多信息，以及一些处理属性的技巧，如何监视属性值的变化以及如何延迟属性初始化。

### Methods 方法

方法仅仅是驻留在类中的函数。在本章中，您将仔细研究方法，并了解如何将方法添加到其他人创建的类上。

### Advanced Classes 高级类

了解了创建类的基础知识之后，在本章中，您将看到面向对象编程的更高级方面，包括继承和限制成员可见性。

### Enum Classes 枚举类

当您的数量可以包含一组有限的离散值时，枚举很有用。了解如何定义和使用枚举类，以及使用枚举类和何时表达式的一些示例。

### Interfaces 接口

当您要创建同时包含状态和行为的类型时，使用类。当您需要主要允许行为规范的类型时，最好使用接口。

了解如何创建和使用接口。

### Generics 泛型

在某些时候，您将需要能够创建超出常规类和函数可用范围的抽象。您将学习如何使用泛型来增强您的类和函数的功能。

## 第四节：中级主题

您已经到了本书的最后一节！在本节中，您将探究一些重要但又更中间的主题，以完善您的 Kotlin 学徒制。

### Kotlin / Java互操作性

Kotlin 从一开始就被设计为与 Java 和 Java 生态系统 *100％* 兼容。了解如何在单个项目中一起使用 Kotlin 和 Java，以及如何在两者之间来回调用。

### 例外情况

没有软件可以避免错误情况。了解如何在 Kotlin 中使用异常来对何时以及如何处理错误提供一些控制。

### 函数编程

Kotlin 不仅是一种面向对象的编程语言，它还提供了函数编程领域中的许多构造。通过学习如何将函数用作参数并从其他函数返回值，了解如何将函数视为一等公民。

### 约定和运算符重载

您将学习约定的概念，并了解如何使用约定实现操作符重载并编写更简洁但仍可读的代码。

### Kotlin 协程

Kotlin 协程使您可以简化异步代码并使之更具可读性。您将学习线程和协程之间的区别，并查看使用的协程示例。

### 用 Kotlin 编写脚本

Kotlin 不仅在 Android 或 JVM上 有用，而且还可以用于在命令行中编写脚本。了解如何使用 Kotlin 作为脚本语言。

###  Kotlin / Native 原生

安装并熟悉将 Kotlin 代码编译为适用于iOS和其他平台的原生二进制文件的 Kotlin / Native 工具。

###  Kotlin 多平台

过使用共享的 Kotlin 模块创建项目，您可以在多个平台之间共享 Kotlin 代码。了解如何创建适用于 iOS 和 Android 的 Multiplatform 项目，以及如何使用期望关键字和实际关键字来实现特定于平台的行为。

## 参考

[^1]: [Kotlin Apprentice](https://www.raywenderlich.com/books/kotlin-apprentice)
[^2]: [Android Apprentice](https://www.raywenderlich.com/books/android-apprentice)
[^3]: [Intellij IDEA](https://www.jetbrains.com/idea/)