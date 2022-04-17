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
package main

import (
	"log"
	"math/rand"
	"time"
)

const TimeOutTime = 3
const MeanArrivalTime = 4

func main() {
	const interval = time.Second * TimeOutTime
	// channel for incoming messages
	var incomeCh = make(chan struct{},1)
	var outCh = make(chan struct{},1)
	log.Println("Start")
	go func() {
		for {
			// On each iteration new timer is created
			select {
			case <-time.After(interval):
				log.Println("THREAD 1 : Sending a timeout to the other thread!")
				outCh <- struct{}{}
			case <-incomeCh:
				log.Println("THREAD 1 : Recieved a heartbeat from the other thread!")
			}
		}
	}()

	go func() {
		for {
			select {
			case <-time.After(time.Duration(rand.Intn(MeanArrivalTime)) * time.Second):
				log.Println("THREAD 2 : Sending a heartbeat to the other thread!")
				incomeCh <- struct{}{}
			case <-outCh:
				log.Println("THREAD 2 : Recieved a timeout from the other thread!")
				// handle timeout response
			}
		}
	}()

	// prevent main to stop for a while
	<-time.After(30 * time.Second)
}

```
Let's take it from the top.
1. Two goroutines: Absolutely necessary, since you're doing two things at the same time.
2. A channel (two in our case): Gotta talk to each other
3. A select-case pattern (?): Okay, why?