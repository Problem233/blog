---
title: 第一章：迈出第一步
toc: true
---

## 环境配置

Idris 的环境配置与其它语言相比非常简单。你需要的有：

- 一个**纯**文本编辑器（最好是代码编辑器）。如果你不知道选什么的话，在 Windows 上可以选用 [Notepad++](https://notepad-plus-plus.org/) 或 [Visual Studio Code](https://code.visualstudio.com/)。
- Idris 编译器。非 Windows 平台的用户可以查看 [这里](https://github.com/idris-lang/Idris-dev/wiki/Installation-Instructions) 的安装说明。如果需要自行编译的话，你需要至少加上 `ffi` 编译选项。Windows 用户可以直接下载 [这里](https://github.com/idris-lang/Idris-dev/wiki/Windows-Binaries) 的安装包，**但是需要手动设置 `PATH` 环境变量**。

本文中我们将使用 1.3.1 版本的 Idris 编译器，任何高于 1.3 且低于 2.0 的版本理论上都适用于本教程。你应该尽可能安装最新的正式版。

## REPL

安装好 Idris，我们的第一步是打开 *REPL*，即 *read-eval-print-loop*，*读取﹣求值﹣输出循环*。如果你之前学习过 Python、Javascript、Haskell 等语言，你应该已经了解 REPL 了。首先启动一个 shell（使用 Windows 安装包的请用 `Win+R` 运行 `cmd` 进入命令提示符），输入 `idris` 指令打开 REPL。这时你会看到类似这样的信息：

```
     ____    __     _                                          
    /  _/___/ /____(_)____                                     
    / // __  / ___/ / ___/     Version 1.X.Y-ZZZZZZ
  _/ // /_/ / /  / (__  )      http://www.idris-lang.org/      
 /___/\__,_/_/  /_/____/       Type :? for help               

Idris is free software with ABSOLUTELY NO WARRANTY.            
For details type :warranty.
Idris>
```

`Idris>` 就是 Idris REPL 的提示符。顾名思义，REPL 的功能就是**读取**你的输入，对其**求值**，然后**输出**，以此**循环**。实际上，shell、Linux 上的 `bc` 计算器等程序都可以看作 REPL。你可以把这个 REPL 当做计算器：

```idris
Idris> 1 + 1
2 : Integer
Idris> 123 + 456
579 : Integer
Idris> 185 * 37
6845 : Integer
```

当然它也可以计算更复杂的表达式：

```idris
Idris> 16 * 79 + 9
1273 : Integer
Idris> 16 * (79 + 9)
1408 : Integer
```

不过你会发现这些表达式无法计算，REPL 输出了错误信息：

```idris
Idris> 176 - 335
(input):1:5:When checking argument smaller to function Prelude.Nat.-:
        Can't find a value of type 
                LTE 335 176
Idris> 18 / 5
Can't find implementation for Fractional Integer
```

这是因为 Idris 是一门*强类型语言*，不允许自动*类型*转换。目前它的*类型推导*器还不是很完善，在 REPL 中不能很好地推断出我们需要的类型。那么，类型是什么呢？

## 类型与表达式

我们之前在 REPL 中输入的内容都是 *表达式*，表达式都具有*值*和类型。按下 `Enter` 键后 REPL 显示的内容中，冒号“`:`”前面的是我们输入的表达式的值，冒号后面的就是它的类型。绝大多数语言都有*类型系统*。类型系统正是 Idris 的强大之处，也是学习 Idris 时最重要的内容。

在我们之前的操作中出现的整数属于 `Nat` 类型或 `Integer` 类型（取决于你所进行的计算）。如果你要明确声明一个表达式为某种类型，可以这样：

```idris
Idris> the Integer (176 - 335)
-159 : Integer
```

> 这里出现的 `the` 是一个函数，我们将在后面了解。

下面我们将了解 Idris 中的一些基本类型。

### 自然数：`Nat`

`Nat` 类型就是自然数，它包含整个 $\mathbb N$ 集合。你可以对它进行加、减、乘、整除操作。我们在前面已经尝试过加、减、乘操作，这里要注意 `Nat` 的减法要求被减数大于等于减数，否则就会出现前面那样的错误：

```idris
Idris> the Nat 176 - 335
(input):1:5:When checking argument smaller to function Prelude.Nat.-:
        Can't find a value of type 
                LTE 335 176
```

Nat 还支持整除操作，即舍去余数的除法。它的运算符是 <code>\`div\`</code>，如下：

```idris
Idris> the Nat 7 `div` 3
2 : Nat
Idris> the Nat 179 `div` 34
5 : Nat
```

> 这里的 `+`、`-`、`*`、<code>\`div\`</code> 实际上都是函数，我们将在后面讲到。

我们还可以用另一种方式来表示 `Nat` 类型的数值。`Z` 表示 0，`S n` 表示 n + 1，如下：

```idris
Idris> Z
0 : Nat
Idris> S Z
1 : Nat
Idris> S (S Z)
2 : Nat
```

这种表示方法虽然看似没什么用，但它将在后面起到很重要的作用。

### 整数：`Integer`

`Integer` 是 Idris 提供的高精度整数类型。它包含整个 $\mathbb Z$ 集合。它也支持加、减、乘、整除操作，如下：

```idris
Idris> the Integer 2759375936530462549462 + 26493648563659264827465
29253024500189727376927 : Integer
Idris> the Integer 1 - 7293
-7292 : Integer
Idris> the Integer 175639374 * 7294922727
1281275661148652898 : Integer
Idris> the Integer 1756393743620462940472840364 `div` 26492649
66297399841762254143 : Integer
```

Idris 也提供了普通的、更快速的 `Int` 类型。它的范围取决于平台类型。对于 64 位平台，`Int` 类型包含的范围是 $[-2 ^{63}, 2 ^{63} - 1] \cap \mathbb Z$。

### 双精度浮点数：`Double`

`Double` 是标准的 IEEE 754 双精度浮点数，支持加、减、乘、除操作，如下（你能发现浮点数计算存在一定误差）：

```idris
Idris> 1.7 + 6.5
8.2 : Double
Idris> 9.3 - 17.2
-7.899999999999999 : Double
Idris> 1.3 * 2.9
3.77 : Double
Idris> 1.9 / 5.3
0.3584905660377358 : Double
```

### 布尔类型：`Bool`

`Bool` 是 Idris 中的布尔类型。Idris 中的布尔值用 `True` 和 `False` 表示。它支持与（`&&`）、或（`||`）、非（`not`）操作，如下：

```idris
Idris> True && False
False : Bool
Idris> True || False
True : Bool
Idris> not True
False : Bool
```

要注意不像大部分其它语言，Idris 是严格的强类型语言，其他类型的值不能当作 `Bool` 类型使用，如下：

```idris
Idris> 1 && 0
When checking an application of function Prelude.Bool.&&:
        Bool is not a numeric type
Idris> 1.0 || 0.0
(input):1:5-6:When checking an application of function Prelude.Bool.||:
        Type mismatch between
                Double (Type of 1.0)
        and
                Bool (Expected type)
Idris> not 0
When checking an application of function Prelude.Bool.not:
        Bool is not a numeric type
```

### 字符类型：`Char`

`Char` 是 Idris 中的字符类型，用 `'` 包围字符表示，特殊字符可以使用转义符号，如 `'\''` 表示 `'` 字符、`'\\'` 表示 `\` 字符、`'\n'` 表示换行符。与其它更接近与底层的语言相比，Idris 中的字符类型包含所有 Unicode 字符，如 `'😂'` 这样的字符。Idris 中的字符类型也不能当作数值参与计算，如 `'a' + 7` 是不允许的。如果想进行这样的计算，就要手动转换，如 `chr (ord 'a' + 7)` 就等于 `'h'`，其中 `chr`、`ord` 都是标准库中的函数。

### 字符串：`String`

`String` 是 Idris 中的字符串类型。与 Haskell 相比，为了性能，Idris 把它实现为了原始类型。字符串字面量用 `"` 包围字符串表示，对于特殊字符的转义与 `Char` 相同，唯一的区别是在 `String` 中 `"'"` 不需要转义，而 `"\""` 需要转义。

`String` 类型支持字符串连接运算（`++`）。标准库还提供了很多操作 `String` 的函数，如下：

```idris
Idris> "hello" ++ " world"
"hello world" : String
Idris> strCons 'h' "ello"
"hello" : String
Idris> strHead "hello"
'h' : Char
Idris> strTail "hello"
"ello" : String
```

<!--
### 列表：`List`

`List` 是 Idris 中的列表类型。-->

待续……

\[上一章\] [\[返回目录\]](/posts/a-idris-tutorial-from-zero/) \[下一章\]
