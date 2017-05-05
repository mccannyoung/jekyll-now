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

Notice the lack of parenthesis around the if statement, go isn't big on parentheses. 
