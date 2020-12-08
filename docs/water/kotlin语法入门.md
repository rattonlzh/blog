# kotlin语法入门

主函数和打印函数

``` kotlin
fun main() {
    println("Hello world!")
}
```

包声明和包导入和java一样

``` kotlin
package my.demo

import kotlin.text.*

// ...
```

简写抛出异常

``` kotlin
val s = person.name ?: throw IllegalArgumentException("Name required")
```

函数定义

``` kotlin
// first
fun sum(a: Int, b: Int): Int {
    return a + b
}

// second
fun sum(a: Int, b: Int) = a + b

// third
fun printSum(a: Int, b: Int): Unit {
    println("sum of $a and $b is ${a + b}")
}

// fourth
fun printSum(a: Int, b: Int) {
    println("sum of $a and $b is ${a + b}")
}
```



只读变量

``` kotlin
val a: Int = 1  // immediate assignment
val b = 2   // `Int` type is inferred
val c: Int  // Type required when no initializer is provided
c = 3     
```

字符串替换

``` kotlin
val s2 = "${s1.replace("is", "was")}, but now is $a"
```

if-else结构与java一致，但支持range

``` kotlin
if (x in 1..y+1) {
    println("fits in range")
}
```

多分支结构when，类似java的switch

``` kotlin
fun describe(obj: Any): String =
    when (obj) {
        1          -> "One"
        "Hello"    -> "Greeting"
        is Long    -> "Long"
        !is String -> "Not a string"
        else       -> "Unknown"
    }

// another
//是否在collections中
when {
    "orange" in items -> println("juicy")
    "apple" in items -> println("apple is fine too")
}
```



三元条件表达式

``` kotlin
fun maxOf(a: Int, b: Int) = if (a > b) a else b
```

可选null

```kotlin
fun parseInt(str: String): Int? {
    // ...
}
```

类型检查

``` kotlin
fun getStringLength(obj: Any): Int? {
    if (obj !is String) return null

    // `obj` is automatically cast to `String` in this branch
    return obj.length
}
```

for循环

``` kotlin
val items = listOf("apple", "banana", "kiwifruit")
// in
for (item in items) {
    println(item)
}
// step
for (x in 1..10 step 2) {
    print(x)
}
//reverse
for (x in 9 downTo 0 step 3) {
    print(x)
}
```

while循环与java一致

迭代器

``` kotlin
val numbers = mutableListOf("one", "four", "four") 
val mutableListIterator = numbers.listIterator()
mutableListIterator.next()
mutableListIterator.add("two")
mutableListIterator.next()
mutableListIterator.set("three")   
println(numbers)
```

默认it

``` kotlin
val fruits = listOf("banana", "avocado", "apple", "kiwifruit")
fruits
  .filter { it.startsWith("a") }
  .sortedBy { it }
  .map { it.toUpperCase() }
  .forEach { println(it) }
```

类声明和构造方法

``` kotlin
class InitOrderDemo(name: String) {
    val firstProperty = "First property: $name".also(::println)
    
    init {
        println("First initializer block that prints ${name}")
    }
    
    val secondProperty = "Second property: ${name.length}".also(::println)
    
    init {
        println("Second initializer block that prints ${name.length}")
    }
}

```

类声明即加注解的主构造方法

``` kotlin
class Customer public @Inject constructor(name: String) { /*...*/ }
```

备用构造方法

``` kotlin
class Person {
    var children: MutableList<Person> = mutableListOf<>()
    constructor(parent: Person) {
        parent.children.add(this)
    }
}

class Person(val name: String) {
    var children: MutableList<Person> = mutableListOf<>()
    constructor(name: String, parent: Person) : this(name) {
        parent.children.add(this)
    }
}
```

封闭构造方法

``` kotlin
class DontCreateMe private constructor () { /*...*/ }
```

开放继承，默认不能被继承

``` kotlin
open class Base //Class is open for inheritance
```

构造父类

``` kotlin
open class Base(p: Int)

class Derived(p: Int) : Base(p)
```

``` kotlin
// 备用构造方法调用父类构造方法
class MyView : View {
    constructor(ctx: Context) : super(ctx)

    constructor(ctx: Context, attrs: AttributeSet) : super(ctx, attrs)
}
```

声明可重载

``` kotlin
open class Shape {
    open fun draw() { /*...*/ }
    fun fill() { /*...*/ }
}

class Circle() : Shape() {
    override fun draw() { /*...*/ }
}
```

不能用val重载var

```kotlin
interface Shape {
    val vertexCount: Int
}

class Rectangle(override val vertexCount: Int = 4) : Shape // Always has 4 vertices

class Polygon : Shape {
    override var vertexCount: Int = 0  // Can be set to any number later
}
```

接口

``` kotlin
interface Shape {
    val vertexCount: Int
}

class Rectangle(override val vertexCount: Int = 4) : Shape // Always has 4 vertices

class Polygon : Shape {
    override var vertexCount: Int = 0  // Can be set to any number later
}

```

内部类与外部类

``` kotlin
class FilledRectangle: Rectangle() {
    fun draw() { /* ... */ }
    val borderColor: String get() = "black"
    
    inner class Filler {
        fun fill() { /* ... */ }
        fun drawAndFill() {
            super@FilledRectangle.draw() // Calls Rectangle's implementation of draw()
            fill()
            println("Drawn a filled rectangle with color ${super@FilledRectangle.borderColor}") // Uses Rectangle's implementation of borderColor's get()
        }
    }
}

```

多继承

``` kotlin
open class Rectangle {
    open fun draw() { /* ... */ }
}

interface Polygon {
    fun draw() { /* ... */ } // interface members are 'open' by default
}

class Square() : Rectangle(), Polygon {
    // The compiler requires draw() to be overridden:
    override fun draw() {
        super<Rectangle>.draw() // call to Rectangle.draw()
        super<Polygon>.draw() // call to Polygon.draw()
    }
}

```

抽象类

``` kotlin
open class Polygon {
    open fun draw() {}
}

abstract class Rectangle : Polygon() {
    abstract override fun draw()
}

```

collections

``` kotlin
fun printAll(strings: Collection<String>) {
        for(s in strings) print("$s ")
        println()
    }
    
fun main() {
    val stringList = listOf("one", "two", "one")
    printAll(stringList)
    
    val stringSet = setOf("one", "two", "three")
    printAll(stringSet)
```

mutable可变集合(支持读写)

```
MutableListOf
MutableMapOf
MutableSetOf
```

增删查api

```
.add
.remove
.contains
.count
```

Map默认是红黑树实现

HashMap是散列表

zip类似python的zip

entries类似java的Map.Entry

聚集运算

```kotlin
val numbers = listOf("one", "two", "three", "four", "five", "six")

println(numbers.groupingBy { it.first() }.eachCount())
```

 

别名

```kotlin
typealias NodeSet = Set<Network.Node>
```

 

安全类型

默认不允许为null

```kotlin
var a: String = "abc" // Regular initialization means non-null by default
a = null // compilation error
```

问号允许为null

```kotlin
var a: String? = "abc" // Regular initialization means non-null by default
a = null // ok
```

强制非空

```kotlin
val l = b!!.length// 如果b为空则抛出异常
```

 

安全转换

```kotlin
val aInt: Int? = a as? Int//如果转换失败则为null
```

 

过滤可空集合的空元素

```kotlin
val nullableList: List<Int?> = listOf(1, 2, null, 4)

val intList: List<Int> = nullableList.filterNotNull()
```

 

 