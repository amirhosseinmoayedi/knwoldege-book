#go #golang #web #book_summery #test
#  <font color="#92d050">Testing</font>
Go offers a few testing-focused libraries in the standard library.
* The main `test` library is the testing package.
* the `net/http` httptest package.

If you have a `server.go` file, you should also have a `server_test.go` file that contains all the tests you want to run on the server.go file. The server_test.go file **must be in the same package** as the server.go file.

you create functions with the following form:
```go
func TestXxx(*testing.T) { … }
```
* * *
## <font color="#fac08f">Unit testing</font>

A good gauge of whether a part of a program is a **unit is if it can be tested independently**.
A unit typically takes in data and returns an output, and unit test cases correspondingly pass data into the unit and check the resultant output to see if they meet the expectations.

> although most of the time you focus on writing code that implements features and delivery functionality, it’s equally important that the code be testable.

sample test:
```go
package main

import (
    "testing"
)

func TestDecode(t *testing.T) {
    post, err := decode("post.json")
    if err != nil {
        t.Error(err)
    }
    if post.Id != 1 {
        t.Error("Wrong id, was expecting 1 but got", post.Id)
    }
    if post.Content != "Hello World!" { t.Error("Wrong content, was expecting 'Hello World!' but got", post.Content)
    }
}

func TestEncode(t *testing.T) {
    t.Skip("Skipping encoding for now")
}
```

The `testing.T` struct is one of the two main structs in the package, and it’s the **main struct that you’ll be using to call out any failures or errors**.

### <font color="#d99694">Fail Test </font>
The `testing.T` struct has a number of useful functions:
- **Log**—Similar to fmt.Println; records the text in the error log.
- **Logf**—Similar to fmt.Printf. It formats its arguments according to the given format and records the text in the error log.
- **Fail**—Marks the test function as having failed but allows the execution to continue.
- **FailNow**—Marks the test function as having failed and stops its execution.

![3b42e0b2ec726a0bcc4c949e35a53e90.png](../../../../_resources/3b42e0b2ec726a0bcc4c949e35a53e90.png)
the **Error** function is a **combination of calling the Log function**, **followed by the Fail function**. The **Fatal** function is a **combination of calling the Log function followed by the FailNow function**, and soo on.

### <font color="#00b050">Runnig</font> 
Running The Tests:
```bash
go test
go test –v -cover
```
if you want more information, you can use the **verbose** `(-v)` flag, and if you want to know the **coverage** of the test case against your code, you can give it the coverage `(-cover)` flag.

### <font color="#ffff00">Skip Test</font>
Go provides a **Skip** function in `testing.T` that allows you to **skip test** cases if you’re not ready to write them. The Skip function is also useful if you have test cases that run a very long time and you want to skip them if you only want to run a sanity check.

In addition to skipping tests, you can pass in a short `(-short)` flag to the go test command, and **using some conditional logic in the test case, you can skip the running of parts of a test**.

```go
func TestLongRunningTest(t *testing.T) {
    if testing.Short() {
        t.Skip("Skipping long-running test in short mode")
    }
    time.Sleep(10 * time.Second)
}
```

### <font color="#548dd4">parallel</font>
you can run unit test cases in **parallel** in order to speed up the tests.
```go
func TestParallel_1(t *testing.T) {
    t.Parallel()
    time.Sleep(1 * time.Second)
}
```
running in parallel:
```bash
go test –v –parallel 3
```

### <font color="#31859b">Benchmarking</font>
```go
func BenchmarkXxx(*testing.B) { … }
```

```go
package main

import (
    "testing"
)

func BenchmarkDecode(b *testing.B) {
    for i := 0; i < b.N; i++ { // run the function n time
        decode("post.json")
    }
}
```
we use pointer to the `testing.B`.

To run benchmark test cases, use the bench `(-bench)` flag when executing the go test command, To run all benchmarks files, just use the dot `(.)`.
```bash
go test -v -cover -short –bench .
```

**The number of times a benchmark runs is dictated by Go**. **This number can’t be specified directly by the user**, **although you can specify the amount of time it has to run in**, therefore limiting the number of loops run. **Go will run as many iterations as needed to get an accurate measurement**.

the test subcommand has a `–test.count` flag that lets you specify how many times to run each test and benchmark (**the default is one time**).

The `-run` flag indicates which functional tests to run.
* * *
## HTTP Testing
The `httptest` package provides facilities to **simulate a web server**, allowing you to use the client functions of the `net/http` package to send an HTTP request and capturing the HTTP response that’s returned.

Testing HTTP Request:
![da132078889e1091f11bf53f7d937f78.png](../../../../_resources/da132078889e1091f11bf53f7d937f78.png)

```go
package main

import (
    "encoding/json"
    "net/http"
    "net/http/httptest"
    "testing"
)
func TestHandleGet(t *testing.T) {
    mux := http.NewServeMux() // Creates a multiplexer to run test on
    mux.HandleFunc("/post/", handleRequest) // Attaches handler you want to test

    writer := httptest.NewRecorder() //Captures returned HTTP response
    request, _ := http.NewRequest("GET", "/post/1", nil) // Creates request to handler you want to test
    mux.ServeHTTP(writer, request) // Sends request to tested handler 

    if writer.Code != 200 { // Checks ResponseRecorder for results
        t.Errorf("Response code is %v", writer.Code)
    }
    var post Post
    json.Unmarshal(writer.Body.Bytes(), &post)
    if post.Id != 1 {
        t.Error("Cannot retrieve JSON post")
    }
}
```

Every test case runs independently and **starts its own web server for testing**.
use the `httptest.NewRecorder` function to create a `ResponseRecorder` struct. This struct will be used to store the response for inspection.

Go’s testing package provides a `TestMain` function that allows you to do whatever **setup** or **teardown** is necessary. A typical TestMain function looks something like this:

```go
func TestMain(m *testing.M) {
    setUp()
    code := m.Run()
    tearDown()
    os.Exit(code)
}
```

where **setUp** and **tearDown** are functions you can define to do setup and teardown for all your test case functions.
**Note that setUp and tearDown are run only once for all test cases**.
**The individual test case functions are called by calling the Run function on m**. **Calling the Run function returns an exit code**, which you can pass to the os.Exit function.

* * *
## Test doubles and [[Dependency Injection (DI)]]
One popular way of making the unit test cases more **independent** is using **test doubles**. Test doubles are **simulations of objects, structures, or functions** that are used during testing when it’s inconvenient to use the actual object, structure, or function.

**However, this approach requires design prior to coding the program. If you don’t have the idea of using test doubles in mind during design, you might not be able to do it at all**.

One of the ways you can design for test doubles is to use the **dependency injection design pattern**.

> Dependency injection is a software design pattern that allows you to decouple the dependencies between two or more layers of software.

This is done through passing a **dependency to the called object, structure, or function**. This dependency is used to perform the action instead of the object, structure, or function.
