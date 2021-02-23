# 简介

go是编译型语言。

编译型：等同于全文翻译，代码写完，通过编译器翻译成可执行文件.c,c++,golang,java

解释型：等同于实时翻译，解释器读一行代码，解释一行，机器执行一行。python,php,javascript,ruby

## 环境搭建

### 1. 安装golang

https://golang.google.cn/doc/install

下载安装包，解压之后将路径加到PATH中去

```
$ cat ~/.zshrc
# go的安装路径
export GOROOT=/home/mikya/install/go
# 存放bin,pkg,src的路径
export GOPATH=/home/mikya/workspace/go
# GOPATH中的bin目录
export GOBIN=/home/mikya/workspace/go/bin
export PATH=/home/mikya/bin:${GOPATH}/bin:${GOROOT}/bin:${ANDROID_HOME}/tools:${ANDROID_HOME}/build-tools/27.0.3:${ANDROID_HOME}/platform-tools:/home/mikya/install/gradle/gradle-4.6/bin:/home/mikya/bin:${PATH}
```

### 2. 命令简介

go build 生成二进制文件，执行二进制文件即可

go run = go build + 执行二进制。将二进制文件放到临时目录，再执行临时目录中的二进制文件

go install = go build  + 将二进制文件放在GOPATH的bin/pkg目录下。

### 3. 目录结构

```
$GOPATH
|-- bin，存放go install编译生成的可执行文件
|-- pkg，存放go install编译生成的包文件
|-- src，存放go源文件和依赖
```

## 快速上手

### 1. 包管理

- 一个文件夹可称为一个包

- 一个包中可以创建多个文件

- 在同一个包下的文件中必须使用package指定包名且包名是相同的。package指定的包名通常都是和外面的文件夹相同，但也可以不同。

**包分类：**

- main包：main包中必须有main()函数，这个main包中的main函数是程序的总入口。编译的就是一个可执行文件
- 非main包：用来对代码进行分类，分别放在不同的包和文件中。

### 2. 输出

内置函数，官方声明有删除下面函数的可能:

- print

- println

fmt包,推荐:

- fmt.Print

- fmt.Println
- fmt.Printf

### 3. 注释

- 单行注释：//
- 多行注释：/*  */

## 变量

### 1. 变量名

- 包含字母、数字、下划线，数字不能开头，由于go支持utf-8所以这个字母可以是任何语言的字母；

  ```go
  var 中文变量 = "变量是中文"
  fmt.Println(中文变量)
  ```

- 不能使用go内置关键字；

- 采用驼峰命令，如果这个变量支持别的包访问，则首字母应该大写。

- 作用域越小，变量名越短；作用域越大，变量名越长。

### 2. 声明及初始化

四种主要声明关键字：var(变量),const(常量),type(类型),func(函数)

- var name type

  声明变量,变量没有赋值的时候，则会默认填充该类型的零值。

  | 类型   | 零值  |
  | ------ | ----- |
  | string | ""    |
  | int    | 0     |
  | bool   | false |

- var name type =  val

  初始化变量

- var name = val

  go可以根据变量值推导变量的类型，所以可以省略type。此种方式适用于对变量类型要求不严格的场景。

- name :=  val

  短命名，等同于var name = val。常用于局部初始化变量的场景。

- var name1,name2,name3 type

  同时声明多个相同类型的变量

- 声明多个不同类型的变量

  ```go
  var (
  	name = "多"
      age = 18
      hobby = "运动"
      gender string //默认值是""
  )
  ```

**==声明的局部变量一定要使用，否则go编译器就报错；全局变量可以声明不使用==**

### 3. 作用域

在{}中定义的变量，只能在{}中使用，其他地方无法使用。查找变量时，从当前{}找，找不到就去上一级找，依次类推，直至找到全局变量。

- 全局变量

  在go文件的最外层中定义的变量，文件中任一位置都可以使用。定义全局变量要**使用var**,不要用短声明的方式。

  对包里的所有源文件可见。

- 局部变量

  定义在{}中的变量，只能在所定义的{}中使用。可以使用任何声明和初始化方式。

### 4. 变量内存分配

```go
name := "tom"
```

![001](/home/mikya/workspace/notebooks/images/001-1611053938777.png)

```go
name := "tom"
nickname := name
```

![001](/home/mikya/workspace/notebooks/images/001-1611054052597.png)

```go
name := "tom"
nickname := name
//name重新赋值不会影响nickname
name = "jerry"
```

![001](/home/mikya/workspace/notebooks/images/001-1611054294337.png)

**注意事项**：

1. 值类型：

   使用int,string,bool数据类型时，遇到变量的赋值会拷贝一份。不会互相影响

2. 引用类型

   修改某个变量将会影响另外的变量

## 常量

常量是不能修改的变量，所以在定义常量的时候要指定常量的值。一般常量都是定义成全局常量。

### 1. 声明及初始化

- const name type = val

- const name = val
- 多个常量

```go
const (
	v1 = 123
    v2 = 456
)
```

### 2. iota

声明常量时的计数器。

```go
const (
	v1 = 1
    v2 = 2
    v3 = 3
    v4 = 4
)

const (
	v1 = iota //0
    v2 //1
    v3 //2
    v4 //3
)

const (
	v1 = iota +2 //2
    v2 //3
    v3 //4
    v4 //5
)

const (
	v1 = iota +2 //2
    _  //3
    v2 //4
    v3 //5
    v4 //6
)
```

## 流程控制

### 1. 条件语句

```go
if 条件 {
    code
} else {
    code
}

if 条件1 {
    code
} else if 条件2 {
    code
} else if 条件3 {
    code
} else {
    code
}

```

### 2. switch

```go
switch 表达式/变量 {
case 1:
    code
case 2:
    code
default:
    code
}
```

**表达式的值和变量的类型要和case后面指定的值类型一致，否则无法比较。**

### 3. for循环

go中只提供了一种循环方式

```go
#无限循环
for {
    fmt.Println("loop")
    time.Sleep(time.Second * 1)
}

for 条件 {
    code
}

//i只能在for循环中使用，因为是在for循环中生命
for i := 0; i < 3; i++ {
	fmt.Println(i)
}
//省略;隔开的第三列
for i := 0; i < 3; {
	fmt.Println(i)
	i++
}
```

### 4. continue

for循环会终止当前循环，开始下一次循环

```go
func main() {
loop:
	for i := 0; i < 3; i++ {
		for j := 1; j < 4; j++ {
			if j == 2 {
                //终止loop所指向的当前循环
				continue loop
			}
			fmt.Println(i, j)
		}
	}
}
```

### 5. break

跳出所在的循环

```
func main() {
loop:
	for i := 0; i < 3; i++ {
		for j := 1; j < 4; j++ {
			if j == 2 {
                //终止loop所指向的循环
				break loop
			}
			fmt.Println(i, j)
		}
	}
}
```

### 6. goto

跳到指定行，并向下执行代码。

```go
func main() {
	var name string = "jerry"
	if name == "tom" {
		goto svip
	} else if name == "jerry" {
		goto vip
	}

	fmt.Println("预约")
vip:
	fmt.Println("排号")
svip:
	fmt.Println("检查")
}
```

## 运算符

### 1. 算术运算符

![](/home/mikya/workspace/notebooks/images/001-1611629794286.png)

### 2. 关系运算符

![](/home/mikya/workspace/notebooks/images/002.png)

### 3. 逻辑运算符

![](/home/mikya/workspace/notebooks/images/003.png)

### 4. 位运算符

![](/home/mikya/workspace/notebooks/images/004.png)

**计算机中的存储、运算、网络传输等，实质上都是二进制操作**

```go
//1.按位与，都是1才得1
r1 := 5 & 99 //十进制的1
5  -> 0000101
99 -> 1100011
      0000001 -> 1

//2.按位或,只要有1就得1
r1 := 5 | 99 //十进制的103
5  -> 0000101
99 -> 1100011
      1100111 -> 103

//3.按位异或,两者不一样是才得一
r1 := 5 ^ 99 //十进制的102
5  -> 0000101
99 -> 1100011
      1100110   -> 102

//4. 按位向左移动,等价于x*2^n,
r1 := 5 << 2 //十进制的20
5  -> 101
向左移动两位：10100 -> 20 // 等同于5*2^2=20

//5. 按位向右移动 //等同于x除以2^n,向下取整
r1 := 5 >> 2 //十进制的1
5  -> 101
向左移动两位：1 -> 1 // 5 / (2^2) =1.25,向下取整是1

//6.比较清除，以前面的值为基准，两个位置都是1,则前面的位置设为0
r1 := 5 ^ 99 //十进制的4
5  -> 0000101
99 -> 1100011
      0000100 -> 4
```

### 5.  赋值运算符

![](/home/mikya/workspace/notebooks/images/005.png)

### 6. 指针运算符

![](/home/mikya/workspace/notebooks/images/006.png)

### 7. 运算符优先级

![](/home/mikya/workspace/notebooks/images/007.png)

## 进制

计算机底层本质上是二进制。各个进制之间是可以相互转换的。

### 1. 单位

- 位，b,bit,一个二进制位置
- 字节，B,byte8,位为一个字节
- 千字节，KB，1024个字节
- 兆字节，M，1024个千字节
- 千兆，G，1024个兆字节
- 万亿字节，T，1024个G

```txt
1T = 1024G=1024*1024M=1024*1024*1024KB=1024*1024*1024*1024B=1024*1024*1024*1024*8b
```

### 2. 编码

#### 2.1 ascii码

最多用八位二进制个字符，2^8=256个字符

#### 2.2 unicode(万国码)

- ucs2(unicode用两个字节表示所有字符),16位表示所有情况，2^16=65535，支持常见文字
- ucs4,用32位表示所有情况，2^32，支持特殊的字符，emoji图标。

**ucs2和ucs4应该根据具体业务场景来选择。**

#### 2.3 utf-8

utf-8对unicode进行压缩。由于ucs4都是使用4个字节表示码位，导致同样码位的字符会占用更多的空间，故而在网络传输和硬盘存储时都不会用unicode字符集的码位存储，而是把码位转换（压缩）成utf-8等编码的二进制再进行传输和硬盘存储。

- 第一步：根据码位选择转换模板

  ```
    码位范围（十六进制）                转换模板   
    0000 ~ 007F              0XXXXXXX   
    0080 ~ 07FF              110XXXXX 10XXXXXX
    0800 ~ FFFF              1110XXXX 10XXXXXX 10XXXXXX  
    10000 ~ 10FFFF            11110XXX 10XXXXXX 10XXXXXX 10XXXXXX  
    例如：      
    "B"  对应的unicode码位为 0042，那么他应该选择的一个模板。      
    "ǣ"  对应的unicode码位为 01E3，则应该选择第二个模板。      
    "武" 对应的unicode码位为 6B66，则应该选择第三个模板。      
    "沛" 对应的unicode码位为 6C9B，则应该选择第三个模板。      
    "齐" 对应的unicode码位为 9F50，则应该选择第三个模板。      
    注意：一般中文都使用第三个模板（3个字节），这也就是平时大家说中文在utf-8中会占3个字节的原因了。
  ```

- 第二步：码位以二进制展示，再根据模板进行转换

  ```
  码位拆分： 
  "武"的码位为6B66，则二进制为 0110101101100110
  根据模板转换：  
  6    B    6    6    
  0110 1011 0110 0110    
  ----------------------------    
  1110XXXX 10XXXXXX 10XXXXXX    使用第三个模板    
  11100110 10XXXXXX 10XXXXXX    第一步：取二进制前四位0110填充到模板的第一个字节的xxxx位置   
  11100110 10101101 10XXXXXX    第二步：挨着向后取6位101101填充到模板的第二个字节的xxxxxx位置 
  11100110 10101101 10100110    第二步：再挨着向后取6位100110填充到模板的第三个字节的xxxxxx位置
  最终，"武"对应的utf-8编码为 11100110 10101101 10100110
  ```

除了utf-8之外，其实还有一些其他的 utf-7/utf-16/utf-32 等编码，他们跟utf-8类似，但没有utf-8应用广泛。

## 数据类型

### 1. 整型

- 有符号

  `int`,`int8`,`int16`,`int32`,`int64`,`rune`(int32同义词,常用于指明值是unicode码点)

  数字范围:-2^n-1^~ 2^n-1^-1

- 无符号

  `uinit`,`uint8`,`uint16`,`uint32`,`uint64`,`byte`(uint8同义词,用于强调一个值是原始数据而非量值),`uintptr`(大小不明确，但足以完整存放指针，仅用于底层编程)

  数字范围：0~2^n^-1

int和uint,是根据平台定的，可能是32位，也可能是64位，与系统位数一致。但是在同样的硬件平台，编译器可能选用不同大小。

#### 1.1 整型之间转换

类型之间的转换需要显示转换type(variant_name),`int16(v1)`.

```go
var v1 int8 = 1
var v2 int16 = 2
v3 := int16(v1) + v2 // v3是int16
```

- 低位转成高位,没有问题

- 高位转成地位，如果超出低位所在的数字范围，则会轮回低位表示数值范围

  ```go
  var v1 int16 = 130
  v3 := int8(v1) // v3的值是-126
  
  //int8表示范围是-128~127
  130超出127三个数，则进入轮回,超出的数再次从最小的数值开始轮循
  128 --> -128
  129 --> -127
  130 --> -126 //最终130转成-126
  ```


### 2 常见数学运算

```go
math.Abs(-19)               // 取绝对值
math.Floor(3.14)             // 向下取整
math.Ceil(3.14)              // 向上取整
math.Round(3.3478)          // 就近取整
math.Round(3.5478*100) / 100) // 保留小数点后两位
math.Mod(11, 3)         // 取余数，同11 % 3
math.Pow(2, 5)     // 计算次方，如：2的5次方
math.Pow10(2)   // 计算10次方，如：2的10次方
math.Max(1, 2)          // 两个值，取较大值
math.Min(1, 2)          // 两个值，取较小值
```

### 4 指针/nil/声明变量/new

#### 4.1 声明变量

```go
var v1 int //初始值为0
v2 := 99
```

![variant](/home/mikya/notes/images/variant.jpeg)



#### 4.2 指针

两种方式都会会开辟两块内存地址。使用指针会节省内存地址

```go
var v3 *int
v4 := new(int)
```

![point](/home/mikya/notes/images/point.jpeg)

#### 4.3 new关键字

new用于创建内存并进行内部数据的初始化，并返回一个指针类型

#### 4.4 nil

nil是指go语言中的空值

### 5 超大整型

```go
var 
```



### 6 数据转换

#### 2.1 数据转字符串

##### 2.1.1法一：fmt.Sprintf

%b,%d,%o,%x等格式

```go
	x := 123
	f := 0.1
	var x1 int64 = 25
	//转换整数
	y := fmt.Sprintf("%d",x)
	//包含数字以外的其他信息
	//转换浮点数
	fy := fmt.Sprintf("f=%f",f)
	fmt.Println(y,reflect.TypeOf(y)) //123 string
	fmt.Println(fy,reflect.TypeOf(fy)) //f=0.100000 string
```



##### 2.1.2 法二：strconv包的函数

中文包的帮助文档：https://studygolang.com/pkgdoc

strconv.Itoa(==i int==)

strconv.FormatBool(b bool)

以下方法可以指定使用的进制，故而可以用于进制之间的转换，**参数都是64位的**

strconv.FormatInt(==i int64==, base int)

strconv.FormatUint(==i uint64==, base int)

strconv.FormatFloat(==f float64==, fmt byte, prec, bitSize int)

```go
//Itoa中只能传int类型
result := strconv.Itoa(x)
fmt.Println(result,reflect.TypeOf(result)) // 123 string
//FormatInt,第一个参数必须是int64,第二个代表使用什么进制转换(必须在2到36之间)
x1y := strconv.FormatInt(x1,10)
x1y2 := strconv.FormatInt(x1,16)
fmt.Println(x1y,reflect.TypeOf(x1y)) // 25 string
fmt.Println(x1y2,reflect.TypeOf(x1y2)) // 19 string
//FormatFloat
fy1 := strconv.FormatFloat(f,'f',3,32)
fy2 := strconv.FormatFloat(f,'f',10,64)
fmt.Println(fy1,reflect.TypeOf(fy1)) // 0.100 string
fmt.Println(fy2,reflect.TypeOf(fy2)) // 0.1000000000 string
```

#### 2.2 字符串转数据

使用strconv包中的函数。==要确保字符串能转成有效的数据，否则直接转换成目标类型的默认值==。

func Atoi(s string) (==i int==, err error)

func ParseBool(str string) (==value bool==, err error)

以下方法可以指定使用的进制，故而可以用于进制之间的转换，**返回值的类型都是64位的**

func ParseInt(s string, base int, bitSize int) (==i int64==, err error)

func ParseUint(s string, base int, bitSize int) (==n uint64==, err error)

```go
	s1 := "hello"
	s2:= "123"
	s3:="true1"

	s1y, _ := strconv.Atoi(s1)
	//返回0,因为hello不能转成数据
	fmt.Println(s1y,reflect.TypeOf(s1y)) //0 int

	s2y, _ := strconv.Atoi(s2)
    //使用8进制转换，为防止数据范围溢出，使用int10范围来限制，八进制的123转成十进制的83
	s2y1, _ := strconv.ParseInt(s2,8,10)
	fmt.Println(s2y,reflect.TypeOf(s2y)) //123 int
	fmt.Println(s2y1,reflect.TypeOf(s2y1)) //83 int64

	s3y, _ := strconv.ParseBool(s3)
	fmt.Println(s3y,reflect.TypeOf(s3y)) //false bool

```





