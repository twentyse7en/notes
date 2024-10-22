---
title: "React Native: New Architecture"
date: 2024-10-22T21:40:41+05:30
tags: [
    "react",
    "react-native",
    "app",
]
---

# React Native: New Architecture

Here we are trying to explore the issues with current react native architecture and what's exciting about new architecture.

## What is React Native?
React native allows developers to create native apps. Native apps can be android, ios, tv, web. There tagline itself is "Learn once, write anywhere". How are they making this possible?

<img width="600" src="https://raw.githubusercontent.com/twentyse7en/journal/refs/heads/mainhttps://raw.githubusercontent.com/twentyse7en/journal/refs/heads/main/content/images/what_is_rn.png" />

- Developer can write business logic in javascript just like react.
- For building UI, React Native provide building blocks like View
- For using hardware and other resources there will be third party libraries (for using camera and gps)

With all this we can build each platform. You may need to adjust few things according to each platform.

*Other Stuffs going under the hood* : The Javascript part will be compiled by Metro bundler (something like webpack, but optimized for mobile usecase). Java (android) and Obj-C (ios) will be compiled and both of them together form the App bundle, that user can install. Javascript run in Javascript Virtual Machine, previous it was JavascriptCore (the engine used by safari browser), now it is Hermes (Meta created it's own Javascript engine). Native stuff run in Native side.

As Javascript and Native stuff are independent, with codepush (which is going to deprecated soon) or alternative solution, javascript only changes we can release OTA, no release is required through playstore or appstore. That's a cool part.

<img width="600" src="https://raw.githubusercontent.com/twentyse7en/journal/refs/heads/main/content/images/rn_files.png" />

## Issues with current Architecture

Javascript run in one thread and native stuff run in another thread. The communication happen by react native bridge. 

Let's go by an example:  Consider the user is clicking on a button.

1. Message is passed to javascript script thread, in JSON format
2. Javascript Thread receives the message, trigger the handler function
3. Based on the new changes, a new message is sent to Native side for updating the UI.
4. Native thread receives the message and update the UI.

<img width="600" src="https://raw.githubusercontent.com/twentyse7en/journal/refs/heads/main/content/images/rn_bridge.png" />

The interaction between two sides are via bridge through message transfer, which is asynchrounous. There will be lot of overhead for JSON serialiazation, deserialization. As you can see already, if there is lot of interaction app can lag. For example, when you are scrolling a long list of data you can often see white screen.

Bridge is *async*, changes passed might not immediately reflected. This means that users may see intermediate states or visual jumps between rendering the initial layout and responding to layout measurements.

For Example: While entering Amount add comma seperation.

<img width="600" src="https://raw.githubusercontent.com/twentyse7en/journal/refs/heads/main/content/images/ui_slow_update.png" />

Another example is positioning tooltip based on some layout calculation. `useLayoutEffect` is broken, there is no way currently to do this without user seeing intermediate view.

### limitation of current architecture
- Communication view Bridge is slow
- can't synchronously update UI
- All modules load at startup which increases time to interaction.

## New architecture
<img width="600" src="https://raw.githubusercontent.com/twentyse7en/journal/refs/heads/main/content/images/new_architecture.png" />

### Javascript Interface
> "JSI is an interface that allows JavaScript to hold a reference to a C++ object and vice-versa. With a memory reference, you can directly invoke methods without serialization costs." ~ React Docs

so no more bridge, no more overhead. Javascript can directly call native part.
- Java/Object-C methods/Object will be exposed to javascript via `HostObject`
- Javscript will hold reference to HostObject and through this reference access the methods and props directly on Java/Object-C API
- This can be done:
	- synchronously on same thread
	- asynchronously on different thread

### Turbo Modules
Turbo modules are native modules in new architecture. This can be loaded when required, thus reduce the startup time.

### Fabric
This is renderer responsible for translating react components into host platform components. Now we can priorities render, if there is high prioirty render, it can run on main thread. The renderer is able to measure and render React surfaces synchronously (tooltip issue can be resolved). Easier to implement server side rendering for React Native.

### Codegen
There will be boilerplate code required to bind native modules with javascript, with codegen this will can be automatically generated.

<img width="600" src="https://raw.githubusercontent.com/twentyse7en/journal/refs/heads/main/content/images/rn_codegen.png" />

### History
- This was announced in 2018
- Meta already uses it
- from v0.68, there is option to choose new architecture

## Future exploration
- We haven't explored issues with the new architecture, in this podcase Mark explains few issues and why he created NitroModules as a replacement of TurboModules
- Will try to create a working example of old architecture and new architure showing tooltip or update issue mention earlier in blog 🤞


