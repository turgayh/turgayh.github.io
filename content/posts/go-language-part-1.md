---
title: "Go Language [Part 1]"
date: 2020-07-11T22:47:08+03:00
draft: false
toc: false
images:
tags:
  - go
  - programming
  - vim
---

### Programming in Go

- I set out to see what I can build with Go, so let's find out. Let's start with the definition on Go's own website:

    > Go is an open source programming language that makes it easy to build **simple**, **reliable**, and **efficient** software.

- I'll work through the tutorial with those highlighted words in mind and see in which situations I can say "yes, that's true".

### Local Setup and Vim

You can download Go from its official site, [**https://go.dev**](https://go.dev). I'll use Vim as my editor. You can use a full IDE or another text editor, but using Vim is a good way to get a bit deeper into things.

I set up Vim on macOS; I can't say how it works on Windows.

### Tutorial Walkthrough 1

The first part of the documentation is examples of the language's syntax.

#### Hello World!!

```go
package main

import "fmt"

func main() {
    fmt.Println("Hello, World!!")
}
```

A small first step into the Go language.
