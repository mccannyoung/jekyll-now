---
layout: post
title: Golang Basics For Java/C# developers part 1
---

Here is the obligatory "Hello World" in Go.

```golang
package main

import "fmt"

func main() {
	fmt.Println("Hello, 世界")
}
```

Let's look at this.

```golang
package main
```

Go uses packages to organize code into modules. [Modular design](https://en.wikipedia.org/wiki/Module_pattern) is a very important concept in good software design. The package that contains the func main() you want to run when running your program must be named main.

```golang
import "fmt"
```

The line imports the fmt package. fmt, as well as os, json, net, and [a bunch of others](https://golang.org/pkg/) are packages in the standard library, but you certainly aren't limited to them. You can also include third party packages from github. If you wanted to be able to connect to rabbitmq, you could use the github.com/streadway/amqp package, importing it looks like this:

```golang
import "github.com/streadway/amqp"
```

If you want to import multiple packages, you can use an alternative format, "factored import" the requires a little bit less typing (and is considered good style).  

```golang
import (
	"fmt"
	"os"
	"sync"
	"sync/atomic"
)
```

Next let's look at the function. 

```golang
func main() {
	fmt.Println("Hello, 世界")
}
```

If a function is named main, it functions the same as main in many other languages, as in it will be the function called when you run the file. 

Notice the function it calls, from the imported "fmt" package, the function Println is called. Not that unlike main, Println is capitalized, this is because a function need so to be capitalized to be exposed outside the package, or exported. 

Let's take a closer look at the code from the last blog post:

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

Let's look at the function f.

```golang
func f(from string) {
    for i := 0; i < 3; i++ {
        fmt.Println(from, ":", i)
    }
}
```

The signature is different than what you may be used to in C++ or Java, the type and the variables name are reversed. More complex go signatures can look like the following:

```golang
func add(x int, y int) int {
``` 
This function, add takes in two ints, x and y and returns an int

The parameters can be consolidated, because go is a pretty terse language, to:

```golang 
func add( x, y int) int {
```
Neat huh? 

If you want it to return two values (like if you wanted a function that took in a full name and returned the first and last name as separate strings) the signature would look something like: 

```golang
funct parsename(fullname string) (string, string){
```

If you want to look super fancy, you can even name your output:

```golang
funct parsename(fullname string) (firstName, lastName string){
```

Moving on, let's look at the loop.

```golang
 for i := 0; i < 3; i++ {
        fmt.Println(from, ":", i)
 }
 ```
 
 I'm going to assume you understand [for loops](https://en.wikipedia.org/wiki/For_loop), the feature of interest I'd like to point out is 
 
```golang
i := 0
```

In traditional strongly typed object oriented languages, that line would be:

```java
int i = 0;
```

In go, := means "declare and assign it the value on the right side." It's equvalent to var x = 0 in C#.

If you want to be as explicit as possible the above golang line can turn into:

```golang
var i int
i = 0
```
Alternatively 

```golang
var i =0 
```
Is also valid, go will be happy as a clam as long as it can tell what variable is what type, either by explicit type declaration or by the value assigned to it. If no value is assigned to a variable, the default value is 0, false, or empty string  ("") as appropriate for the type.

The comments explain the goroutines, and the anonymous function. 

```golang
fmt.Scanln(&input)
```

Anyone familar with C/C++ the ampersand should be a dead give away that we're in pointer territory. What's being passed to the Scanln is the address of the variable "input". Like C/C++ the astrix is also used, to get the value at the address of a pointer. 

```golang
package main
 
import "fmt"
 
func main() {
	var a int   // a variable of type 'int'
	var b int   // another 'int'
	var c *int  // a variable of type 'pointer to int'
 
	a = 42      // assigning value '42' to 'a'
	b = a       // copying the value from 'a' to 'b'
	c = &a      // assigning the address of 'a' to 'c'
 
	fmt.Printf("Values:\n a = %v\n b = %v\n c = %v\n*c = %v\n", a, b, c, *c)
 
	a = 21      // assigning a new value of '21' to 'a'
 
	fmt.Printf("Values:\n a = %v\n b = %v\n c = %v\n*c = %v\n", a, b, c, *c)
	fmt.Printf("Types:\n a:  %T\n b:  %T\n c: %T\n*c:  %T\n", a, b, c, *c)
}
```

Taken from [here](http://piotrzurek.net/2013/09/20/pointers-in-go.html)


The output from this program is:
```c
Values:
 a = 42
 b = 42
 c = 0xc04203e1d0
*c = 42
Values:
 a = 21
 b = 42
 c = 0xc04203e1d0
*c = 21
Types:
 a:  int
 b:  int
 c: *int
*c:  int

Program exited.
```

[Here](https://dave.cheney.net/2017/04/26/understand-go-pointers-in-less-than-800-words-or-your-money-back) is a very good article about pointers in go. 

Now go has something called arrays, which should be familiar. Sadly arrays in go are fixed size. If you need a dynamic list of variables, you want what is called a slice.  

Part 2 coming next week
