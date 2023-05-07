#go #golang #concureny

##  <font color="#92cddc">Goroutine pools</font>
A goroutine pool is a <font color="#92cddc">group of pre-allocated goroutines</font> that are ready to <font color="#92cddc">execute tasks as soon as they become available</font>. Goroutine pools can be useful in situations <font color="#92cddc">where creating and tearing down goroutines is expensive</font>, or when you <font color="#92cddc">want to limit the number of concurrent tasks</font> that can be executed at once.

```go
type Worker struct {
    ID         int
    TaskQueue  chan Task
    QuitSignal chan bool
}

func (w *Worker) Start() {
    go func() {
        for {
            select {
            case task := <-w.TaskQueue:
                task.Execute(w.ID)
            case <-w.QuitSignal:
                return
            }
        }
    }()
}

type Task interface {
    Execute(workerID int)
}

type Pool struct {
    Workers    []*Worker
    TaskQueue  chan Task
    QuitSignal chan bool
}

func NewPool(numWorkers int) *Pool {
    workers := make([]*Worker, numWorkers)
    taskQueue := make(chan Task)
    quitSignal := make(chan bool)

    for i := 0; i < numWorkers; i++ {
        workers[i] = &Worker{
            ID:         i,
            TaskQueue:  taskQueue,
            QuitSignal: quitSignal,
        }
        workers[i].Start()
    }

    return &Pool{
        Workers:    workers,
        TaskQueue:  taskQueue,
        QuitSignal: quitSignal,
    }
}

func (p *Pool) Submit(task Task) {
    p.TaskQueue <- task
}

func (p *Pool) Shutdown() {
    close(p.QuitSignal)
}
```

## <font color="#c3d69b">Fan-out/fan-in</font>
is a pattern where a <font color="#c3d69b">group of goroutines</font> (the "<font color="#c3d69b">fan-out</font>" phase) <font color="#c3d69b">perform some work in parallel</font>, and then <font color="#c3d69b">pass their results to a single goroutine</font> (the "<font color="#c3d69b">fan-in</font>" phase) that <font color="#c3d69b">aggregates the results</font>.

```go
func worker(id int, jobs <-chan int, results chan<- int) {
    for j := range jobs {
        time.Sleep(time.Second)
        results <- j * 2
        fmt.Printf("Worker %d processed job %d\n", id, j)
    }
}

func main() {
    numJobs := 10
    jobs := make(chan int, numJobs)
    results := make(chan int, numJobs)

    // start 3 workers
    for i := 1; i <= 3; i++ {
        go worker(i, jobs, results)
    }

    // send 10 jobs
    for i := 1; i <= numJobs; i++ {
        jobs <- i
    }
    close(jobs)

    // collect results
    for i := 1; i <= numJobs; i++ {
        result := <-results
        fmt.Printf("Result %d: %d\n", i, result)
    }
}
```

## <font color="#d99694">Pipeline</font>
is a pattern where a<font color="#d99694"> series of goroutines are connected by channels</font>, and <font color="#d99694">each goroutine performs a specific task on the data that it receives from the previous goroutine</font>. This pattern can be useful<font color="#d99694"> for processing large amounts of data in a streaming fashion</font>.

```go
func gen(nums ...int) <-chan int {
    out := make(chan int)
    go func() {
        for _, n := range nums {
            out <- n
        }
        close(out)
    }()
    return out
}

func sq(in <-chan int) <-chan int {
    out := make(chan int)
    go func() {
        for n := range in {
            out <- n * n
        }
        close(out)
    }()
    return out
}

func main() {
    c := gen(2, 3)
    out := sq(c)

    for n := range out {
        fmt.Println(n)
    }
}
```

## <font color="#ffc000">Context</font>
The context package provides a way to<font color="#ffc000"> propagate cancellation signals</font> and other request-scoped values across API boundaries and between processes and goroutines. This pattern can be useful for <font color="#ffc000">managing resources and timeouts in concurrent programs</font>.
```go
func process(ctx context.Context) {
    for {
        select {
        case <-ctx.Done():
            fmt.Println("Context cancelled!")
            return
        default: // run agian and agian because of the for until cancel is called
            fmt.Println("Processing...")
            time.Sleep(1 * time.Second)
        }
    }
}

func main() {
    ctx := context.Background()
    ctx, cancel := context.WithTimeout(ctx, 5*time.Second)
    defer cancel()

    go process(ctx)

    select {
    case <-time.After(10 * time.Second):
        fmt.Println("Timeout!")
    }
}
```

## <font color="#b2a2c7">Semaphore</font>
A semaphore<font color="#b2a2c7"> is a synchronization primitive that can be used to limit the number of concurrent access to a shared resource</font>. In Go, the `sync` package provides a `Semaphore` type that can be used to implement semaphores.
```go
type SafeCounter struct {
    mu    sync.Mutex
    count int
}

func (c *SafeCounter) Inc() {
    c.mu.Lock()
    defer c.mu.Unlock()
    c.count++
}

func (c *SafeCounter) Value() int {
    c.mu.Lock()
    defer c.mu.Unlock()
    return c.count
}

func main() {
    var wg sync.WaitGroup
    counter := SafeCounter{}

    for i := 0; i < 1000; i++ {
        wg.Add(1)
        go func() {
            counter.Inc()
            wg.Done()
        }()
    }

    wg.Wait()
    fmt.Println(counter.Value())
}
```