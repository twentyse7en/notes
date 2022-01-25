---
title: "🔄 `let` in for loop"
date: 2022-01-22T12:44:41+05:30
tags: [
    "js",
    "closure",
]
---

I was going through closures in javascript, found something interesting.

```js
for(var i = 0; i < 3; ++i) {
    console.log(setTimeout(() => {
	console.log(i);
    }
    ), 1000);
};

/*
output
3
3
3
*/
```
This is pretty much what we expect, after 1 second value of i will be 3,
then setTimout call the callback fn, when callback try to reference i,
it's value will be 3 in that environment. Ok, let's move to next example.

```js
let i;
for(i = 0; i < 3; ++i) {
    console.log(setTimeout(() => {
	console.log(i);
    }), 1000)
}

/*
output:
3
3
3
*/
```

that's also like previous example, let's try another one. let's declare `i`
inside `for`


```js
for(let i = 0; i < 3; ++i) {
    console.log(setTimeout(() => {
	console.log(i);
    }), 1000)
}

/*
output:
0
1
2
*/
```
Well 😱, what just happened?

If there is `let` declaration in the head of `for`, the variable will
be declared for each iteration. for more info checkout this stack overflow [answer](https://stackoverflow.com/a/30900289)

before this, I was having this mental model of `for` loop

```js
{
 let i = 0;
 if(i < 3){
  setTimeout(...)
  i += 1;
  }
 if(i < 3){
  setTimeout(...)
  i += 1;
  }
 if(i < 3){
  setTimeout(...)
  i += 1;
 }
}
```

Now if we use `let` in `for` loop header

```js
{
    let i = 0;
    {
	let i = prev_i;
	if (i < 3){
	    setTimeout(...)
	    i++;
	}
    }
    {
	let i = prev_i;
	if (i < 3){
	    setTimeout(...)
	    i++;
	    }
    }
    {
	let i = prev_i;
	if (i < 3){
	    setTimeout(...)
	    i++;
	}
    }
}
```

so now each callback fn in `setTimeout` can correctly access
the `i` in those iteration.
