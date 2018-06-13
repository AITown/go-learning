同一个文件夹下不能有两个package 否则 go build 会报错
如下：gobuildTest 下有两个包 mian包跟addnum包


add.go
package addnum

Add  数字之和
func Add(a, b int) int {
    return a + b
}

main.go
package main

import (
    addnum
    fmt
)

func main() {
    sum = addnum.Add(1, 2)
    fmt.Println(sum, sum)

}

如果两个包在一个文件夹里  go build 会报错


如下：包addnum 包在addnum文件夹里（最好包名跟文件名一致）


这种情况下 main包引用了addnum包 
package main

import (
    gobuildTestaddnum

    fmt
)

func main() {
    sum = addnum.Add(1, 2)
    fmt.Println(sum, sum)

}
则只需要 go build main.go 即可

2.同一个文件夹下一个包，多个文件问题
如下图：gobuildTest文件夹下有两个go文件 


add.go
package main

func add(a, b int) int {
    return a + b
}

main.go
package main

import (
    fmt
)

func main() {
    sum = add(1, 2)
    fmt.Println(sum, sum)

}

编译1：go build 文件夹名 会生成一个与文件夹名一样的exe 运行即可


编译2：go build每一个go文件， 会生成一个build后的第一个 .go文件的名字的exe

