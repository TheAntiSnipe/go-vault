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

**Code anatomy**

The `errors` package is another one of the inbuilt packages that comes with Go. It introduces the `error` datatype and allows you to throw an error back with `errors.New()`. Here, we throw back a string `"empty name"`.  See [errors package - errors - pkg.go.dev](https://pkg.go.dev/errors#example-New)

Another key takeaway here is how the function is declared here; Notice this:
```go
func Hello(name string) (string,error)
```
See how it returns two values? Apparently, any function in Go can return multiple values. This is apparently a feature made to do better than C's way of error handling, where, say, if you were writing something to stdout and failed, you get a negative error code and the actual error is kept somewhere volatile that you can never really access. Go, on the other hand, lets you do handy stuff like what we saw above. A good reference of this is [Effective Go - The Go Programming Language](https://go.dev/doc/effective_go), specifically, [Effective Go - Multiple returns](https://go.dev/doc/effective_go#multiple-returns)

Moving on, we now need to work on `hello.go` so that it can handle the error thrown when the name is empty.

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
 message, err := greetings.Hello("")

 if err != nil {
 log.Fatal(err)
 }

 fmt.Println(message)
}
```

**Code anatomy**
We start with the first change that we just made, the `"log"` import. There's a little bit going on here  with `log.SetPrefix()`... Basically, whatever is in the brackets will be printed. It's a prefix. Duh.
Anyway, next comes `log.SetFlags(0)`. In this context, the `0` means we don't want any other logging data. There's a lot of options here. In fact, `log.SetFlags(0)` is about five times slower than the most optimal way to set logging to off. [logging - How to disable a log.Logger - Stack Overflow](https://stackoverflow.com/questions/10571182/how-to-disable-a-log-logger) for more. Also, some more code examples of working with the logging system in general: [Golang SetFlags Examples, log.SetFlags Golang Examples - HotExamples](https://golang.hotexamples.com/examples/log/-/SetFlags/golang-setflags-function-examples.html)

