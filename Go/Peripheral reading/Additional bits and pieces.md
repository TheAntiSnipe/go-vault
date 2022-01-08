1. If you have consecutive function inputs of the same datatype, you can declare them without datatype for all but the last one.
**Eg:**
```go
func doThis(a int, b int, c int) int {
	return a + b + c
}
```
can be written as:
```go
func doThis(a, b, c int) int {
	return a + b + c
}
```

2. If you have multiple returns like that, they can also be treated the same way. Also, if you have named returns, you can send something called a naked return!
**Eg:**
```go
func doThis(a, b, c int) (p int, q int, r int) {
	p = a + b
	q = b + c
	r = a + c
	return p, q, r
}
```
can be written as:
```go
func doThis(a, b, c int)(p, q, r int) {
	p = a + b
	q = b + c
	r = a + c
	return
}
```
The naked return basicallly just sends back all the named returns.

3. Another important rule in Go is that there can be no variables declared in the short syntax in the global scope. **Every statement in the global scope begins with a keyword.**
```go
a:=10 // Invalid!

func main() {
	a:=10 // Valid!
}
```

4. There's a **"factoring syntax"** for datatypes in Go as well, just like there is one for library imports:
```go
var (
	ToBe   bool       = false
	MaxInt uint64     = 1<<64 - 1
	z      complex128 = cmplx.Sqrt(-5 + 12i)
)
```

5. **Go needs explicit conversions for variables. Don't forget.**

6. Also, remember that the `const` keyword exists in Go. Used in the same way as `var`.

7. `for` is Go's `while`

8. Variables declared in an `if` clause using the short declaration we saw in [[Writing a RESTful API with Gin]] (line 110) can also be freely used in the `else` clause.

9. 