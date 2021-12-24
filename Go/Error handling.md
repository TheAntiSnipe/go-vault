# Personal notes

Now we begin with handling errors. Let's jump right into our `greetings.go` code:

`greetings.go`
```go
package greetings

import ("fmt"
"errors")

func Hello(name string) (string,error) {
	if name == "" {
	return "",errors.New("empty name")
	}
	message := fmt.Sprintf("Hello, %s!", name)
	return message,nil
}
```