#go #golang #book_summery 
## Lesson 30
In Go, an independently running task is known as a **goroutine.**

**Running an gorutine:**
```go
go sleepyGopher()
```

Each time we use the `go` keyword, <u>a new goroutine is started.</u>
All goroutines appear to run at the same time.
**They might not technically run at the same time, though, because computers only have a limited number of processing units.**

these **processors usually spend some time on one goroutine** before proceeding to another, using a technique known as ***<font color="#e36c09">time sharing</font>***, process is between<u> go and os</u>.

**A <font color="#548dd4">channel</font> can be used to send values safely from one goroutine to another.**

To create a channel <font color="#ffff00">make</font>,*Channels* have a type that’s specified when you make them.

This channel only send and receive *integers*:
```go
c := make(chan int)
```

To send a value, **point the arrow toward the channel expression**, as if the arrow were telling the value on the right to flow into the channel.
The send operation *will wait until* something (in another goroutine) tries to *receive on the same channel*.
**While it’s waiting, the sender can’t do anything else, although all other goroutines will continue running freely** (assuming they’re not waiting on channel operations too).
```go
c <- 99 // send to the channel 

r := <-c // receive from the channel
```

the Go standard library provides a nice function, `time.After`, to help.
<font color="#ffff00">It returns a channel that receives a value after some time has passed</font> (the goroutine that sends the value is part of the Go runtime).

The *select* statement looks like the *switch* statement . Each case inside a select holds a channel receive or send. **select waits until one case is ready and then runs it and its associated case statement**. It’s as if select is looking at both channels at once and takes action when it sees something happen on either of them.

```go
timeout := time.After(2 * time.Second)

for i := 0; i < 5; i++ {
    select {
    case gopherID := <-c:
        fmt.Println("gopher ", gopherID, " has finished sleeping")
    case <-timeout:
        fmt.Println("my patience ran out")
	return
    }
}
```

When *there are no cases in the select statement*, it **will wait forever**. That might be useful to stop the main function returning when you’ve started some goroutines that you want to leave running indefinitely.

If you try to use a `nil` channel, it won’t panic—instead, the operation (send or receive) <font color="#e36c09">will block forever</font>, like a channel that nothing ever receives from or sends to. The exception to this is `close` . **If you try to close a nil channel, it will panic**.

> Consider a loop containing a select statement. We may not want to wait for all the channels mentioned in the select every time through the loop. For example, we might only try to send on a channel when we have a value ready to send. We can do that by using a channel variable that’s only non-nil when we want to send a value When a goroutine is waiting to send or receive on a channel, we say that it’s blocked a blocked goroutine takes no resources (other than a small amount of memory used by the goroutine itself).

**When one or more goroutines end up blocked for something that can never happen, it’s called <font color="#e36c09">deadlock</font>, and your program will generally crash or hang up.**

```go
func main() {
    c := make(chan int)
    <-c
}
```

> pipeline, is useful for processing large streams of data without using large quantities of memory

A worker in the pipeline repeatedly receives a value from its *upstream* neighbor, does something with it, and sends the result *downstream*

Go lets us close a channel to signify that no more values will be sent

```go
close(c)
```

When a channel is closed, you can’t write any more values to it (you’ll get a panic if you try), and any read will return immediately with the zero value for the type (the empty string in this case).

How do we tell whether the channel has been closed? Like this:

```go
v, ok := <-c
```

**If we use a channel in a range statement, it will read values from the channel until the channel is closed.**

* * *

## lesson 31

This kind of situation is known as a **race condition** because it’s as if the goroutines are **racing to use the value**.

>The Go compiler includes functionality that tries to find race conditions in your code.

**It’s okay if two goroutines read from the same thing at the same time, but if you read or write at the same time as another write, you’ll get undefined behavior.**

The word *mutex* is short for mutual exclusion. Goroutines can use a mutex to exclude each other from doing something at the same time. The something in question is up to the programmer to decide.

Mutexes have two methods: `Lock` and `Unlock`

*To use the mutex properly, we need to make sure that any code accessing the shared values locks the mutex first, does whatever it needs to, then unlocks the mutex*.
>If any code doesn’t follow this pattern, we can end up with a race condition.

Because of this, mutexes are almost always kept internal to a package. The package knows what things the mutex guards, but the Lock and Unlock calls are nicely hidden behind methods or functions.

**Unlike channels, Go mutexes aren’t built into the language itself**. Rather, they’re available in the `sync` package.

We don’t need to initialize the mutex before using it—the zero value is an unlocked mutex.
```go
package main

import "sync"

var mu sync.Mutex

func main() {
	mu.Lock()
	defer mu.Unlock() // The lock is held until we return from the function.
}
```

>In Go, you should assume that no method is safe to use concurrently unless it’s explicitly documented, as we’ve done here.

If we block to wait for something when we’ve locked the mutex, we may be locking others out for a long time. Worse, if we somehow try to lock the same mutex, we’ll deadlock—the Lock call will block forever because we’re never going to give up the lock while we’re waiting to acquire it!

A worker is often written as a for loop containing a select statement.

Some other programming languages use an event loop —a central loop that waits for events and calls registered functions when they occur. By providing goroutines as a core concept, Go avoids the need for a central event loop. Any worker goroutine can be considered an event loop in its own right.