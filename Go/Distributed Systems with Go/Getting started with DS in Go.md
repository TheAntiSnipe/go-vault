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
