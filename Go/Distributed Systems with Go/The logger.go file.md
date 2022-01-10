# Personal notes

Okay, so first, we need to have a piece of code that handles log writing and viewing. A commit log entry is made when we perform some sort of a transaction. Clearly, it's got this structure going on. 

For one, in case we get many requests to access or modify it, those requests should be made mutually exclusive: Mutex is one of the major factors we've seen in distributed systems.

Also, the most important thing is contents: The contents are basically strings stored in slices for our implementation.

**log.go**
```go
package server

import (
	"fmt"
	"sync"
)

type Log struct {
	mutex   sync.Mutex
	records []Record
}

type Record struct {
	Value  []byte `json:"value"`
	Offset uint64 `json:"offset"`
}

var ErrOffsetNotFound = fmt.Errorf("offset not found")

func NewLog() *Log {
	return &Log{}
}

func (c *Log) Append(record Record) (uint64, error) {
	c.mutex.Lock()
	defer c.mutex.Unlock()
	record.Offset = uint64(len(c.records))
	c.records = append(c.records, record)
	return record.Offset, nil
}

func (c *Log) Read(offset uint64) (Record, error) {
	c.mutex.Lock()
	defer c.mutex.Unlock()
	if offset >= uint64(len(c.records)) {
		return Record{}, ErrOffsetNotFound
	}
	return c.records[offset], nil
}
```

**Code anatomy**
What do we have going on here? First off, imports: The `server` package and the `sync` module make an appearance here. Like before, we'll be seeing more of these as we move along.

Onto the main code! First, we have the `Log` struct:
```go
type Log struct {
	mutex   sync.Mutex
	records []Record
}
```
It contains the `mutex` attribute, courtesy of the `sync` module's `sync.mutex`, and `records`, of type `[]Record`. This is basically a slice of records. Now what's a record? We see that in the next subsection.

```go
type Record struct {
	Value  []byte `json:"value"`
	Offset uint64 `json:"offset"`
}
```
Okay, so a `Record` struct is defined here. It contains byte data formatted value. Basically a slice of bytes (characters). Easy enough.

```go
var ErrOffsetNotFound = fmt.Errorf("offset not found")
```
Okay, what's going on here? We know that Go doesn't look kindly upon global vars, so what's up with this error declaration that (we'll see) is only referenced once in the entire codebase right now?
Well, this is called a "sentinel error". It's declared like this so that people using our lib can easily pull its value for comparison.

> For people who call that function, they can easily check if the returned error is that value or not by using `err == yourpackage.ErrOffsetNotFound` (which is discouraged due to wrapping) or `errors.Is(err, yourpackage.ErrOffsetNotFound)`

\- `hhhapz#8936`, Go discord server
> It's for correct error checking, string eq doesn't always work correctly

\- `_diamondburned_#4507`, Go discord server

An example of sentinel errors in prod code: [io package - io - pkg.go.dev](https://pkg.go.dev/io@go1.17.5#pkg-variables)
Further reading: [Constant errors | Dave Cheney](https://dave.cheney.net/2016/04/07/constant-errors)

Next up! 

```go
func NewLog() *Log {
	return &Log{}
}

func (c *Log) Append(record Record) (uint64, error) {
	c.mutex.Lock()
	defer c.mutex.Unlock()
	record.Offset = uint64(len(c.records))
	c.records = append(c.records, record)
	return record.Offset, nil
}

func (c *Log) Read(offset uint64) (Record, error) {
	c.mutex.Lock()
	defer c.mutex.Unlock()
	if offset >= uint64(len(c.records)) {
		return Record{}, ErrOffsetNotFound
	}
	return c.records[offset], nil
}
```

These three functions are going to be made open to users of the package. newLog simply instantiates an empty log, simple enough, 
```go
func (c *Log) Append(record Record) (uint64, error) {
	c.mutex.Lock()
	defer c.mutex.Unlock()
	record.Offset = uint64(len(c.records))
	c.records = append(c.records, record)
	return record.Offset, nil
}
```
simply appends-- Oh, wait, what's up with this syntax?
```go
func (c *Log) Append(record Record) (uint64, error)
```
Okay, we've never seen this specific way of declaring a function. What is this `c *Log` thingy in brackets behind the function name?

Well, as we know, Go doesn't have the concept of classes. To allow for a function to be latched onto a struct, we use these things. They're called "methods on types". So basically, once someone creates a `Log` object, they can just do `objectname.Append()`.

`c.mutex.Lock()` locks the current instance to prevent reads or writes while the operation is going on. `defer c.mutex.Unlock()` is a deferred unlock that happens once the current function has finished executing. Other than that, both the rest of the function and the rest of the remaining code is trivial. Overall, this code contains a whole bunch of handler functions that pick up information and record/recall it as required.

Tags: #go, #backend , #mutex, #sentinel-errors, #methods-on-types