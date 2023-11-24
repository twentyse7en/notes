---
title: "Closure"
date: 2022-01-22T12:44:41+05:30
tags: [
    "javascript",
    "closure",
]
---

what is closure?
> A closure is the combination of a function bundled together (enclosed)
> with references to its surrounding state (the lexical environment).
> In other words, a closure gives you access to an outer function’s scope
> from an inner function. In JavaScript, closures are created every time a
> function is created, at function creation time.
> <cite> MDN [^1]</cite>

[^1]: closure definition [mdn](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Closures)

In simple words we can access variables declared outside the function. Wow!

```js
function foo() {
    let message = 'hello world';

    function speak() {
	console.log(message);
    }

    function setMessage(newMessage) {
	message = newMessage;
    }

    return [speak, setMessage];
}

const [greet, setMessage] = foo();

greet(); // hello world
setMessage("I'm Batman");
greet(); // I'm Batman
```

Here in the example, `speak` and `setMessage` was able to access `message`
which was declared in `foo`'s scope.
TaDa!! That's it.
