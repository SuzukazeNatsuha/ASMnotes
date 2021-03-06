[TOC]

# 基本语言元素

## 变量

可以在data区添加变量，例：

```assembly
.data
 var dword 0 ;声明一个dword类型的变量var，并且赋值0
```

## 整型常量

由一个可选前置符号，若干数字，以及一个可选基数字符构成，即：

```
[( + | - )] digits [ radix ]
```

radix字符表如下：

| 字符  |   含义   | 字符 |      含义      |
| :---: | :------: | :--: | :------------: |
|   h   | 十六进制 |  r   |    编码实数    |
| q / o |  八进制  |  t   | （备用）十进制 |
|   d   |  十进制  |  y   | （备用）二进制 |
|   b   |  二进制  |      |                |

**注意：以字母开头的十六进制数字前面必须加0，以防识别为标识符。**

## 整型常量表达式

包含算术运算符和整型常量的算术表达式。其计算结果必须是一个整数，并可以用32位存放。这种表达式只在汇编时计算。

**建议：多使用括号去表明操作顺序。**

## 实数常量

浮点数常量，表示十进制实数和编码十六进制实数。包括一个可选符号，整数，小数点，可选的小数部分整数，指数。

```
[( + | - )] integer.[integer] [exponent]
```

其中指数部分表示如下：

```
E [(+ | -)] integer
```

**注意：至少要有一个整数部分和一个小数点。**

编码实数表示的是十六进制实数，用IEEE浮点数格式表示短实数。

## 字符常量

用单引号或者双引号包含的一个字符，汇编器保存其ASCII码的数值。

## 字符串常量

用单引号或者双引号包含的一个字符序列。

## 保留字

具有特殊意义并且只能在正确的上下文使用，**没有大小写[^1]之分**：

- 指令助记符
- 寄存器名称
- 伪指令
- 属性
- 运算符
- 预定义符号

## 标识符

用于标识子程序，变量，常数和代码标签：
- 包含最多247字符
- 不区分大小写[^1]
- 第一个字符必须为字母，_ ，@  ，? ，$ 之中的任意一个
- 标识符不能与[保留字](#保留字)相同

**注意：一般情况下，尽量避免使用 @ 或者 _ 作为第一个字符。**

## 伪指令

嵌入源代码的命令，由汇编器识别和执行。伪指令**不在**运行时执行，但是它们可以定义变量、宏和子程序；为内存段分配名称，以及其他操作。默认状态下也不区分大小写[^1]。
**注意区分指令与伪指令：**

```assembly
var dword 21 ;dword是伪指令，声明var是dword类型的变量
mov eax,var ;mov是指令，将var的值移入寄存器eax
```

### 定义段

伪指令的一个重要功能就是定义程序区段。程序中不同的段有不同的作用，例：

```assembly
.data ;变量定义段

.code ;可执行指令区段

.stack ;定义运行时堆栈
```

### 指令

在程序汇编编译时变得可执行。运行程序时由CPU加载执行。一条指令有四个组成部分：

- 可选的标号 label
- 指令助记符 mnemonic
- 操作数 operands
- 可选的注释 comment

```
[ label: ] mnemonic operands [ ;comment ]
```

#### 标号

指令和数据的位置标记。位于命令或者变量的前端，表示命令或变量的地址。标号有两种类型：

##### 数据标号

表示变量的位置，例：

```assembly
count dword 15 ;count为标号
```

可以在一个标号后面定义多个数据，例：

```assembly
array dword 1, 2
      dword 3, 4
```

##### code区标号

必须用 : 结束。代码标号用作跳转和循环指令的目标，例：

```assembly
;多行代码的标号
target:
	mov eax,5
	add eax,6
	jmp target
	
;单行标号
Label1: mov eax,5
Label2: mov eax,6
```

标号命名规则和标识符一致。只要每个标号在其封闭子程序中唯一，则可以多次使用同名标号。

#### 指令助记符

标记指令的短单词。例如：

```assembly
mov eax,1
sub eax,0
inc eax
```



#### 操作数

指令输入输出的数值。个数范围为0~3。每个操作数可以为**寄存器、内存操作数、整数表达式、输入输出端口**。

操作数有固有顺序。通常在含有多个操作数的指令中，第一个为目标操作数，后面的是源操作数，例：

```assembly
imul eax,ebx,5 ;eax为目标操作数，ebx和5为源操作数；将ebx和5相乘，结果复制进eax。
```



#### 注释

单行注释为 ; 

多行注释用comment伪指令和自定义符号构成，例：

```assembly
comment %
程序清单的开始部分通常包含以下信息：
 1.程序目标说明
 2.程序创建者或者修改者名单
 3.程序创建或者修改日期
 4.程序实现技术的说明
%
```



#### 空指令

NOP指令在程序空间占**一个字节**，不做任何操作，通常用于对其代码地址边界。

x86处理器被设计为从**dword偶数倍地址**处加载代码和数据，这能使得加载速度更快。







[^1]:可以在运行编译器时添加 -Cp使得大小写敏感。