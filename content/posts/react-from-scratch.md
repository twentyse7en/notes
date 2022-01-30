---
title: "React From Scratch"
date: 2022-01-30T11:20:42+05:30
draft: true
---

React is javascript library for building user interfaces. I was
interested to know about magic behind the react. Rodrigo pombo
have done a great work on his article [build your on react](https://pomb.us/build-your-own-react/).
I tried creating a [react clone](https://github.com/twentyse7en) with the help of this blog.
I highly recommend to go through his article for deeper understanding.
This blog will be a comprehensive notes.

## Basics
At first I thought I was writing html inside javascript. What I was
actually writing was jsx.

> JSX (JavaScript Syntax Extension and occasionally referred as JavaScript XML)
> is a React extension to the JavaScript language syntax which provides
> a way to structure component rendering using syntax familiar to many
> developers. It is similar in appearance to HTML.

Tools like babel convert this to javascript. [Try](https://babeljs.io/repl) yourself,
it's fun.
```js
const App = () => {
  return (
      <div className='container'>
	  <h1> hello </h1>
	  <p> world </p>
      </div>
    )
}

// converted to
const App = () => {
  return /*#__PURE__*/React.createElement("div", { className: "container"},
	  /*#__PURE__*/React.createElement("h1", null, " hello "),
	  /*#__PURE__*/React.createElement("p", null, " world ")
	 );
};
```

Other basic building block are
1. [document.createElement](https://developer.mozilla.org/en-US/docs/Web/API/Document/createElement) for creating a new html element
2. [doucment.createTextNode](https://developer.mozilla.org/en-US/docs/Web/API/Document/createTextNode) for creating a text node
3. [node.appendChild](https://developer.mozilla.org/en-US/docs/Web/API/Node/appendChild) for appending a new child to node

## Getting started

There two primary tools required `React.createElement` and `render`.

### createElement
for getting started we only need few details.
1. what is the type of element
2. what are the properties
3. children of this element

```js
// a, b are just other elements

{
  "type": "div",
  "props": {
	    "id": "container",
	    "children": [a, b]
    }
}
```
for simplicity for our code there is also `createTextElement`. It's type will
be `TEXT_ELEMENT`.

### render

#### Level 1. Adding elements to DOM

We have element created by `createElement`. We can call `document.createElement`
to create element, then add it's properties, and recurse it's children add do the
same. Append the children to it's parent using `node.appendChild`.
For `TEXT_ELEMENT` we will call `createTextNode`.

Know we can see our jsx on screen by [now](). Hurray!

