---
title: "Simple HTTP server with GO"
date: 2023-10-28T12:44:41+05:30
tags: [
    "go",
    "web",
    "server",
]
---

## Motivation
I was curious how a http request was working. Yeah, I too had networking in my
college, but wasn't that curious back then. Now for living I do web development for past two years. I took a pause and wanted to know how things were working under the hood.

## What are we builting
A http server in go, without a http package.

But why? There is a lot of framework that let you build stuff without much effort. We are explorers right? we want to know how things work.

## The plan
> “If you wish to make an apple pie from scratch, you must first invent the universe" ~ Carl Sagan

HTTP is built on top of TCP, which is responsible for sending the bytes from one machine to another. So our basic building block is TCP.

This is how our server works.
- listen to tcp connection on port
- run infinite loop to accept any request
- concurrently accept any request using thread
- send back a response

## What happens when we make a request in browser

When we are looking for "http://localhost:3000" on a browser
1. Browser will be sending a `http` get request to server.
2. Server listen to the request
3. understands browser is requesting for index.html (whatever)
4. send back the data to browser
5. close the connection

## Coding time
[gist](https://gist.github.com/twentyse7en/cd0bf90f154a60d6056c127d572bc269) for the entire code

we will be exploring the important parts of the server.

1. Listening to requests
```go
func main() {
    listener, err := net.Listen("tcp", ":8080")
    if err != nil {
        fmt.Println("Error starting server", err)
        return
    }
    // free resourse when function returns
    defer listener.Close()

    // server is ok to start
    fmt.Println("server is listening on port 8080")

    // we need a infinite loop, which will serve the request
    for {
        // blocks the loop untill a request is made
        conn, err := listener.Accept();
        fmt.Println("conn is accepted")

        if err != nil {
            fmt.Println("Error accepting connection:", err)
            continue
        }
        // handle the request
        // it shouldn't block the next request
        // so running in seperate thread
        // this is go routine
        go handleRequest(conn)
    }
}
```

2. Process the request and send back response
```go
func handleRequest(conn net.Conn) {
    defer conn.Close()
    
    // send a response
    response := "HTTP/1.1 200 OK\r\nContent-Length: 12\r\n\r\nHello, world!"
    conn.Write([]byte(response))
}
```

'Hello, world!' will be visible on the browser 😁.

