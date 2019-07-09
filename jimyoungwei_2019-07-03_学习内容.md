### Kotlin基础学习内容

***

**基本要素** 

> 函数：
>
> > 1.函数的基本格式
> >
> > 函数的声明以关键字fun开始，函数名称紧随其后，括号中的是参数列表，参数列表后面紧接返回类型，它们之间用一个冒号隔开。
> >
> > ```
> > // 函数名称  参数列表  返回类型  
> > fun max(a: Int, b: Int): Int {
> >     return if (a > b) a else b //函数体
> > }
> > ```
> >
> > 注意：在kotlin中，函数可以定义在文件的最外层，不需要把它放在类中，且可以省略每行代码结尾的分号。这里的if是表达式，而不是语句。语句和表达式的区别在于，表达式有值，并且能作为另一个表达式的一部分使用；而语句总是包围着它的代码块中的顶层元素，并且没有自己的值。在java中，所有的控制结构都是语句。而在kotlin中，除了循环（for、while和do/while）以外的大多数控制结构都是表达式。对于赋值操作，在java是表达式，而在kotlin是语句。
> >
> > 2.表达式函数体
> >
> > 如果函数体写在花括号中，则该函数有代码块体；如果它直接返回了一个表达式，则是表达式体。
> >
> > ```
> > fun max(a: Int, b: Int): Int = if (a > b) a else b
> > //类型推断：对于表达式体函数而言，编译器会分析并推断出其返回类型，可以省略，不需显式地写出，但为了阅读性，建议还是加上
> > fun max(a: Int, b: Int) = if (a > b) a else b  //同样成立
> > ```
> >
> > 注意：只有表达式体函数的返回类型可以省略。但是，对于有返回值的代码块体函数，必须显式地写出返回类型和return语句。
>
> 变量：
>
> >1.变量的基本格式
> >
> >变量的声明以关键字开始，然后是变量名称，最后可以加上类型(如果有初始器，可以不加)
> >
> >```
> >val age: Int = 23 
> >val name = "jimyoungwei"  //没有加类型，但由于有初始化值，会推断出变量为String类型
> >val num = 7.5e6  //该浮点型常量被推断为Double类型
> >```
> >
> >注意：如果变量没有初始化器，需要显式地指出它的类型
> >
> >```
> >val age: Int
> >age = 23
> >```
> >
> >2.可变变量和不可变量
> >
> >val（来自value）——不可变引用。使用val声明的变量不能在初始化之后再赋值。它对应java中的final变量。
> >
> >var（来自variable）——可变引用。这样变量的值可以被改变。它对应的是普通(非final)的java变量。
> >
> >注意：
> >
> >尽管val引用的自身是不可变的，但它指向的对象可能是可变。
> >
> >```
> >fun a() {
> >    //声明不可变引用
> >    val languages = arrayListOf("Java")
> >    //改变引用指向的对象
> >    languages.add("kotlin")
> >}
> >
> >fun main() {
> >    println(a())
> >    //输出为 kotlin.Unit
> >    //如果一个函数不返回任何有用的值，它的返回类型是Unit
> >}
> >```
> >
> >var关键字允许变量改变自己的值，但它的类型却是不可改变的。
> >
> >```
> >var age = 23
> >age = "too old"  //错误：类型不匹配
> >```
>
> 字符串模板：
>
> >和许多脚本语言一样，kotlin可以在字符串字面值引用局部变量，只需要在变量名称前面加上字符$。如果需要引用更复杂的表达式，只需要把表达式用花括号括起来。
> >
> >```
> >fun main(args: Array<String>){
> >   val name = if (agrs.size > 0) args[0] else "kotlin"
> >   println("Hello, $name!")
> >   // 等价于java中的println("Hello," + name +"!")
> >   println("Hello, ${if (args.size > 0) args[0] else "someone"}!")
> >   //使用花括号括起来
> >}
> >```

**类和属性**

>类：
>
>> kotlin的值对象(只有数据没有其他代码)和java中的JavaBean等同。kotlin中，public是默认的可见性，可以省略。
>>
>> ```
>> /* Java */
>> public class Person{
>>     private final String name;
>>     
>>     public Person(String name){
>>         this.name = name;
>>     }
>>     
>>     public String getName(){
>>         return name;
>>     }
>> }
>> /* 转换成kotlin */
>> class Person(val name: String)
>> ```
>
>属性：
>
>>在java中，字段和其访问器的组合叫属性，在kotlin中，属性是头等的语言特性，完全代替了字段和访问器方法。在类中声明一个属性和声明一个变量一样：使用val和var关键字。声明为val的属性是只读，而var属性是可变。
>>
>>```
>>class Person(
>>   //只读属性：生成一个字段和一个简单的getter
>>   val name: String,       
>>   var isMarried:Boolean
>>   //可写属性：一个字段、一个getter和一个setter
>>)
>>```
>>
>>注意：如果属性的名称以is开头，getter不会增加任何的前缀；而它的setter名称中的is会被替换成set。所以在java中为isMarried()，在kotlin为setMarried()。
>
>自定义访问器：
>
>> ```
>> class Rectangle(val height: Int,val width: Int){
>>     val isSqure: Boolean
>>         get(){     //声明属性的getter
>>             return height == width  
>>         }
>> }
>> 
>> fun main() {
>>     val rectangle = Rectangle(42, 33)
>>     println(rectangle.isSqure)
>>     //输出为 false
>> }
>> ```
>>
>> 

**表示和处理选择：枚举和“when”**

>枚举：
>
>> kotlin中，enum是一个软关键字：只有当它出现在class前面时才有特殊的意义，在其他地方可以把它作为普通的名称使用。与此不同的是，class仍然是一个关键字，要继续使用名称clazz和aClass声明变量。
>>
>> ```
>> enum class Color(
>>     val r: Int, val g: Int, val b: Int   //声明枚举常量的属性
>> ) {
>>     //在每个常量创建的时候指定属性值，注意这里的分号，这是kotlin语法中唯一必须使用分号的地     //方：如果要在枚举类中定义任何方法，就要使用分号把枚举常量列表和方法定义分开
>>     RED(255, 0, 0), GREEN(0, 255, 0), BLUE(0, 0, 255);
>>     //给枚举类定义一个方法
>>     fun rgb() = (r * 256 + g) * 256 + b
>> }
>> ```
>
>“when”：
>
>> kotlin中的when对应于java中的switch语句，但它是有返回值的表达式。区别于java的switch语句，不再需要在每个分支都写上break语句（java中遗漏break通常会导致bug）。如果匹配成功，只有对应的分支会执行。也可以把多个值合并到同一个分支，只需要用逗号隔开这些值。
>>
>> ```
>> fun getWarmth(color: Color) = when (color) {
>>     Color.RED, Color.YELLOW -> "warm"
>>     Color.BLUE -> "cold"
>>     Color.GREEN -> "neutral"
>> }
>> fun main() {
>>    println(getWarmth(Color.YELLOW))
>> }
>> // 输出结果是 warm
>> ```
>>
>> kotlin没有三元运算符，因为if表达式有返回值，这点和java不同，尝试用when代替if。
>>
>> ```
>> interface Expr  //定义一个接口
>> class Num(val value: Int): Expr  //简单的值对象 
>> class Sum(val left: Expr,val right: Expr): Expr  
>> //Sum运算的实参可以是任何Expr：Num或者另外一个Sum
>> 
>> //使用有返回值的if表达式
>> fun eval(e: Expr): Int = 
>> if(e is Num){         //关键字is：用来检查判断一个变量是否是某种类型
>>     e.value
>> }else if(e is Sum){
>>     eval(e.right) + eval(e.left)
>> }else{
>>     throw IllegalArgumentExpection("Unknown expression")
>> }
>> 
>> //使用when代替if层叠
>> fun eval(e: Expr): Int =   
>>     when(e){  
>>         is Num -> e.value
>>         is Sum -> eval(e.right) + eval(e.left)
>>         else -> throw IllegalArgumentExpection("Unknown expression")
>>     }
>> ```
>>
>> 注意：if和when都可以使用代码块作为分支体。但需要遵循一个规则——“代码块中最后的表达式就是结果”。同样的规则对try主体和catch子句也生效。

**迭代：“while”循环和”for“循环**

>"while"循环：
>
>> kotlin有while循环和do-while循环，它们的语法和java中相应的循环没有区别，格式如下。
>>
>> ```
>> //当condition为true时执行循环体
>> while(condition){
>>    /*...*/
>> }
>> 
>> //循环体第一次无条件执行后，当condition为true时才执行
>> do{
>>    /*...*/
>> }while(condition)
>> ```
>
>迭代数字：区间和数列:
>
>>kotlin中没有常规的java for循环，此处引入区间的概念。区间的本质就是两个值之间的间隔：一个起始值，一个结束值。使用 .. 运算符来表示区间。如果能迭代区间中所有的值，这样的区间被称为数列。区间的迭代还可以带步长。
>>
>>```
>>fun fizzBuzz(i: Int) = when {
>>   i % 15 -> "FizzBuzz "  
>>   i % 3 -> "Fizz "
>>   i % 5 -> "Buzz "
>>   else -> "$i "
>>}
>>//在整数区间1..100迭代
>>for (i in 1..100){
>>   print(fizzBuzz(i))
>>}
>>//输出结果  1 2 Fizz 4 Buzz Fizz 7 ...
>>
>>//迭代方式：降序，步长为2
>>for (i in 100 downTo 1 step 2){
>>   print(fizzBuzz(i))
>>}
>>//输出结果 Buzz 98 Fizz 94 92 FizzBuzz 88 ...
>>```
>
>迭代map：
>
>>kotlin可以根据键来访问和更新map的值，可以使用map[key]读取值，并使用map[key] = value 设置，不需要调用get 和 put。
>>
>>```
>>// kotlin
>>val binaryReps = TreeMap<Char, String>()
>>binaryReps[c] = "cat"
>>//等价于它的java版本：
>>//binaryReps.put(c,"cat")
>>```
>>
>>可以用如下的展开语法在迭代集合的同时跟踪当前的下标。不需要再创建一个单独的变量来存储下标并手动增加它。
>>
>>```
>>val list = arrayListOf("1","2","3")
>>for((index, element) in list.withIndex()){  //迭代集合时使用下标
>>  println($index: $element)
>>}
>>/* 输出结果   0：1
>>*          	1：2
>>*           2：3
>>*/
>>```
>>
>
>”in“检查集合和区间的成员：
>
>>使用in运算符来检查一个值是在区间中，或者它的逆运算，!n，来检查这个值是否不在区间中。
>>
>>```
>>fun checkInput(c: Char) = when (c){
>>   in '0'..'9' -> "It's a digit!"
>>   in 'a'..'z','A'..'Z' -> "It's a letter!"
>>   }
>>   //输入 println(checkInput('j'))
>>   //输出结果  It's a letter!
>>```
>
>异常：
>
>> kotlin中异常机制和java比较类似，同样使用catch和finally子句的try结构来处理异常。和java最大的区别就是throws子句没有出现在代码中，在java中，如IOException等受检异常需要显式地处理，必须声明函数能抛出的所有受检异常。而kotlin并不区分受检异常和未检异常。不用指定函数抛出的异常，而且可以处理也可以不处理异常。
>>
>> ```
>> fun readNumber(reader: BufferedReader){
>>    val number = try{   //不同于if，try总是需要用花括号把语句主体括起来
>>       Integer.parseInt(reader.readLine())
>>    }catch(e: NumberFormatException){
>>       null     //发生异常的情况下使用null
>>    }
>>    
>>    println(number)
>> }
>> fun main(){
>>    val reader = BufferReader(StringReader("not a number"))
>>    readerNumber(reader)
>> }
>> //输出结果 null
>> ```
>>
>> 注意：无论是try块还是catch，代码块中最后一个表达式就是结果。需要避免在finally块中使用return语句，会导致try、catch块中的return语句失效。

**创建集合**

>kotlin没有采用自己的集合类，而是采用标准的java集合类。以下为几个例子，其他的类似。
>
>```
>val list = arrayListOf(1,2,3)
>val set = hashSetOf(1,2,3)
>val map = hashMapOf(1 to "one", 2 to "two", 3 to "three")
>
>println(list.javaClass)
>//class java.util.ArrayList
>println(set.javaClass)
>//class java.util.HashSet
>println(map.javaClass)
>//class java.util.HashMap
>```

**默认参数值**

> kotlin可以在声明函数的时候，指定参数的默认值，可以避免创建重载函数。以joinToString()为例。
>
> ```
> fun <T> joinToString(
>         collection: Collection<T>,
>         separator: String = ", ",     //有默认值的参数
>         prefix: String = "",
>         postfix: String = ""
> ): String
> 
> //joinToString(list,", ", "", "")
> //1, 2, 3
> //joinToString(list)
> //1, 2, 3
> //joinToString(list,"; ")
> //1; 2; 3
> 
> ```
>
> 注意：参数的默认值是被编码到被调用的函数中，而不是调用的地方。如果改变了参数的默认值并重新编译该函数，没有给参数重新赋值的调用者，将会开始使用新的默认值。而考虑到与java的交互，从java中调用kotlin函数的时候，必须显式地指定所有参数值。可以使用@JvmOverloads注解它。这个注解会指示编译器生成java重载函数，从最后一个开始省略每个参数。

**消除静态工具类：顶层函数和属性**

>1.顶层函数
>
>>在kotlin中可以将函数放在代码文件的顶层，不要从属于任何的类。这些放在文件顶层的函数依然是包内的成员。如果需要从包外访问，只需要import。kotlin编译生成的类的名称对应于包含函数的文件的名称（如：文件 join.kt  -->  public class JoinKt），这个文件中的所有顶层函数编译为这个类的静态函数。
>>
>>如果要改变包含kotlin顶层函数的生成的类的名称，需要为这个文件添加@JvmName注解，将其放到这个文件的开头，位于包名的前面。
>>
>>```
>>@file:JvmName("StringFunctions")   //注解指定类名
>>package strings                   //包的声明跟在文件注解之后
>>fun joinToString(...): String(...)
>>
>>/* Java */
>>import strings.StringFunctions;
>>StringFunctions.joinToString(list, ", ", "", "");
>>```
>
>2.顶层属性
>
>> 和函数一样，属性也可以放在文件的顶层，这样的值会被存储到一个静态的字段中。如果想要把一个常量以public static final的属性暴露给java，可以使用const来修饰它（适用于所有的基本数据类型的属性以及String类型）
>>
>> ```
>> const val NORMAL_TYPE = 1
>> /* java */
>> public static final Int NORMAL_TYPE = 1
>> ```

**扩展函数和属性**

>1.扩展函数
>
>>扩展函数其实就是一个类的成员函数，不过定义在类的外面。扩展函数可以直接访问被扩展的类的其他方法和属性。和在类内部定义的方法不同的是，扩展函数不能访问私有的或者受保护的成员。
>>
>>```
>>package strings
>>//   接收者类型:String           接收者对象：this
>>fun String.lastChar(): Char = this.get(this.length - 1)
>>fun main(){
>>   println("kotlin".lastChar())
>>}
>>//输出结果  n
>>```
>>
>>kotlin允许使用和导入类一样的语法来导入单个的函数，可以使用关键字as来修改导入的类或者函数名称，这是解决命名冲突问题的唯一方式。
>>
>>```
>>import strings.lastChar as last
>>val c = "kotlin".last()
>>```
>>
>>实质上，扩展函数是静态函数，它把调用对象作为了它的第一个参数。调用扩展函数，不会创建适配的对象或者任何运行时的额外消耗。扩展函数的静态性质决定了扩展函数不能被子类重写。
>>
>>```
>>
>>fun View.showOff() = println("It is a view!")
>>fun Button.showOff() = println("It is a button!")
>>fun main(){
>>    val view: Viwe = Button()
>>    view.showOff()      //扩展函数被静态地解析
>>}
>>//输出结果  It is a view!
>>```
>>
>>这里输出结果如此是因为当给基类和子类分别定义了一个同名的扩张函数，当函数被调用时是由该变量的静态类型所决定，而不是这个变量的运行时类型。如果一个类的成员函数和扩展函数有相同的签名，成员函数会被优先使用。
>
>2.扩展属性
>
>>扩展属性提供了一种方法，用来扩展类的API，可以用来访问属性，用的是属性语法而不是函数的语法。和扩展函数一样，扩展属性也像接收者的一个普通的成员属性一样。声明一个可变的扩展属性如下。
>>
>>```
>>var StringBuilder.lastChar: Char
>>    get() = get(length - 1)
>>    set(value: Char){
>>      this.setCharAt(length - 1, value)
>>    }
>>fun main(){  
>>    println("kotlin".lastChar)
>>    val sb = StringBuilder("kotin?")
>>    sb.lastChar = '!'
>>    println(sb)
>>}
>>//输出结果   n
>>//输出结果   kotlin!
>>```

**处理集合：可变参数、中缀调用和库支持**

> 1.扩展Java集合API：
>
> > kotlin中的集合和java的类相同之下，对API做了扩展，例子如下。
> >
> > ```
> > val strings: List<String> = listOf("one", "two", "three")
> > val numbers: Collection<Int> = setOf(1, 2, 3)
> > println(strings.last())
> > //输出  three
> > println(numbers.max())
> > //输出  3
> > ```
>
> 2.可变参数：
>
> > kotlin中的可变参数使用vararg修饰符代替java中的三个点。两者之间的区别在于当需要传递的参数已经包装在数组中时，调用该函数的语法。在java中，可以按原样传递数组，而kotlin则要求显式地解包数组，以便每个数组元素在函数中能作为单独的参数来调用。这个技术称为展开运算符，使用方法为在对应的参数前面放一个*。
> >
> > ```
> > val a = arrayOf(2, 3)
> > val b = asList(1, *a, 4, 5)
> > fun main(){
> >     println(b)
> > }
> > //输出结果 [1, 2, 3, 4, 5]
> > ```
>
> 3.中缀调用：
>
> > 中缀调用是一种特殊的函数调用，它没有添加额外的分隔符，函数名称是直接放在目标对象名称和参数之间的。
> >
> > ```
> > 1.to("one")  //这里的to是中缀调用
> > 1 to "one"   //两种调用方式等价
> > ```
> >
> > 中缀调用可以与只有一个参数的函数一起使用，无论是普通的函数还是扩展函数。要允许使用中缀符号调用函数，需要使用 infix 修饰符来标记它。
> >
> > ```
> > infix fun Any.to(other: Any) = Pair(this, other)
> > //to函数会返回一个Pair类型的对象，Pair是kotlin标准库中的类。Pair和to的声明都用到了泛型。
> > val (number, name) = 1 to "one"
> > ```

**字符串和正则表达式的处理**

>1.分割字符串：
>
>>kotlin对字符串的分割和Java一样使用split方法，但它是具有不同参数的重载的扩展函数，更加强大。
>>
>>```
>>//指定多个分隔符
>>println("12.345-6.A".splif(".", "-"))
>>//输出结果 [12, 345, 6, A]
>>
>>//kotlin可以使用扩展函数toRegex将字符串转换为正则表达式
>>println("12.345-6.A".spilt("\\.|-".toRegex()))
>>//输出结果 [12, 345, 6, A]
>>```
>
>2.正则表达式和三重引号的字符：
>
>>使用String的扩展函数来解析文件路径
>>
>>```
>>fun parsePath(path: String) {
>>    val directory = path.substringBeforeLast("/")
>>    val fullName = path.substringAfterLast("/")
>>
>>    val fileName = fullName.substringBeforeLast(".")
>>    val extension = fullName.substringAfterLast(".")
>>
>>    println("Dir: $directory, name: $fileName, ext: $extension")
>>}
>>fun main(){
>>    parsePath("/Users/jimyoung/kotlin/chapter.doc")
>>}
>>//输出结果 Dir: /Users/jimyoung/kotlin, name: chapter, ext: doc
>>```
>>
>>使用正则表达式解析文件路径
>>
>>```
>>fun parsePathWithRegex(path: String) {
>>    //三重引号的字符串，不需要对任何字符进行转义，包括反斜线。
>>    val regex = """(.+)/(.+)\.(.+)""".toRegex()
>>    val matchResult = regex.matchEntire(path)
>>    if (matchResult != null) {
>>        val (directory, filename, extension) = matchResult.destructured
>>        println("Dir: $directory, name: $filename, ext: $extension")
>>    }
>>}
>>fun main(){
>>    parsePathWithRegex("/Users/jimyoung/kotlin/chapter.doc")
>>}
>>//输出结果 Dir: /Users/jimyoung/kotlin, name: chapter, ext: doc
>>```
>
>3.多行三重引号的字符串：
>
>>多行字符串包含三重引号之间的所有字符，包括用于格式化代码的缩进。一个三重引号的字符串可以包含换行，而不用专门的字符，比如\n。
>>
>>```
>>val kotlinLogo = """
>>    | //
>>    |//
>>    |/ \
>>""".trimIndent()
>>
>>val fileAddress = """C:\\Users\jimyoungwei\kotlin"""
>>fun main(){
>>    println(kotlinLogo)
>>    println(fileAddress)
>>}
>>//输出结果
>>| //
>>|//
>>|/ \
>>C:\\Users\jimyoungwei\kotlin
>>```

**局部函数**

>局部函数主要是用来解决常见的代码重复问题，它可以访问所在函数中的所有参数和变量。
>
>```
>class User(val id: Int, val name: String, val address: String)
>
>fun User.validateBeforeSave() {
>    fun validate(value: String, fieldName: String) {  //声明一个局部函数
>        if (value.isEmpty()) {
>            throw IllegalArgumentException("Can't save user $id: empty $fieldName")
>            //可以直接访问到User的属性
>        }
>    }
>    validate(name, "Name")
>    validate(address, "Adress")
>}
>
>fun saveUser(user: User) {
>    user.validateBeforeSave()  //调用扩展函数
>    /* ... */
>}
>```
>
>注意：扩展函数也可以被声明为局部函数。但深度嵌套的局部函数不利于理解，不建议使用多层嵌套。

***

jimyoungwei危伟俊 
研发部-Client研发中心-Android开发组 

深圳市富途网络科技有限公司 
手机：18689496800 
邮箱：[jimyoungwei@futunn.com](mailto:jimyoungwei@futunn.com) 
地址：深圳市南山区科苑北路科兴科学园C栋3单元9楼 