+++
author = "Abijith b"
title = "Hoisting"
date = "2022-01-20"
tags = [
    "javascript",
    "hoisting",
]
+++

# What will be the output?

```js
let b = 'hello (*_*)'

function barFoo() {
	console.log('b from barFoo is', b);
	let b = 'hello';
}

barFoo();
```

It will throw a Reference Error. But why? we have already declared and
initialised b on outer scope. So it might be available till we declare the `b`
again. well thats not how things work in javascript, lets checkout hoisting.

# What about this?

```js
var b = 'hello (*_*)'

function barFoo() {
	console.log('b from barFoo is', b);
	var b = 'hello';
}

barFoo();
```

now the output is `b from barFoo is undefined`. Let's check what's happening.

# Hoisting

> JavaScript Hoisting refers to the process whereby the interpreter appears to
> move the declaration of functions, variables or classes to the top of their
> scope, prior to execution of the code.
> — <cite>MDN[^1]</cite>

[^1]: Hoisting definition: [MDN docs](https://developer.mozilla.org/en-US/docs/Glossary/Hoisting).

So interpreter rearranges the declaration.

```js
var b = 'hello (*_*)'

function barFoo() {
    var b;  // declaration
    console.log('b from barFoo is', b);
    b = 'hello'; // assignment
}

barFoo();
```

## But why did there was reference error while using `let`?
> Variables declared with let and const are also hoisted
> but, unlike var, are not initialized with a default value.
> An exception will be thrown if a variable declared with
> let or const is read before it is initialized.
> - <cite> MDN [^2] </cite>
[^2]: let and const hoisting: [MDN docs](https://developer.mozilla.org/en-US/docs/Glossary/Hoisting#let_and_const_hoisting)

checkout [ECMA](https://262.ecma-international.org/9.0/#sec-let-and-const-declarations) for additional addition information.
