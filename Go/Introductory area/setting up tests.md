# Personal notes

Right, so about adding tests into Go. First off, naming scheme.
Tests should always be named `testname_test.go`. The `_test.go` part is necessary, it tells Go that this is a test file.

Starting off, let's take a look at a test file. We write it in the `greetings` folder, in a file named `greetings_test.go`.

`greetings_test.go`
```go
package greetings

import (
	"regexp"
	"testing"
)

func TestHelloName(t *testing.T) {
	name := "Mihir"
	want := regexp.MustCompile(`\b` + name + `\b`)
	msg, err := Hello("Mihir")
	if !want.MatchString(msg) || err != nil {
		t.Fatalf(`Hello("Gladys") = %q %v, want match for %#q, nil`, msg, err, want)
	}
}

func TestHelloEmpty(t *testing.T) {
	msg, err := Hello("")
	if msg != "" || err == nil {
		t.Fatalf(`Hello("") = %q, %v, want match for "", error`, msg, err)
	}
}

```

**Code anatomy**

Here, the first thing we notice are the imports. `import("regexp" "testing")` import the inbuilt regex and testing modules. 
Next, we see the two test functions, `TestHelloName()` and `TestHelloEmpty()`. These two functions are the two testcases we have defined.
> A short note on Go convention
> Go convention dictates that test functions must be named according to the scheme Test*Description*().


Okay, onto the meat and potatoes. Let's take a closer look at `TestHelloName()`. 

```go
func TestHelloName(t *testing.T) {
	name := "Mihir"
	want := regexp.MustCompile(`\b` + name + `\b`)
	msg, err := Hello("Mihir")
	if !want.MatchString(msg) || err != nil {
		t.Fatalf(`Hello("Gladys") = %q %v, want match for %#q, nil`, msg, err, want)
	}
}
```
Here, we see a testing object `t *testing.T` in the declaration and no return type. A return is not needed by Go in this context: The test object is responsible for returning stuff when a test fails or passes.
The next thing that we see is that we have a variable `want` declared as follows:
```go
want := regexp.MustCompile(`\b` + name + `\b`)
```
`regex.MustCompile()` is a function that allows us to hold a compiled regular expression. Having a name at any point in the string defines a successful run for us. So we store the precompiled expression in the variable want.
If we don't get what we want as an output, we set up a `t.Fatalf()` command. `t.Fatalf()` is a type of return built into the testing object that tells Go "Hey, this test failed, get me outta here." And we can pass a message through it! The `f` part at the end of `Fatal`, as you may have guessed, is a classic "print this to stdout" request.  For more analogous information, see [[fmt print routines]].

The rest of the code is straightforward. `TestHelloEmpty()` is similarly trivial to understand.

When running a test, we use `go test` for a testing output that tells us whether the test passed or failed, and a `go test -v` command for a more verbose testing output.

Tags: #go, #testing, #regex