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
Here, we see a testing object `` in the declaration and no return type. A return is not needed by Go in this context: The test object is responsible for returning stuff when a test fails or passes.
