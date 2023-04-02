---
title: "What's new in React 18"
date: 2023-04-01T12:44:41+05:30
tags: [
    "js",
    "react",
    "upgrade"
]
---

## How to upgrade
Install latest `react` and `react-dom`
```bash
npm install react@18 react-dom@18
```
Change your `index.js`
```js
import ReactDom from 'react-dom/client';
import App from './App';

// ReactDOM.render(<App />, document.getElementById('root'));
const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(<App />);
```
Yay! that's it. Welcome to react 18 🎉

## Changes to take care
So before jumping into latest features, let's go through some changes that might effect our existing code.

### Automatic batching

let's check how react handles multiple state updates **before react 18**.   
1. **Event handlers**: state updates will be batched. There will be only one re-render.
    ```js
    const handleClick = () => {
    setCounter();
    setActive();
    setValue();
    };

    //re-rendered once at the end.
    ```
2. Won’t batch for **other scenarios**
    ```js
    fetch('/network').then(() => {
    setCounter(); //re-rendered 1 times
    setActive(); //re-rendered 2 times
    setValue(); //re-rendered 3 times
    });

    //Total 3 re-renders
    ````

In **React 18**, state update will be batched.


As you can tell, previous implementation is not performant. React 18 introduces automatic batching which allows all state updates - even within promises, setTimeouts and event callbacks to be batched. This significantly reduces the work that React has to do in the background.

[Further Read](https://react.dev/blog/2022/03/29/react-v18#new-feature-automatic-batching)

### useEffect will be called twice

This will be a **development-only** change in strict mode.

*why this change?*

In the future, react like to add a feature that allows React to add and remove sections of the UI while preserving state. For example, when a user tabs away from a screen and back, React should be able to immediately show the previous screen. To do this, React will support remounting trees using the same component state used before unmounting.

*Impact in our code*

 Most effects will work without any changes, but some effects do not properly clean up subscriptions in the destroy callback, or implicitly assume they are only mounted or destroyed once. So make sure to **cleaup properly**.

 *Current working*
 ```
 * React mounts the component.
    * Layout effects are created.
    * Effects are created.
* React simulates effects being destroyed on a mounted component.
    * Layout effects are destroyed.
    * Effects are destroyed.
* React simulates effects being re-created on a mounted component.
    * Layout effects are created
    * Effect setup code runs
 ```

[Further Read](https://legacy.reactjs.org/docs/strict-mode.html#ensuring-reusable-state)

## Concurrency
As we have seen in automatic batching multiple state updates will be batched. Suppose if we need to prioritize one state update over other. Concurrency give us those tools, to mark the update urgent or not.


```html

Without concurrency                    With concurrency


first update    second update         ┌─────────────────────┐
                                      │ setUser(u1)         │
                                      └─────────────────────┘
┌─────────────┐ ┌──────────────┐      ┌──────────────┐
│ setUser(u1) │ │ setShow(true)│      │ setShow(true)│
└─────────────┘ └──────────────┘      └──────────────┘

───────────────────────────────►     ─────────────────────────►
```

we can use this tool to prioritize `setShow(true)` over `setUser(u1)`.

With concurrency, React can interrupt, pause, resume, or abandon a render. This allows React to respond to the user interaction quickly even if it is in the middle of a heavy rendering task. Before React 18, rendering was a single, uninterrupted, synchronous transaction and once rendering started, it couldn’t be interrupted.

Transitions can be used to mark UI updates that do not need urgent resources for updating. For eg: when typing in a typeahead field, there are two things happening - a blinking cursor that shows visual feedback of your content being typed, and a search functionality in the background that searches for the data that is typed.

Showing a visual feedback to the user is important and therefore urgent. Searching is not so urgent, and hence can be marked as non-urgent. *These **non-urgent updates are called transitions**. By marking non-urgent UI updates as "transitions" , React will know which updates to prioritize making it easier to optimize rendering and get rid of stale rendering.*

## Suspense on the server
SSR allow you to render html on server and send it to the browser. But some component might be slow to render. So this might slow down SSR. we can add suspense there. so it will be only rendered in browser, other html can be sent.

## Reference
- [Official upgrade guide](https://react.dev/blog/2022/03/08/react-18-upgrade-guide)