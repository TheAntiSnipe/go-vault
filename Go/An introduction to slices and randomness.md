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
Anyway!

**Code anatomy**
