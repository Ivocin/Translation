# ECMAScript 2021: 最终功能集确定

> * 原文地址：[ECMAScript 2021: the final feature set](https://2ality.com/2020/09/ecmascript-2021.html)
> * 原文作者：[Axel Rauschmayer](http://dr-axel.de/)
> * 本文永久链接：[https://github.com/Ivocin/Translation/Blogs/ECMAScript-2021-the-final-feature-set.md](https://github.com/Ivocin/Translation/blob/master/Blogs/ECMAScript-2021-the-final-feature-set.md)
> * 翻译、校对：[Ivocin](https://github.com/Ivocin/)

**更新于 2021-03-09:** 今天，[ES2021 候选提案](https://github.com/tc39/ecma262/releases/tag/es2021-candidate-2021-03) 发布了其最终功能集的版本。如果它能够在今年 6 月的 ECMA 大会上通过，就会成为官方的标准。本文描述了有哪些新的内容。

## 1、ECMAScript 2021 的编辑

这个版本的编辑是：

*   [Jordan Harband](https://twitter.com/ljharb)
*   [Shu-yu Guo](https://twitter.com/_shu)
*   [Michael Ficarra](https://twitter.com/smooshMap)
*   [Kevin Gibbons](https://twitter.com/bakkoting)

## 2、关于 ECMAScript 的版本说明

注意，自从 [TC39 进程](https://exploringjs.com/impatient-js/ch_history.html#tc39-process)制定以来，ECMAScript 版本的重要性就降低了很多。现在真正重要的是提案处于哪个阶段：一旦提案到了第 4 阶段，那么它就可以使用了。但是即使这样，你仍然需要检查你的引擎是否支持该功能。

## 3、ES2021 功能（第 4 阶段提案）

*   [`String.prototype.replaceAll`](https://exploringjs.com/impatient-js/ch_regexps.html#replace-replaceAll) (Peter Marshall, Jakob Gruber, Mathias Bynens)
  
*   [`Promise.any()`](https://exploringjs.com/impatient-js/ch_promises.html#Promise.any) (Mathias Bynens, Kevin Gibbons, Sergey Rubanov)
  
*   WeakRefs (Dean Tribble, Mark Miller, Till Schneidereit, Sathya Gunasekaran, Daniel Ehrenberg) [[proposal](https://github.com/tc39/proposal-weakrefs)]
  
*   [Logical assignment operators](https://exploringjs.com/impatient-js/ch_operators.html#logical-assignment-operators) (Justin Ridgewell, Hemanth HM)
  
*   Underscores (`_`) as separators in [number literals](https://exploringjs.com/impatient-js/ch_numbers.html#numeric-separator-number-literals)  以及  [bigint literals](https://exploringjs.com/impatient-js/ch_bigints.html#numeric-separator-bigint-literals) (Sam Goto, Rick Waldron)
  

## 4、常见问题

### 4.1 阶段的含义是什么？

阶段是指 “TC39 进程“的成熟阶段。更多信息可以查看“JavaScript for impatient programmers” 中的[“TC39 进程” 部分](https://exploringjs.com/impatient-js/ch_history.html#tc39-process)。

### 4.2 [我最喜欢的提案功能] 现在怎么样了？

如果你想查看不同的提案功能现在处于什么阶段，请查阅 [ ECMA-262 GitHub 仓库的 README 文件](https://github.com/tc39/ecma262/blob/master/README.md)。

### 4.3 有官方的 ECMAScript 功能列表吗？ 

当然，TC39 仓库列出了 [已完成提案](https://github.com/tc39/proposals/blob/master/finished-proposals.md) 以及它们是在哪个 ECMAScript 版本被引入的说明。

## 5、ES2021 的免费书籍

以下书籍包括了到 ECMAScript 2021 的 JavaScript，并且可以免费在线阅读：

*   [“JavaScript for impatient programmers”](https://exploringjs.com/impatient-js/)
*   [“Deep JavaScript”](https://exploringjs.com/deep-js/)