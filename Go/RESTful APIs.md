# Personal notes

Heavily derived from [What is a REST API? | IBM](https://www.ibm.com/cloud/learn/rest-apis)

So what defines a RESTful API?

1. If you're calling for a resource named X, no matter who you are, the identifier for that resource, the Uniform Resource Identifier (URI), for X remains the same. 
2. The server and the client are completely independent of one another. The client only needs to know the URI of the resource in order to access it. No other information should be necessary.
3. Another important thing that defines a REST API is that it is stateless. The user's state doesn't matter. Everything the server needs to process the user's request should already be in the URI itself. Conversely, the server does not and need not store anything related to the client. This saves both sides space.
4. Resources need to be cacheable on the client or server side, which increases responsiveness on the client side and scalability on the client side.
5. The server does not need to be able to tell whether the request was made by the client or the intermediary. This facilitates a layered system architecture.
6. An optional thing is to be able to send code. This code should only be run on demand.

More hands-on stuff: [REST API concepts and examples - YouTube](https://www.youtube.com/watch?v=7YcW25PHnAA)