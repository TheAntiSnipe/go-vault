# Personal notes

**Starting off with what makes Go, Go**

- Compiles and runs code faster than many interpreted languages
- Writes highly concurrent code
- Runs directly on hardware
- Works on a ton of modern features like packages while also excluding stuff like classes.

> Google developed Go and its standard library as an answer to software problems at Google: multicore processors, networked systems, massive computation clusters—in other words, distributed systems, and at large scale in terms of lines of code, programmers, and machines.

\- Travis Jeffery, *Distributed Services with Go*

The starting point the book picks, and the point I'll be starting from as well, is to build something called a "commit log json over http service".

**So first off, what is that?**

Let's break it down. A ***commit log*** is a log of transactions made successfully, as we've seen in databases.
Now what is ***json over http***? Well, json is a very easily parseable format for data. Pretty much every modern programming language supports a json parser out of the box, or has convenient libs that allow us to parse it. http is a very universal data transfer protocol for data transfer over the web. json over http is basically the act of sending json formatted data using http. Yes, it's just some fancy terminology for what we already did in [[Writing a RESTful API with Gin]].


**The log.go file**
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
Further readi