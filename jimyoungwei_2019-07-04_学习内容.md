### Kotlin基础学习内容

***

#### 类的修饰符

>类属性修饰符。标准类本身特性。
>
>| 修饰符     | 相关成员               | 评注                                          |
>| ---------- | ---------------------- | --------------------------------------------- |
>| final      | 不可继承，不能被重写   | 类中成员默认使用                              |
>| abstract   | 必须被重写             | 只能在抽象类中使用；抽象成员不能有实现        |
>| enum       | ——                     | 枚举类                                        |
>| open       | 可以被重写             | 需要明确地表明                                |
>| annotation | ——                     | 注解类                                        |
>| override   | 重写父类或接口中的成员 | 如果没有使用final表明，重写的成员默认是开放的 |
>
>访问权限修饰符
>
>| 修饰符         | 类成员       | 顶层声明     |
>| -------------- | ------------ | ------------ |
>| public（默认） | 所有地方可见 | 所有地方可见 |
>| internal()     | 模块中可见   | 模块中可见   |
>| protected      | 子类中可见   | ——           |
>| private        | 类中可见     | 文件中可见   |
>
>注意：kotlin中一个外部类不能看到其内部（或者嵌套）类中的private成员。

#### 主构造器

>kotlin 中的类可以有一个 主构造器，以及一个或多个次构造器，主构造器是类头部的一部分，位于类名称之后: 
>
>```
>class Person constructor(firstName: String) {}
>```
>
>如果主构造器没有任何注解，也没有任何可见度修饰符，那么constructor关键字可以省略。如果构造器有注解，或者有可见度修饰符，这时constructor关键字是必须的，注解和修饰符要放在它之前。
>
>```
>class Person(firstName: String) {
>}
>```
>
>主构造器中不能包含任何代码，初始化代码可以放在初始化代码段中，初始化代码段使用 init 关键字作为前缀。
>
>```
>class Person constructor(firstName: String) {
>    init {
>        println("FirstName is $firstName")
>    }
>}
>```
>
>注意：主构造器的参数可以在初始化代码段中使用，也可以在类主体定义的属性初始化代码中使用。
>一种简洁语法，可以通过主构造器来定义属性并初始化属性值（可以是var或val）。
>
>```
>class People(val firstName: String, val lastName: String) {
>    //...
>}
>```

#### 次构造函数

>类也可以有二级构造函数，需要加前缀 constructor:
>
>```
>class Person { 
>    constructor(parent: Person) {
>        parent.children.add(this) 
>    }
>}
>```
>
>如果类有主构造函数，每个次构造函数都要，或直接或间接通过另一个次构造函数代理主构造函数。在同一个类中代理另一个构造函数使用 this 关键字：
>
>```
>class Person(val name: String) {
>    constructor (name: String, age:Int) : this(name) {
>        // 初始化...
>    }
>}
>```
>
>如果一个非抽象类没有声明构造函数(主构造函数或次构造函数)，它会产生一个没有参数的构造函数。构造函数是 public 。如果不想该类有公共的构造函数，就需要声明一个私有的空的主构造函数：
>
>```
>class DontCreateMe private constructor () {
>}
>```
>
>可以像函数参数一样为构造方法参数声明一个默认值：
>
>```
>class User(val name: String, val isSubscribed: Boolean = true)
>```
>
>注意：如果所有的构造方法参数都有默认值，编译器会生成一个额外的不带参数的构造方法来使用所有的默认值。

#### 抽象类

>抽象是面向对象编程的特征之一，类本身，或类中的部分成员，都可以声明为abstract的。抽象成员在类中不存在具体的实现。 
>
> 注意：无需对抽象类或抽象成员标注open注解。
>
>```
>open class Base {
>    open fun f() {}
>}
>
>abstract class Derived : Base() {
>    override abstract fun f()
>}
>```

#### 嵌套类

>```
>class Outer {                  // 外部类
>    private val bar: Int = 1
>    class Nested {             // 嵌套类
>        fun foo() = 2
>    }
>}
>
>fun main(args: Array<String>) {
>    val demo = Outer.Nested().foo() // 调用格式：外部类.嵌套类.嵌套类方法/属性
>    println(demo)    // == 2
>}
>```
>
>注意：嵌套类不是内部类，不能访问外部类的实例。

#### 内部类

>内部类使用 inner 关键字来表示。内部类会带有一个对外部类的对象的引用，所以内部类可以访问外部类成员属性和成员函数。
>
>```
>class Outer {
>    private val bar: Int = 1
>    var v = "成员属性"
>    /**嵌套内部类**/
>    inner class Inner {
>        fun foo() = bar  // 访问外部类成员
>        fun innerTest() {
>            var o = this@Outer //获取外部类的成员变量
>            println("内部类可以引用外部类的成员，例如：" + o.v)
>        }
>    }
>}
>
>fun main(args: Array<String>) {
>    val demo = Outer().Inner().foo()
>    println(demo) 
>    //输出结果   1
>    val demo2 = Outer().Inner().innerTest()     // 内部类，Outter后边有括号
>    println(demo2)   
>    //输出结果    内部类可以引用外部类的成员，例如：成员属性
>}
>```
>
>注意：要访问来自外部作用域的 this，使用this@label，其中 @label 是一个 代指 this 来源的标签。

#### 匿名内部类

>使用对象表达式来创建匿名内部类：
>
>```
>class Test {
>    var v = "成员属性"
>
>    fun setInterFace(test: TestInterFace) {
>        test.test()
>    }
>}
>
>/**
> * 定义接口
> */
>interface TestInterFace {
>    fun test()
>}
>
>fun main(args: Array<String>) {
>    var test = Test()
>
>    /**
>     * 采用对象表达式来创建接口对象，即匿名内部类的实例。
>     */
>    test.setInterFace(object : TestInterFace {
>        override fun test() {
>            println("对象表达式创建匿名内部类的实例")
>        }
>    })
>}
>```
>
>注意：特别注意这里的 **object : TestInterFace**，这个 object 是 Kotlin 的关键字，指对象声明。要实现匿名内部类，就必须使用 object 关键字，不能随意替换其它单词。

#### 数据类

>Kotlin 可以创建一个只包含数据的类，关键字为 data：
>
>```
>data class User(val name: String, val age: Int)
>```
>
>编译器会自动的从主构造函数中根据所有声明的属性提取以下函数：
>
>-  `equals()` / `hashCode()` 
>-  `toString()`  格式如 `"User(name=jimyoungwei, age=23)"` 
>-  `componentN() functions` 对应于属性，按声明顺序排列
>-  `copy()` 函数
>
> 如果这些函数在类中已经被明确定义了，或者从超类中继承而来，就不再会生成。 
>
> 为了保证生成代码的一致性以及有意义，数据类需要满足以下条件：
>
>- 主构造函数至少包含一个参数。 
>- 所有的主构造函数的参数必须标识为`val` 或者 `var` ;
>- 数据类不可以声明为 `abstract`, `open`, `sealed` 或者 `inner`;
>- 数据类不能继承其他类 (但是可以实现接口)。
>
>复制使用 copy() 函数，可以使用该函数复制对象并修改部分属性, 对于上文的 User 类，使用copy类赋值User数据类并修改age属性 。
>
>```
>data class User(val name: String, val age: Int)
>
>fun main(args: Array<String>) {
>    val jimyoungwei = User(name = "jimyoungwei", age = 23)
>    val olderName = jimyoungwei.copy(age = 2)
>    println(jimyoungwei)
>    println(olderName)
>}
>//输出结果  User(name=jimyoungwei, age=23)
>           User(name=jimyoungwei, age=2)
>```
>
>组件函数允许数据类在解构声明中使用：
>
>```
>val jimyoungwei = User("jimyoungwei", 23)
>val (name, age) = jimyoungwei
>println("$name, $age years of age") 
>//输出结果   jimyoungwei, 23 years of age
>```

#### 密封类

>密封类用来表示受限的类继承结构：当一个值为有限几种的类型, 而不能有任何其他类型时。在某种意义上，他们是枚举类的扩展：枚举类型的值集合 也是受限的，但每个枚举常量只存在一个实例，而密封类 的一个子类可以有可包含状态的多个实例。声明一个密封类，使用 sealed 修饰类，密封类可以有子类，在kotlin1.1之后其子类可以在同一文件的任何位置定义。sealed 不能修饰 interface ,abstract class(会报 warning,但是不会出现编译错误)。
>
>```
>sealed class Expr
>data class Const(val number: Double) : Expr()
>data class Sum(val e1: Expr, val e2: Expr) : Expr()
>object NotANumber : Expr()
>
>fun eval(expr: Expr): Double = when (expr) {
>    is Const -> expr.number
>    is Sum -> eval(expr.e1) + eval(expr.e2)
>    NotANumber -> Double.NaN
>}
>//使用密封类的关键好处在于使用when表达式的时候，如果能够验证语句覆盖了所有情况，就不需要为该语句再添加一个else子句了。
>```

#### 类委托

>在委托模式中，有两个对象参与处理同一个请求，接受请求的对象将请求委托给另一个对象来处理。kotlin 直接支持委托模式，更加优雅，简洁。kotlin 通过关键字 by 实现委托。
>
>类的委托即一个类中定义的方法实际是调用另一个类的对象的方法来实现的。以下实例中派生类 Derived 继承了接口 Base 所有方法，并且委托一个传入的 Base 类的对象来执行这些方法。
>
>```
>// 创建接口
>interface Base {   
>    fun print()
>}
>
>// 实现此接口的被委托的类
>class BaseImpl(val x: Int): Base {
>    override fun print() { print(x) }
>}
>
>// 通过关键字 by 建立委托类
>class Derived(b: Base): Base by b
>
>fun main(args: Array<String>) {
>    val b = BaseImpl(10)
>    Derived(b).print() // 输出 10
>}
>```
>
>注意：在 Derived 声明中，by 子句表示，将 b 保存在 Derived 的对象实例内部，而且编译器将会生成继承自 Base 接口的所有方法, 并将调用转发给 b。

#### 实现接口

>kotlin 接口与 java 8 类似，使用 interface 关键字定义接口，允许方法有默认实现。一个类或者对象可以实现一个或多个接口。接口的成员默认都是open的。
>
>```
>interface MyInterface {
>    fun bar()    // 未实现
>    fun foo() {  // 默认实现 
>      // 可选的方法体
>      println("foo")
>    }
>}
>class Child : MyInterface {
>    override fun bar() {
>        // 方法体
>        println("bar")
>    }
>}
>fun main(args: Array<String>) {
>    val c =  Child()
>    c.foo();
>    c.bar();
>}
>//输出结果  foo
>           bar
>```

#### 接口中的属性

>接口中的属性只能是抽象的，不允许初始化值，接口不会保存属性值，实现接口时，必须重写属性：
>
>```
>interface MyInterface{
>    var name:String //name 属性, 抽象的
>}
> 
>class MyImpl:MyInterface{
>    override var name: String = "jimyoungwei" //重写属性
>}
>```

#### 函数重写

>实现多个接口时，可能会遇到同一方法继承多个实现的问题。
>
>```
>interface A {
>    fun foo() { print("A") }   // 已实现
>    fun bar()                  // 未实现，没有方法体，是抽象的
>}
> 
>interface B {
>    fun foo() { print("B") }   // 已实现
>    fun bar() { print("bar") } // 已实现
>}
> 
>class C : A {
>    override fun bar() { print("bar") }   // 重写
>}
> 
>class D : A, B {
>    override fun foo() {
>        super<A>.foo()
>        super<B>.foo()
>    }
> 
>    override fun bar() {
>        super<B>.bar()
>    }
>}
> 
>fun main(args: Array<String>) {
>    val d =  D()
>    d.foo();
>    d.bar();
>}
>//输出结果  ABbar
>```

***

jimyoungwei危伟俊 
研发部-Client研发中心-Android开发组 

深圳市富途网络科技有限公司 
手机：18689496800 
邮箱：[jimyoungwei@futunn.com](mailto:jimyoungwei@futunn.com) 
地址：深圳市南山区科苑北路科兴科学园C栋3单元9楼 



