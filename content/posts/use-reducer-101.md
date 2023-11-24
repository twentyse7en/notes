---
title: "useReducer 101"
date: 2023-02-28T12:44:41+05:30
tags: [
    "javascript",
    "react",
    "hooks",
    "readability"
]
---

## Do we need `useReducer`?
We already have `useState` which manages the state. If we look at functional level `useReducer` doesn't bring anything new to table. Both are used to store the value and manipulate it. So fundamental question is then why do even care about `useReducer`?

## Code readability
When we are working on a project with other member, one of the most important aspect of the code is how readable it is. I expect the code to be readable and maintainable. What I feel about `useReducer` is that it makes the more readable.

## Code lifecycle
What I have seen in my codebase is that, we start with simple states in the component. May be one/two `useState` or a simple object to hold the state. When component grows we will have more state for the same logic, multiple handler to manipulate state and a lot of condition where we have to update the state. Even if we write the code, later in time it will be difficult to understand. It's nightmare if it is someone else's code.

## How to organise the code
So basics things we have discussed so far is:
1. State
2. Updating the state
3. When to update the state

## `useReducer` 🎉
Let's see a example and see how the code is organised.

The state

```js
const initialTasks = [
  {id: 0, text: 'Visit Kafka Museum', done: true},
  {id: 1, text: 'Watch a puppet show', done: false},
  {id: 2, text: 'Lennon Wall pic', done: false},
];

const [tasks, dispatch] = useReducer(tasksReducer, initialTasks);
```

The reducer: updating the state
```
function tasksReducer(tasks, action) {
  switch (action.type) {
    case 'added': {
      return [
        ...tasks,
        {
          id: action.id,
          text: action.text,
          done: false,
        },
      ];
    }
  }
}
```
The dispatch: trigerring the update

```js
function handleAddTask(text) {
    dispatch({
      type: 'added',
      id: nextId++,
      text: text,
    });
  }
```
## When writing `reducer` 
keep in mind that action types should ideally describe “**what the user did**” rather than “**how you want the state to change**”. This makes it easier to later add more features.

When we are sending the message, we need to clear the message, we could reuse the `edited_message` condition and be happy, but instead react prefer to add another state, `sent_message` to describe what user did.

I know we are lazy, but for a readable future let's do it!

```js
case 'edited_message': {
      return {
        ...state,
        message: action.message,
      };
    }
    case 'sent_message': {
      return {
        ...state,
        message: '',
      };
    }
```
