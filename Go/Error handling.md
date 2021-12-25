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

The `errors` package is another one of the inbuilt packages that comes with Go. It introduces the `error` datatype and allows you to throw an error back with `errors.New()`. Here, we throw back a string `"empty name"`.  

Another key takeaway here is how the function is declared here; Notice this:
```go
func Hello(name string) (string,error)
```
See how it returns two values? Apparently, any function in Go can return multiple values. This is apparently a feature made to do better than C's way of error handling, where, say, if you were writing something to stdout and failed, you get a negative error code and the actual error is kept somewhere volatile that you can never really access. Go, on the other hand, lets you do handy stuff like what we saw above.

