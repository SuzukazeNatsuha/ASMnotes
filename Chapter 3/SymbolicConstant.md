[TOC]

# 符号常量

通过为整数表达式或文本指定标识符来创建符号常量。符号**不预留存储空间**，只在汇编器扫描程序的时候使用，并且在运行的时候不会改变。下面是变量和符号的对比：

|      | 符号 | 变量 |
| :----: | :----: | :----: |
| 内存占用 | × | √ |
| 更改内容 | × | √ |

## 等号伪指令

等号伪指令把一个符号名称和一个[整数表达式](BasicLanguageElements.md#整型常量表达式)连接起来。例：

```assembly
name = expression
```

通常表达式是一个**32位**的整数。当程序在汇编时，在预处理阶段，所有的name将会被替换成expression。

使用等号伪指令，用一个符号去替换一个表达式，可以便于阅读和维护程序。

### 当前地址计数器

这是最重要的符号之一，用 $ 表示。例：

```assembly
var1 dword $
```

意思是用var1的偏移量来为var1赋值。

### 键盘定义

程序通常定义符号来识别常用的数字键盘代码。例：

```assembly
Esc_Key = 27
```

### 使用DUP操作符

为了简化程序的维护，使用符号来当作DUP的计数器。例：

```assembly
array1 byte 20 dup('a')

count = 20
array2 byte count dup('a')
```

### 重定义

用 = 定义的符号可以在一个程序内被重复定义。例：

```assembly
count = 20
array dword count dup(5)
count = 40
```

数值的改变并不会影响程序的运行顺序。

## 计算数组和字符串的大小

在编程中，通常会用到数组或者字符串的大小，此时如果要显式地声明其大小，容易造成错误。结合使用符号 $ 和数组或者字符串的首地址，可以很轻松地获取数组或字符串的大小。例：

```assembly
array byte 'hello there',0
arraySize = ( $ - array )
```

**注意：arraySize或者其他表示大小的变量必须紧跟在数组或者字符串的后面。**

### 字数组和双字数组

如果数组包含的不是byte的时候，就可以用他们占据的存储空间除以单个元素的大小来得到长度。例：

```assembly
array1 word 1001h,1002h,1003h,1004h
length = ($ - array1) / 2

array2 dword 10000001h,10000002h,10000003h
length2 = ($ - array2) / 4
```

## EQU伪指令

EQU伪指令把一个符号和一个整数表达式或者一个任意文本连接起来。它有三种格式：

```assembly
name1 equ expression
name2 equ symbol
name3 equ <contant>
```

第一种的表达式必须有效；第二种的符号必须是用 = 或者EQU定义过的；第三种任何文本都可以出现在< >中，包括但不限于字符串，非整数值。

**注意：用EQU定义的值不能被改变。**

## TEXTEQU伪指令

类似于EQU，TEXTEQU创建了一个文本宏。它有三种格式：

```assembly
name1 textequ <text>
name2 textequ textmacro
name3 textequ %constExpr
```

第一种分配文本；第二种分配已有的文本宏；第三种是整数常量表达式。

文本宏可以被相互构建，例：

```assembly
move textequ <mov>
cmd textequ <move eax,5>
```

和equ不同的是，textequ可以被重定义。
