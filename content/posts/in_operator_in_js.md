+++
author = "Abijith B"
title = "in operator (JS)"
date = 2022-04-08T22:04:16+05:30
tages = ['js', 'web']
+++

It's easy to get started at JS, also very easy to make bugs.
It may be due to our prejudice or may be JS's fault. At the
end of the day I have to write `js`, As I have no other choice
at my work.

## What happened
So breif of what happened today. My task was to check if a string
exists on array of strings. coming from a python background, I thought
of using `in` operator.
```python
`'hello' in ['hi', 'hello']`. # true
```
I know there is `in` operator in JS. So I assumed it should be like checking
if a value exists in a iterable.(Yep my bad, _I assumed_).<br/>
To quickly verify my assumption, I tried following code snippet in `node`

```js
1 in [1, 2, 3, 4] // true
```

I was happy and without any second thoughts I used that on my original problem.
But it failed. Wait what? So as usual I did goto holy JS docs website [mdn](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/in)

## `in` Operator in JS
> The in operator returns true if the specified property is in the specified object or its prototype chain.

Why did my node snippet worked?<br/>
why not, array is object, and index are property. So js compiler wasn't looking
for value `1`, It was checking for if there is index `1`.

## What's weird?

JS land makes be very uncomfortable. If I get a chance I will escape and will never
come here :p

```js
let trees = new Array('redwood', 'bay', 'cedar')
delete trees[0]

for (let tree of trees)
    console.log(tree);

// undefined
// bay
// cedar
```

When we delete a val from array, we need to move other elements
to the front right. Not necessarily, but plebeian in like me
was exepecting that. May be JS is for far superior people with
less prejudice. (I should move to rust)
