# IanSeyler/easy_x86-64 [![explain]][source] [![translate-svg]][translate-list]

<!-- [![size-img]][size] -->

[explain]: http://llever.com/explain.svg
[source]: https://github.com/chinanf-boy/Source-Explain
[translate-svg]: http://llever.com/translate.svg
[translate-list]: https://github.com/chinanf-boy/chinese-translate-list
[size-img]: https://packagephobia.now.sh/badge?p=Name
[size]: https://packagephobia.now.sh/result?p=Name

「 汇编之 Intel 语法的表面简介 」

[中文](./readme.md) | [english](https://github.com/IanSeyler/easy_x86-64)

---

## 校对 ✅

<!-- doc-templite START generated -->
<!-- repo = 'IanSeyler/easy_x86-64' -->
<!-- commit = '9000b7d515e1bd3d03e508fd178c441051a91203' -->
<!-- time = '2013-04-17' -->

| 翻译的原文 | 与日期        | 最新更新 | 更多                       |
| ---------- | ------------- | -------- | -------------------------- |
| [commit]   | ⏰ 2013-04-17 | ![last]  | [中文翻译][translate-list] |

[last]: https://img.shields.io/github/last-commit/IanSeyler/easy_x86-64.svg
[commit]: https://github.com/IanSeyler/easy_x86-64/tree/9000b7d515e1bd3d03e508fd178c441051a91203

<!-- doc-templite END generated -->

- [x] index.zh.md

### 贡献

欢迎 👏 勘误/校对/更新贡献 😊 [具体贡献请看](https://github.com/chinanf-boy/chinese-translate-list#贡献)

## 生活

[hIf help, **buy** me coffee —— 营养跟不上了，给我来瓶营养快线吧! 💰](https://github.com/chinanf-boy/live-need-money)

---

“我是小巧计算器的操作员。” - Kraftwerk

最近对汇编很感兴趣（无论是现实的[6502](http://skilldrick.github.com/easy6502/)，或虚构的 DCPU-16；我甚至在 2007 年创建了自己的虚拟 8 位 CPU，名为 i808）但没有关注当今计算机中最受欢迎的架构。如果您在台式机，笔记本电脑或服务器上阅读此内容，那么您的计算机很可能使用的是 x86-64（或 x86）。x86-64 是 32 位 x86 架构的 64 位超集，AMD 或 Intel 的任何现代 CPU 都支持它。本文档将重点介绍 x86-64 中最常用的部分。

汇编语言是计算机中最低级别的抽象，人类仍然可以轻松阅读。汇编语言直接转换为计算机处理器执行的字节。

学习汇编是一项有用的练习，可以让您更深入地了解“引擎盖下”发生的事情。虽然绝大多数编程是通过诸如 C，C ++，Java 等高级语言完成的，但如果执行速度是高优先级，那有时编写部分汇编代码段也是很可以的嘛(还能装个 B)。例如，对于 3D 游戏或科学过程进行大量数学计算的代码片段，就显示了汇编语言实现的可加速性。

在本文档中，我们将使用“Intel”[语法](http://en.wikipedia.org/wiki/X86_assembly_language#Syntax)，而不是'AT＆T'。因此，会是以下的，多参数操作码形式：

```
opcode destination, source
```

x86-64 汇编语言中，带前缀“0x”的任何数字（以及本文档中的扩展名）都是十六进制（十六进制）格式。如果您不熟悉十六进制数字，我建议您在开始之前，阅读[维基百科文章](http://en.wikipedia.org/wiki/Hexadecimal)。

## 寄存器

寄存器可能是 x86-64 架构中最复杂的部分，主要是由于传统的 32 位和 16 位 x86 架构产生的复杂性。x86-64 有 16 个 64 位通用寄存器，名为 R0-R15。这些寄存器可以按位大小分解为单独的部分，也可以通过旧 x86 名称引用。[这里](http://www.sandpile.org/x86/gpr.htm)可以找到有关寄存器名称和故障的更多信息。

例如，R0 是 64 位寄存器（也称为四字）。如果您只想使用 32 位，则该部分可以由 R0D（双字）引用，16 位由 R0W（一字）引用，或者由 R0B（一字节）引用 8 位。

这些 D，W 和 B 参考是来自[字(word)是 16 位](http://en.wikipedia.org/wiki/Computer_word)那时的延续：

- 8 位 = 1 字节或'半字'
- 16 位 = 2 字节= 1 个字
- 32 位 = 4 字节= 2 个字= 1 个双字
- 64 位 = 8 字节= 4 个字= 1 个四字

根据具体的寄存器，某些操作码会出现进一步的复杂情况。这将在乘法和除法部分中，进行更详细的探讨。

## 基本操作

最基本的操作是为寄存器分配值，或在两个寄存器之间移动值。在 x86-64 中，这称为移动或**mov**。这个术语具有误导性，因为没有任何移动行为；它只是被复制或存储。

```
mov rax, 15						; 将值 15 存储在 rax 中
mov rcx, rax					; 将 rax 中的值复制到rcx
mov rbx, 18446744073709551615	; 将最大可能的64位数 存储在rbx中
```

## 加和减

我们可以把具体寄存器**add**到一起：

```
mov rax, 11				; 将值 11 存储在 rax 中
mov rcx, 500			; 将值 500 存储在 rcx 中
add rax, rcx			; 将 rcx 中的值添加到rax
```

我们也可以把一个值**add**到寄存器：

```
mov rax, 25				; 将值 25 存储在 rax 中
add rax, 12				; 添加 12 到 rax; rax 现在包含 37
```

我们可以用一个寄存器值，**sub**另一个寄存的值：

```
mov r15, 1337			; 将值 1337 存储在 r15 中
mov r12, 55				; 将值 55 存储在 r12 中
sub r15, r12			; 从 r15 中，减去 r12 中的值
```

我们也可以帮一个寄存器值，**sub**一个值：

```
mov rcx, 123			; 将值 123 存储在 rax 中
sub rcx, 24				; 从 rcx 中减去24; rcx 现在包含 99
```

加和减，可以与任何可用的寄存器一起使用。

## 乘法和除法

在本节中我们将使用**mul**和**div**操作码。这些操作更复杂，突出了几个寄存器的独特用途。

```
mov rax, 50				; 将值 50 存储在 rax 中
mov rcx, 12				; 将值 12 存储在 rcx 中
mul rcx					; 通过 rcx 乘以 rax。在这种情况下，rax 将设置为 600
```

初始号码必须存储在 rax 中。rax 可以乘以任何其他寄存器中的值。结果将存储在 rdx:rax 中。

```
mov rax, 800			; 将值 800 存储在 rax 中
mov rdx, 0				; 将 rdx 清除为0
mov rbx, 100			; 将值 100 存储在rbx中
div rbx					; 用 rbx 除 rdx:rax。在这种情况下，rdx将设置为 0，rax将设置为 8
```

寄存器 rdx:rax 必须持有被除数，而任何其他寄存器都可以持有除数。div 操作码执行后，商存储在 rax 中，余数存储在 rdx 中。

## 分支

分支允许我们根据特定条件重定向程序流。可以使用比较，来检查这些条件。

比较（cmp），允许我们比较两个寄存器的内容，系统标志将根据比较结果设置。然后，我们可以根据这些系统标志，更改代码执行。

让我们尝试一个简单的 c'for'循环。

```
	mov rax, 0			; 将rax设置为0
increment_loop:
	add rax, 1			; 在 rax 中添加 1
	cmp rax, 10			; 将 rax 中的值与10进行比较
	jne increment_loop	; 如果它们不相等，则跳转到increment_loop
```

上述代码将循环 10 次。jne 指的是“如果不相等则跳转”。这意味着如果 rax 不包含值 10，执行将跳转回“increment_loop”。还有许多其他跳转命令：

| jmp | 跳转-不查看系统标志的直接跳转 |
| --- | ----------------------------- |
| je  | 相等时跳                      |
| jne | 如果不等于，则跳转            |
| jl  | 小就跳                        |
| jle | 小于或等于时跳转              |
| jg  | 大就跳                        |
| jge | 大于或等于时跳转              |

另一种分支是函数调用(call)。函数调用允许我们跳转到代码的特定部分，当函数调用完成时，该部分将返回到函数调用离开的位置。

```
	mov rax, 14					; 将 rax 设置为 14
	mov rcx, 23					; 将 rcx 设置为 23
	call add_and_subtract_one	; 调用该函数
	cmp rax, 5
	je test_function_sucess		; 如果 rax == 5 然后跳转，如果没有则继续下一行

	...

add_and_subtract_one:			; 将 rcx 添加到 rax，然后减去 1 的函数
	add rax, rcx
	sub rax, 1
	ret
```

## 访问内存

寄存器可用于读取和写入系统内存。mov 操作码的使用方式与我们之前看到的类似。除了，我们可以使用一个字面值，表示内存地址，其被封装在[方括号][square brackets]内。

```
mov rax, [0x200000]		; 将 64 位值，从内存地址 0x200000 复制到rax
mov [0x402000], rbx		; 将 64 位值，从 rbx复制到内存地址0x402000
```

## 栈

栈是用于存储临时信息的内存区域。栈是后进先出（LIFO）数据结构。`push`(推送) 操作将添加到列表顶部，`pop`(弹出) 操作将从列表顶部删除一个项目。如果您将数字 5、7 和 15 推到栈中，您将首先将它们弹出为 15，然后再弹出为 7，最后弹出为 5。在汇编中，您可以将寄存器推到栈上，稍后再将其弹出——当您想保存寄存器的值，同时将该寄存器用于其他用途时，这种功能非常有用。

```
mov rax, 25				; 将值 25 存储在 rax 中
push rax				; 将 rax 中的值推入栈
mov rax, 12				; 将值 12 存储在 rax 中
pop rax					; 将栈中的第一个值,弹出到 rax。在这种情况下，rax 再次设置为 25。
```

没有要求说，就一定要同一个寄存器来回 push 和 pop。例如，这两个部分的结果相同：

```
mov rcx, rax			; 将 rax 中的值复制到rcx

push rax				; 将 rax 中的值推入栈
pop rcx					; 将栈中的第一个值弹出到rcx
```

## 进一步阅读

本文档仅蹭到了 x86-64 体系结构中，可用的操作码和功能的表面。

Intel 软件开发人员手册可以在他们的网站上找到。AMD 手册可以在他们的网站上找到。

x86-64 汇编的*厚脸皮插头*的使用，可以在 BaretalOS（源代码）中看到，它完全由我自己，用汇编编写的。

// EOF
