#go #golang #web #book_summery 
## The *net/http* library is divided into two parts:
1.  <font color="#92d050">Client</font>: Client, Response, Header, Request, Cookie.
2.  <font color="#31859b">Server</font>: Server, ServeMux, Handler/HandleFunc, ResponseWriter, Header, Request, Cookie.
## Server
```go
server := http.Server{
    Addr: "127.0.0.1:8080",
    Handler: nil,
}
server.ListenAndServe()
```

serving <font color="#e36c09">HTTPS</font>:
```go
server.ListenAndServeTLS("cert.pem", "key.pem")
```
Starting up a server is easy, **but it doesn’t do anything**. If you access the server, you’ll get only a 404 [[HTTP]] response code.

anything that has this method signature is a <font color="#fac08f">handler</font>:
```go
ServeHTTP(http.ResponseWriter, *http.Request)
```

creating and handler that is method of and struct:
```go
package main

import (
	"fmt"
	"net/http"
)

type HelloHandler struct{}
func (h *HelloHandler) ServeHTTP (w http.ResponseWriter, r *http.Request) {
	fmt.Fprintf(w, "Hello!")
}

func main() {
	hello := HelloHandler{}
	server := http.Server{
		Addr: "127.0.0.1:8080",
	}
	http.Handle("/hello", &hello)
	server.ListenAndServe()
}
```

### <font color="#b2a2c7">cross-cutting</font>
*Logging*, along with a number of similar functions like *security* and *error handling*, is what’s commonly known as a **cross-cutting** concern.

These functions are common and<u> we want to avoid adding them everywhere</u>, which causes code **duplication and dependencies**.

A common way of cleanly separating **cross-cutting** concerns away from your other logic is ***chaining***.
Example(Like middlewares):
```go
...
http.HandleFunc("/hello", log(hello)) 
...
```

### <font color="#31859b">Multiplexer</font>
`ServeMux` is an HTTP request multiplexer. It accepts an HTTP request and redirects it to the correct handler according to the URL in the request.

For any registered URLs that don’t end with a slash `(/)`, ServeMux will try to match the exact URL pattern. If the URL ends with a slash (/), ServeMux will see if the requested URL starts with any registered URL.

One of the main complaints about ServeMux is that it doesn’t support variables in its pattern matching against the URL. 
can't handle well: `/thread/123`

### <font color="#ffc000">The Principle of Least Surprise</font>:
The Principle of Least Surprise, is a general principle in the design of all things (including software) that says that <font color="#ffc000">when designing, we should do the least surprising thing</font>. <font color="#ffc000">The results of doing something should be obvious, consistent, and predictable</font>.

### <font color="#ffff00">HTTP 2</font>
Go 1.6 includes HTTP/2 by default when you start up a server with HTTPS. 
