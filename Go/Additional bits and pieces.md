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
func doThis(a,b,c int) (p int, q int, r int) {
	p:=a+b
	q:=
}
```