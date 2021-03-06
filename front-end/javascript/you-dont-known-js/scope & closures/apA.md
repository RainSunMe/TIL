# You Don't Know JS: Scope & Closures

# Appendix A: Dynamic Scope

在第2章中，我们讨论了“动态作用域”与“词法作用域”模型的对比，后者是作用域在JavaScript（事实上，大多数其他语言）中的工作方式。

我们将简要地审视一下动态作用域，以便确定这种对比。但是，更重要的是，动态作用域实际上是JavaScript中另一个机制（`this`）的近亲，我们在本书系列的“this＆Object Prototypes”标题中介绍了它。

正如我们在第2章中看到的，词法作用域是关于引擎如何查找变量以及它将在何处找到变量的规则集。词法作用域的关键特征是，它是在编写代码时（假设你不使用`eval()`或`with`进行欺骗）被定义的。

动态作用域似乎意味着，并且有充分的理由，有一个模型可以在运行时动态确定作用域，而不是静态地在被定义时。实际情况就是如此。让我们通过代码说明：

```js
function foo() {
	console.log( a ); // 2
}

function bar() {
	var a = 3;
	foo();
}

var a = 2;

bar();
```

词法范围认为对`foo()`中的RHS引用`a`将被解析为全局变量`a`，这将导致输出值`2`。

相比之下，动态作用域本身并不关心声明函数和作用域的方式和位置，而是关注它们是 **在什么位置调用** 。换句话说，作用域链基于调用堆栈，而不是代码中作用域的嵌套。

因此，如果JavaScript具有动态作用域，则在执行`foo()`时，**理论上** 下面的代码将导致输出为`3`。

```js
function foo() {
	console.log( a ); // 3  (not 2!)
}

function bar() {
	var a = 3;
	foo();
}

var a = 2;

bar();
```

怎么会这样？因为当`foo()`无法解析`a`的变量引用时，它不会沿着嵌套（词法）作用域链向上找，而是向上查找调用堆栈，以查找调用`foo()`的位置。由于`foo()`是从`bar()`调用的，它会检查`bar()`作用域中的变量，并找到值为`3`的`a`变量。

很奇怪？你现在可能正在这么想。

但这仅仅是因为你可能只研究过（或者至少深入考虑过）词汇作用域的代码。所以动态作用域对你来说似乎很陌生。如果你只用动态作用域的语言编写代码，它看起来很自然，而词法作用域就是奇怪的。

需要说明的是，JavaScript **实际上并没有动态范围** 。它有词汇作用域。简单明了。但`this`机制有点像动态范围。

关键对比：**词法作用域是写代码的时候定义的，而动态作用域（`this`）是运行时定义的** 。词法作用域关注 *声明函数的位置* ，但动态作用域关注 *从哪里* 调用的函数。

最后：`this`关心函数的调用方式，它显示了`this`机制与动态作用域的概念有多密切相关。要深入了解`this`，请阅读 “this＆Object Prototypes”。

