#go #golang  #book_summery 

## lesson 12

In Go, the functions, variables, and other identifiers that begin with an *uppercase*
letter are **<font color="#ffff00">exported</font> and become available to other packages**.

This shortcut is optional, but it’s used elsewhere, such as the Contains function in the strings package, **which accepts two parameters of type *string***:
`func Contains(s, substr string) bool`

Println is said to be a *<font color="#ffff00">variadic</font>* function.

A<font color="#ffff00"> variadic function</font> in Go is a function that can accept a **variable number of arguments**. The type of the arguments is specified using an ellipsis (`...`) followed by the type of the arguments.
```go
package main

import "fmt"

func sum(numbers ...int) int {
	total := 0
	for _, number := range numbers {
		total += number
	}
	return total
}

func main() {
	result := sum(1, 2, 3, 4, 5)
	fmt.Println("The sum of the numbers is:", result)
}

```
> Note that a variadic function can only accept arguments of the same type. If you need to accept arguments of different types, you'll need to use different methods such as type switches or interface{} arguments.

> Passing args to golang func is by value or by copy

<hr>
## lesson 13

<font color="#ffff00">Types</font> in Go are not just **aliases**, they may have the **same primitive type values**, but they are <u>not the same type</u>, so you need to convert to that type if you want to perform operations.
```go
type kelvin float64
var degree kelvin = 13.5
var degree2 float64 = 13.5
degree == degree2 // error degree == degree2 (mismatched types kelvin and float64)
```

In Go, you can **attach** functions, called <font color="#e36c09">methods</font>,<u> to a user-defined type</u>. This means that you can define behavior for your custom type that is specific to that type. Methods are attached to a specific type and can only be called on values of that type.

On the other hand,<u> predefined types (such as `int`, `float64`, etc.) are part of the language and cannot have methods attached to them</u>. This means that you can't add custom behavior to these types. You can only use the methods that are already defined for these types, such as mathematical operations like addition, subtraction, etc.

Unlike a regular function, **a method is called on a specific instance of a type**, and the instance is passed as a parameter to the method, known as the <font color="#ffff00">receiver</font>.

**The receiver is specified before the method name**, separated by a `(type)` declaration, like this: `func (receiverType receiver) methodName()`. In this way, the method operates on the receiver value and has access to the fields and other values associated with the receiver.
```go
type person struct {
    name string
    age  int
}

func (p person) greet() string {
    return "Hello, my name is " + p.name
}

func main() {
    p := person{"John Doe", 30}
    fmt.Println(p.greet())
}

```
while methods **can have multiple parameters**, they<font color="#0070c0"> must have exactly one receiver</font>. This receiver is like an implicit parameter that is always passed to the method.
<hr>
## unit 14

In Go you can assign functions to variables, pass functions to functions, and even write functions that return functions. **Functions are first-class** *they work in all the places that integers, strings, and other types work*.

If our function *takes an function as argument* , we should decleare it like this:
`Name func() output_of_func`

```go
func measureTemperature(samples int, sensor func() kelvin) 
```

Decleare type of func:
```go
type sensor func() kelvin
```

An *anonymous function*, also called a **function literal** in Go, is a function without a name, Unlike regular functions, function literals **are closures because they keep references to variables in the surrounding scope**.

Decleare anonymous function:
```go
var f = func() {
fmt.Println("Dress up for the masquerade.")
}
```

Decleare and use function immediately:
```go
func() {
fmt.Println("Functions anonymous")
}()
```
