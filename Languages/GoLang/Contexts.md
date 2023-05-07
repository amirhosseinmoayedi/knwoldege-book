#go #golang 

## What is <font color="#ffc000">Context</font>
Many functions in Go use the `context` package to gather <u>additional information about the environment they’re being executed in</u>, and will typically provide that context to the functions they also call. By using the `context.Context` interface in the `context` package and passing it from function to function, programs can convey that information from the beginning function of a program, such as `main`, all the way to the deepest function call in the program. 

The `Context` function of an `http.Request`, for example, will provide a `context.Context` that includes information about the client making the request and will end if the client disconnects before the request is finished.

It’s also **recommended** to put the `context.Context` parameter <u>as the first parameter in a function</u>, and you’ll see it there in the Go standard library.


Contexts can be a powerful tool with all the values they can hold,<font color="#e36c09"> but a balance needs to be struck between data being stored in a context and data being passed to a function as parameters</font>. It may seem tempting to *put all of your data in a context* and use that data in your functions instead of parameters, **but that can lead to code that is hard to read and maintain**. <font color="#e36c09">A good rule of thumb is that any data required for a function to run should be passed as parameters.</font>

## <font color="#c3d69b">TODO</font> and <font color="#fac08f">Background</font>
```go

package main

import (
	"context"
	"fmt"
)

func doSomething(ctx context.Context) {
	fmt.Println("Doing something!")
}

func main() {
	ctx := context.TODO()
	doSomething(ctx)
}
```
There is two way two create <font color="#ffff00">empty contex</font>:
1. `context.TODO` 
		You can use this as a placeholder when you’re not sure which context to use.
2. `context.Background`
		like the `TODO` ,but it’s designed to be <u>used where you intend to start a known context</u>. 

>  If you’re unsure which one to use, `context.Background` is a **good default option**.


## <font color="#95b3d7">WithValue</font>
To add a new value to a context, use the `context.WithValue` function in the `context` package.
```go
...

func doSomething(ctx context.Context) {
	fmt.Printf("doSomething: myKey's value is %s\n", ctx.Value("myKey"))
}

func main() {
	ctx := context.Background()

	ctx = context.WithValue(ctx, "myKey", "myValue")

	doSomething(ctx)
}
```
`context.WithValue` function **didn’t modify the context you provided**. Instead, <u>it wrapped your parent context inside another one with the new value</u>.



## context <font color="#31859b">Done</font> method
Another powerful tool `context.Context` provides is a **way to signal to any functions using it that the context has ended and should be considered complete**. By signaling to these functions that the context is done, they know to stop processing any work related to the context that they may still be working on.

The `context.Context` type provides a method called `Done` that can be checked to see whether a context has ended or not.

This method returns a <font color="#ffff00">channel</font> <u>that is closed</u> when the context is **done**.

The `Done` method works because <u>no values are ever written to its channel</u>, and<u> when a channel is closed that channel will start to return `nil` values for every read attempt</u>.

```go
ctx := context.Background()
resultsCh := make(chan *WorkResult)

for {
	select {
	case <- ctx.Done():
		// The context is over, stop processing results
		return
	case result := <- resultsCh:
		// Process the results received
	}
}
```

If the example’s `select` statement had a `default` branch without any code in it, it wouldn’t actually change how the code works, it would just cause the `select` statement to end right away and the `for` loop would start another iteration of the `select` statement. This leads to the `for` loop executing **very quickly** because it will never stop and wait to read from a channel. When this happens, the `for` loop is called a _busy loop_ because instead of waiting for something to happen, the loop is busy **running over and over again**. This <u>can consume a lot of CPU because the program never gets a chance to stop running to let other code execute</u>. Sometimes this functionality is useful, though, such as if you want to check if a channel is ready to do something before you go and do another non-channel operation.


## <font color="#d99694">WithCancel</font>
Similar to including a value in a context with `context.WithValue`, it’s possible to associate a “cancel” function with a context using the `context.WithCancel` function. This function receives a parent context as a parameter and returns a new context as well as a function that can be used to cancel the returned context.
```go
package main

import (
	"context"
	"fmt"
	"time"
)

func doSomething(ctx context.Context) {
	ctx, cancelCtx := context.WithCancel(ctx)
	
	printCh := make(chan int)
	go doAnother(ctx, printCh)

	for num := 1; num <= 3; num++ {
		printCh <- num
	}

	cancelCtx()

	time.Sleep(100 * time.Millisecond)

	fmt.Printf("doSomething: finished\n")
}

func doAnother(ctx context.Context, printCh <-chan int) {
	for {
		select {
		case <-ctx.Done():
			if err := ctx.Err(); err != nil {
				fmt.Printf("doAnother err: %s\n", err)
			}
			fmt.Printf("doAnother: finished\n")
			return
		case num := <-printCh:
			fmt.Printf("doAnother: %d\n", num)
		}
	}
}

...
```

The key is the `cancel func` that handles three procedures.

-   Cancel the current context.
-   Call all the `cancel func` in the sub-contexts. Here the cancellation propagates to all the sub-contexts in recursion.
-   Unregister itself from the parent.

> -   Don’t use `WithTimeout` or `WithCancel` to wrap context that may be canceled.


## <font color="#00b0f0">WithDeadLine</font>
Using `context.WithDeadline` with a context allows you to **set a deadline for when the context needs to be finished**, and it will automatically end when that deadline passes.
You’ll then receive a new context and a function to cancel the new context as return values. Similar to `context.WithCancel`, when a deadline is exceeded, it will only affect the new context and any others that use it as a parent context.
```go
...

func doSomething(ctx context.Context) {
	deadline := time.Now().Add(1500 * time.Millisecond)
	ctx, cancelCtx := context.WithDeadline(ctx, deadline)
	defer cancelCtx()

	printCh := make(chan int)
	go doAnother(ctx, printCh)

	for num := 1; num <= 3; num++ {
		select {
		case printCh <- num:
			time.Sleep(1 * time.Second)
		case <-ctx.Done():
			break
		}
	}

	cancelCtx()

	time.Sleep(100 * time.Millisecond)

	fmt.Printf("doSomething: finished\n")
}

...
```
The `defer cancelCtx()` isn’t necessarily required because the other call will always be run, but it can be useful to keep it in case there are any `return` statements in the future that cause it to be missed.

## <font color="#366092">WithTimeout</font>
The `context.WithTimeout` function can almost be considered more of a helpful function around `context.WithDeadline`. With `context.WithDeadline` you provide a specific `time.Time` for the context to end, but by using the `context.WithTimeout` function you only need to provide a `time.Duration` value for how long you want the context to last.
```go
...

func doSomething(ctx context.Context) {
	ctx, cancelCtx := context.WithTimeout(ctx, 1500*time.Millisecond)
	defer cancelCtx()

	...
}

...
```


