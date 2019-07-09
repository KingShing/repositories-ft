### kotlin学习笔记

* 覆盖方法总是使用与基类型方法相同的默认参数值。子类不能再有默认值

* 如果一个默认参数在一个无默认值的参数之前，那么该默认值只能通过使用[命名参数](http://www.kotlincn.net/docs/reference/functions.html#命名参数)调用该函数来使用：

  ```Kotlin
  fun foo(bar: Int = 0, baz: Int) { …… }
  
  foo(baz = 1) // 使用默认值 bar = 0
  ```

  

