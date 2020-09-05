# Go 的隐藏 #pragmas

这是一篇关于编译器指令的文章，也就是俗称的 pragmas，来源于去年我在上海 GopherChina 上关于相似主题的演讲。

## 先回顾下历史

在讨论 Go 之前，让我们先来讨论一些关于 pragmas 的知识，还有它们的历史。许多语言都有属性或者指令的概念，用来改变编译过程中源代码的解释方式。例如，Perl 有 [use](https://perldoc.perl.org/functions/use.html) 函数：

``` Perl
use strict;
use strict "vars";
use strict "refs";
use strict "subs";
```

**use** 可以启用特性，或者让编译器以不同的方式解释程序源代码，使得编译器更严谨或者启用新的语法模式。

Javascript 具有类似的语法。ECMAScript 5 通过选项模式来扩展语言。例如：

``` JavaScript
"use strict";
```

当 Javascript 解释器碰到 **"use strict";** 的时候，会启用所谓的 **严格模式** 来解析 Javascript 源代码。

Rust 也类似，使用属性语法来启用 **非稳定** 的编译器或者标准库特性。

``` Rust
#[inline(always)]
 fn super_fast_fn() { ... }

#[cfg(target_os = "macos")]
mod macos_only { ... }
```
inline(always) 属性告知并一起必须将 super_fast_fn 函数内联。target_os 属性告知编译器只有在 OS X 系统下编译 macos_only 模块。

pragma 这个名字起源于 ALGOL 68，当时叫 pragmats，是单词 pragmatic 的缩写。当在 1970 年代被 C 语言采纳的时候，该名字再次被缩写成 #pragma。并且由于 C 的广泛流传，完全融入了程序员的时代精神。

``` C
#pragma pack(2)
struct T {
    int i;
    short j;
	double k;
};
```

上例告诉编译器该结构需要进行 2 直接对齐，所以 double k 变量会在 T 地址的 6 字节偏移处开始，而不是通常的 8 (缺省是4字节对齐)。

C 的 #pragma 指令造就了大量的编译器特定扩展，如 gcc 的 __builtin 指令。

## Go 有 pragmas 吗？

现在已经了解了一些 pragmas 的历史，或许可以开始问：Go 是否具有 pragmas ？

就像前面看到的，#pragma 像 #include 和 #define 一样，是通过预处理器来实现 C 风格的语言，但是 Go 没有预处理器或者宏，因此，问题只剩下：Go 是否具有 pragmas ？

事实证明，即使 Go 没有宏，也没有预处理器，Go 确实支持 pragmas。它们由编译器作为注释实现。

为了澄清这一点，在 [Go 编译器的源代码](https://github.com/golang/go/blob/master/src/cmd/compile/internal/gc/lex.go#L64)中，它们确实被称为 pragmas。

所以很明显，pragma 的名字和思想并没有消失。

本文仅中关注编译器能识别的一部分 pragmas，一方面是由于列表经常变化，但主要原因是对于程序员来说，不是所有的都有用。

这里有一些例子激发你的兴趣。

``` Go
//go:noescape 
func gettimeofday(tv *Timeval) (err Errno)
```




----------------

via: https://dave.cheney.net/2018/01/08/gos-hidden-pragmas

作者：[Dave Cheney](https://dave.cheney.net/about)
译者：[arthurlee](https://github.com/arthurlee)
校对：[校对者ID](https://github.com/校对者ID)

本文由 [GCTT](https://github.com/studygolang/GCTT) 原创编译，[Go 中文网](https://studygolang.com/) 荣誉推出
