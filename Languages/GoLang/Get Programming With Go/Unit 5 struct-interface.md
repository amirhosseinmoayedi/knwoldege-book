#go #golang #book_summery 
## lesson 21

How to decleare an **struct**:
```go
var curiosity struct {
	lat float64
	long float64
}
curiosity.lat = -4.5895 
curiosity.long = 137.4417 // setting and value in sruct
```

*Structures group related values together*, making it simpler and *less error-prone* to pass them around.

Defining an type for an struct:
```go
type location struct {
    lat float64
    long float64
}
var spirit location 
```

We can use short decleartion for types of structs:
```go
type location struct {
lat, long float64 // both have same type 
}
opportunity := location{lat: -1.9462, long: 354.4734} // assining value while making new struct 

spirit := location{-14.5684, 175.472636} // we can assing value witout key but it will be assigned in order 
```

You can modify the `%v` format verb with a *plus sign* `+` to print out the field names,

Struct are *<font color="#76923c">copied by value</font>*.

```go
bradbury := location{-4.5895, 137.4417}
curiosity := bradbury
curiosity.long += 0.0106
```

**We can create slices which contains struct**
```go
import "encoding/json"

bytes, err := json.Marshal(curiosity) // serialize

os.Exit(1)
```

Go’s json package requires that fields have an **initial uppercase letter** and **multiword field names use CamelCase** by convention.
You may want JSON keys in **snake_case**, particularly when interoperating with Python or Ruby.
The fields of a structure can be tagged with the field names you want the json package to use.

```go
type location struct {
	Lat float64 `json:"latitude"`
	Long float64 `json:"longitude"`
}
curiosity := location{-4.5895, 137.4417}
bytes, err := json.Marshal(curiosity)
```

Struct tags are ordinary strings associated with the fields of a structure.
Raw string literals \` are preferable, because quotation marks don’t need to be escaped with a backslash, as in the less readable `"json:\"latitude\"".`
<hr>
## lesson 22

> The Go language has *types*, *methods on types*, and *structures*.
	Together, these provide much of the functionality that classes do for other languages, without needing to introduce a new concept into the language.

Attaching method to an struct:
```go
type coordinate struct {
    d, m, s float64
    h rune
}

func (c coordinate) decimal() float64 {
    sign := 1.0
    switch c.h {
        case 'S', 'W', 's', 'w':
        sign = -1
    }
    return sign * (c.d + c.m/60 + c.s/3600)
}
```

Example of constructor in go:
```go
type location struct {
    lat, long float64
}
func newLocation(lat, long coordinate) location {
    return location{lat.decimal(), long.decimal()}
}
```

Go *doesn’t have* a language feature for **constructors**.
Functions in the form `newType` or `NewType` are *used to construct* a value of said type.

<u>function calls are prefixed with the name of the package they belong to</u>. This allows Go programs to easily differentiate between functions with the same name but defined in different packages. For example, if there is a function called "Print" in two packages "fmt" and "print", you would call the "fmt.Print" or "print.Print" function to make clear which one you are calling.
<hr>
## lesson 23

<font color="#31859b">Composition</font> describes a class that references one or more objects of other classes in instance variables.

Gophers use *composition with <font color="#76923c">structures</font>*, and Go provides a special language feature called *<font color="#e36c09">embedding</font> to forward methods*.

Composition in go:
```go
type celsius float64
type temperature struct {
    high, low celsius
}
type location struct {
    lat, long float64
}

type report struct {
    sol int
    temperature temperature
    location location
}
bradbury := location{-4.5895, 137.4417}
t := temperature{high: -1.0, low: -78.0}
report := report{sol: 15, temperature: t, location: bradbury}
```

if you create method for an type or struct it is *accessible from accessing the inner struct in other struct*.

you can also *expose the mehtod directly* to higher struct for this perpose create method for *higher struct and call the inner method*.

What isn’t so convenient is manually writing methods to forward from one type to another, Such **repetitive code, called boilerplate**, adds nothing but clutter.

<u>Go will do <font color="#ffff00">method forwarding</font> for you with <font color="#ffff00">struct embedding</font>.</u>

To embed a type in a structure, specify the type *without a field name*
```go
type report struct {
    sol int
    temperature // not `location location`
    location
}
// if temperature has method called averge()
report.average()
report.temperature.average()
```

Embedding *doesn’t only forward methods*. Fields of an inner structure are accessible from the outer structure.

You can embed **any type in a structure**, *not just structures*.

if two methods with same names in two embeded struct exists if you access them from the inner struct like : `report.location.days()` but  it's no problem.
The Go compiler is smart enough to only point out a name collision if it’s a problem, if you access them from the upper struct (composited or composition struct) it will cause an error. this called ***Ambiguous selector***.

Resolving an ambiguous selector error is straightforward. create method manually for the upper struct and call the method you want.
```go
func (r report) days(s2 sol) int {
    return r.sol.days(s2)
}
```

>Inheritance is a different way of thinking about designing software. With inheritance, a rover is a type of vehicle and thereby inherits the functionality that all vehicles share.
> With composition, a rover has an engine and wheels and various other parts that provide the functionality a rover needs. A truck may reuse several of those parts, but there is no vehicle type or hierarchy descending from it.
	Favor object composition over class inheritance.
	 —Gang of Four,

<hr>
## lesson 24

The Go standard library has an *interface* for **writing**. It goes by the name of `Writer`, and with it you can write text, images, comma-separated values (CSV), compressed archives,  the screen, a file on disk, or a response to a web request.
With the help of a single interface, Go can write any number of things to any number of places, <font color="#ffff00">Writer</font> is very flexible.

With interfaces, code can express abstract concepts such as a thing that
writes. <font color="#92cddc">Think of what something can do, rather than what it is</font>.

The majority of types focus on the **values they store**, The interface type is different.
Interfaces are concerned **with what a type can do**, not the value it holds.

```go
var t interface {
    talk() string
}
```

The variable `t` can hold any *value of any type* that satisfies the interface. More specifically, a *type will satisfy the interface if it declares a method* named talk that accepts no arguments and returns a string.

Computer scientists say that interfaces provide *<font color="#ffff00">polymorphism</font>*, which means “many shapes.”

Typically interfaces are declared as named types that can be reused. There’s a convention of naming interface types with an `-er` suffix: *a talker* is anything that talks.

we can create interface for every types, built in or not.

Go encourages composition over inheritance, using simple, <font color="#e36c09">often one-method interfaces</font> … , that serve as clean, comprehensible boundaries between components.

