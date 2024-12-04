# Golang

### 简介

- Go，类C语言，性能和并发处理能力强，编译和运行效率高，在服务器端开发、云计算、分布式系统用途广泛     

- Go语言没有类和继承的概念，通过结构体和interface实现面向对象，不支持函数重载；    

- Go语言支持指针，引入协程(goroutine)实现并发，通过Channel实现goroutine间通讯，函数方法支持多个返回值，通过 recover 和 panic 来代替异常机制

### go的程序结构

包含包声明、导入包、函数、语句和表达式、注释  

```go
// 包声明
package main

// 导入包
import "fmt"

// 函数定义 函数名（参数） 返回值
func main() {
    // 表达式
    fmt.Println("Hello, World!")
}
```



### 变量和常量

```go
// 声明变量，变量名称在前，类型在后，变量声明时可以直接初始化  
var name string = "Hello Go"
// 短变量声明并初始化，只能在函数体内使用（这里是举例子没写函数），go语言会进行类型推导
n := 10

// 声明常量
const pi = 3.1415
const e = 2.7182
// 或
const (
	pi = 3.1415
	e = 2.7182
)

//iota常量生成器,iota出现时将被置为0，可以理解为const语句块的行索引
const{
	a=iota  //0
	b       //1
	c       //2
}
```



### 数据类型

注：go不支持隐式数据类型转换，需显示转换    

数值型（整型、浮点型、其他数值类型）    

布尔型    

字符串型     

派生型（指针、数组、结构体struct、接口、切片、Map）    

```go
// 数值型
var i int = 10
var f float32 = 12.34

// 布尔型
var b bool = true
cBool := true

// 字符串型
var s string = "Hello Go"
var arr1 [5]int // 声明了一个int类型的数组
arr2 := [3]int{1,2,3} // 声明了一个长度为3的int数组
arr3 := [...]int{2, 4, 6, 8, 10} // 声明了一个长度为5的int数组,元素是2, 4, 6, 8, 10

// 切片会自动扩容
var mySlice []int //声明一个int切片
mySlice = []int{2, 3, 5, 7, 11, 13} // 创建包含6个元素的切片
mySlice = []int{2, 3, 5, 7, 11, 13}[1:4] // 创建包含原切片元素1-4的切片

// map类型
var m map[string]int
m["key1"] = 1
```

#### 指针

- 用&获取变量地址，如&ptr，可赋值给指针  

- 指针类型：基本数据类型前面加*号声明指针类型，比如 *int    

- 指针取值：对普通变量使用&取地址后会获得这个变量的指针，然后就可以对指针使用*操作进行指针取值    

```go
var x int = 10
var p *int = &x // 获取变量x的地址，并赋值给指针p
fmt.Println("x的值:", x) // 输出: x的值: 10
fmt.Println("p指向的值:", *p) // 输出: p指向的值: 10
fmt.Println("x的地址:", p) // 输出x的地址
```

#### 数组

```go
// 数组定义
var arr [5]int
 
// 初始化数组
arr = [5]int{1, 2, 3, 4, 5}
```

#### 切片

- 定义切片和定义数组很像，区别就是定义一个切片不需要指定长度；  

- 切片实际上是对底层数组的引用   

- 切片长度指元素个数，切片容量指底层数组的长度  

```go
//切片定义
var s1 []int // 可变长

//从数组创建切片
arr := [5]int{10, 20, 30, 40, 50}
s := arr[1:4]  // 从arr数组的第1到第3个元素创建一个切片
fmt.Println(s) 

//make函数创建切片
s := make([]int, 3)    // 创建一个长度为3的切片, 容量默认3
s := make([]int, 3, 5) // 创建一个长度为3，容量为5的切片
```



#### Map

Go语言的map底层实现基于哈希表

```go
func main() {
	//字面量初始化
	test := map[string]int{
		"测试1": 1,
		"测试2": 2,
	}
	for key, value := range test {
		fmt.Println(key, value)
	}
	//使用make初始化map
	makeMap := make(map[string]int)
	makeMap["测试3"] = 3
	makeMap["测试4"] = 4
	for key, value := range makeMap {
		fmt.Println(key, value)
	}
}
```

#### 结构体

```go
// 定义
type Person struct {
    Name string
    Age  int
    Address string
}

// 创建结构体实例，可以直接在定义结构体时初始化字段值
p1 := Person{Name: "Alice", Age: 30, Address: "Beijing"}

// 创建结构体实例，可以使用new函数创建一个结构体的指针，然后设置指针的字段值
p2 := new(Person)
p2.Name = "Bob"
p2.Age = 25
p2.Address = "Shanghai"
```



### 控制与循环语句

#### if条件语句 

```go
if(a==10){
	println("success")
}else{
	println("failed")
}
```

#### switch语句

```go
//switch后面加变量名；switch语句不需要写break，默认带break；case执行完后自动跳出；
switch day {
case 1:
    fmt.Println("星期一")
case 2:
    fmt.Println("星期二")
case 3:
    fmt.Println("星期三")
default:
    fmt.Println("其他")
}

//switch使用fallthrough将不跳出循环,后面的语句无条件执行 
switch num {
case 1:
	println("1")
	fallthrough
case 2:
	println("2")
	fallthrough
default:
	println("default")
	fallthrough
}
```

#### select语句

select 类似switch，一般专门用于处理通道（channel）的操作

如果有多个可运行的case会随机选择一个执行，没有case可运行就会阻塞，直到有case可运行；  

```go
select { //不停的在这里检测
    case <-chanl : //检测有没有数据可以读
    //如果chanl成功读取到数据，则进行该case处理语句
    case chan2 <- 1 : //检测有没有可以写
    //如果成功向chan2写入数据，则进行该case处理语句
    //假如没有default，那么在以上两个条件都不成立的情况下，就会在此阻塞
    default:
    //如果以上都没有符合条件，那么则进行default处理流程
    }
```

#### for循环

```go
// go只有for循环，没有while关键字：
// 标准的for循环语句，初始化变量、循环判断条件，后处理，这三个部分都可以不指定
for i := 0; i < 10; i++ {
   fmt.Println(i)
}

// 类似while循环的使用
i := 0
for i < 10 {
    fmt.Println(i)
    i++
}

// 无限循环
for {
    fmt.Println("This loop will run forever.")
}
```

range循环，通常与for循环结合使用，range可以对slice/map/数组、字符串进行迭代循环

#### 循环控制

- break  中断for/switch/select

- continue

- goto   跳转

```go
//for循环加上标签loop,go to loop跳到loop  
Loop:
for i < 5 {
    fmt.Println(i)
    i++
    goto Loop
}
```



### 函数

- 函数返回值类型写在最后面，支持多返回值      

- 不能用容器变量接收多返回值，只能用多个变量接收    

- 函数传参默认是值传递    

```go
package main

import "fmt"

// 定义一个函数，返回两个数的和和差
func calc(a int, b int) (int, int) {
    sum := a + b
    diff := a - b
    return sum, diff
}

func main() {
    sum, diff := calc(10, 3)
    fmt.Println("Sum:", sum, "Difference:", diff)
}
```



### 并发编程-通道

协程goroutine可以理解为轻量级的线程，可以理解为执行并发的子程序  

通道可以在goroutine之间传递数据，实现数据的同步和共享  

- 无缓冲通道：直接用 make(chan T) 创建，是默认通道类型

- 缓冲通道：用 make(chan T, capacity) 创建，capacity 是通道的缓冲区大小  

  缓冲通道允许在缓冲区未满时发送数据，在未空时接收数据，给通道写入数据时不能超过缓冲区容量  

通道定义完写入数据、读取数据时缓冲区**类似队列**，数据先入先出   

```go
// 创建一个字符串类型无缓冲通道
ch := make(chan string)

// 创建一个缓冲容量为 3 的整型通道
ch1 := make(chan int, 3)
 
// 发送数据到通道
ch <- "Hello"
 
// 从通道接收数据
msg := <-ch
fmt.Println(msg)
```



### 错误处理

```go
//errors是实现了error接口的类型，用来返回错误信息
func divide(x, y int) (int, error) {
    if y == 0 {
        return 0, errors.New("division by zero")
    }
    return x / y, nil
}

// panic函数用于引发程序中的严重错误，导致程序崩溃;
// recover函数用于在发生panic时进行错误恢复
func readConfig(filename string) error {
    //defer语句用于延迟执行函数调用，会在函数执行完毕之后执行，无论函数是正常返回还是发生了错误
    defer func() {
        if r := recover(); r != nil {
            fmt.Println("Recovered from panic:", r)
        }
    }()
    
    if _, err := os.Stat(filename); err != nil {
        panic("config file not found")
    }
}
```