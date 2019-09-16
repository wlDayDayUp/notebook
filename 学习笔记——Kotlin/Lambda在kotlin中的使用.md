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



## 成员引用 (::运符)

> :: 运算符可以将函数转换成值，这样就可以当做函数参数传递，:: 称为成员引用，简化了，创建一个调用单个方法或者访问单个属性的函数值，:: 把类名和要引用的成员（一个方法或者一个属性）名称分隔开

```kotlin
val getAge = {person: Person -> person.age}
// 等同于
val getAge = Person::age
```



#Lambda与集合