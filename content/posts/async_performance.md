+++
author = "Abijith b"
title =  "Async and Performance"
date = 2022-05-22T11:14:59+05:30
tags = [
    "javascript",
    "YDJS",
    "async"
]
+++

This is the summary of book [Async & Performance](https://github.com/getify/You-Dont-Know-JS/blob/1st-ed/async%20&%20performance/README.md#you-dont-know-js-async--performance)
from You Don't Know JS series. When I started JS journey Asynchronous was a new pattern.
I didn't deeply explored at that point. Now It's very critical time to deeply
understand this pattern, as it is very important part of everyday JS coding
It might introduce lot of bugs due to partial knowledge.

# Callback
### How can callback introduce asynchrony?

```js
// A
let data = []

setTimeout(callbackFn() => {
    // B
    data.push(1);
    console.log(data);
}, 0)

// C
data.push(2);
console.log(data);
```

Here the code is executed in order A → C → B, Even if the `setTimeout` is given
a delay of 0ms, It will push the callBackFn in callBack queue. Functions in callback
queue is taken only after executing current iterationg of event loop. This was
a new pattern for me, till js I have seen `if`, `for` which manage the control
flow using different condition, but here the code will be executed some time after.
**Time** factor was something new. Let's discuss issue with this simple callback.

### Limitation of Human brain
For humans it's difficult to plan asynchronously. So it's difficult to code
and understand asynchronous code.
> The cause of callback hell is when people try to write JavaScript in a way
where execution happens visually from top to bottom.
~ [callback hell](http://callbackhell.com/)

```js
fs.readdir(source, function (err, files) {
  if (err) {
    console.log('Error finding files: ' + err)
  } else {
    files.forEach(function (filename, fileIndex) {
      console.log(filename)
      gm(source + filename).size(function (err, values) {
        if (err) {
          console.log('Error identifying file size: ' + err)
        } else {
          console.log(filename + ' : ' + values)
          aspect = (values.width / values.height)
          widths.forEach(function (width, widthIndex) {
            height = Math.round(width / aspect)
            console.log('resizing ' + filename + 'to ' + height + 'x' + height)
            this.resize(width, height).write(dest + 'w' + width + '_' + filename, function(err) {
              if (err) console.log('Error writing file: ' + err)
            })
          }.bind(this))
        }
      })
    })
  }
})
```

One solution for solving callback hell is to split the code
to different meaning full function.

```js
document.querySelector('form').onsubmit = formSubmit

function formSubmit (submitEvent) {
  var name = document.querySelector('input').value
  request({
    uri: "http://example.com/upload",
    body: name,
    method: "POST"
  }, postResponse)
}

function postResponse (err, response, body) {
  var statusMessage = document.querySelector('.status')
  if (err) return statusMessage.value = err
  statusMessage.value = body
}
```

### Trust issues

This is when you pass the callback to some third party function, such
as analytics module. We have no gurantee how many times this function
will trigger or even execute. Personally I haven't added callback to
analytics, but when I have used analytics, they will retry multiple
times when there is network error. Some times it's unpredictable, beware
when passing callbacks to third party code.
