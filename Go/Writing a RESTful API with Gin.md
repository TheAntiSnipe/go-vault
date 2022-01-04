# Personal notes

### Starting off, what even IS a RESTful API?
A RESTful API is defined by certain architectural constraints. Note that REST stands for Representational State Transfer. What characterizes a REST API? This is really outside the scope of this specific vault, but we'll keep a peripheral reading section for it anyway. [[RESTful APIs]]

### Okay, let's get started

I'm just going to dump the full code here:

```go
package main

import (
	"net/http"

	"github.com/gin-gonic/gin"
)

type album struct {
	ID     string  `json:"id"`
	Title  string  `json:"title"`
	Artist string  `json:"artist"`
	Price  float64 `json:"price"`
}

var albums = []album{
	{ID: "1", Title: "X", Artist: "Ed Sheeran", Price: 34.66},
	{ID: "2", Title: "In a perfect world", Artist: "Kodaline", Price: 29.99},
	{ID: "3", Title: "Montevallo", Artist: "Sam Hunt", Price: 29.99},
}

func getAlbums(c *gin.Context) {
	c.IndentedJSON(http.StatusOK, albums)
}

func postAlbums(c *gin.Context) {
	var newAlbum album
	if err := c.BindJSON(&newAlbum); err != nil {
		return
	}

	albums = append(albums, newAlbum)
	c.IndentedJSON(http.StatusCreated, newAlbum)
}

func getAlbumByID(c *gin.Context) {
	id := c.Param("id")
	for _, album := range albums {
		if album.ID == id {
			c.IndentedJSON(http.StatusOK, album)
			return
		}
	}
	c.IndentedJSON(http.StatusNotFound, gin.H{"message": "album not found"})
}

func main() {
	router := gin.Default()
	router.GET("/albums", getAlbums)
	router.GET("/albums/:id", getAlbumByID)
	router.POST("/albums", postAlbums)
	router.Run("localhost:8080")
}

```

As we have so far, we'll start off the top and work our way down.

**Code anatomy**
The first thing we notice is the two imports. By this time, we know what's up. `net/http` allows us to use the various HTTP status codes that we're gonna be using down the line. `github.com/gin-gonic/gin` links to the GitHub repo for the gin library, which is what we're gonna see a lot of, now. What's Gin though? The short version: An extremely fast web framework written in Go that has the highest routing speed benchmark scores of any web framework as of 1/3/2022.

The line after that is pretty cool:

```go
type album struct {
	ID     string  `json:"id"`
	Title  string  `json:"title"`
	Artist string  `json:"artist"`
	Price  float64 `json:"price"`
}
```
So this is a structure declaration, done in a similar way to C, but inverted in form as usual. Now, here's the rabbithole: What's going on with the 
```go
`json:"id"`
```
part? Keeping it simple, it's called a struct tag. It maps the text you see here to input values. How it is declared and how it works under the hood is, however, a part of a freakin' massive rabbit hole in Go. I think I'll be approaching this later, but I'll still make a small note of this over here: [[reflect]]

With that out of the way, let's look at the var declaration for `albums`. `albums` is a slice of `album` structures that is defined by the respective parameters, simple enough.

Now to the meat and potatoes: The three functions that handle requests. `getAlbums`, `postAlbums`, and `getAlbumByID`.

`getAlbums`
```go
func getAlbums(c *gin.Context) {
	c.IndentedJSON(http.StatusOK, albums)
}
```
The function accepts a variable c that is of type `*gin.Context`.  We'll be working with `gin.Context` quite a bit more in API design,  here's a docs source: [gin.Context](https://pkg.go.dev/github.com/gin-gonic/gin#Context). For now, though, all we're doing with it is we're shooting off an indented json packed into the response body as a response to the HTTP request, with an "ok" response and `albums` as the payload. [gin.Context.IndentedJSON](https://pkg.go.dev/github.com/gin-gonic/gin#Context.IndentedJSON)

`postAlbums`
```go
func postAlbums(c *gin.Context) {
	var newAlbum album
	if err := c.BindJSON(&newAlbum); err != nil {
		return
	}

	albums = append(albums, newAlbum)
	c.IndentedJSON(http.StatusCreated, newAlbum)
}
```
This is a bit more complex. Only because of how the `if` statement is handled, though. Go, like most langs, has the ideal of not polluting the global scope. The if statement here: 
```go
if err := c.BindJSON(&newAlbum); err != nil {
		return
	}
```
shows some consideration towards that end. The `err` variable is declared in a scope local to the `if` statement, and if it's found to be nil, the `c.BindJson(&newAlbum)` still assigns a new value to the contents of newAlbum, but the locally scoped `err` variable dies. 

For the sake of readability, this block can actually be written as
```go
func postAlbums(c *gin.Context) {
	var newAlbum album
	err := c.BindJSON(&newAlbum)
	if err != nil {
		return
	}

	albums = append(albums, newAlbum)
	c.IndentedJSON(http.StatusCreated, newAlbum)
}
```
`err` won't die, but it's fine unless we're working on some very memory intensive task and these errors that are scoped to the function start becoming a performance issue. They generally won't, so this remains a convention issue. Other than that, we simply show the user that the new album has been added to the album list after appending it.

`getAlbumByID`
```go
func getAlbumByID(c *gin.Context) {
	id := c.Param("id")
	for _, album := range albums {
		if album.ID == id {
			c.IndentedJSON(http.StatusOK, album)
			return
		}
	}
	c.IndentedJSON(http.StatusNotFound, gin.H{"message": "album not found"})
}
```

Here, we assign `c.Par`