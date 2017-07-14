# Kotlin学习笔记-基本语法
在2017年5月的Google IO大会上，Google宣布Kotlin成为Android的官方语言，随后的2017年6月的TIOBE排行榜中，Kotlin就首次挤进了编程语言TOP50，可见Kotlin的爆发之迅速。作为一个Android开发者需要立刻学习这门新的语言，本文是Kotlin学习笔记的第二篇，本篇讲讲Kotlin的基本语法。

## 包
与java类似，在源文件的开头定义包。不过与java不同，包名不必和文件夹路径一致，源文件可以放在任意位置。

```
package com.firefly.demo
import java.util.*
```

## 函数

下面定义一个函数接受两个 int 型参数，返回值为 int ：

```
fun sum(a: Int, b: Int): Int {
    return a + b
}

fun main(args: Array<String>) {
  print("sum of 3 and 5 is ")
  println(sum(3, 5))
}
```

下面这个函数只有一个表达式函数体以及一个自推导型的返回值：

```
fun sum(a: Int, b: Int) = a + b

fun main(args: Array<String>) {
  println("sum of 19 and 23 is ${sum(19, 23)}")
}
```

下面这个函数，定义一个返回没有意义的值：

```
fun printSum(a: Int, b: Int): Unit {
  println("sum of $a and $b is ${a + b}")
}

```

无返回值的函数也可以去掉Unit，比如：

```
fun printSum(a: Int, b: Int) {
  println("sum of $a and $b is ${a + b}")
}
```

## 局部变量

一次赋值（只读）的局部变量：

```
fun main(args: Array<String>) {
  val a: Int = 1  // 立刻赋值
  val b = 2   // `Int` 类型是自推导的
  val c: Int  // 没有初始化器时要指定类型
  c = 3       // 推导型赋值
}
```

可修改的变量：

```
fun main(args: Array<String>) {
  var x = 5 // `Int` type is inferred
  x += 1
}
```

## 注释

与 java 和 javaScript 一样，Kotlin 支持单行注释和块注释。

```
// 单行注释

/*  哈哈哈哈
    这是块注释 */
```
与 java 不同的是 Kotlin 的 块注释可以级联。

## 字符串模板

字符串可以包含模板表达式。一个模板表达式由一个 $ 开始并包含另一个简单的名称：

```
val i = 10
val s = "i = $i" // 识别为 "i = 10"
```

或者是一个带大括号的表达式：

```
val s = "abc"
val str = "$s.length is ${s.length}" //识别为 "abc.length is 3"
```

## 条件表达式

类似java的条件表达式：

```
fun maxOf(a: Int, b: Int): Int {
    if (a > b) {
        return a
    } else {
        return b
    }
}
```

不同于java的"？："，Kotlin中的if当表达式：

```
fun maxOf(a: Int, b: Int) = if (a > b) a else b
```

## 空值

Kotlin为了避免空指针出现，要求当空值可能出现时应该明确指出该引用可空。

下面的函数是当 str 中不包含整数时返回空:

```
fun parseInt(str : String): Int?{
	//...
}
```

调用返回可空值的函数时，需要像java一样做null判断，避免出现空指针：

```
fun parseInt(str: String): Int? {
  return str.toIntOrNull()
}

fun printProduct(arg1: String, arg2: String) {
  val x = parseInt(arg1)
  val y = parseInt(arg2)

  // 直接使用 x*y 会产生错误因为它们中有可能会有空值
  if (x != null && y != null) {
    // x 和 y 将会在空值检测后自动转换为非空值
    println(x * y)
  }
  else {
    println("either '$arg1' or '$arg2' is not a number")
  }    
}
```

或者：

```
if (x == null) {
    println("Wrong number format in arg1: '${arg1}'")
    return
}
if (y == null) {
    println("Wrong number format in arg2: '${arg2}'")
    return
}

// x and y are automatically cast to non-nullable after null check
println(x * y)
```

## 类型判断及转换

Kotlin使用 is 操作符来做类型判断：

```
//判断是否为String型
if (obj is String) {
}
```

如果对不可变的局部变量或属性进行过了类型检查，就不需再做类型转换：

```
fun getStringLength(obj: Any): Int? {
  if (obj is String) {
    // 因为已经用if做过类型判断，obj将会在这个分支中自动转换为 String 类型
    return obj.length
  }

  // obj 在类型检查分支外仍然是 Any 类型
  return null
}
```

## for 循环

类似java的foreach:

```
val items = listOf("apple", "banana", "kiwi")
for (item in items) {
    println(item)
}

for (index in items.indices) {
    println("item at $index is ${items[index]}")
}
```

## while 循环

与java中的while用法一样：

```
val items = listOf("apple", "banana", "kiwi")
var index = 0
while (index < items.size) {
    println("item at $index is ${items[index]}")
    index++
}
```

## when 表达式

类似java中的switch，不同于switch的变量只能为int或byte，when可以为任意对象：

```
fun describe(obj: Any): String =
when (obj) {
    1          -> "One"
    "Hello"    -> "Greeting"
    is Long    -> "Long"
    !is String -> "Not a string"
    else       -> "Unknown"
}
```

## ranges

可以使用 in 操作符检查数值是否在某个范围内，或不在范围内：

```
val x = 10
val y = 9
if (x in 1..y+1) {
    println("fits in range")
}

val list = listOf("a", "b", "c")
if (-1 !in 0..list.lastIndex) {
    println("-1 is out of range")
}
if (list.size !in list.indices) {
    println("list size is out of valid list indices range too")
}
```

使用范围内迭代：

```
for (x in 1..5) {
    print(x)
}
```

或者使用步进：

```
for (x in 1..10 step 2) {
    print(x)
}
for (x in 9 downTo 0 step 3) {
    print(x)
}
```

