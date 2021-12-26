# Personal notes

Okay, in this one we're gonna mess around a little with functions, randomness, and one of go's classic and very useful datatypes, known as slices.

First, source code:
`greetings.go`
```go
package greetings

import (
 "errors"
 "fmt"
 "math/rand"
 "time"
)


func Hello(name string) (string, error) {

 if name == "" {
 return "", errors.New("empty name")
 }

 message := fmt.Sprintf(RandomGreet(), name)
 return message, nil
}


func init() {
 rand.Seed(time.Now().UnixNano())
}

func RandomGreet() string {

 randomGreeting := []string{"Player %v has entered the game!", 
							"Do you not know who this is? It's %v, slayer of demons, bane of the Fallen! You shall kneel before them!", 
							"May the force be with you, %v"}
 return randomGreeting[rand.Intn(len(randomGreeting))]
}
```

Also, I feel like this would be incomplete without this output I legit got out of my first run of the code:

```
C:\Users\TheAntiSnipe\Documents\Death Valley\hello>go run . 
Do you not know who this is? It's Paws, slayer of demons, bane of the Fallen! You shall kneel before them!
```
![](https://static.boredpanda.com/blog/wp-content/uploads/2020/03/5e6782b89ee19_1nyn34drl0l31__700.jpg)
*Such paws, much scary, wow, am kneel*

Anyway!

**Code anatomy**
Two things we see right off the bat are the two new module imports. `"math/rand"` and `"time"` are both modules we need to use... Well, not `"time"`, actually, that's only there to seed the random distribution we're using. We can just use something like, say `42` or whatever we want here.

Next up, the `init` function. This is another one of those functions that Go runs by itself. It runs right after the global vars of the code are assigned. This is where we assign the random seed. This might be of interest in the context of init: [Effective Go - The Go Programming Language](https://go.dev/doc/effective_go#init).
Other than that, there's the function we use within the random seed declaration, the one about time... `time.Now().UnixNano()`. Unix is a well-known timescale that measures how much time has passed since January 1, 1970. When you say `time.Now.UnixNano()`, you're basically asking the machine how much time has passed since that time, in nanoseconds. Other than that, further reading can be done in [time package - time - pkg.go.dev](https://pkg.go.dev/time#Now)

Another object of interest is the declaration of a slice.

```go

randomGreeting := []string{"Player %v has entered the game!", 
						   "Do you not know who this is? It's %v, slayer of demons, bane of the Fallen! You shall kneel before them!", 
						   "May the force be with you, %v"}
```
Okay, so here, we see a declaration template for a slice: An array of strings that, on initialization, contains these strings seen above. A weird declaration, in fact an opposite of how we do it in CPP, but pretty easy to get used to. General template:
```go
varname := [numberOfElements]datatype{elements} 
```

Then we return one of those indices randomly:

```go
return randomGreeting[rand.Intn(len(randomGreeting))]
```

