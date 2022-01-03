# Personal notes

### Starting off, what even IS a RESTful API?
A RESTful API is defined by certain architectural constraints. Note that REST stands for Representational State Transfer. What characterizes a REST API?

1. If you're calling for a resource named X, no matter who you are, the identifier for that resource, the Uniform Resource Identifier (URI), for X remains the same. 
2. The server and the client are completely independent of one another. The client only needs to know the URI of the resource in order to access it. No other information should be necessary.
3. Another important thing that re