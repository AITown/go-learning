1. 一个包（类似于类），里面标识符（包括常量、变量、类型、函数名、结构字段等等）大写相当于public 小写相当于private
2. 
``` go {.lin-numbers}
//表示连个int的参数 返回一个string类型
func sum(a, b int) string {
	return strconv.Itoa(a + b)
}
``` 
3. range 的用法：
``` go {.line-numbers}
str := "abc"
    //迭代打印每个元素 默认返回2个值，一个是元素的位置，一个是元素本身
    for i, date := range str {
        fmt.Printf("str[%d]=%c\n", i, date)
    }
    
```
4. go语言局部变量不能声明但不使用，否则编译会报错
5. a, b = b, a。 可以交换两个变量的值
6. 程序正常退出的代码为0	即Program exited	with	code	0；如果程序因为异常而被终止，则 会返回非零值，如：1（控制台）。
7. 类型别名 type 如：type bigint int64 则：bigint 是int64的别名
7、  Switch 默认包含break可以不写，case后写fallthrough是每个case都执行
8、 不定参数：
func myfunc(args ...int){}
9、参数传递：
func myfunc(args ...int){}
func myfunc2(args ...int){ myfunc(args...)}
10、go语言不支持默认参数
11、defer 延迟调用 
defer fmt.Printf("str[%d]=%c\n", i, date) 最后调用 ，先进后出
       defer 标志的即使异常也会执行 
11、import io "fmt" io为fmt的别名 2:  import _"fmt"  _作用是导入包，只调用init 函数
12、指针：
默认是nil 没有null 
操作符&是取变量地址。* 是通过指针访问目标对象
不支持指针运算 ，不支持-> 运算 ，直接用"."访问目标成员
    var a int = 10
    var p *int
    p = &a //把a的地址给p
    fmt.Printf("p=%v,&a=%v\n", p, &a)
    *p = 666 //*p操作的不是p的内存 而是p所指向的内存就是a 所以a=666
    fmt.Printf("*p=%v,a=%v\n", *p, a)

13、new 分配内存 用于值类型  int struct
p:=new（int） 
*p=666

14、 指针传递
func main() {
    a, b := 10, 20
    swap(&a, &b)
    fmt.Printf("交换后：a=%d，b=%d\n", a, b)
}
func swap(a, b *int) {
    *a, *b = *b, *a
}

15、数组: 
值类型
没有初始化的元素自动为0
数组的容量必须在初始化固定下来
var a[10]int=[5]int{1,2,3,4}
d:=[5]int{2:10}//定义下标2的值为10
a := [...]int{1, 2, 3, 4, 5, 6}

16、切片
引用类型
arr：=[5]int{1,2,3,0,0}
s:=arr[0:3:5]//切片三个参数  low  hign  max   代表：数组的第一个起始下标 、终止下标，容量（下标是左闭右开）
//数组的len跟cap是固定的
    a := [5]int{}
    //切片[]为空，或者为... len、cap可以不固定
    s := []int{}
    s = append(s, 11) //给切片末尾加一个成员
//借助make函数初始化(切片类型，长度，容量)，容量可忽略，跟长度一样
    s2：=make([]int,5,10)
切片操作的数组，切片的对应值变了，数组的值也变了

17、切片
append  如果超过原来的容量则容量变成2倍。
    var a []int
    a = append(a, 10, 20, 30)
//... 展开切片
    a = append(a, a...)
    fmt.Println(a)
copy

18、sort
a := [...]int{1, 6, 2, 3, 4, 5, 6}
 index := sort.SearchInts(a[:], 5)
  fmt.Println(index) 
输出：5 默认输出排序后的结果


字符串：
    str := "hello world"
    //转化字符串
    s1 := []rune(str)
    s1[0] = 't'
    fmt.Println(string(s1))

18、字典：
map[int]string{1:"123",2:"234"}  无序的，每次循环打印顺序不一样
map 没有cap，只有len
创建：m2：=make（map[int]string）  
m3 := make(map[int]string, 3)
    fmt.Println("长度=", len(m3))  输出：0

操作：赋值。如果已经存在 则修改内容
添加，map底层自动扩容与切片的append类似
遍历：无序的，每次的结果不一样
删除：delete（map,key）
19、锁
  互斥锁  读写锁
 sync.Mutex
 sync.RWMutex

20、终端读写：

 os.Stdin 标准输入
 os.Stdout 标准输出

import (
    "bufio"
    "fmt"
    "os"
)

func main() {
//bufio 带缓冲区的读写
    reader := bufio.NewReader(os.Stdin)
    str, _ := reader.ReadString('\n')
    fmt.Println("this is Stdin", str)

21、序列化

包："encoding/json"
json.Marshal//序列化
json.Unmarshal//反序列化  

22、单元测试

包：testing
运行命令：go test 
go test -v 可以查看详细信息
文件名必须以_test.go结尾
函数名必须以Test开头 参数必须以test开头，参数后面必须.T 如 testing.T

23、 获取路径//获取当前程序执行的路径
path, err := os.Getwd()
    if err != nil {
        fmt.Println("获取路径错误：", err)
    }
    fmt.Println(path)
24、判断路径是否存在
// 如果返回的错误为nil,说明文件或文件夹存在
// 如果返回的错误类型使用os.IsNotExist()判断为true,说明文件或文件夹不存在
// 如果返回的错误为其它类型,则不确定是否在存在
func PathExists(path string) (bool, error) {
    _, err := os.Stat(path)
    if err == nil {
        return true, nil
    }
    if os.IsNotExist(err) {
        return false, nil
    }
    return false, err
}
25、 创建路径
    //没有则创建
    e := os.MkdirAll(path, os.ModeDir)
    if e != nil {
        err = e
        return
    }
26、


19、go buid 指定目录

生成到bin下
20、路径应用 

 add 代码：
package calac

func Add(i, j int) int {
    return i + j
}
main.go 代码：
package main

import (
    "PathReferenceLearn/calac"
    "fmt"
    
)

func main() {
    sum := calac.Add(1, 2)
    fmt.Println(sum)
}

21、结构体：
type student struct {
    id   int
    name string
}
func main() {
    //顺序初始化。每个必须初始化
    st1 := student{1, "gjf"}
    //指定初始化，没有的为0
    st2 := student{id: 1}

    //先定义一个普通结构体变量
    var st3 student
    //再定义一个指针变量，保存st3的地址
    var p1 *student
    p1 = &st3
    //通过指针操作，p1 (*p1) 等价
    p1.id = 10
    (*p1).name = "gjf"

//通过new操作
    p2:=new(student)
    p2.id=9
    p2.name="gjf"




12、go install


6、下面列举了 Go 代码中会使用到的 25 个关键字或保留字：
break default func interface select
case defer go map struct
chan else goto package switch
const fallthrough if range type
continue for import return var
除了以上介绍的这些关键字，Go 语言还有 36 个预定义标识符： 
append bool byte cap close complex complex64 complex128 uint16
copy false float32 float64 imag int int8 int16 uint32
int32 int64 iota len make new nil panic uint64
print println real recover string true uint uint8 uintptr
