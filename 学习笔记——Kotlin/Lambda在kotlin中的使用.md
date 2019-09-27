@[TOC](目录)

# 概述

> lambda表达式：可以传递给其他函数的一小段代码，可以把通用的代码抽取成函数，简化开发，避免繁琐冗余的匿名内部类的实现方式。

```Java
// Java版，给按钮点击事件
button.setOnClickListener(new OnClickListener(){
  @Override
	public void onClick (View view) { 
		// 执行逻辑
  }
});
```

```kotlin
button.setOnClickListener{
  // 执行逻辑
};
```



# Lambda表达式语法

> 它可以被保存在一个变量中，可以当作值进行传递，也可以直接声明并传递给函数

## 声明

```kotlin
{a: Int, b: Int -> a + b }
```

1. **{} ** 必不可少
2. **->** 分隔了参数和函数体， **左边参数列表** ； **右边函数体**
3. 可以 **将其声明保存为变量**，把这个变量当作普通函数对待

```kotlin
val add = { a: Int, b: Int -> a + b }    
println(add(10, 10))
// >> 20
```

4. 也可以直接进行调用， **不建议** ，可以使用run{}库函数代替

```kotlin
{ a: Int, b: Int -> a + b }(10, 10)
// >> 20
```

5. 函数参数声明为Lambda表达式语法

```kotlin
fun add(result: (Int, Int) -> Int)
```



## 约定说明（kotlin对Lambda表达式使用的）

1. Lambda表达式是函数调用的 **最后一个参数**，它可以放在括号外面
2. Lambda表达式是函数的唯一参数，则调用函数的 **()** 可以省略
3. 如果Lambda参数的类型可以被推导出来，就不要显式地指定它，比如下面的resultCode，可以推导出类型为Int
4. 如果当前上下文期望的是 **只有一个参数的lambda表达式** 且这个参数的类型可以推断出来，那么可以使用默认参数名称 it 代替命名参数

```kotlin
fun main() {
		// 模拟一个函数，接受一个lambda函数（一个Int类型参数，无返回类型）
    fun doXXX(result: (Int) -> Unit) {
        result(200)
    }
		
  	// 调用 
    doXXX{ resultCode ->
        if (resultCode == 200) {
            println("执行成功了")
        }
    }
  
  	// 使用默认参数名称 it 代替命名参数
    doXXX(2000) {
        if (it == 200) {
            println("执行成功了")
        }
    }
}
// >> 执行成功了
```



## Lambda在作用域中访问变量

> 在函数内部使用 lambda, 可以访问这个函数的参数，还有在 lambda 之前定义的局部变量。

```kotlin
fun main() {
    fun doXXX(p1: String, doing: (String, String) -> Unit) {
        val p2 = "我是局部变量"
        doing(p1, p2)
    }
	
    doXXX("我是实参1") { p1, p2 ->
        // 打印出实参，局部变量            
        println("$p1 $p2")
    }
}
```



## 成员引用 (::运符) 在lambda里的使用

> 已经定义了一个函数或者方法，想把这个函数或方法当作lambda传递给函数，可以通过一个lambda调用定义的函数或方法，这未免有点累赘，而 **::** 成员引用运算符，它可以创建一个 **调用单个方法** 或者 **访问单个属性的函数值** ，:: 把类名和要引用的成员（一个方法或者一个属性）名称分隔开——为了简化操作，**如果是顶层函数，:: 右边的类名可以省略**

```kotlin
fun doPhone(phone: String) {
    println("打电话给 $phone")
}

fun doXXX(doing: (String) -> Unit) {
    doing("1333333333");
}

fun main() {

    doXXX(::doPhone)	// 通过成员引用简化调用

    doXXX { doPhone(it) } // 通过lambda调用定义好的函数
}
// >> 打电话给 1333333333
```

- 类的成员方法的引用

```kotlin
// 如果是类的成员方法的引用
Person::age 等同于 {p:Person -> p.age}
```

- 类构造方法的引用——存储或者延期执行创建类的实例的动作

```kotlin
data class Person(val name:String)

fun main(){
  val createPerson = ::Person
  val person = createPerson("wlDayDayUp")
  println(person)
}
```

- 扩展函数的引用

```kotlin
fun String.doXXX() = "$this Hi !"

fun main() {
	val doing =	String::doXXX // doing 变量就是函数值（lambda表达式）{a:String -> "$a Hi !"}
  
  doing("123")
}

// >> 1213 Hi ！
```



#集合的函数式API

> 集合的函数式API，最大的好处就是简化代码，在这些API中，filter、map函数是基础

## filter

> filter函数遍历集合并选出应用 **给定lambda后返回true** 的那些元素，**结果是一个新的集合** 
>
> **作用** ：filter 可以移除集合不想要的元素，过滤元素

```kotlin
fun main(){
  val list = listOf<Int>(1, 2, 3, 4, 5, 6, 7, 8, 9, 10)

  val filter = list.filter { it % 2 == 0 }

  println(filter)
}
// >> [2, 4, 6, 8, 10]
```

## map

> map函数，对集合中的每一个元素应用给定的函数并把结果收集到一个新集合
>
> **作用** : 元素的交换

```kotlin
fun main(){
  val list = listOf<Int>(1, 2, 3, 4, 5, 6, 7, 8, 9, 10)
  
  val map = list.map{
    it * it
  }
  
  println(map)
}
// >> [1, 4, 9, 16, 25, 36, 49, 64, 81, 100]
```

## 组合使用filter、map

> 最后结果需要获取年龄大于30的人名字

```kotlin
data class Person(val name: String, val age: Int)

fun main(){
  val persons = listOf<Person>(
        Person("p1", 28),
        Person("p2", 21),
        Person("p3", 33),
        Person("p4", 38)
    )
		
  	// 注意，这部分可以用成员引用，简写
    val person = persons.filter { it.age > 30 }.map { it.name }
		// 简写
	  // val person = persons.filter { it.age > 30 }.map(Person::name)
  
    println(person)
}
```

## Map数据类型（filterKeys、filterValues、mapKeys、mapValues）

> filterKeys、mapKeys: 分别对map的key过滤和替换
>
> filterValues、mapValues: 分别对map的Value过滤和替换

```kotlin
fun main(){
   val mapOf = mapOf<Int, String>(0 to "xxx", 1 to "yyy")

   val mapValues = mapOf.mapValues { it.value.toUpperCase() }

   println(mapValues)
}
// >> {0=XXX, 1=YYY}
```

​																																		

**——未完待续**

