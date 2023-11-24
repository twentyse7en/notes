---
title: "Reconciliation"
date: 2022-01-28T20:23:35+05:30
tags: [
    "javascript",
    "react",
    "frontend",
    "reconciliation",
]
---

> Reconciliation ~
> The process of comparing different records
> in order to check that they add up to the same
> total or to explain any difference between them.

React re-render when state or props changes. React internally
creates a new tree of react elements. Which needs to be compared
with old tree to update the UI.

The state-of-art algorithms take O(n^3) for generating the minium
number of operations to transform one tree to another. Which sounds
bit scary. So what's React's solution? React uses [heuristic]()
algorithm based on two assumption.

### 1. Two elements of differnt type will produce different tree
```html
<div>
    <MainApp />
</div>

<main>
    <MainApp />
</main>
```

### 2. Using keys
The developer can hint at which elements may be stable across
different renders with key prop

```html
<div>
    <span key="2022"> Batman </span>
    <span key="2312"> Superman </span>
</div>

<div>
    <span key="9666"> Joker </span>
    <span key="2022"> Batman </span>
    <span key="2312"> Superman </span>
</div>
```
We only need to update `<span key="9666"> Joker </span>`. It will be easier
for react to compare if unique key are provided.

*NB* : Don't use unstable keys like using `Math.random()` which require unnecessary
DOM node recreation leading to preformance degradation and lost state in child
components.

## Decision Making

### Elements of different type

As in the example one, root element `div` is replaced by `main`, they are
two different element. So full rebuild is required.

#### Steps:
1. Old dom nodes are destroyedComponent
2. Component will receive `ComponentWillUnmount()`
3. New DOM nodes are inserted to DOM
4. Components will receive `UNSAFE_ComponentWillMount()`
5. then `ComponentDidMount()`

### DOM Elements of same type

Say if both where `div` then react will only update the changes in attribute.

### Component Elements of same type
State is maintained across renders

#### Steps:
1. React updates the props
2. `UNSAFE_componentWillRecieveProps()`
3. `UNSAFE_componentWillUpdate()`
4. `ComponentDidUpdate()`
5. next `render()` method is called and diffing repeats on new tree

