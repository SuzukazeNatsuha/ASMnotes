## 程序模板

通常asm小程序可以有一个固定简单结构：

```assembly
.386 ;表示为x86程序

.model flat,stdcall ;声明内存模式为flat，子程序调用规范为stdcall

.stack 4096 ; 设置堆栈大小为4096byte
ExitProcess proto, dwExitCode:dword ;声明exitprocess函数原型，其为一个标准Windows服务
								;包含函数名，proto关键字，参数列表	
.data
;insert vars

.code
main proc
; type codes here
	invoke ExitProcess,0
main endp
end main
```

