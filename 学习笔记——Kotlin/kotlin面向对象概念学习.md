@[TOC](目录)

# 简述

> Kotlin 是支持面向对象编程方式的，而面向对象编程，**“类”** 和 **“对象”** 是重要的两个概念。类:用于描述客观世界中某一类对象的共同特征，而对象：是类的具体存在。
>
> 面向对象的三大特征：封装（private、protected、internel、public访问控制符来实现良好的封装）、继承（子类继承父类就可以继承父类的属性和方法，访问权限允许的话也可以直接调用父类的方法）、多态

# 类（class）和对象(object)

可以把类当作一种自定义的类型，可以用它定义变量，它定义的变量都是引用类型

## 定义类

```kotlin
[修饰符] class 类名 [constructor 主构造器]{
  零个到多个次构造器定义...
  属性...
  方法...
}
```

```kotlin
class Person{
  
}
```

#### 修饰符：（可以修饰类、属性、方法）

1. private、protected、internel、public （只能同时存在一个）

2. final、open、abstract（只能同时存在一个），**open与final意思相反，open修饰类表示可以被继承，修饰方法、属性可以被重写** ，如：```public open class Person{}```

#### 构造器：一个类创建对象的根本途径

1. 主构造器：定义类是紧跟类名后面，使用关键字 **constructor(属性定义)** ，如果没有任何注解或修饰符，可以省略关键字 **constructor**，如：```class Person( name: String){}```  ，***主构造器里面可以直接使用var、val定义属性，而且kotlin也会自动生成对应的get、set方法***

2. 如果没有为非抽象类，定义主（次）构造器，kotlin会自动生成一个无参的主构造器，默认public

3. 次构造器可以没有或多个

#### 定义类中——属性

1. 关键字：var、val

   var：定义读写属性

   val：定义只读属性，不能修改

2. Get\Set方法：

   可以自定义属性的get、set方法，如果没有自定义，kotlin会为var属性自动生成get、set方法，为val生成get方法

#### 定义类中——方法

语法和定义函数一样

```kotlin
class Person{
  var name = ""
  var age = 0
  
  fun sayMsg(msg:String){
    println(msg)
  }
}
```



##  使用类

1. 定义变量
2. 创建对象
3. 派生子类

### 对象的产生和使用

不需要使用new关键字，直接使用对象的构造器，就可以生产对象

```kotlin
val person = Person("xxx", 29)
person.age										// 访问属性
person.name = "ssssss" 				// 修改属性值
person.sayMsg("hello world") 	// 调用方法
```



## 类方法转函数（类名::方法名）

```kotlin
class Person(){
  fun sayMsg(){
    println("hello world")
  }
}

fun main(){
	val p =  Person()
  val sayMsg = Person::sayMsg // 将Person类中的方法转成函数并赋值给变量
  sayMsg(p)
}
```



## 类方法调用——中辍表示法

使用关键字 **infix** 修饰方法，注意：此方法**只能有一个形参** ，语法糖

```"s" to "123" 相当于 Pair("s","123")```

```kotlin

class Total(var nameList: ArrayList<String>) {

}

class Person(var name: String, val age: Int) {

    var nameList = arrayListOf<String>()

    infix fun add(p: Person): Total {
        nameList.add(this.name)
        nameList.add(p.name)
        return Total(nameList)
    }
}

fun main(){
  val p = Person("ssss",29)
  p add Person("xxxx",29)
}
```



## componentN 方法与解构

kotlin允许将一个对象的N个属性“解构”给多个变量

```kotlin
class User(var name:String, var age:Int){
  operator fun component1() = name
  operator fun component2() = age
}

fun main(){
	val (name, age) = User("www",29)
  // 这个时候，User对象，属性值就可以直接“解构”出来
}
```

**注意**

1. 必须提供operator fun componentN()的方法
2. “解构时，不需要某个属性，可以使用 _ 跳过 ”，```val(_ , age) = user```，这样就不解构name属性



### 通过Map的遍历，理解对象的解构

```kotlin
fun main() {
  	// 初始化map
    val map = mapOf<String, Int>("key1" to 100, "key2" to 101)
		
  	// 遍历，
    for ((key, value) in map) {
        println(key)
        println(value)
    }
}
```

1. map作为可迭代对象，迭代出来的元素类型为Map.Entry

2. kotlin给Map.Entry对象添加了componentN方法

   ```kotlin
   public inline operator fun <K, V> Map.Entry<K, V>.component1(): K = key
   
   public inline operator fun <K, V> Map.Entry<K, V>.component2(): V = value
   ```

3. 所有遍历map可以写成上面那样



### 数据类（默认支持解构，函数通过数据类返回多个值）

1. 数据类使用 data 修饰，```data class Person(var name:String, var age:Int)```
2. 主构造器至少需要有一个参数，所有参数需要用 var、 val 声明为属性 
3. 数据类不能用 abstract、open、sealed 修饰，也不能定义成内部类 
4. kotlin自动生成 quals()、hashCode() 方法，自动重写 toString()方法
5. **为每个属性自动生成 operator 修饰的 omponentN（）方法 〉**，这样就支持了解构
6. 生成copy()方法用于完成对象复制

```kotlin
data class Person(var name:String, var age:Int)

fun main(){
  val p = Person("xxx",29)
  val (name, age) = p
}
```



## 自定义属性的getter、setter方法

定义 getter、setter方法时无须使用 fun 关键字

```kotlin
class User(name:String, age:Int){
  var name =  name
  var age = age
  
  var userInfo
  	get() = "$name $age"
  	set(value){
			if("-" in value){
        name = "xxxxxx"
        age = 29292
      }
    }
}
```





