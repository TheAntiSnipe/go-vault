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
part? Keeping it simple, it's called a struct tag. It maps the text you see here to input values. It is, however, a part of a freakin' massive rabbit hole in Go. I think I'll be approaching this later, but I'll still make a small note of this over here: [[reflect]]