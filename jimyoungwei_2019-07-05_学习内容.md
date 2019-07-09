### Kotlin基础学习内容

***

#### Lambda编程

>**lambda表达式和成员引用**
>
>>1.简介
>>
>>>lambda表达式，简称lambda，本质上就是可以传递给其他函数的一小段代码，作为函数参数的代码块。如果lambda刚好是函数或者属性的委托，可以用成员引用代替。
>>
>>2.语法
>>
>>>kotlin的lambad表达式始终用花括号包围。注意实参并没有用括号括起来。箭头把实参列表和lambda的函数体隔开。可以把lambda表达式存储在一个变量中，把这个变量作为普通函数对待(即通过相应实参调用它)：
>>>
>>>```
>>>val sum = { x: Int, y: Int -> x + y}
>>>println(sum(1, 2))
>>>//输出结果 3
>>>```
>>>
>>>当lambda是函数唯一的实参时，可以去掉调用代码中空括号对：
>>>
>>>```
>>>people.maxBy{p:Person -> p.age}
>>>```
>>>
>>>当实参名称没有显式地指定时，默认名称it会生效。
>>>
>>>```
>>>people.maxBy{it.age}
>>>```
>>>
>>>如果用变量存储lambda，那么就没有可以推断出参数类型的上下文，必须显式地指出参数类型：
>>>
>>>```
>>>val getAge = {p:Person -> p.age}
>>>people.maxBy(getAge)
>>>```
>>
>>3.在作用域中访问变量
>>
>>>当在函数内声明一个匿名内部类的时候，能够在这个匿名内部类引用这个函数的参数和局部变量。如果在函数内部使用lambda，也可以访问这个函数的参数以及在lambda之前定义的局部变量。当这里与Java的一个区别就是，在kotlin中不会仅限于访问final 变量，在lambda内部也可以修改这些变量。从lambda内访问外部变量，称变量被lambda捕捉。
>>>
>>>```
>>>fun printProblemCounts(responses: Collection<String>) {
>>>    var clientErrors = 0
>>>    var serverErrors = 0
>>>    responses.forEach {
>>>        if (it.startsWith("4")) {
>>>            clientErrors++
>>>        } else if (it.startsWith("5")) {
>>>            serverErrors++
>>>        }
>>>    }
>>>    println("$clientErrors client errors, $serverErrors server errors")
>>>}
>>>fun main(){
>>>     val responses = listOf("200 OK", "418 I'm a teapot", "500 Internal Server Error")
>>>    printProblemCounts(responses)
>>>}
>>>//输出结果  1 client errors, 1 server errors
>>>```
>>>
>>>注意：默认情况下，局部变量的生命周期被限制在声明这个变量的函数中。但如果它被lambda捕捉了，使用这个变量的代码可以被存储并稍后再执行。而对非final变量而言，它的值被封装再一个特殊的包装器中，对这个包装器的引用会和lambda代码一起存储。如果lambda被用作事件处理器或者在其他异步执行的情况，对局部变量的修改只会在lambda执行的时候发生。
>>
>>4.成员引用
>>
>>>成员引用提供了简明语法，来创建一个调用单个方法或者访问这个单个属性的函数值。双冒号把类名称与要引用的成员（一个方法或者一个属性）名称隔开。
>>>
>>>```
>>>val getAge = Person::age
>>>//                类::成员
>>>//注意，不管引用的是函数还是属性，都不要在成员引用的名称后面加括号。
>>>```
>>>
>>>可以引用顶层函数（不是类的成员）
>>>
>>>```
>>>fun salute() = println("jimyoung")
>>>fun main(){
>>>   run(::salute)
>>>}
>>>//输出结果  jimyoung
>>>```
>>>
>>>可以用构造方法引用存储或者延期执行创建类实例的动作。构造方法引用的形式是在双冒号后指定类名称
>>>
>>>```
>>>data class Person(val name:String, val age: Int)
>>>fun main(){
>>>    val createPerson = ::Person
>>>    val p = createPerson("jimyoung",23)
>>>    println(p)
>>>}
>>>//输出结果  Person(name=jimyoung, age=23)
>>>```
>>>
>>>还可以引用扩展函数
>>>
>>>```
>>>fun Person.isAdult() = age >=18
>>>val predicate = Person::isAdult
>>>```
>
>**集合的函数式API**
>
>>1.filter 和 map
>>
>>>filter函数遍历集合并选出应用给定lambda后会返回true的那些元素，但它并不会改变那些元素。
>>>
>>>```
>>>val list = listOf(1,2,3,4)
>>>fun main(){
>>>    println(list.filter{it % 2 == 0})
>>>}
>>>//输出结果  [2, 4]
>>>```
>>>
>>>map函数对集合中的每个元素应用给定的函数并把结果收集到一个新集合。结果是一个新集合，包含的元素个数不变，但每个元素根据给定的判断式做了变换。
>>>
>>>```
>>>val people = listOf(Person("jimyoung",23), Person("bob",22))
>>>fun main(){
>>>    println(people.map{it.name})
>>>}
>>>//输出结果 [jimyoung, bob]
>>>```
>>>
>>>还可以对map应用过滤和变化函数。filerKeys和mapKeys过滤和变化map的键，filterValues和mapValues过滤和变换对应的值。
>>
>>2.对集合应用判断式：all、any、count、find
>>
>>>all：是否所有元素都满足判断式
>>>
>>>```
>>>val canIn22 = {p: Person -> age <= 22}
>>>val people = listOf(Person("jimyoung",23), Person("bob",20))
>>>fun main(){
>>>    println(people.all(canIn22))
>>>}
>>>//false
>>>```
>>>
>>>any：检查集合中是否至少存在一个匹配的元素
>>>
>>>```
>>>val canIn22 = {p: Person -> age <= 22}
>>>val people = listOf(Person("jimyoung",23), Person("bob",20))
>>>fun main(){
>>>    println(people.any(canIn22))
>>>}
>>>//true
>>>```
>>>
>>>count方法只是跟踪匹配元素的数量，不关心元素本身。
>>>
>>>find：找到一个满足判断式的元素。如果有多个匹配的元素就返回其中第一个元素；如果没有一个元素能满足判断式，返回null。
>>
>>3.把列表转换成分组的map：groupBy
>>
>>>把所有元素按照不同的特征划分成不同的分组
>>>
>>>```
>>>val people = listOf(Person("jimyoung",23), Person("bob",20))
>>>fun main(){
>>>     println(people.groupBy{ it.age })
>>>}
>>>//输出结果  {20=[Person(name=bob, age=20)],23=[Person(name=jimyoung, age=23)]}
>>>```
>>
>>4.处理嵌套集合中的元素：flatMap 和 flatten
>>
>>>flatMap：根据作为实参给定的函数对集合中的每个元素做变换，然后把多个列表合并成一个列表。
>>>
>>>```
>>>val strings = listOf("abc","def")
>>>fun main(){
>>>   println(strings.flatMap{it.toList()})
>>>}
>>>//输出结果  [a,b,c,d,e,f]
>>>```
>>>
>>>如果不需要做任何的变换，只需要合并一个集合，可以使用flatten函数：listOfLists.flatten()。
>
>**惰性集合操作**
>
>>1.序列
>>
>>>链式集合函数会及早地创建中间集合，每一步的中间结果都被存储在一个临时列表。当大数据量时，应该使用序列，序列中的元素是惰性的。kotlin惰性集合操作的入口时Sequence接口，表示一个可以逐个列举元素的元素序列。
>>>
>>>```
>>>people.asSequence()
>>>      .map(Person::name)
>>>      .filter{it.startWith("A")}
>>>      .toList()
>>>```
>>
>>2.执行序列操作
>>
>>>序列操作分为两类：中间和末端。一次中间操作返回的是另一个序列，新序列知道如何变换原始序列中的元素。一次末端操作返回的是一个结果，结果可能是集合、元素、数字或者其他从初始集合的变换序列中获取的任意对象。中间操作始终都是惰性的，末端操作触发执行了所有的延期计算。
>>>
>>>```
>>>sequence.map{...}.filter{...}.toList()
>>>         /*   中间操作       */ 末端操作
>>>```
>>
>>3.创建序列
>>
>>>使用generateSequence函数亦可以创建序列。
>>>
>>>```
>>>fun File.isInsideHiddenDirectory()=generateSequence(this){it.parentFile}.any{it.isHidden}
>>>fun main(){
>>>   val file = File("/User/sb/.HiddenDir/a.txt")
>>>   println(file.isInsideHiddenDirectory())
>>>}
>>>//输出结果 true
>>>```
>

***

jimyoungwei危伟俊 
研发部-Client研发中心-Android开发组 

深圳市富途网络科技有限公司 
手机：18689496800 
邮箱：[jimyoungwei@futunn.com](mailto:jimyoungwei@futunn.com) 
地址：深圳市南山区科苑北路科兴科学园C栋3单元9楼 