#go #golang #book_summery 
## lesson 26 

A *pointer* is a variable that points to the **address of another variable**.

> In computer science, pointers are a form of *indirection*, and indirection can be a powerful tool.

**Many crashes and security vulnerabilities can be tied back to the misuse of pointers**, This gave rise to several languages that don’t expose pointers to programmers.

Go does have pointers, but with an <font color="#366092">emphasis on memory safety</font>

Pointers in Go adopt the *well-established syntax used by `C`*.
There are two symbols to be aware of, the `ampersand (&)` and the `asterisk (*)`, ***though the asterisk serves a dual purpose***,

The address operator, represented by an *ampersand*, **determines the address of a variable in memory**

```go
answer := 42
fmt.Println(&answer) // Prints 0x1040c108
```

The `address operator (&)` provides the memory address of a value.
The reverse operation is known as *<font color="#e36c09">dereferencing</font>*, which provides the value that a memory address refers to

```go
answer := 42
fmt.Println(&answer) // print memory address
address := &answer
fmt.Println(*address) // print values of memory address
```

How might the Go compiler know the difference between *dereferencing* and
*multiplication*?
*Multiplication* is an infix operator **requiring two values**, whereas *dereferencing* **prefixes a single variable**.

The *asterisk* in `*int` denotes that the *type is a <font color="#366092">pointer</font>*.
In this case, it can point to other variables of type `int`.

*An asterisk prefixing a type denotes a pointer type*, whereas *an asterisk prefixing a variable name is used to dereference the value that variable points to*.

```go
canada := "Canada"
var home *string // pointer type of string

fmt.Printf("home is a %T\n", home) // print *string
home = &canada // turn to pointer type

fmt.Println(*home) // defering , print Canada
```

**Pointer can point to any variable and value**.

```go
var administrator *string

scolese := "Christopher J. Scolese"
administrator = &scolese
fmt.Println(*administrator) // print Christopher J. Scolese 

bolden := "Charles F. Bolden"
administrator = &bolden
fmt.Println(*administrator) // print Charles F. Bolden 
```

We can change value from pointer or the value, effect is applied to both:
```go
bolden = "Charles Frank Bolden Jr."
*administrator = "Maj. Gen. Charles Frank Bolden Jr."
```

<u>Two pointer that points to a single memory address are equall</u>.

```go
value := "test"

pointer1 := &value 
pointer2 := pointer1

fmt.Println(pointer1==pointer2) // print true
```
assigning pointer to another value will **make copy of it**.

two variable with the same value are equall but their pointers are different.

Pointers are frequently used with structures, the timmy variable **holds a memory address pointing to a person structure**:
```go
type person struct {
    name, superpower string
    age int
}
timmy := &person{
    name: "Timothy",
    age: 10,
}

timmy.superpower = "flying"
fmt.Printf("%+v\n", timmy) // &{name:Timothy, superpower:flying ,age:10}
```

it <font color="#ffff00">isn’t necessary to dereference structures</font> to access their fields.

As with structures, composite literals for *arrays* can be prefixed with the address operator `(&)` to create a new pointer to an array. Arrays also provide *automatic dereferencing*

```go
superpowers := &[3]string{"flight", "invisibility", "super strength"}
fmt.Println(superpowers[0]) // print flight
```

Composite literals for *slices* and *maps* can also be prefixed with the address operator (&), **but there’s no automatic dereferencing**.

Pointers are used to enable mutation across function and method boundaries.

Function and method parameters are passed by value in Go.

passing paramateres by reference
```go
func birthday(p *person) {
    p.age++
}

...

rebecca := person{
    name: "Rebecca",
    superpower: "imagination",
    age: 14,
}
birthday(&rebecca)
...
```

Method *receivers* are similar to parameters(*function params*).

it does not matter if you create struct with `&` ,the method reciver must be with `*` to be passed by refrence.

<font color="#ffff00">Not every structure should be mutated</font>. The standard library provides a great example in the time package. The methods of the time.Time type never use a pointer, receiver, preferring to return a new time instead, as shown in the next listingو After all, tomorrow is a new day.

Go provides a handy feature called *interior pointers*, **used to determine the memory address of a field inside of a structure**.

you can take the memory address of any interior field of struct when the need arises.

Though slices tend to be preferred over arrays, using arrays can be appropriate when **there’s no need to <font color="#953734">change their length</font>**.

Not all mutations require explicit use of a pointer. Go uses pointers behind the scenes for **some of the built-in collections**.

maps aren’t copied when assigned or passed as arguments. **Maps
are pointers in disguise, so pointing to a map is redundant**.

A slice is represented internally as a structure with three elements:
1. a pointer to an array
2. the capacity of the slice
3. and the length

the internal pointer allows the underlying data to be mutated when a slice is passed directly to a function or method.

An explicit pointer to a slice is only useful when modifying the slice itself: the length, capacity, or starting offset.

```go
func reclassify(planets *[]string) {
    *planets = (*planets)[0:8] // change the slice lenght to 8
}
func main() {
	planets := []string{
	"Mercury", "Venus", "Earth", "Mars",
	"Jupiter", "Saturn", "Uranus", "Neptune",
	"Pluto",
	}
	reclassify(&planets)
	fmt.Println(planets)
}
```

Pointers can be useful, but they also add complexity. It can be more difficult to follow code when values could be changed from multiple places.
Use pointers when it makes sense, but don’t overuse them. Programming languages
that don’t expose pointers often use them behind the scenes, such as when composing a class of several objects. With Go you decide when to use pointers and when to not use them.
<hr>
## lesson 27

The word `nil` is a noun that means nothing or zero. In the Go programming language, **nil is a zero value**.

**A pointer with nowhere to point has the value nil**.
the `nil` identifier is the zero value for slices, maps, and interfaces too.

*Dereference* a `nil` pointer, and the program will **crash**

Avoiding *panic* is fairly straightforward. **It’s a matter of guarding against a nil pointer dereference with an if statement**.

Go will happily call methods even when the receiver has a nil value. A nil receiver behaves no differently than a nil parameter.
**This means methods can guard against nil values**.

```go
func (p *person) birthday() {
    if p == nil {
        return
    }
    p.age++
}
```

if you *access `nil` value field of an struct* , it will crash.

When a variable is declared as a function type, **its value is nil by default**

```go
var fn func(a, b int) int
fmt.Println(fn == nil)
```

A *slice* that’s declared without a composite literal or the make built-in will have a value of nil.

*An empty slice and a nil slice aren’t equivalent, but they can often be used interchangeably*.

As with slices, a *map* declared without a composite literal or the make built-in has a value of nil.

If a function only reads from a map or slice, it’s fine to pass the function nil instead of making an empty map.

When a variable is declared to be an interface type without an assignment, the zero value is nil.

**nil value that doesn’t compare as equal to nil**.

The `%#v` format verb is shorthand to see both type and value for *formating*

>Its not the best option to use nil when ever there is no value

----------
## lesson 28

multiple return values provide a simple and consistent mechanism to return errors to calling functions.
If a function can return an *error*, the convention is to use the last return value for errors.
The <font color="#e36c09">caller should check if an error occurred immediately after calling a function</font>.
**If no errors occurred, the error value will be nil**
```go
files, err := ioutil.ReadDir(".") 
if err != nil { 
    fmt.Println(err) 
    os.Exit(1) 
}
```

When an error occurs, **the other return values generally<font color="#953734"> shouldn’t</font> be relied on**.
They may be set to the zero values for their type, but some functions may return partial data or something else entirely.

One strategy to reduce error-handling code is to isolate an error-free subset of a program from the inherently error-prone code.

To ensure that that the file is closed correctly, you can make use of the `defer` keyword.
**Go ensures that all deferred actions take place before the containing function returns**.
In the following listing, every return statement that follows defer will result in the `f.Close()` method being called.

```go
func proverbs(name string) error {
    f, err := os.Create(name)
    if err != nil {
        return err
    }
    defer f.Close()
    _, err = fmt.Fprintln(f, "Errors are values.")
    if err != nil {
        return err // will call f.Close()
    }
    _, err = fmt.Fprintln(f, "Don’t just check errors, handle them gracefully.")
    return err // will call f.Close()
}
```

> Returning an error gives the caller a chance to decide how to handle the error.

You can *defer* <u>any function or method, and like multiple return values</u>, <u>defer isn’t specifically for error handling</u>.

Create error with custome message:
```go
errors.New("out of bounds")
```

Using a *type assertion*, you can convert an interface to the underlying concrete type:
```go
var g Grid
err := g.Set(10, 0, 15)
if err != nil {
    if errs, ok := err.(SudokuError); ok {
    fmt.Printf("%d error(s) occurred:\n", len(errs))
    
    for _, e := range errs {
        fmt.Printf("- %v\n", e)
    }
    }
}
```

Go doesn’t have *exceptions*, but it does have a similar mechanism called *panic*.
When a panic occurs, the program will crash, as is the case with unhandled exceptions in other languages.

>Though error values are generally preferable to panic, panic is often better than
`os.Exit` in that panic will run any *deferred* functions, whereas `os.Exit` does not.

To keep panic from crashing your program, Go provides a *recover* function,
Deferred functions are executed before a function returns, even in the case of panic.
**If a deferred function calls recover, the panic will stop, and the program will continue running**.

```go
package main

import "fmt"

func recoverFunc() {
    if r := recover(); r != nil {
        fmt.Println("Recovered from panic:", r)
    }
}

func divide(a, b int) int {
    defer recoverFunc()
    if b == 0 {
        panic("Cannot divide by zero!")
    }
    return a / b
}

func main() {
    result := divide(5, 2)
    fmt.Println("Result:", result)

    result = divide(5, 0)
    fmt.Println("Result:", result)
}
```
