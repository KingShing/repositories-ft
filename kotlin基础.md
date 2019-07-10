## kotlin基础

[TOC]



###  1 导读

* 本文内容较为精简，适合刚接触kotlin的同学，快速学习kotlin基本用法,详尽的系统性学习，还是建议阅读相关书籍。

###  2 快速体验

 * 下面是kotlin常用语法的基本示例，当然也有其他的更简洁的写法。

  ```kotlin
  package cn.kingshing.ktlearning
  
  /* 枚举 */
  enum class Sex(val num:Int)
  {
      male(1),
      female(2),
      unknown(0)
  }
  
  
  /* 接口 */
  interface selfIntroduction{ 
      //method 可以有默认实现
  	fun sayHello()
  }
  
  /* kotlin 中class 默认是final的 ，继承、实现关系用 “：” 表示 */
  open class AClass:selfIntroduction
  {
      /* 属性 */
  	var mName:String //var  setter、getter
  	val mSex:Sex  //val  只有 getter 
  
      /* 次构造器 */
  	constructor(name:String,sex:Sex=Sex.male){
  		this.mName = name
  		this.mSex = sex
  	}
  	
      /* 常规方法 */
      fun methodA() {
          println("hello , $mName ! ")
      }
  
      /* 实现接口的方法 */
     	override fun sayHello(){
     		methodA();
    	}
  }
  
  
  /* 类的继承 */
  class BClass:AClass{
  	
      /* 带权限的属性 */
  	private var mFavorite:String
  	
      /* 调用父类的次构造器 ，注意这里变量前面不能加var 或者val */
  	constructor(favorite:String,name:String,sex:Sex=Sex.female):super(name,sex){
  		this.mFavorite = favorite
  	}
  	
      /* 重写父类的方法 */
  	override fun sayHello(){
  		println("$mName like $mFavorite !")
  	}
  }
  
  
  fun main()
  {
     var p = AClass("kingshing") // 创建对象
     println("sex: ${p.mSex}") //属性
     println("name:${p.mName}")
     p.sayHello()
     println("----------")
     var p2 = BClass(name ="cxk",favorite="playing basketball ") 
     println("sex: ${p2.mSex}") 
     println("name:${p2.mName}")
     p2.sayHello()
  }
  
  /* output */
  sex: male
  name:kingshing
  hello , kingshing ! 
  ----------
  sex: female
  name:cxk
  cxk like playing basketball  ! 
  ```

  

### 3 基础语法

  * 可变变量定义：var 关键字

    ```kotlin
    // var <标识符> : <类型> = <初始化值>
    var name:String = "joy"
    ```

  * 不可变变量定义：val 关键字，只能赋值一次的变量(类似Java中final修饰的变量)

    ```kotlin
    // val <标识符> : <类型> = <初始化值>
    val name:String = "king"
    ```

  * NULL 检查机制

    ```kotlin
    //类型后面加?表示可为空
    var age: String? = "23" 
    //强制调用，抛出空指针异常
    val ages = age!!.toInt()
    //尝试调用，不做处理返回 null
    val ages1 = age?.toInt()
    //?：左侧不为空，返回左侧，为空返回右侧
    val ages2 = age?.toInt() ?: -1
    ```

  * 区间

    ```kotlin
    for (i in 1..4) print(i) // 输出“1234”
    
    for (i in 4..1) print(i) // 什么都不输出
    
    if (i in 1..10) { // 等同于 1 <= i && i <= 10
        println(i)
    }
    
    // 使用 step 指定步长
    for (i in 1..4 step 2) print(i) // 输出“13”
    
    for (i in 4 downTo 1 step 2) print(i) // 输出“42”
    
    
    // 使用 until 函数排除结束元素
    for (i in 1 until 10) {   // i in [1, 10) 排除了 10
         println(i)
    }
    ```

    

### 4 数据类型

* Kotlin 的基本数值类型包括 Byte、Short、Int、Long、Float、Double 等。
* 不同于 Java 的是，字符不属于数值类型，是一个独立的数据类型。

| 类型   | 位宽度 |
| :----- | ------ |
| Double | 64     |
| Float  | 32     |
| Long   | 64     |
| Int    | 32     |
| Short  | 16     |
| Byte   | 8      |

* 字面常量
  * 长整型以大写的 L 结尾：123**L**
  * 16 进制以 0x 开头：**0x**0F
  * 2 进制以 0b 开头：**0b**00001011
  * 注意：8进制不支持

* 比较问题
  
* Kotlin 中没有基础数据类型，只有封装的数字类型，所以在 Kotlin 中，三个等号 **===** 表示比较对象地址，两个 **==** 表示比较两个值大小。
  
* 类型转换

  * 由于不同的表示方式，较小类型并不是较大类型的子类型，较小的类型不能隐式转换为较大的类型。 这意味着在不进行显式转换的情况下我们不能把 Byte 型值赋给一个 Int 变量。

  * ```kotlin
    val b: Byte = 1 // OK, 字面值是静态检测的
    val i: Int = b // 错误
    
    val b: Byte = 1 // OK, 字面值是静态检测的
    val i: Int = b.toInt() // OK  同理还有toDouble()、toFloat()等
    ```

* 位操作

  | kotlin | java | 操作说明     |
  | ------ | ---- | ------------ |
  | shl    | <<   | 左移位       |
  | shr    | \>>  | 右移位       |
  | ushr   | \>>> | 无符号右移位 |

* 数组

  * 数组用类 Array 实现，并且还有一个 size 属性及 get 和 set 方法，由于使用 [] 重载了 get 和 set 方法，所以我们可以通过下标很方便的获取或者设置数组对应位置的值。

  * ```kotlin
    // 数组的创建两种方式：
    //一种是使用函数arrayOf()；
    //一种是使用工厂函数。
    fun main(args: Array<String>) {
        
        //[1,2,3]
        val a = arrayOf(1, 2, 3)
       
        //[0,2,4]
        val b = Array(3, { i -> (i * 2) })
    
        //读取数组内容
        println(a[0])    // 输出结果：1
        println(b[1])    // 输出结果：2
    }
    ```

* 字符串
  * 和 Java 一样，String 是不可变的。
  * 方括号 [] 语法可以很方便的获取字符串中的某个字符，也可以通过 for 循环来遍历。
  * kotlin可以用 '''  ''' ，来支持多行字符串。

* 字符串模板
  * 字符串可以包含模板表达式 ，即一些小段代码，会求值并把结果合并到字符串中。
  *  模板表达式以美元符（$）开头，可参考文章开头的示例。

### 5 控制语句、循环语句

* if 表达式

  * 和java不同，kotlin中if是表达式，是可以作为一个值返回的，如下

  * ```kotlin
    // 作为表达式
    val max = if (a > b) a else b
    ```

* 使用区间 

  * 这种逻辑 1<= a <= 10

  * ```kotlin
    /* java */
    if(a>=1 && a<=10)
    
    /* kotlin */
    if(a in 1..10)
    ```

* 使用when，代替switch

  * ```kotlin
    when (x) {
        1 -> print("x == 1")
        2 -> print("x == 2")
        3,5 -> print("x ==3 或者 x == 5 ")
        in 6..10 -> print("x 在[6,10]区间上")
        else -> { // 注意这个块
            print("x 不是 1 、2、3、5，也不在6到10的区间上")
        }
    }
    ```

    

* for循环

  * for 循环可以对任何提供迭代器（iterator）的对象进行遍历 

  * ```kotlin
    //不带索引
    for (item in collection) 
    	print(item)
    
    //带索引的遍历
    for ((index, value) in array.withIndex()) {
        println("the element at $index is $value")
    }
    ```

    

* do..while、while、return，break，continue 这些都和java一样（略）

### 6 接口、类、枚举

* **Kotlin 接口(interface)**
  * 接口中方法与 Java 8 类似，使用 interface 关键字定义接口，允许方法有默认实现。
  * 接口中的属性，只能是抽象的，不允许初始化值，接口不会保存属性值，实现接口时，必须重写属性。

* **kotlin 类（class）**

  * Kotlin 类可以包含：构造函数和初始化代码块、函数、属性、内部类、对象声明。

  * class 默认是final，不能被继承，如需要被继承，class 前必须加上修饰符open(抽象类除外)。

  * 内部类必须inner关键字表示，持有对外部类的引用，不加关键字的叫嵌套类，不持有外部类的引用。

  * ```kotlin
    class Person {
    
        var lastName: String = "zhang"
            get() = field.toUpperCase()   // 将变量赋值后转换为大写
            set
    
        var no: Int = 100
            get() = field                // 后端变量，（后面会有详细解释）
            set(value) {
                if (value < 10) {       // 如果传入的值小于 10 返回该值
                    field = value
                } else {
                    field = -1         // 如果传入的值大于等于 10 返回 -1
                }
            }
    
        var heiht: Float = 145.4f
            private set
    }
    
    // 测试
    fun main(args: Array<String>) {
        var person: Person = Person()
    
        person.lastName = "wang"
    
        println("lastName:${person.lastName}")
    
        person.no = 9
        println("no:${person.no}")
    
        person.no = 20
        println("no:${person.no}")
    
    }
    
    /*output*/
    lastName:WANG
    no:9
    no:-1
    ```

  * 主构造器

  * ```kotlin
    //如果构造器有注解，或者有可见度修饰符，这时constructor关键字是必须的，注解和修饰符要放在它之前。
    class Person constructor(name: String) {
        var mName:String
        init {
            this.mName = name
        }
    }
    
    //上述代码可以简化
    class Person(var mName:String){
        
    }
    ```

  * 次构造器

  * ```kotlin
    //如果类有主构造函数，每个次构造函数都要，或直接或间接通过另一个次构造函数代理主构造函数。
    //在同一个类中代理另一个构造函数使用 this 关键字：
    //调用父类的构造函数，用super()
    class Person{
        var mName:String
        constructor(name:String){
            this.mName = name
        }
    }
    
    class Staff  constructor(name: String) {  // 类名为 Staff
        // 大括号内是类体构成
        var url: String = "http://www.futu5.com"
        var country: String = "CN"
        var siteName = name
    
        init {
            println("初始化网站名: ${name}")
        }
        // 次构造函数
        constructor (name: String, alexa: Int) : this(name) {
            println("Alexa 排名 $alexa")
        }
    }
    ```

    

* **关于field理解**

  * 这个问题对 Java 开发者来说十分难以理解，网上有很多人讨论这个问题，可以参考这个帖子：https://stackoverflow.com/questions/43220140/whats-kotlin-backing-field-for/43220314 ，其中最关键**Remember in kotlin whenever you write foo.bar = value it will be translated into a setter call instead of a PUTFIELD.**

  * 也就是说，在 Kotlin 中，任何时候当你写出“一个变量后边加等于号”这种形式的时候，比如我们定义 **var name: String** 变量，当你写出 **name= ...** 这种形式的时候，这个等于号都会被编译器翻译成调用 **setter** 方法；而同样，在任何位置引用变量时，只要出现 **name** 变量的地方都会被编译器翻译成 **getter** 方法。那么问题就来了，当你在 **setter** 方法内部写出 **name = ...** 时，相当于在 **setter** 方法中调用 **setter** 方法，形成递归，进而形成死循环,**field**就是为了解决这个问题的。

  * ```kotlin
    var no: Int = 100
        get() = field                // 后端变量
        set(value) {
            if (value < 10) {       // 如果传入的值小于 10 返回该值
                field = value
            } else {
                field = -1         // 如果传入的值大于等于 10 返回 -1
            }
        }
    
    // 以上这种写法是正确的，因为使用了 field 关键字，但是如果不用 field 关键字会怎么样呢？
    //例如：
    
    var no: Int = 100
        get() = no
        set(value) {
            if (value < 10) {       // 如果传入的值小于 10 返回该值
                no = value
            } else {
                no = -1         // 如果传入的值大于等于 10 返回 -1
            }
        }
    //注意这里我们使用的 Java 的思维写了 getter 和 setter 方法，那么这时，如果将这段代码翻译成 Java 代码会是怎么样呢？如下：
    int no = 100;
    public int getNo() {
        return getNo();
    }
    
    public void setNo(int value) {
        if (value < 10) {
            setNo(value);
        } else {
            setNo(-1);
        }
    }
    //翻译成 Java 代码之后就很直观了，在 getter 方法和 setter 方法中都形成了递归调用，显然是不正确的，最终程序会出现内存溢出而异常终止。
    ```

* 枚举

  * 枚举类最基本的用法是实现一个类型安全的枚举。

  * 枚举常量用逗号分隔,每个枚举常量都是一个对象。

  * ```kotlin
    enum class Color{
        RED,BLACK,BLUE,GREEN,WHITE
    }
    
    //自定义值
    enum class Shape(value:Int){
        ovel(100),
        rectangle(200)
    }
    
    
    fun main(args: Array<String>) {
        var color:Color=Color.BLUE
    
        println(Color.values()) //[LColor;@1b6d3586
        println(Color.valueOf("RED")) //RED
        
        println(color.name) //BLUE
        println(color.ordinal) //2
    
    }
    ```

    

### 7 泛型

* 泛型，即 "参数化类型"，将类型参数化，可以用在类，接口，方法上。与 Java 一样，Kotlin 也提供泛型，为类型安全提供保证，消除类型强转的烦恼。

  * 泛型类

  * ```kotlin
    class Box<T>(t: T) {
        var value = t
    }
    //创建类的实例时我们需要指定类型参数:
    val box: Box<Int> = Box<Int>(1)
    // 或者
    val box = Box(1) // 编译器会进行类型推断，1 类型 Int，所以编译器知道我们说的是 Box<Int>。
    ```

    

  * 泛型方法

  * ```kotlin
    fun main(args: Array<String>) {
        val age = 23
        val name = "kingshing"
        val bool = true
    
        doPrintln(age)    // 整型
        doPrintln(name)   // 字符串
        doPrintln(bool)   // 布尔型
    }
    
    fun <T> doPrintln(content: T) {
    
        when (content) {
            is Int -> println("整型数字为 $content")
            is String -> println("字符串转换为大写：${content.toUpperCase()}")
            else -> println("T 不是整型，也不是字符串")
        }
    }
    ```

    

  * 泛型约束

    * 上界 ：

    * ```kotlin
      fun <T : Comparable<T>> sort(list: List<T>) {
          // ……
      }
      //Comparable 的子类型可以替代 T
      sort(listOf(1, 2, 3)) // OK。Int 是 Comparable<Int> 的子类型
      
      //对于多个上界约束条件，可以用 where 子句：
      fun <T> copyWhenGreater(list: List<T>, threshold: T): List<String>
          where T : CharSequence,
                T : Comparable<T> {
          return list.filter { it > threshold }.map { it.toString() }
      }
      //事实上，没有指定上界的类型形参将会使用Any?这个默认上界。
      ```

  * 型变

    * Kotlin 中没有通配符类型，它有两个其他的东西：声明处型变（declaration-site variance）与类型投影（type projections）。

    * 声明处的类型变异使用协变注解修饰符：in、out，消费者 in, 生产者 out。

    * 使用 out 使得一个类型参数协变，协变类型参数只能用作输出，可以作为返回值类型但是无法作为入参的类型：

    * ```kotlin
      // 定义一个支持协变的类
      class Person<out A>(val a: A) {
          fun foo(): A {
              return a
          }
      }
      
      fun main(args: Array<String>) {
          var strCo: Person<String> = Person("a")
          var anyCo: Person<Any> = Person<Any>("b")
          anyCo = strCo
          println(anyCo.foo())   // 输出 a
      }
      
      // in 使得一个类型参数逆变，逆变类型参数只能用作输入，可以作为入参的类型但是无法作为返回值的类型：
      // 定义一个支持逆变的类
      class Person1<in A>(a: A) {
          fun foo(a: A) {
          }
      }
      
      fun main(args: Array<String>) {
          var strDCo = Person1("a")
          var anyDCo = Person1<Any>("b")
          strDCo = anyDCo
      }
      ```

      

  * 星投影（自我感觉没啥实际用）

    * 有些时候, 你可能想表示你并不知道类型参数的任何信息, 但是仍然希望能够安全地使用它. 这里所谓"安全地使用"是指, 对泛型类型定义一个类型投射, 要求这个泛型类型的所有的实体实例, 都是这个投射的子类型。
    * 如果一个泛型类型中存在多个类型参数, 那么每个类型参数都可以单独的投射. 比如, 如果类型定义为interface Function<in T, out U\> , 那么可以出现以下几种星号投射:
      * Function<*, String> , 代表 Function<in Nothing, String> ;
      * Function<Int, *> , 代表 Function<Int, out Any?> ;
      * Function<*,* > , 代表 Function<in Nothing, out Any?> .

    * 星投影 与 any？的区别

      * 前者表示一种类型，只是无法知道该类型是什么；后者表示任意类型。例如 List<*> 中的元素只能是一种数据类型，但 List<Any?> 的元素可以是任意类型。

    * 出现背景

      * ```kotlin
        
        /* java */
        ArrayList list = new ArrayList();
        
        //上面的写法在java中是可以的，这种用法被称为“原始类型”，在kotlin中使用下面的写法
        
        /* kotlin */
        var list:ArrayList<*> = arrayListOf(1,"kotlin")
        
        ```

        