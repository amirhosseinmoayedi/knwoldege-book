#notes #go #golang 

A statement can precede conditionals; **any variables declared in this statement are available in the current and all subsequent branches**.
```go
if num := 9; num < 0 {
    fmt.Println(num, "is negative")
} else if num < 10 {
    fmt.Println(num, "has 1 digit")
} else {
    fmt.Println(num, "has multiple digits")
}
```

> Note that you don’t need parentheses around conditions in Go, but that the braces are required.

`switch` statment to find the type:
```go
whatAmI := func(i interface{}) {  
   switch t := i.(type) {  
   case bool:  
      fmt.Println("I'm a bool")  
   case int:  
      fmt.Println("I'm an int")
    ...
   default:  
      fmt.Printf("Don't know type %T\n", t)  
   }  
}
```

Unlike arrays, **slices are typed only by the elements they contain** (not the number of elements).

When *importing* a package, you can refer only to its **exported names**. <u>Any "unexported" names are not accessible from outside the package</u>.

If an <font color="#ffff00">initializer</font> is present, the type can be **omitted**; the variable will take the type of the initializer.
```go
var c, python, java = true, false, "no!"
```

But when the right hand side contains an **untyped numeric constant**, the new variable may be an `int`, `float64`, or `complex128` depending on the *precision of the constant*:
```go
i := 42           // int
f := 3.142        // float64
g := 0.867 + 0.5i // complex128
```

Shifting:
```go
const (
	// Create a huge number by shifting a 1 bit left 100 places.
	// In other words, the binary number that is 1 followed by 100 zeroes.
	Big = 1 << 100
)
```

```go
for { // always true
}
for true {
}
```

<font color="#ffff00">Deferred function calls</font> are pushed onto a stack. When a function returns, its deferred calls are executed in last-in-first-out(<font color="#e36c09">LIFO</font>) order.


>You can declare a method on non-struct types, too.

### There are two reasons to use a pointer receiver:
1. The first is so that the method can <font color="#fac08f">modify the value that its receiver points to.</font>
2. The second is to <font color="#fac08f">avoid copying the value on each method call</font>. This can be more efficient if the receiver is a large struct, for example.

**empty interface**:
```go
interface{}
```
> An empty interface may hold values of any type.


The `error` type is a built-in interface similar to `fmt.Stringer`:
```go
type error interface {
    Error() string
}
```

The `io` package specifies the `io.Reader` interface, which represents the read end of a stream of data.
```go
func (T) Read(b []byte) (n int, err error)
```

## <font color="#ffff00">Stringers</font>
One of the most ubiquitous interfaces is `Stringer` defined by the `fmt` package.
```go
type Stringer interface {
    String() string
}
```
A `Stringer` is a type that can describe itself as a string. The `fmt` package (and many others) look for this interface to print values.

## <font color="#953734">Nil</font> interface values
A nil interface value holds<u> neither value nor concrete type</u>.
Calling a method on a nil interface is a run-time error because there is no type inside the interface tuple to indicate which _concrete_ method to call.
```go
var i I // I is an interface 
```

## Function <font color="#31859b">closures</font>
<u>A closure is a function value that references variables from outside its body.</u> The function may access and assign to the referenced variables; in this sense the function is "bound" to the variables.

## Named return Value
A `return` statement without arguments returns the named return values.
This is known as a "naked" return.
```go
func split(sum int) (x, y int) {
	x = sum * 4 / 9
	y = sum - x
	return // will return x, y 
}
```
> Naked return statements<u> should be used only in short functions</u>, as with the example shown here. They can harm readability in longer functions.

## The zero value is:
- `0` for numeric types,
- `false` for the boolean type, and
- `""` (the empty string) for strings.
- `nil` for pointer, slice, map.

> Goroutines run in the same address space, so access to shared memory must be synchronized.

Channels can be _buffered_:
```go
ch := make(chan int, 100)
```
<u>Sends to a buffered channel block only when the buffer is full.Receives block when the buffer is empty.</u>

The loop `for i := range c` receives values from the channel repeatedly until it is **closed**.
Channels aren't like files; <font color="#ffff00">you don't usually need to close them</font>. Closing is only necessary when the receiver must be told there are no more values coming, such as to terminate a `range` loop.


<font color="#e36c09">Closures</font> <u>can also be recursive</u>, but this requires the closure to be declared with a typed `var` <u>explicitly before it’s defined</u>:
```go
var fib func(n int) int

fib = func(n int) int {
        if n < 2 {
            return n
        }
        return fib(n-1) + fib(n-2)
    }
```


