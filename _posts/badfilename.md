---
layout: post
title: Golang Basics part 2
---

Now that we've looked at a couple example programs, and gone over some of the nuts and bolts let's continue. Last time we saw a for-loop that was virtually identical in content to loops we see in C/C++, Java, etc. Let's see how else for loops can work in go. 

```golang
package main

import "fmt"

func main() {
	sum := 1
	for sum < 10 {
		sum += sum
	}
	fmt.Println("The sum is %v", sum)
}

```

Wow, that looks funky, no intial variable declaration, no iteration, it should remind you strongly of a while loop. There is no while loop in go, all loops are for loops. 

But wait, what happens if we go down this rabbit hole further?

```golang
package main

import "fmt"

func main() {
	sum := 1
	for {
		sum += sum
		if sum > 10 {
			break
		}
	}
	fmt.Println("The sum is ", sum)
}
```

for doesn't require any values, and with no values it's an infinite loop. 

Notice the lack of parenthesis around the if statement, go isn't big on parentheses (it's delightfully terse). Speaking of terse, you can include short statements in the if statement, for example:

```golang
func pow(x, n, lim float64) float64 {
	if v := x - n; v < lim {
		return v
	}
	return lim
}
```

Which can be helpful, v is only defined for the scope of the if statement. 

Switch statement example, classic fizzbuzz:

```golang
import (
	"fmt"
)
func main(){
	fizzbuzz(3)
	fizzbuzz(5)
	fizzbuzz(15)
	fizzbuzz(17)
}
func fizzbuzz(x int){
	modFizz := x%3 == 0 
	modBuzz := x%5 == 0 
	switch {
	case modFizz:
		fmt.Println("Fizz")
	case modBuzz:
		fmt.Println("Buzz")
	default:
		fmt.Println(x)
	}
}
```
A couple things:
- Notice how there's no condition, a condition is not required for switch on go.
- If you try to run it, this will fail for the third example (15 will not return FizzBuzz, only Fizz). Unlike switch statements in other languages, there's no default fallthrough. 

Here's a version that works:

```golang
import (
	"fmt"
)
func main(){
	fizzbuzz(3)
	fizzbuzz(5)
	fizzbuzz(15)
	fizzbuzz(17)
}
func fizzbuzz(x int){
	modFizz := x%3 == 0 
	modBuzz := x%5 == 0 
	switch {
	case modFizz && modBuzz:
		fmt.Println("FizzBuzz")
	case modFizz:
		fmt.Println("Fizz")
	case modBuzz:
		fmt.Println("Buzz")
	default:
		fmt.Println(x)
	}
}
```

Next up is defer, defer is useful if you're using locks, or have some other reasons to declare a function not to be called until later.

```golang
func BillCustomer(c *Customer) error {
    c.mutex.Lock()
    defer c.mutex.Unlock()
    
    if err := c.Bill(); err != nil {
        return err
    }
    
    if err := c.Notify(); err != nil {
        return err
    }
    
    // ... do more stuff ...
    
    return nil
}
```
[example from here](https://kylewbanks.com/blog/when-to-use-defer-in-go)


Now, that example is obvious in how it works (it unlocks the customer as soon as everything else is done). A good question is what happens if there are multiple defer statements in a function.

```golang
package main

import "fmt"

func main() {

	for i := 0; i < 5; i++ {
		defer fmt.Println(i)
	}

	fmt.Println("done")
}
```

Now what would you guess this would return? Obviously "done" will be the first thing returned, what will be the second? Would it be "1" or "4"? 

Defer works as by putting at the return on a stack, it's a FILO stack though, so 4 is the correct answer. 

