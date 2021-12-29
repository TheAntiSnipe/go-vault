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