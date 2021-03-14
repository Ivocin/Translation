# [译]JavaScript: 带你彻底搞懂 this

> * 原文地址：[JavaScript: What is the meaning of this?](https://web.dev/javascript-this/)
> * 原文作者：[Jake Archibald](https://web.dev/authors/jakearchibald/)
> * 原文发布时间：2021-03-08
> * 本文永久链接：[https://github.com/Ivocin/Translation/Blogs/javascript-what-is-the-meaning-of-this.md](https://github.com/Ivocin/Translation/blob/master/Blogs/javascript-what-is-the-meaning-of-this.md)
> * 翻译、校对：[Ivocin](https://github.com/Ivocin/)


搞明白 JavaScript 中 `this` 的值有时候会很棘手，本文带你彻底搞懂 `this`。

JavaScript 的 `this` 往往会成为许多笑话的笑柄，因为它相当复杂。然而，我发现很多开发人员为了避免处理 `this`，用了更加复杂和特定领域的处理。如果你对 `this` 还不熟悉，希望本文能帮助到你。下面进入我的 `this` 指南。

我将从最具体的情况开始，以最不具体的情况结束，本文的结构类似与一个大的 `if (…) … else if () … else if (…) …` 语句，所以你可以直接跳转到匹配你代码情况的章节。



1. [如果是箭头函数](#arrow-functions)
2. [否则，如果使用 `new` 调用函数/类](#new)
3. [否则, 函数被 `bind` 了 `this`](#bound)
4. [否则, 如果 `this` 在调用时设置](#call-apply)
5. [否则, 如果使用父对象(`parent.func()`) 调用函数](#object-member)
6. [否则, 如果函数或者其父作用域使用严格模式](#strict)
7. [否则](#otherwise)

## 如果是箭头函数：<span id="arrow-functions"></span>

```js
const arrowFunction = () => {
  console.log(this);
};
```

在这种情况下，`this` 的值**永远**与父作用域的 `this` 相同。

```js
const outerThis = this;

const arrowFunction = () => {
  // 永远输出 `true`:
  console.log(this === outerThis);
};
```

箭头函数非常优秀，因为其内部 `this` 的值无法被改变，它与外部的 `this` **永远** 相同。

### 其他例子 <span id="other-examples"></span>

使用箭头函数， `this` 的值**无法**被 [`bind`](#bound) 改变：

```js
// 输出为 `true` - bind `this` 被忽略：
arrowFunction.bind({foo: 'bar'})();
```

使用箭头函数，`this` 的值**无法**被 [`call` 或 `apply`](#call-apply) 改变：

```js
// 输出为 `true` - call `this` 被忽略：
arrowFunction.call({foo: 'bar'});
// 输出为 `true` - apply `this` 被忽略：
arrowFunction.apply({foo: 'bar'});
```

使用箭头函数，`this` 的值**无法**通过将函数作为另一个对象的成员变量来调用改变：

```js
const obj = {arrowFunction};
// 输出为 `true` - 父对象被忽略：
obj.arrowFunction();
```

使用箭头函数，`this` 的值**无法**通过将函数作为构造函数来调用而改变：

```js
// TypeError: arrowFunction is not a constructor
new arrowFunction();
```

### “绑定” 实例方法 <span id="'bound'-instance-methods"></span>

对于实例方法，如果想要确保 `this` 始终指向类实例，最好的方法是使用箭头函数和 [class fields](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes/Public_class_fields)：

```js
class Whatever {
  someMethod = () => {
    // 永远是 Whatever 的实例：
    console.log(this);
  };
}
```

这个模式在将实例方法作为组件内的事件监听器时十分有用（如 React 组件或者 Web Components）。

上面的代码貌似打破了“`this` 的值**永远**与父作用域的 `this` 相同”的规则，但是如果你将 class fields 看作将对象设置到构造函数的语法糖，那么就好理解了：

```js
class Whatever {
  someMethod = (() => {
    const outerThis = this;
    return () => {
      // 永远输出 `true`:
      console.log(this === outerThis);
    };
  })();
}

// …大致等于：

class Whatever {
  constructor() {
    const outerThis = this;
    this.someMethod = () => {
      // Always logs `true`:
      console.log(this === outerThis);
    };
  }
}
```

其他模式包括在构造函数中绑定现有函数，或在构造函数中对函数赋值。如果你由于某种原因不能使用 class fields，则在构造函数中对函数赋值是一种合理的选择：

```js
class Whatever {
  constructor() {
    this.someMethod = () => {
      // …
    };
  }
}
```

## 否则，如果使用 `new` 调用函数/类： <span id="new"></span>

```
new Whatever();
```

上面代码会调用 `Whatever`（或者它的构造函数，如果它是类），并将 `this` 设置为 `Object.create(Whatever.prototype)` 的结果。

```js
class MyClass {
  constructor() {
    console.log(
      this.constructor === Object.create(MyClass.prototype).constructor,
    );
  }
}

// 输出为 `true`:
new MyClass();
```

使用旧式的构造函数结果也一样：

```js
function MyClass() {
  console.log(
    this.constructor === Object.create(MyClass.prototype).constructor,
  );
}

// 输出 `true`:
new MyClass();
```

### 其他例子 <span id="other-examples-2"></span>

使用 `new` 调用，`this` 的值**无法**被 [`bind`](#bound) 改变：

```js
const BoundMyClass = MyClass.bind({foo: 'bar'});
// 输出为 `true` - bind `this` 被忽略：
new BoundMyClass();
```

使用 `new` 调用，`this` 的值**无法**通过将函数作为另一个对象的成员变量来调用改变：

```js
const obj = {MyClass};
// 输出为 `true` - 父对象被忽略：
new obj.MyClass();
```

## 否则, 函数被 `bind` 了 `this`： <span id="bound"></span>

```js
function someFunction() {
  return this;
}

const boundObject = {hello: 'world'};
const boundFunction = someFunction.bind(boundObject);
```

每当 `boundFunction` 被调用，它的 `this` 值就是通过 `bind` 传入的值（`boundObject`）。

```js
// 输出 `false`:
console.log(someFunction() === boundObject);
// 输出 `true`:
console.log(boundFunction() === boundObject);
```

-----

**Warning**: 避免使用 `bind` 将函数绑定到其外部的 `this`。使用[箭头函数](#arrow-functions)替代，因为这样 `this` 可以在函数声明就能清楚地看出来，而非在后续代码中看到。
不要使用 `bind` 设置 `this` 为与父对象无关的值；这通常是出乎意料的，这也是 `this` 获得如此糟糕名声的原因。考虑将值作为参数传递；它更加明确，并且可以使用箭头函数。



### 其他例子 <span id="other-examples-3"></span>

使用 `bind` 调用函数，`this` 的值**无法**被 [`call` 或 `apply`](#call-apply) 改变：

```js
// 输出为 `true` - call `this` 被忽略：
console.log(boundFunction.call({foo: 'bar'}) === boundObject);
// 输出为 `true` - apply `this` 被忽略：
console.log(boundFunction.apply({foo: 'bar'}) === boundObject);
```

使用 `bind` 调用函数，`this` 的值**无法**通过将函数作为另一个对象的成员变量来调用改变：

```js
const obj = {boundFunction};
// Logs `true` - parent object is ignored:
console.log(obj.boundFunction() === boundObject);
```

## 否则, 如果 `this` 在调用时设置: <span id="call-apply"></span>

```js
function someFunction() {
  return this;
}

const someObject = {hello: 'world'};

// 输出 `true`:
console.log(someFunction.call(someObject) === someObject);
// 输出 `true`:
console.log(someFunction.apply(someObject) === someObject);
```

`this` 的值就是传递给 `call`/`apply` 的对象。


----

**警告**: 不要使用 `bind` 设置 `this` 为与父对象无关的值；这通常是出乎意料的，这也是 `this` 获得如此糟糕名声的原因。考虑将值作为参数传递；它更加明确，并且可以使用箭头函数。

不幸的是，`this` 可能会被如 DOM 事件监听器之类的函数设置为其他值，使用它会导致代码难以理解：

不要这样：

```js
element.addEventListener('click', function (event) {
  // 输出 `element`, 因为 DOM 将 `this` 设置为
  // click 绑定的元素上
  console.log(this);
});
```

我会避免在上述场景中使用 `this`，我会这样使用：

```js
element.addEventListener('click', (event) => {
  // 理想情况, 从父作用域获得它：
  console.log(element);
  // 但是如果你不想这么做，可以从 event 对象获取它：
  console.log(event.currentTarget);
});
```

## 否则, 如果使用父对象(`parent.func()`) 调用函数： <span id="object-member"></span>

```js
const obj = {
  someMethod() {
    return this;
  },
};

// 输出 `true`:
console.log(obj.someMethod() === obj);
```

在这种情况下，函数作为 `obj` 的成员变量被调用，所以 `this` 指向 `obj`。这是在调用时发生的，因此如果没有使用父对象调用，或者使用一个不同的父对象调用，该连接会断开：

```js
const {someMethod} = obj;
// 输出 `false`:
console.log(someMethod() === obj);

const anotherObj = {someMethod};
// 输出 `false`:
console.log(anotherObj.someMethod() === obj);
// 输出 `true`:
console.log(anotherObj.someMethod() === anotherObj);
```

`someMethod() === obj` 为 `false`，因为 `someMethod` **不是** 作为 `obj` 的成员变量被调用的。尝试执行以下操作时，可能会遇到此陷阱：

```js
const $ = document.querySelector;
// TypeError: Illegal invocation
const el = $('.some-element');
```

这个报错是因为 `querySelector` 实现会寻找它的 `this` 值，并期望其某种 DOM 节点，上述代码破坏了连接。为了正确实现上述功能，可以这样写：

```js
const $ = document.querySelector.bind(document);
// 或者：
const $ = (...args) => document.querySelector(...args);
```

有趣的事实：并不是所有的 API 都在其内部使用了 `this`。Console 方法（如 `console.log`）就改为了不使用 `this` 引用，因此 `log` 方法不需要绑定 `console`。

---

**警告**: 不要使用 `bind` 设置 `this` 为与父对象无关的值；这通常是出乎意料的，这也是 `this` 获得如此糟糕名声的原因。考虑将值作为参数传递；它更加明确，并且可以使用箭头函数。

## 否则, 如果函数或者其父作用域使用严格模式： <span id="strict"></span>

```js
function someFunction() {
  'use strict';
  return this;
}

// 输出 `true`:
console.log(someFunction() === undefined);
```

在这种情况下，`this` 的值是 `undefined`。如果父作用域处于[严格模式](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Strict_mode)（而且所有模块都处在严格模式），则不需要在函数内部使用 `'use strict'`。

---

**警告**: 不要依赖这个。我的意思是，有更简单的方式来得到一个 `undefined` 值 😀。

## 否则: <span id="otherwise"></span>

```js
function someFunction() {
  return this;
}

// 输出 `true`:
console.log(someFunction() === globalThis);
```

在这种情况下，`this` 的值与 `globalThis` 相同。

---

很多人（包括我）把 `globalThis` 称为 `global` 对象，但这不是 100% 技术正确的。在 [Mathias Bynens with the details](https://mathiasbynens.be/notes/globalthis#terminology) 中，有它为什么叫 `globalThis` 而不是 `global` 的原因。

---

**警告**: 避免使用 `this` 指向 `global` 对象（对，我仍然这么叫它）。改为使用[`globalThis`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/globalThis)，它更加明确。


## 结语 <span id="Phew"></span>

好了，这就是我理解的 `this` 的全部了。如果有任何问题或者我有所遗漏，请[给我发推](https://twitter.com/jaffathecake)。

感谢 [Mathias Bynens](https://twitter.com/mathias), [Ingvar Stepanyan](https://twitter.com/RReverser), 和 [Thomas Steiner](https://twitter.com/tomayac) 的审阅。


