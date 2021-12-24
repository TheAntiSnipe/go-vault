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