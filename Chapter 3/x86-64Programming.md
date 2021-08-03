# 64位编程

和32位编程不同的是，64位编程在格式上作出了一些改动：

## 版本、内存类型和堆栈说明

在64位编程中，下面的代码就不再需要了：

```assembly
.386
.model flat,stdcall
.stack 4096
```

## PROTO关键字

在64位编程中，使用PROTO关键字的语句不再需要参数了：

```assembly
ExitProcess proto
```

## INVOKE伪指令

64位的**MASM**不再支持INVOKE伪指令。例：

如果想使用ExitProcess函数来结束程序，在32位中：

```assembly
invoke ExitProcess,0
```

但是在64位中：

```assembly
mov ecx,0
call ExitProcess
```

## END伪指令

64位中，END伪指令不需要再指明程序入口。例：

```assembly
ExitProcess proto
.code
main proc
	mov eax,5
	add eax,6
	mov ecx,0
	call ExitProcess
main endp
end
```

## 64位寄存器

在一些32位寄存器和变量容纳不下的情况下，可以使用64位寄存器和变量。例：

```assembly
.data
	var64 dq 0

.code
	mov rax,var64
```

