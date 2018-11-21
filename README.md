Golang 征途

- [Program Pactice](https://github.com/Neras/golang-poker/tree/program-pactice)
- [Golang Basic](./basic)
- [Golang Professional](./professional)
- [Go语言圣经-章节练习代码](./gopl.io/)

**NOTE**
- 变量声明
    ```go
    s := "" // 短变量声明，最简洁，但是只能用在函数内部，而不能用于包变量
    var s = string // 形式以来于字符串的默认初始化零值机制，被初始化为""
    var s = "" // 用得很少，除非同时声明多个变量
    var s string = "" // 显式地标明变量的类型，当变量类型与初值类型相同时，类型冗余，但如果两者类型不同，变量类型就必须了
    ```
- 指针：取地址符`&`，放在一个变量前使用就会返回响应变量的内存地址。这个地址可以存储在一个叫做指针的特殊数据类型中。对于任何一个变量var，如下表达式都是正确的：`var == *(&var)`
- 在Go中，函数是不允许被重载的
- 递归--计算斐波那契数列
- 闭包：匿名函数同样被称为闭包，他们被允许调用定义在其他环境下的变量。闭包可使得某个函数捕捉到一些外部状态。另一种表示方式为：一个闭包继承了函数所声明时的作用域。这种状态（作用域内的变量）都被共享到闭包的环境中，因此这些变量可以在闭包中被操作，直到被销毁。闭包经常被用作包装函数：他们会预先定义好一个或多个参数以用于包装。另一个不错的应用就是使用闭包来完成更加简洁的错误检查。
  1. 闭包函数保存并累积其中的变量的值，不管外部函数退出与否，它都能继续操作外部函数中的局部变量。
- 切片（slice）是对数组一个看许片段的引用
  1. new(T) 为每个新的类型T分配一片内存，初始化为 0 并且返回类型为*T的内存地址：这种方法 返回一个指向类型为 T，值为 0 的地址的指针，它适用于值类型如数组和结构体；它相当于`&T{}`。 
  2. make(T) 返回一个类型为 T 的初始值，它只适用于3种内建的引用类型：切片、map 和 channel。
  3. 改变切片长度的过程称之为切片重组**reslicing**
- map是一种特殊的数据结构：一种元素对（pair）的无序集合，pair的一个元素是key，对应的另一个元素的是value，所以这个结构也称为关联数组或字典。map这种数据结构在其他编程语言中也称为字典、hash和HashTable等（对比`JSON`）
  1. map是引用类型，内存用make方法来分配
  2. map初始化`var map1 = make(map[keytype]valyetype)`，或简写为`map1 := make(map[keytype]valuetype)
  3. **不要使用new，永远用make来构造map**
- struct
  1. 试图`make()`一个结构体变量，会引发一个编译错误；`new()`一个映射并试图使用数据填充它，将会引发运行时错误。
- 方法：Go方法是作用在接收者（`receiver`）上的一个函数，接收者是某种类型的变量。因此方法是一种特殊类型的函数。
  ```go
  func (recv receiver_type) methodName(parameter_list) (return_value_list) {...}
  ```
  `recv`就像面向对象语言中的`this`或`self`
  1. 如果类型定义了`String()`方法，它会被用在`fmt.Printf()`中生成默认的输出：等同于使用格式化描述符`%V`产生的输出。`fmt.Print()`和`fmt.Println()`也会自动使出`String()`方法
- GC:通过调用`runtime.GC()`函数可以显示的触发GC。
- 接口：Go语言中的接口痘痕间断，通常它们会包含0个，最多3个方法。
  1. 添加[**第一个例子：使用`Sorter`接口排序**](./interface/sortmain.go)
- 空接口或者最小接口：不包含任何方法，它对实现不做任何要求
  ```go
  type Any interface{}
  ```
  
**PROFESSIONAL**
- `OpenFile`函数有三个参数：文件名、一个或多个标志（使用逻辑运算符"|"连接），使用文件的权限。通常会用到一下标志：
  1. `os.O_RDONLY`: 只读
  2. `os.O_WRONLY`: 只写
  3. `os.O_CREATE`: 创建：如果指定文件不存在，就创建该文件。
  4. `os.O_TRUNC`: 截断：如果指定文件已存在，就将该文件的长度截为0。
  
  在读文件的时候，文件的权限是被忽略的。
- 切片提供了Go中处理I/O缓冲的标准方式，[在一个切片缓冲内使用无线for循环读取文件](./professional/RWData/cat2.go)
- `fmt.Fprintf()`函数签名为：
  ```go
  func FprintF(w io.Writer, format string, a ...interface{}) (n int, err error)
  ```