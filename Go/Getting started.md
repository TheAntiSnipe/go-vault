# Personal notes

**Getting started with Go**
Writing a basic Go script here:

```go
package main
import "fmt"
func main() {
    fmt.Println("Hello, World!")
}
```

To run this, you first need dependency tracking enabled.
Say the name of this file is `helloworld.go` and you want to enable dependency tracking. Just run `go mod init helloworld` and you're good to go!

Why though?
Well, this makes less sense when we're talking local machines, apparently,  but here goes.
> When your code imports packages contained in other modules, you manage those dependencies through your code's own module. That module is defined by a go.mod file that tracks the modules that provide those packages. That go.mod file stays with your code, including in your source code repository.

Okay, so say you've got <mark style="background: undefined;">packages</mark> that are in other modules that you depend on. You gotta manage your dependencies properly! Hence, Go has a <mark style="background: undefined;">go.mod</mark> file that tracks the modules that provide the packages that you depend on.

By the way, the go.mod file for our sample code above looks like this:
```go
module helloworld
go 1.17
```


**Code anatomy**

`main` is a package that you import. In Go's context, a package is a way to group functions and it is made of all the files in the same directory. Specifically, the main package contains the `main()` function. This function is the entry point of the executable program in question. So if your code has, like, seven files, **only one of them is the main entry point, and thus it's the only place where you import main.** The function itself is implicit and you only have to declare it. Go knows what it is, and it will call it properly.

`import fmt` refers to the formatting package `fmt` which is actually a really mainstream import that allows you to do stuff like write to the console and, well, format stuff! This is one of the standard library packages that's a part of Go's default package set. 

What follows is pretty elementary: 
```go
fmt.Println("Hello world")
```

is just a classic print syntax.

**Running the code**

Ez.
```go run .```
A classical go command. Lotsa go commands exist, and this one just runs whatever is in the directory you're in.

**go help**
Here's some stuff from there:
[[List of things in go help]]

**Running code that comes from an external package**
 This is a pretty common case. Say you've got a piece of code that was written by someone else and put into an external package. How do you do that?

 There's a website named https://pkg.go.dev that has a ton of packages built by the community. Take a look there if you need something that may have been made before.

 So for our first little test, we import a package called Quote, and make a function call to `quote.Go()`.
 ```go
package main

import "fmt"
import "rsc.io/quote"

func main() {
    fmt.Println(quote.Go())
} 
```

This is simple enough, but next we have to make a call to `go mod tidy`. Go then handles the adding of the quote module as a requirement, and also adds the `go.sum` file that authenticates the module. For more detail, see [[module authentication in Go]].

Here's how that looks when it's all done:

`go.mod`
```go 
module helloworld
go 1.17

require rsc.io/quote/v4 v4.0.1
require (
 golang.org/x/text v0.0.0-20170915032832-14c0d48ead0c // indirect
 rsc.io/sampler v1.3.0 // indirect
)```

`go.sum`
```go
golang.org/x/text v0.0.0-20170915032832-14c0d48ead0c h1:qgOY6WgZOaTkIIMiVjBQcw93ERBE4m30iBm00nkL0i8=
golang.org/x/text v0.0.0-20170915032832-14c0d48ead0c/go.mod h1:NqM8EUOU14njkJ3fqMW+pc6Ldnwhi/IjpwHt7yyuwOQ=

rsc.io/quote/v4 v4.0.1 h1:i/LHLEinr65wwTCqlP4OnMoMWeCgnFIZFvifdXNK+5M=
rsc.io/quote/v4 v4.0.1/go.mod h1:w/DafQky66grMesu3uPhdDMS3knhBippwwemZtMOyCI=
rsc.io/sampler v1.3.0 h1:7uVkIFmeBqHfdjD+gZwtXXI+RODJ2Wc4O7MPEh/QiW4=
rsc.io/sampler v1.3.0/go.mod h1:T1hPZKmBbMNahiBKFy5HrXp6adAjACjK9JXDnKaTXpA=
```

Tags: #go, #backend