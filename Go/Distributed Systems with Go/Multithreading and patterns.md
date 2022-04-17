# Personal notes
![[multithread-goal.jpeg]]
The above is what I wanna achieve.

First things first, need to get timeout and reception's interconnection working.
Desirable behavior:

1. Both timeout and reception boot up at the same time, and both run indefinitely.
2. Timeout resets itself if it hears a heartbeat from reception.
3. Reception handles some behavior if it hears from timeout.

Reference code from SO:
```go

```
Let's take it from the top.
1. Two goroutines: Absolutely necessary, since you're doing two things at the same time.
2. A channel (two in our case): Gotta talk to each other
3. A select-case pattern (?): Okay, why?