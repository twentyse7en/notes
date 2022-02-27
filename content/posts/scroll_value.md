+++
title = "scrollTop, scrollHeight and clientHeight"
author = "Abijith b"
date = 2022-02-27T20:14:19+05:30
tags = [
    "css",
    "web",
]
+++

I was assigned to make InfiniteScroll component in react. Luckly I got
a working [InfiniteScroll](https://github.com/ankeetmaini/react-infinite-scroll-component#readme).
But It support infinite scrolling to one direction alone. My use case requires to load
new data if user scroll to top and bottom. Now I should construct one.

Basically the task was to trigger API, if scroll cross some threshold in top
and bottom. Initially the scroll will be in middle.


<img src='/infinite_scroll_1.png' alt="infinite scroll with threshold"/>
I was using data from scroll event for first time, It was little confusing.
We need few data to check if scroll crossed the threshold.
1. Top length till scroll
2. Length of scroll
3. Threshold

## ScrollTop
ScrollTop gives you the top length till scroll. Well what is confusing about
this right? can you guess what will be top scroll for this scenerio?
The height of that container is 200px.

<img src='/infinite_scroll_2.png' alt="guess top scroll" />

So the length seems like 20% of height of the container. So it should
be around 40px.

But it's not, According to MDN
> The Element.scrollTop property gets or sets the number of pixels that an element's content is scrolled vertically.

It isn't ~20% of container height, but of total height if it overflowed.

Answer: ~192px

## clientHeight
Time for next quiz, what will the length of scroll?
<img src='/infinite_scroll_3.png' alt="guess client height" />

Well length seems similar to `topScroll` right, so it should be around 192px.
But what does that length signifies?

The length of scroll is 200px, It is the height of the container.

> clientHeight can be calculated as: CSS height + CSS padding - height of horizontal scrollbar (if present).

## scrollHeight

<img src='/infinite_scroll_4.png' alt="" />
Length till bottom of scroll = scrollTop + clientHeight

So next we need a threshold, [InfiniteScroll](https://github.com/ankeetmaini/react-infinite-scroll-component#readme)
supports two kind of threshold, in pixel and in percentage. Percentage of what?
Actual height if there was no overflow, that was scrollHeight gives you.

> Measurement of the height of an element's content, including content not visible on the screen due to overflow.

## Misc
When I was adding data to top, ideally my `scrollTop` should change, else
it will trigger another API call. Unfortunately `scrollTop` remain same.
Fix was to add difference between `scrollHeight` after updation to `scrollTop`

```js
if (isTopUpdation) {
    scrollTop += scrollHeight - prevScrollHeight;
}
```
