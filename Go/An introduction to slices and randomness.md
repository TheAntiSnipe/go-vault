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

Also, I feel like this would be incomplete without this output I legit got out o