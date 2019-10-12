@[TOC](目录)

# 简述

> 函数是执行特定任务的一段代码，实现代码的复用。
>
> Kotlin支持局部函数——函数里面的函数，是Lambda的基础
>
> Lambda 表达式可以作为 函数参数，函数返回值，让程序更加简洁

# 函数定义、调用

```kotlin
fun 函数名(形参列表) [ : 返回类型] {
  // 函数体，如果函数有返回值，需要return
}

// 实例1，没有返回类型，省略了返回类型 : Unit
fun eat(food: String) {
    println("正在吃 $food")
}

// 实例2，计算两个数的和，有返回类型
fun addNum(x:Int,y:Int):Int{
    return x + y
}

// 实例3，如果函数体只需要返回一个表达式结果，如实例2，可以简写：这个时候Kotlin会根据上下文推导出返回类型
fun addNumNew(x:Int,y:Int) = x + y

fun main() {
  eat("汉堡🍔")
  addNum(10,10)
  addNumNew(20,20)
}
```

## 1. 函数形参

### 1.1 可以给函数形参设置默认值，调用函数的时候可以不传值

```kotlin
fun sayHello(msg :String = "Hello World")
```

### 1.2 调用函数时，可以以命名参数的方式

```kotlin
sayHello(msg = "你好👋")
```

### 1.3 可变长形参，参数变成一个数组

```kotlin
fun sayHello(vararg msg: String) {
    for (s in msg) {
        println(s)
    }
}

fun main() {
    sayHello("1","2","3","4")
}
```

## 2. 函数返回类型

1. 无返回类型，可以显示指定返回类型为 :Unit ，也可以省略
2. 返回类型为单一表达式，可以使用 = 且省略{}, 如上实例3

## 3. 递归、尾递归函数

函数递归：函数体内调用自身，隐式循环，需要注意停下来的条件
尾递归：是Kotlin支持的一种编程方式，当函数将调用自身作为它执行的最后一行代码， 且递归调用后没有更多代码时，可使用尾递归语法。尾递归不能在异常处理的位 catch finally 块中使用，需要使用 tailrec 修饰。

```kotlin
// 递归
fun fact(n: Int): Int {
    return if(n == 1){
        1
    }else{
        n * fact(n - 1)
    }
}
// 尾递归
tailrec fun factNew(n: Int, value: Int = 1): Int =
    if (n == 1) value else factNew(n - 1, value * n)

fun main() {
    println(fact(3))
}
```

## 4. 函数重载

与Java 类似，Kotlin 许定义多个同名函数，只要形参列表 **或** 返回值类型不同

## 5. 局部函数

在函数体内定义的函数称为局部函数，**默认情况下，局部函数对外是隐蔽的，这时它只在封闭它的函数内有效，而封闭函数也可以把局部函数返回出去，这样局部函数的作用域就变大了

```kotlin
fun test(type: String, a: Int, b: Int): Int {
  // 局部函数
	fun add(a: Int, b: Int) = a + b

	fun sub(a: Int, b: Int) = a - b

	fun defualtFun(a: Int, b: Int) = a * b

	return when (type) {
       "add" -> add(a, b)
       "sub" -> sub(a, b)
       else -> defualtFun(a, b)
   }
}

// 将局部函数返回出去
fun testNew(type: String): (Int, Int) -> Int {
    fun add(a: Int, b: Int) = a + b

    fun sub(a: Int, b: Int) = a - b

    fun defualtFun(a: Int, b: Int) = a * b

    return when (type) {
        "add" -> ::add // 返回局部函数
        "sub" -> ::sub
        else -> ::defualtFun
    }
}

fun main(){
	testNew("add")(10,10)
}
```

## 6. 高阶函数

Kotlin中的函数有属于自己的 **类型** ，**函数类型，和其他类型一样，都可以定义变量、作为函数的形参类型、作为函数的返回值类型** ，通过函数类型变量，可以让程序更加灵活

### 6.1 使用函数类型

#### 6.1.1 函数类型：**形参列表 -> 返回值类型** 如: ``` (Int, Int) -> Int``` ，如果没有返回值，且形参只有一个，可以省略->返回值类型，直接（Int）

#### 6.1.2 使用函数类型定义变量

```kotlin
fun main{
  // 定义一个函数类型变量f1，接收两个Int类型值，返回Int结果
  var f1 : (Int, Int) -> Int
  // 定义一个函数类型变量f2，接收String类型参数，无返回值
  var f2 : (String)
  
  f1(10,10) // 
}
```

#### 6.1.3 给函数类型变量赋值

```kotlin
fun add(a: Int, b:Int) = a + b

// 可以直接使用lambda表达式，赋值
fun main(){
  // 声明并初始化函数类型变量 f1
	var f1 : (Int, Int) -> Int = { a, b -> a + b }
  f1(10, 20)
 	
  // 通过 ::函数名，获取函数的引用
  var f2 :(Int, Int) -> Int = ::add
  f2(10, 20)
}
```

**备注：**

>  当直接访问某个函数的函数引用，而不是调用函数时，需要在函数名前添加两个冒号，而且不能在函数后面添加圆括号一一 旦添加圆括号，就变成了调用函数，而不是访问函数引用。

#### 6.1.4 使用函数类型作为形参类型

有时候需要定义一个函数，该函数的大部分计算逻辑都能确定，但某些处理逻辑暂时无法确定，需要动态改变，如果希望调用函数时能动态传入这些代码，就需要在函数中定义函数类型的形参，这样即可在调用该函数时传入不同的函数作为参数即可

```kotlin
fun f1(listData: List<Int>, fn: (Int) -> Int): ArrayList<Int> {
    // 存放结果的列表
    val result: ArrayList<Int> = ArrayList()
    // 遍历数据源
    for (d in listData) {
        // 执行逻辑，并保存结果
        result.add(fn(d))
    }
    return result
}

fun main(){
	val resultList = f1(arrayListOf(1, 2, 3, 4, 5, 6)) {
		it * 2
	}
	println(resultList)
}
```

#### 6.1.5 使用函数类型变量作为函数返回值

```kotlin
// 将局部函数返回出去
fun testNew(type: String): (Int, Int) -> Int {
    fun add(a: Int, b: Int) = a + b

    fun sub(a: Int, b: Int) = a - b

    fun defualtFun(a: Int, b: Int) = a * b

    return when (type) {
        "add" -> ::add // 返回局部函数
        "sub" -> ::sub
        else -> ::defualtFun
    }
}

// 使用Lambda 表达式代替局部变量的返回
fun test(type: String): (Int, Int) -> Int {
    return when (type) {
        "add" -> { a, b -> a + b }
        "sub" -> { a, b -> a - b }
        else -> { a, b -> a * b }
    }
}

```

## 7. Lambda 表达式

Lambda 表达式，本质上就是一个代码块，可以 **直接复制给变量（函数类型变量）** 或 **直接调用**

#### 7.1 语法

```kotlin
{
  (形参列表) -> 
 		// 函数执行体
}
```

#### 7.2 使用Lambda表达式

```kotlin
fun main(){
  // 这个时候不需要显示写f1的类型，kotlin自动推到可以确定
	val f1 = { a:Int, b:Int -> a + b }
  
  // 直接调用 { a:Int, b:Int -> a + b }(10, 11)
  f1(10, 11)
}
```

**备注：**

> 如果函数参数，最后一个是Lambda表达式，可以将其放在 () 外面，(){ .... }

## 8. 内联函数

函数调用会产生 定的时间和空间开销，如果被调用的表达式或函数的代码量本身不大，而且该表达式或函数经常被调用，可以考虑使用 **内联函数** ——为了避免产生函数调用的过程，我们可以考虑直接把被调用的表达式或函数的代码“嵌入” 原来的执行流中，就是编译器负责去“复制、粘贴” 到原来的执行代码中

### 8.1 使用内联函数——inline关键字修饰带函数形参的函数

```kotlin
inline fun f1(i: Int, fn: (Int) -> Int) = fn(i)

fun main() {
    println(f1(1000){
        it * 2
    })
}
// 内联函数f1，相当于把{it * 2}，直接拷贝到f1里面了，这个是编译器完成的
```

### 8.2 部分禁止内联——noinline

```kotlin
inline fun f1(i: Int, fn: (Int) -> Int, noinline fn2:(String) ) = fn(i)
```

