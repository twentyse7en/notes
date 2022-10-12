---
title: "Singleton Pattern"
date: 2022-10-12T12:44:41+05:30
tags: [
    "js",
    "singleton",
]
---
# Singleton
Singletons are classes which can be instantiated once, and can be accessed globally. This single instance can be shared throughout our application, which makes Singletons great for managing global state in an application.

# Usecase
I want to connect to a chat service once and use the instance of connection across components (React) instead of connecting every time.

# Boilerplate

```js
class Connection {
    static _instance;
    client;

    constructor() {
        if (Connection._instance) {
            return Connection._instance;
        }
        this._instance = this;
        this.client = new connectionCode({
            apiKey: APIKEY
        })
    }

    getConnection() {
        return this.client;
    }
}
```

# Code walkthrough
ref: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes/static

This `_instance` will be shared across all class. So once we create a class and we can assign that instance to `_instance`, if we are creating a new class we can check `_instance` to verify we have created a class before.

client is to store the connection.
```
static _instance
client
```

Neither static methods nor static properties can be called on instances of the class. Instead, they're called on the class itself.

here we check if the class is constructed before, yes: return the previous instance. No: create a new instance and assign this instance to `_instance`
```
    constructor() {
        if (Connection._instance) {
            return Connection._instance;
        }
        this._instance = this;
        this.client = new connectionCode({
            apiKey: APIKEY
        })
    }
```

Method to access the connection.
```
Neither static methods nor static properties can be called on instances of the class. Instead, they're called on the class itself.
```

## Exporting
```
const singletonConnection = Object.freeze(new Connection());
export default singletonConnection;
```