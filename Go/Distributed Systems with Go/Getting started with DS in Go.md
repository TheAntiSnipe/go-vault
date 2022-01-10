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

Okay, so first