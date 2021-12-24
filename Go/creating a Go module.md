# Personal notes

This notes section is based off of this tutorial section from the official Go docs: [Tutorial: Create a Go module - The Go Programming Language](https://go.dev/doc/tutorial/create-module)

Two files:
	- A library module that is intended to be imported by other libs/apps.
	- A caller application.

Okay, so starting off, let's have a folder called 'greetings'. In this folder, we run `go mod init example.com/greetings`.

> Some troubleshooting, in case people run into the error I did: Go may not recognize the go command on the vscode integrated terminal when working on a fresh install. To troubleshoot, just restart your PC. No idea why that happens.

Now, we write a small script in there that simply returns a string greeting a person given a name:

`greetings.go`
```go
package greetings
import "fmt"

func Hello(name string) string {
 message := fmt.Sprintf("Hello, %s!", name)
 return message
}
```

**Code anatomy**
Let's take a look at this function declaration, seeing as how this is our first time declaring one:
```go
func Hello(name string) string {
 message := fmt.Sprintf("Hello, %s!", name)
 return message
}
```
The first thing we notice is the tokens:
`func` - Initializes the function
`name string` - The variable `name` is a `string`.
`func Hello(name string) string {` - The function's name is Hello, it accepts a string, and calls it `name` within its scope. It returns a string datatype to the caller.

The second interesting thing that we haven't seen so far is the variable declaration syntax: 
```go
message:=fmt.Sprintf("Hello, %s!", name)
```

Okay, here we go. First things first, `:=` implies an assignment. If you use `=`, you need to have declared the variable first. `:=` is what you use when you declare AND assign at the same time.

Alternatively, another angle of attack here would be to write the code as:



Now what's `fmt.Sprintf`? Here's the reference I used to get the answer: [fmt package - fmt - pkg.go.dev](https://pkg.go.dev/fmt)
Here's the short answer: It's the only one of the three functions that returns a string. The others write directly to a standard output and return error codes and byte lengths. For the long answer, check out [[fmt print routines]].

Another thing to note is the Pythonic auto-formatting. Just a sidenote. Other than that, we can move on.



Tags: #go, #modules, #function-standard