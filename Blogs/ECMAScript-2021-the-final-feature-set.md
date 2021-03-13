# ECMAScript 2021: the final feature set

> * 原文地址：[ECMAScript 2021: the final feature set](https://2ality.com/2020/09/ecmascript-2021.html)
> * 原文作者：[2ality](https://2ality.com/)
> * 本文永久链接：[https://github.com/Ivocin/Translation/Blogs/ECMAScript-2021-the-final-feature-set.md](https://github.com/Ivocin/Translation/Blogs/ECMAScript-2021-the-final-feature-set.md)
> * 译者：[Ivocin](https://github.com/Ivocin/)
> * 校对者：[Ivocin](https://github.com/Ivocin/)

**Update 2021-03-09:** Today, [the ES2021 candidate](https://github.com/tc39/ecma262/releases/tag/es2021-candidate-2021-03) was released, with the final feature set of that version. If approved by the Ecma General Assembly in June 2021, it officially becomes a standard. This blog post describes what’s new.

## 1 The editors of ECMAScript 2021
--------------------------------------------------------------------

The editors of this release were:

*   [Jordan Harband](https://twitter.com/ljharb)
*   [Shu-yu Guo](https://twitter.com/_shu)
*   [Michael Ficarra](https://twitter.com/smooshMap)
*   [Kevin Gibbons](https://twitter.com/bakkoting)

## 2 A word on ECMAScript versions  [#](#a-word-on-ecmascript-versions)
------------------------------------------------------------------

Note that since [the TC39 process](https://exploringjs.com/impatient-js/ch_history.html#tc39-process) was instituted, the importance of ECMAScript versions has much decreased. What really matters now is what stage a proposed feature is in: Once it has reached stage 4, it can be used safely. But even then, you still have to check if your engines of choice support it.

## 3 The features of ES2021 (stage 4 proposals)  
--------------------------------------------------------------------------------------------

*   [`String.prototype.replaceAll`](https://exploringjs.com/impatient-js/ch_regexps.html#replace-replaceAll) (Peter Marshall, Jakob Gruber, Mathias Bynens)
  
*   [`Promise.any()`](https://exploringjs.com/impatient-js/ch_promises.html#Promise.any) (Mathias Bynens, Kevin Gibbons, Sergey Rubanov)
  
*   WeakRefs (Dean Tribble, Mark Miller, Till Schneidereit, Sathya Gunasekaran, Daniel Ehrenberg) [[proposal](https://github.com/tc39/proposal-weakrefs)]
  
*   [Logical assignment operators](https://exploringjs.com/impatient-js/ch_operators.html#logical-assignment-operators) (Justin Ridgewell, Hemanth HM)
  
*   Underscores (`_`) as separators in [number literals](https://exploringjs.com/impatient-js/ch_numbers.html#numeric-separator-number-literals) and [bigint literals](https://exploringjs.com/impatient-js/ch_bigints.html#numeric-separator-bigint-literals) (Sam Goto, Rick Waldron)
  

## 4 FAQ
--------------

### 4.1  What do the stages mean?

They refer to maturity stages of the so-called “TC39 process”. Check [section “The TC39 process”](https://exploringjs.com/impatient-js/ch_history.html#tc39-process) in “JavaScript for impatient programmers” for more information.

### 4.2 How is [my favorite proposed feature] doing?

If you are wondering what stages various proposed features are in, consult [the readme of the ECMA-262 GitHub repository](https://github.com/tc39/ecma262/blob/master/README.md).

### 4.3 Is there an official list of ECMAScript features? 

Yes, the TC39 repo lists [finished proposals](https://github.com/tc39/proposals/blob/master/finished-proposals.md) and mentions in which ECMAScript versions they are introduced.

## 5 Free books on ES2021 
------------------------------------------------

The following books cover JavaScript up to and including ECMAScript 2021 and are free to read online:

*   [“JavaScript for impatient programmers”](https://exploringjs.com/impatient-js/)
*   [“Deep JavaScript”](https://exploringjs.com/deep-js/)