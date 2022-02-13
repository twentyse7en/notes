---
title: "useMemo and useCallback, when to optimize"
date: 2022-02-09T20:12:32+05:30
draft: true
---

> tldr; Don't overuse useMemo and useCallback, Optimization comes at
> a cost.  
>
> use `useMemo` when there is **expensive calculation** that only need to be
> re-calculate when dependencies change, not on every render.  
>
> use `useCallback` when it is passed as **dependency** to other hooks.

## Intro
`useMemo` and `useCallback` is all about memoization. Both accept a function
and a dependency list as arguments. `useCallback` returns memoized function,
while `useMemo` returns the result of running the function. Result will be
stored internally, it will be only recomputed when dependencies changes.

```js
/* useCallback */
const memoizedFn = useCallback((a, b) => {...}, [a, b])

/* useMemo */
const memoizedValue = useMemo((a, b) => {...}, [a, b])
```

## useCallback
### What is memoized function?  
Upon re-render new function will created for same definition.
I'm using a global array `saveHandleClick` to save the function `handleClick`,
to check if function are equal between render.

```
// This is dummy component for demo purpose only (^_^!)

const saveHandleClick = [];

const Counter = () => {
    const [counter, setCounter] = useState(0);
    const handleCounterClick = () => {
        setCounter(prev => prev + 1);
    };
    saveHandleClick.push(handleClick);

    return (
    <>
        <p>{count}</p>
        <button onClick={handleCounterClick}>  </button>
    </>
    );
}
```
when user clicks the button, It wil re-render. Now the handleClick will be
evaluated again and It will get a new reference.
```js
> saveHandleClick[0] === saveHandleClick[1]
> false

/* similar situation
/* same definition, but different reference
> () => {} === () => {}
```

If we wrap `handleClick` in `useCallback` It will give the previous
reference as long as the dependencies as same.  

I have updated the previous example to accomodate the a new state, `diff`.
```js
const saveHandleClick = [];

const Counter = () => {
    const [counter, setCounter] = useState(0);
    const [diff, setDiff] = useState(1);

    const handleCounterClick = useCallback(() => {
       setCounter(prev => prev+diff)
    },
    [diff])

    saveHandleClick.push(handleClick);

    return (
    <>
        <p> {diff} </p>
        <button onClick={handleDiffClick}> diff </button>
        <p> {counter} </p>
        <button onClick={handleCounterClick}> counter </button>
    </>
    )
}
```
1. Scenerio 1: clicking counter

As the dependency `diff` haven't changed, the function refernce will be same across re-render
```
> saveHandleClick[0] === saveHandleClick[1]
> true
```
2. scenerio 2: clicking diff

Now the dependency `diff` have changed, so function reference will change.
```
> saveHandleClick[1] === saveHandleClick[2]
> false
```

**Note**: if `diff` haven't added as a dependency, then function will be old version,
we will get incorrect result. But what about `closure`, it should be able to access
the changes in the outer scope too, right? But on re-render new scope is created. The
saved version is refering to an older scope. Make sure that you include all the necessary
dependency for useCallback, else it will refer to older scope and may produce incorrect results.

My initial impression for `useCallback` was like it saves you some space, because, we are
only storing `memoizedFn` rather that actually creating a new function. But we need to 
take a step back and think at what cost the memoization is happening.

I was under the illusion only storing the required fn only, no memory allocation
for unnecessary new fn.

But in reality new function will be created anyway, if the depency haven't changed,
old one will be returned, else new one will be returned.

as a overhead, we would also need to store dependcies to check if they changed.
So for simple function there is no point of wrapping it on `useCallback`
```
const handleClick = useCallback(() => {...}, [])

// new function will be created any way
const newFn = () => {...}
const handleClick = useCallback(newFn, [])
```

## When to use useCallback

1. debounce search, interesting , why?
example on cliniccloud

2. if function is dependency for other hooks


