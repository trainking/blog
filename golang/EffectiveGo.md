# Effective Go

---

- [Effective Go](#effective-go)
  - [概述](#概述)
  - [格式化](#格式化)
  - [注释](#注释)
    - [多行注释](#多行注释)
    - [单行注释](#单行注释)
    - [注释规范](#注释规范)
      - [包注释](#包注释)
      - [块注释](#块注释)
      - [行注释](#行注释)
  - [命名](#命名)
    - [包名](#包名)
      - [包别名](#包别名)
    - [接口命名](#接口命名)
    - [大小写](#大小写)
    - [Getters/Setters](#getterssetters)
    - [文件命名](#文件命名)
  - [代码结构](#代码结构)
    - [分号与逗号](#分号与逗号)
    - [控制结构](#控制结构)
      - [if](#if)
      - [for](#for)
      - [switch](#switch)
      - [select](#select)
  - [函数](#函数)
    - [动态参数](#动态参数)
    - [多返回值](#多返回值)
    - [命名返回值](#命名返回值)
    - [Defer](#defer)
  - [数据结构](#数据结构)
    - [new](#new)
    - [make](#make)
    - [数组（Array）](#数组array)
    - [切片（Slice）](#切片slice)
      - [判断元素是否在切片中](#判断元素是否在切片中)
    - [Map](#map)
      - [判断值是否存在](#判断值是否存在)
  - [初始化](#初始化)
    - [常量](#常量)
    - [变量](#变量)
    - [init函数](#init函数)
  - [interface](#interface)
    - [interface{}](#interface-1)
    - [interface check](#interface-check)
  - [struct](#struct)
    - [struct{}](#struct-1)
  - [方法](#方法)
  - [附录](#附录)
    - [运算符优先级](#运算符优先级)

## 概述

`Go` 语言的最佳实践文档，以此作为技术时间参考

[https://golang.org/doc/effective_go.html](https://golang.org/doc/effective_go.html)

## 格式化

格式化是最容易引起争论的话题，所以`Go`直接统一了这个话题，使用`go fmt`工具，会将文件统一成标准的格式。

如下：

```
type T struct {
    name    string // name of the object
    value   int    // its value
}
```

关键点如下：

* gofmt使用制表符tab作为缩进工具（结束制表符和空格之争）
* 没有长度限制，代码想多长就多长。如果绝的长度太长可以用**额外的制表符**包裹起来缩进
* `{}`在代码块中必须的，但`if`，`for`, `switch`的判断条件不需要`()`包裹，以为`Golang`重定义了[优先级](#运算符优先级)

> `x<<8 + y<<16` 表达的是`x`左移之后, 与`y`左移之后相加。虽然+号的优先级高于位移运算符，但是go fmt增加了空格，改变了歧义

## 注释

### 多行注释

可以使用类似其他语言的`/**/`块注释，但是其中要注意中间不在需要额外加`*`。

```
/*
Package regexp implements a simple library for regular expressions.

The syntax of the regular expressions accepted is:

    regexp:
        concatenation { '|' concatenation }
    concatenation:
        { closure }
    closure:
        term [ '*' | '+' | '?' ]
    term:
        '^'
        '$'
        '.'
        character
        '[' [ '^' ] character-ranges ']'
        '(' regexp ')'
*/
package regexp
```

### 单行注释

简单的`//`就行了

### 注释规范

优雅的包中，往往需要有**包注释**，**块注释**，**行注释**。

#### 包注释

一个包中，存在许多文件，一般将包注释写在与包同名的文件之中，在`package 包名`之上，采取`/**/`多行注释规范。一般需要包括一下信息（顺序是建议书写顺序）：

1. 包的用途
2. 使用短例子（可选）
3. 特殊声明（可选）

#### 块注释

函数定义和结构提定义，统称为块，其注释方式：

```golang
// User is ...
type User struct {
  ...
}
```

用一行声明用途即可，如果有特殊参数介绍，则增加新的行，只有特别重要的，对外元素(即首字母大写，可在外部访问的)，才用`/**/`注释说明

#### 行注释

即在容易混淆和超长代码中做混淆和标记只用，如一个需要做的功能，加入`TODO`：

```
// TODO 增加IP过滤
```

## 命名

命名规范在`golang`中，是非常重要的。因为设计到`golang`中一些隐藏的特性，所以重视命名规范，减少很多不必要的问题。

### 包名

包名必须是小写, 包名必须与文件所在文件夹一致，如

```
package sqlt
```

只能是放在一个`sqlt`的文件夹中，引用包时，分两种情况：

1. 使用`go mod`建立的项目中，只需要`import "module名/sqlt"`找到包所在路径即可
2. 加入到`GOPATH`中，则只需要加入`import "gopath中的相对路径"`

#### 包别名

1. 重命名包别名，即两个相同包名包引入时，采用`import s "sqlt"`，用变量`s`代替了包名，所以包名本质是要给变量，所以在定义变量时，要注意不要与`包名`冲突，**变量覆盖了包**
2. `.`别名引入，表示包中元素可以直接访问，不在具名调用，如

```
import . "fmt"
```

则调用`fmt.Println`，可以直接使用`Println`调用

3. `_`别名引入，则表示引入包，但是不用具体使用包用元素，包里面的所有`init`函数都会被执行

### 接口命名

1. 接口命名必须是大写
2. 接口中的函数必须使用大写

### 大小写

`Golang`中命名的大小写，有着特殊的含义，首字母用小写命名的元素，无论是**函数**，**结构体**, **全局变量**等等只能在本包内访问，只有首字母用大写命名的函数，才能被外部访问。

所以，一个定义结构体转`json`输出，只有**大写成员**才会被编码进去，因为使用`json`包，是其他包

### Getters/Setters

`Golang`不会为`Getters/Setters`提供自动支持。如果一个私有成员需要被外部有限访问或做格式化访问，可以使用同名函数。如`owner`属性不应该使用`GetOwner`访问，而是使用`Owner`函数访问，如果需要赋值，则可以使用`SetOwner`函数，两者例子如下:

```
owner := obj.Owner()
if owner != user {
    obj.SetOwner(user)
}
```

### 文件命名

文件命名使用小写，如果需要隔开，不适用驼峰命名，同时，`golang`有些特殊约定俗成命名：

* `main.go`是入口文件，一个项目中至允许一个，属于`package main`
* `*_test.go`是对应文件或包的单元测试文件

## 代码结构

### 分号与逗号

`Golang`的每行不需要使用`;`号作为每一行的结束，但，当一行多表达式时：

```go
if err := fn(); err != nil {
  ...
}
```

需要使用`;`进行间隔。

`,`在结构体赋值，`map`，`slice`等进行多行赋值时，需要使用，且最后一个也需要：

```go
var a map[string]string = map[string]string{
  "hello": "aaa",
}
```

而只有一行时，最后一个元素则不需要:

```go
var a map[string]string = map[string]string{"hello": "aaa"}
```

因为，`Golang`使用换行作为代码结束，**本质上，逗号和分号是用来解决歧义使用的！**

### 控制结构

#### if

`if`的基本结构是:

```go
package main

import "fmt"

func main() {

    if 7%2 == 0 {
        fmt.Println("7 is even")
    } else {
        fmt.Println("7 is odd")
    }

    if 8%4 == 0 {
        fmt.Println("8 is divisible by 4")
    }

    if num := 9; num < 0 {
        fmt.Println(num, "is negative")
    } else if num < 10 {
        fmt.Println(num, "has 1 digit")
    } else {
        fmt.Println(num, "has multiple digits")
    }
}
```

* 判断条件**condition**不需要使用`()`包裹，只需要使用空格分隔
* 在`if`中使用`:=`赋值的变量，**仅此结构块中有效**

#### for

`for`的基本结构:

```go
package main

import "fmt"

func main() {

    i := 1
    for i <= 3 {
        fmt.Println(i)
        i = i + 1
    }

    for j := 7; j <= 9; j++ {
        fmt.Println(j)
    }

    for {
        fmt.Println("loop")
        break
    }

    for n := 0; n <= 5; n++ {
        if n%2 == 0 {
            continue
        }
        fmt.Println(n)
    }
}
```

`for`还可以结合`range`结构，用来遍历可迭代结构，例如map和切片:

```go
package main

import "fmt"

func main() {

    nums := []int{2, 3, 4}
    sum := 0
    for _, num := range nums {
        sum += num
    }
    fmt.Println("sum:", sum)

    for i, num := range nums {
        if num == 3 {
            fmt.Println("index:", i)
        }
    }

    kvs := map[string]string{"a": "apple", "b": "banana"}
    for k, v := range kvs {
        fmt.Printf("%s -> %s\n", k, v)
    }

    for k := range kvs {
        fmt.Println("key:", k)
    }

    for i, c := range "go" {
        fmt.Println(i, c)
    }
}
```

> 遍历数组或切片时，第一个返回是下标，第二返回是值
> 遍历map时，第一个返回是key，第二个返回是Vale

#### switch

```go
package main

import (
    "fmt"
    "time"
)

func main() {

    i := 2
    fmt.Print("Write ", i, " as ")
    switch i {
    case 1:
        fmt.Println("one")
    case 2:
        fmt.Println("two")
    case 3:
        fmt.Println("three")
    }

    switch time.Now().Weekday() {
    case time.Saturday, time.Sunday:
        fmt.Println("It's the weekend")
    default:
        fmt.Println("It's a weekday")
    }

    t := time.Now()
    switch {
    case t.Hour() < 12:
        fmt.Println("It's before noon")
    default:
        fmt.Println("It's after noon")
    }

    whatAmI := func(i interface{}) {
        switch t := i.(type) {
        case bool:
            fmt.Println("I'm a bool")
        case int:
            fmt.Println("I'm an int")
        default:
            fmt.Printf("Don't know type %T\n", t)
        }
    }
    whatAmI(true)
    whatAmI(1)
    whatAmI("hey")
}
```

#### select

```go
package main

import (
    "fmt"
    "time"
)

func main() {

    c1 := make(chan string)
    c2 := make(chan string)

    go func() {
        time.Sleep(1 * time.Second)
        c1 <- "one"
    }()
    go func() {
        time.Sleep(2 * time.Second)
        c2 <- "two"
    }()

    for i := 0; i < 2; i++ {
        select {
        case msg1 := <-c1:
            fmt.Println("received", msg1)
        case msg2 := <-c2:
            fmt.Println("received", msg2)
        }
    }
}
```

## 函数

`Golang`函数是一种独特的设计，他允许`动态参数`和`多个返回值`，同时在包中也受**大小写**访问约束。

### 动态参数

定义一个具有动态参数的函数：

```go
func (file *File) Write(b ...byte) (n int, err error)
```

在函数内部使用时，**动态参数**是以一个切片接收，且**动态参数**必须是最后一个参数。

### 多返回值

定义一个多返回值函数:

```
func (file *File) Write(b []byte) (n int, err error)
```

多返回值，意味着，接收函数结果必须要有多个变量，不用结果可以用`_`忽略。

### 命名返回值

当一个函数的返回值被声明：

```go
func (file *File) Write(b []byte) (n int, err error)
```

以为在函数体中，不用在声明两个函数，同时在函数中`return`不带任何值时，默认返回这两个变量

```go

func Test() (n int, err error) {
    ...
    return
}
```

### Defer

`Defer`是Golang函数不使用`try-catch-finally`机制的一种补偿机制，`Defer`之后的代码，在函数退出前执行，用于释放资源这样的共有操作：

```go
func Contents(filename string) (string, error) {
    f, err := os.Open(filename)
    if err != nil {
        return "", err
    }
    defer f.Close()  // f.Close will run when we're finished.

    var result []byte
    buf := make([]byte, 100)
    for {
        n, err := f.Read(buf[0:])
        result = append(result, buf[0:n]...) // append is discussed later.
        if err != nil {
            if err == io.EOF {
                break
            }
            return "", err  // f will be closed if we return here.
        }
    }
    return string(result), nil // f will be closed if we return here.
}
```

需要注意的是，`Defer`可以执行多个函数，但是他是以**LIFO**(后入先出)的方式执行：

```go
for i := 0; i < 5; i++ {
    defer fmt.Printf("%d ", i)
}
```

上面代码是输出结果是`4 3 2 1 0`。

多个`Defer`也是这样：

```go
func main() {
	// for i := 0; i < 5; i++ {
	// 	defer fmt.Println(i)
	// }
	defer fmt.Println(1)
	defer fmt.Println(2)
    defer fmt.Println(3)
    
    // out : 3 2 1
}
```
说明，本质上`Defer`是将延后执行代码，加入到一个`栈`中，在函数结束时，调用此栈。

## 数据结构

### new

`new`是一个内建函数，与其他语言中的不同，它不会初始化内存，而是**清零**（每个类型的零值，是个特殊的存在）。其本质效果是，**建立一个指向类型零值的指针**。

```
p := new(SyncedBuffer)  // type *SyncedBuffer
var v SyncedBuffer      // type  SyncedBuffer
```

本质上是与`&SyncedBuffer{}`等同。

### make

`make`内建函数与`new`不同，她仅用于创建`Slice`, `Map`和`Chan`。返回的也不是一个指针，而是根据类型开辟一个块内存使用。
如：

```
make([]int, 10, 100)
```

三种结构使用`make`结果不同:

* `make([]int, 10, 100)`: 创建切片，第二个参数是初始化长度，第三个是容量
* `make(map[int]int, 10)`: 第二个参数指定容量
* `make(chan int, 1)`: 第二个参数，指定通道使用的缓存，省略和`0`时，无缓存通道，发送和接收都会阻塞

### 数组（Array）

`Go`中的数组是固定的值，主要作用可以避免内存的过度分配，定义上来说：

```Go
array:=[3]int{1,2,3}
```

定义时必须指定其长度，其容量和长度一致，不然就是切片了，可以声明一个根据值来算的长度的数组：

```Go
array := [...]int{1,2,3}
```

数组有以下特性：

* 数组是值，将一个数组复制给另一个数组时，是直接copy所有元素
* 作为函数参数时，数组接收的是该数据副本，而不是指向它的指针
* [10]int与[20]int是两个不同的数组，不可被赋值

### 切片（Slice）

切片是包装数组更通用更动态的结构，在`Golang`中除了特定指定的多维数组（如矩阵）外，大多数时候使用切片来代替数组的使用。

切片是对基础数组的引用，如果一个切片赋值给另一个切片，两个切片是共同引用一个基础数组，对切片的修改，也是对所有调用者可见。

```
a := make([]int, 10, 100)
```

使用`make`分配内存时，会初始化长度，赋予零值，可动态增加到容量最大。

切片元素追加可以使用`append()`函数：

```go
// func append(slice []T, elements ...T) []T

x := []int{1,2,3}
x = append(x, 4, 5, 6)
fmt.Println(x)
```

#### 判断元素是否在切片中

`Golang`标准库没有提供实现，检索元素在不同数据规模中，使用不同算法可能最好的。

可以直接检索:

```go
for _,x := range a {
    if x == "aa" {
        return true
    } 
    return false
}
```

特大数据量时，应该把数据排序，并使用二分法查询，不过最好还是通过包装类型来使用。

### Map

`map`是一种方便且功能强大的映射实现，`key-value`形式的实现，value可以是任意数据类型，而`key`可以是任意实现了**相等运算符**的任何类型，即可以使用`==`判断，可以是整数，浮点数，指针，接口（只要动态类型支持相等）,结构体和数组。**切片不可以**，没有实现相等运算符.

```go
var timeZone = map[string]int{
    "UTC":  0*60*60,
    "EST": -5*60*60,
    "CST": -6*60*60,
    "MST": -7*60*60,
    "PST": -8*60*60,
}
```

亦可以键值赋值：

```go
timeZone["EST"] = -5*60*60
```

#### 判断值是否存在

```go
if _,ok := timeZone["EST"]; ok {
    // do
}
```

## 初始化

`Golang`以包管理组织代码，所以当包引入时，会初始化常量和包变量，以及调用init函数。

### 常量

常量使用`const`关键字声明：

```go
const H = "hello"
```

使用枚举类型的结果，可以使用`itoa`：

```go
const (
    _           = iota // ignore first value by assigning to blank identifier
    KB ByteSize = 1 << (10 * iota)
    MB
    GB
    TB
    PB
    EB
    ZB
    YB
)
```

`itoa`的本质是一个自增的整形，**从0开始**。常量在包被加载时就被初始化，也遵循大小写访问限制

### 变量

包变量使用`var`关键字声明:

```go
var H = "hello"
```
同样的，遵循大小写访问限制

### init函数 

**init函数**是保重的初始化函数，对一些基本资源的初始化，保证程序的正确运行，它有以下特点：

* 无参数无返回值，统一使用`func init(){...}`的形式
* 一个包中可以使用定义多个**init函数**
* 多个**init函数**随机执行，不要依赖他们的顺序
* **init函数**在所有包变量初始化后才执行

## interface

`interface`是**Golang**的接口定义，接口是用来抽象化，向外提供的方法，隐藏实现的细节：

```go
type (

	// Hauthorized_OP 验证操作
	Hauthorized_OP interface {

		// 继承公有接口
		Repository

		// Save 保存数据
		Save(username, t string) error

		// Check 检查
		Check(t string) bool
	}

	hauthorized_op struct {
		collection *mongo.Collection
	}
)
```

接口实现之间是通过隐式实现，不需要专门关键字声明，只需要实现了方法，就是这个接口的实现，这是**鸭子类型**的特性：

```go
func NewHauthorizedOP() (Hauthorized_OP, error) {
    ...
    return &hauthorized_op{
		collection: collection,
	}, nil
}

```

### interface{}

`interface{}`是一个特殊的接口类型，它代表**鸭子类型**的终极，因为这个接口类型，没有定义任何需要实现的方法，所以，它可以接收任何值，作为无泛型的一种补充，比如经常用的一个map:

```go
var m map[string]interface{}
```

### interface check

接口变量可以通过向下强制转型，判断是否是该类型：

```go
if _, ok := val.(json.Marshaler); ok {
    fmt.Printf("value %v of type %T implements json.Marshaler\n", val, val)
}
```

## struct

`struct`是声明结构体的关键字，结构体是最重要的特性，接口可以靠它来实现，业务可以靠它来组织：

```go
type A struct {
    B int
    C interface{}
    d string
}
```

也可以声明一个匿名结构体:

```go
var p struct {
    B int
    C interface{}
    d string
}
```

**结构体成员和方法依旧遵循大小写访问限制**。

### struct{}

`Golang`的最大特点，就是对类型的0值都做了特殊的处理，而`struct{}`也不例外。`struct{}`从字面上看，是一个没有任何成员的结构体，它常常用来做节省内存使用，例如：

```go
package main

import (
	"fmt"
	"unsafe"
)

func main() {
	var a struct {
		c int64
		f struct{}
	}

	var b struct {
		f struct{}
		c int64
	}
	fmt.Println(unsafe.Sizeof(a))
	fmt.Println(unsafe.Sizeof(b))
}
// out: 16 8
```

这段代码中，当`struct{}`作为第一个成员时，它并不占内存，说明`Golang`对此做了特殊处理

## 方法

可以为任何命名类型指定方法，如：

```go
type ByteSlice []byte

func (slice ByteSlice) Append(data []byte) []byte {
    // Body exactly the same as the Append function defined above.
}
```

同时，也可以使用类型的指针作为接受方(receiver):

```go
func (p *ByteSlice) Append(data []byte) {
    slice := *p
    // Body as above, without the return.
    *p = slice
}
```

这二者的区别是，**可以使用指针和值调用值方法，但是只能使用指针调用指针方法**，所以一般情况下，多使用指针来调用

## 附录

### 运算符优先级

|优先级|分类|运算符|结合性|
|:-:|:-:|:-:|:-:|
|1|逗号运算符|,|从左到右|
|2|赋值运算符|=、+=、-=、*=、/=、 %=、 >=、 <<=、&=、^=、\|=|**从右到左**|
|3|逻辑或|\|\||从左到右|
|4|	逻辑与|	&&	|从左到右|
|5|	按位或|	\||	从左到右|
|6|	按位异或|	^|	从左到右|
|7|	按位与|	&|	从左到右|
|8|	相等/不等|	==、!=|	从左到右|
|9|	关系运算符|	<、<=、>、>=|	从左到右|
|10|	位移运算符|	<<、>>|	从左到右|
|11|	加法/减法|	+、-|	从左到右|
|12|	乘法/除法/取余|	*（乘号）、/、%|	从左到右
|13|	单目运算符|	!、*（指针）、& 、++、--、+（正号）、-（负号）|	从右到左
|14|	后缀运算符|	( )、[ ]、->|	从左到右|