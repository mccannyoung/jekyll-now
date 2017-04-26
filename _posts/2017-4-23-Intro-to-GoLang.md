---
layout: post
title: Intro To Golang
---

Hello, 世界! This is a very basic entry to the wonderful world of Golang aka Go. You would think that in 2007 a new language would need a more unique name than "Go" I mean, it's already the name of an [ancient boardgame](https://en.wikipedia.org/wiki/Go_(game)), not to mention there was already a programming languaged named [Go!](https://en.wikipedia.org/wiki/Go!_(programming_language)).  Google's powers that be decided to run with it anyway (which caused [conflicts](https://en.wikipedia.org/wiki/Go!_(programming_language)#Conflict_with_Google)).


Anyway, Go was created in 2007 by three guys ([Robert Griesemer](https://github.com/griesemer), [Rob Pike](https://github.com/robpike), and [Ken Thompson](https://github.com/ken)) at Google, it was released as open source in 2012. If you're a programming language nerd, you may recongize the last name, Ken Thompson developed the programming language [B](https://en.wikipedia.org/wiki/B_(programming_language)), of which C was superseded by C (which then was expanded to C++ which Ken [wasn't a fan of](https://gigamonkeys.wordpress.com/2009/10/16/coders-c-plus-plus/)). 


Believe it or not, Go was created because because Google wanted to create a modern systems language to deal with the [current state of computers and commputational needs](https://golang.org/doc/faq#What_is_the_purpose_of_the_project). Or maybe because the developer behind it just [hated C++](https://en.wikiquote.org/wiki/Ken_Thompson#.22Interview_with_Ken_Thompson.22.2C_2011). Whicch is kind of hilarious, as Google has been known to be a [C++ shop](https://www.quora.com/Which-programming-languages-does-Google-use-internally) ([ps- they have their own style guide](https://google.github.io/styleguide/cppguide.html)). (I would avoid it if I hated C++ just like I avoid coldFusion shops.)


At any rate, Go was developed specficically to support concurrency, it's very simple to spin off a thread to run a function.

```golang
// A _goroutine_ is a lightweight thread of execution.

package main

import "fmt"

func f(from string) {
    for i := 0; i < 3; i++ {
        fmt.Println(from, ":", i)
    }
}

func main() {

    // Suppose we have a function call `f(s)`. Here's how
    // we'd call that in the usual way, running it
    // synchronously.
    f("direct")

    // To invoke this function in a goroutine, use
    // `go f(s)`. This new goroutine will execute
    // concurrently with the calling one.
    go f("goroutine")

    // You can also start a goroutine for an anonymous
    // function call.
    go func(msg string) {
        fmt.Println(msg)
    }("going")

    // Our two function calls are running asynchronously in
    // separate goroutines now, so execution falls through
    // to here. This `Scanln` code requires we press a key
    // before the program exits.
    var input string
    fmt.Scanln(&input)
    fmt.Println("done")
}

```
example taken from: [here](https://gobyexample.com/goroutines)


If you run it the output will be about what you'd expect if you're familiar with threads and concurrency. For those of you who aren't: the function when called directly, it is running on the same thread, so it prints 1, 2, and then 3. Now when the go routine (the line "go f("goroutine")") gets called, it's put on a different thread than the main program. What is the significance of this? Basically there are two threads going, and there is no way to predict what order they will execute in, as thread management is something the operating system manages.

So, it may print out:


>1 (this is from the direct call)
>
>2 (this is from the direct call)
>
>3 (this is from the direct call)
>
>1 (this is from the go routine call)
>
>going
>
>2 (this is from the go routine call)
>
>3 (this is from the go routine call)


It could also be something like...


>1 (this is from the direct call)
>
>2 (this is from the direct call)
>
>3 (this is from the direct call)
>
>1 (this is from the go routine call)
>
>2 (this is from the go routine call)
>
>3 (this is from the go routine call)
>
>going


Or even


>1 (this is from the direct call)
>
>2 (this is from the direct call)
>
>3 (this is from the direct call)
>
>going
>
>1 (this is from the go routine call)
>
>2 (this is from the go routine call)
>
>3 (this is from the go routine call)


Now, you may wonder "Why the heck would I want to not know the exact order things in my program will happen in?" The answer is because it can make your program run faster, especially on machines with more than one processor core. Instead of having one thread doing it all on one processor, you can have 2 or 4 or even 8 processors doing part of the work, so it will finish faster. This is a very powerful concept that can be leveraged on tasks where you can divide up tasks in a way where order doesn't matter. This is especially big in BigData.


Go is also rather straightforward, it's not object oriented, it doesn't have all of the abilities of C#/Java, but not all applications *need* objects, generics, and other options which are missing from Go. 


Anyway, next week I'll be back with some basics about the Go language - what it can and can't do, and a little bit about when it's a good vs bad option. I hope this post has been informative about some of the background of Go. 
