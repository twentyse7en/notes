---
title: "useMemo and useCallback, when to optimize"
date: 2022-02-09T20:12:32+05:30
draft: true
---

> tldr; Don't overuse useMemo and useCallback, Optimization comes at
> a cost.
> use `useMemo` when there is **expensive calculation** that only need to be
> re-calculate when dependencies change, not on every render.
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

## What is memoized function?
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
    saveHandleClick.push(handleCounterClick);

    return (
    <>
        <p>{count}</p>
        <button onClick={handleCounterClick}>  </button>
    </>
    );
}
```
when user clicks the button, It will re-render. Now the `handleCounterClick` will be
evaluated again and new function will be created and assigned to it.
```js
> saveHandleClick[0] === saveHandleClick[1]
> false

/* similar situation
/* same definition, but different reference
> () => {} === () => {}
```

If we wrap `handleClick` in `useCallback` It will give the previous
reference as long as the dependencies as same.

I have updated the previous example to accommodate the a new state, `diff`.
```js
const saveHandleClick = [];

const Counter = () => {
    const [counter, setCounter] = useState(0);
    const [diff, setDiff] = useState(1);

    const handleCounterClick = useCallback(() => {
       setCounter(prev => prev+diff)
    },
    [diff])

    saveHandleClick.push(handleCounterClick);

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
1. Scenerio 1: clicking counter button<br/>
As the dependency `diff` haven't changed, the function reference will be same across re-render
```
> saveHandleClick[0] === saveHandleClick[1]
> true
```
2. Scenerio 2: clicking diff button<br/>
Now the dependency `diff` have changed, so function reference will change.
```
> saveHandleClick[1] === saveHandleClick[2]
> false
```

<i>**Note**: if `diff` haven't added as a dependency, then function will be old version,
we will get incorrect result. But what about `closure`, it should be able to access
the changes in the outer scope too, right? But on re-render new scope is created. The
saved version is refering to an older scope. Make sure that you include all the necessary
dependency for useCallback, else it will refer to older scope and may produce incorrect results.</i>

My initial impression for `useCallback` was like it saves you some space, because, we are
only storing `memoizedFn` rather that actually creating a new function. But we need to
take a step back and think at what cost the memoization is happening.

But in reality new function will be created as argument for `useCallback`, if the dependency haven't changed,
old one will be returned, else new one will be returned. as a overhead, we would also need to store dependencies
to check if they have changed. So for simple function there is no point of wrapping it on `useCallback`
```
const handleClick = useCallback(() => {...}, [])

// new function will be created any way
const newFn = () => {...}
const handleClick = useCallback(newFn, [])
```

# When to use useCallback

1. When you want same instance of function across re-render, like for [debounce](https://dmitripavlutin.com/react-throttle-debounce/)
2. if function is dependency for other hooks

# What about useMemo

`useMemo` is generalized version of `useCallback`, it can memoize any values. If we provide a function, it will execute the function
and saves the result. Use it when you don't want to run expensive function for every render, but when some dependencies changes.

### References
1. https://kentcdodds.com/blog/usememo-and-usecallback
2. https://blog.logrocket.com/react-usememo-vs-usecallback-a-pragmatic-guide/
3. https://nikgrozev.com/2019/04/07/reacts-usecallback-and-usememo-hooks-by-example/


