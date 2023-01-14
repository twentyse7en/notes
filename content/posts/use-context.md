---
title: "useContext 101"
date: 2023-01-14T12:44:41+05:30
tags: [
    "js",
    "react",
    "hooks",
]
---
## Why do we need Context?
Primary way of passing the information to child would be through props. Sometimes we might need to pass the same prop through children in different level, which is called prop drilling. Context allows to avoid prop drilling, we can pass the data to parent and it can be accessed from any children of that parent.

# How to use
1. Create a context
2. *use* the context from the component that needs the data.
3. Provide context from component with the data

```js
// step 1
// file: LevelContext.js
import { createContext } from "react";

export const LevelContext = createContext(1);


// step 2
// file: child.js
// now we have access to data
const level = useContext(LevelContext);


// step 3
// file: parent.js
// pass the Context
<LevelContext.Provider value={level}>
    {...}
</LevelContext>

```

## Why do we need different StateManagement
Context isn't even a state management tool. State is managed by `useState` and `useReducer`. Context help to make available data to a **portion** of your React component tree. If we need App wide state management, we might need Redux, Recoil. Ref[2] have more concise also elaborate explanation.

## Reference
1. https://beta.reactjs.org/learn/passing-data-deeply-with-context
2. https://blog.isquaredsoftware.com/2021/01/context-redux-differences/#tl-dr


