# Personal notes

I decided to handle this problem statement by myself first, going in blind. The goal is as follows:
>In the last changes you'll make to your module's code, you'll add support for getting greetings for multiple people in one request. In other words, you'll handle a multiple-value input, then pair values in that input with a multiple-value output. To do this, you'll need to pass a set of names to a function that can return a greeting for each of them.

Right, so first off, I need to write a new function, and I need to run a loop over all the names and run the Hello function over each. So I designed the code like this:

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

func GreetMultiple(names []string) ([]string, error) {
	outputString := []string{}
	for i := 0; i < len(names); i++ {
		output, err := Hello(names[i])
		outputString = append(outputString, output)
		if err != nil {
			return outputString, err
		}
	}
	return outputString, nil
}

func init() {
 rand.Seed(time.Now().UnixNano())
}

func RandomGreet() string {
 randomGreeting := []string{"Player %v has entered the game!", "Do you not know who this is? It's %v, slayer of demons, bane of the Fallen! You shall kneel before them!", "May the force be with you, %v"}
 return randomGreeting[rand.Intn(len(randomGreeting))]
}
```

`hello.go`
```go
package main

import (
 "fmt"
 "log"
 "example.com/greetings"
)

func main() {
	// Setting up log
	log.SetPrefix("greetings: ")
	log.SetFlags(0)
	// Demo of standard Hello function
	message, err := greetings.Hello("Paws")
	if err != nil {
		log.Fatal(err)
	}
	fmt.Println(message)
	// Demo of GreetMultiple function
	nameList := []string{"Atharva", "Devam", "Mihir", "Yash"}
	messages, err := greetings.GreetMultiple(nameList)
	if err != nil {
		log.Fatal(err)
	}
	for i := 0; i < len(messages); i++ {
		fmt.Println(messages[i])
	}
}
}```

The loops here are pretty crude because I just straight up used the C format. Anyway, it's pretty easy to come up with a working solution to the given problem statement with a basic source code. However, here's the more elegant way to do it:

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
  
func GreetMultiple(names []string) (map[string]string, error) {
	// Note how the map is formatted here
	outputMap := make(map[string]string)
	// Also note the new loop format they use here
	for _, name := range names {
		message, err := Hello(name)
		if err != nil {
			return nil, err
		}
		// Finally, see how outputMap is indexed in name
		outputMap[name] = message
	}
	return outputMap, nil
}
  
func init() {
	rand.Seed(time.Now().UnixNano())
}
  
func RandomGreet() string {
	randomGreeting := []string{"Player %v has entered the game!", "Do you not know who this is? It's %v, slayer of demons, bane of the Fallen! You shall kneel before them!", "May the force be with you, %v"}
	
	return randomGreeting[rand.Intn(len(randomGreeting))]
}
```

`hello.go`
```go
package main

import (
    "fmt"
    "log"

    "example.com/greetings"
)

func main() {
    log.SetPrefix("greetings: ")
    log.SetFlags(0)
    message, err := greetings.Hello("Paws")
    if err != nil {
        log.Fatal(err)
    }
    fmt.Println(message)
    nameList := []string{"Atharva", "Devam", "Mihir", "Yash"}
    messages, err := greetings.GreetMultiple(nameList)
    if err != nil {
        log.Fatal(err)
    }
    for _, name := range nameList {
        fmt.Println(messages[name])
    }
}
```

**Code anatomy**

There's nothing really new going on in `hello.go` once we understand what's happening in `greetings.go`.  So what's up? Well, first off, let's focus on the `GreetMultiple()` function:

```go
func GreetMultiple(names []string) (map[string]string, error) {
    // Note how the map is formatted here
    outputMap := make(map[string]string)
    // Also note the new loop format they use here
    for _, name := range names {
        message, err := Hello(name)
        if err != nil {
            return nil, err
        }
        // Finally, see how outputMap is indexed in name
        outputMap[name] = message
    }
    return outputMap, nil
}
```
 The first thing we notice is the return type: We're using a return type of `map[string]string`. It's a map, with string indices, and string elements. The second thing is how it's declared: 
 ```go
 outputMap := make(map[string]string)
```
Alright, so what's `make()`? Slightly long discussion, go to [[data allocation]]. TL;DR you use `make()` if you want to initialize an empty map, slice or channel, `new()` works for all other usecases but then again, it's up to you.

Right, so you make a new map, and now you fill it up. The loop syntax is new too, let's take a look at that:
```go
for _, name := range names {
        message, err := Hello(name)
        if err != nil {
            return nil, err
        }
```
Right, so what's going on here? First off, the `_`, it's pretty standard stuff. You name a variable that in Go, it is automatically ignored. Now what exactly is going on here? Remember the Python `enum`? Yeah, that's basically what `range` is in Go: Index, followed by content. We only want the content, so we assign the index to the blank identifier `_`. Other than that, there's nothing really fancy going on in the loop here. Oh, though, something pretty cool here:
```go
outputMap[name] = message
```
Right, so what's this? Apparently maps in Go work in a similar vein to defaultdicts in Python. More on this later, maybe? (WIP)

That wraps it up with `greetings.go`, and `main.go`. 

And that's how you handle multiple functions and maps! Also, that wraps up the idea of maps.

Next, we're gonna run some tests in [[setting up tests]]

Tags: #maps-go, #go, #backend , #iteration-methods
