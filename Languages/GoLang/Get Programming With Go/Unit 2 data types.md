#go #golang #book_summery 

## lesson 6

`days := 365.2425` go will keep this as *<font color="#e36c09">float64</font>*

Go has two **floating-point** types.
1. `float64`, *double precision*, **8-bit memory usage**
2. `float32`, *single precision*, **4-bit memory usage**

> Functions in the *math* package operate on `float64` types, so prefer `float64` unless you have a good reason to do otherwise.

In string formatting, `%4.2f` means *<font color="#ffff00">%width.precision</font>* that **Width** specifies the **minimum number of characters** to display, **including the decimal point** and the digits before and after the decimal.

> To minimize **rounding errors**, we recommend that you perform *multiplication* before *division*. The result tends to be more accurate that way.

In Go, the code would <u>compare two floating-point numbers</u> by first calculating the **absolute difference between them** and then **checking if the difference is within a specific tolerance range**. This method of comparison is more reliable than a direct comparison of floating-point numbers, as floating-point arithmetic is subject to precision errors.
```go
package main

import (
	"math"
	"fmt"
)

func main() {
	a := 0.1
	b := 0.2

	// Calculate absolute difference
	diff := math.Abs(a - b)

	// Set a tolerance threshold
	tolerance := 0.000001

	// Compare difference against tolerance
	if diff > tolerance {
		fmt.Println("Numbers are different")
	} else {
		fmt.Println("Numbers are close enough to be considered equal")
	}
}
```


The <font color="#31859b">upper bound</font> of a floating-point number <font color="#31859b">is the largest number it can represent</font>. This number is limited by the number of bits used to store the number in the computer. It is<u> not infinite</u>, but it is usually much larger than the largest whole number that can be represented. The exact value of the upper bound depends on the type of floating-point number being used.
```go
package main

import (
	"fmt"
	"math"
)

func main() {
	fmt.Println("The maximum value of float32 is:", math.MaxFloat32)
	fmt.Println("The maximum value of float64 is:", math.MaxFloat64)
}

```

The upper bound for a floating-point error for a single operation is known as the
*machine epsilon*, which is 2^52 for float64 and 2^23 for float32. Unfortunately, floating-point errors accumulate(rise up - increase) rather quickly.

----------
## lesson 7

Five integer types are **signed**, meaning they can represent both **positive and negative** whole numbers. The most common integer type is a signed integer abbreviated `int` ,The other five integer types are *unsigned*, meaning they’re for **positive** numbers only.


In Go, a literal whole number can be represented in several ways, including decimal (base 10), hexadecimal (base 16), and binary (base 2).
```go
// Decimal literal
a := 42
// Hexadecimal literal 
b := 0x2A 
// Binary literal
c := 0b101010
```
When using<font color="#fac08f"> type inference</font>, Go will always pick the `int` type for a **literal whole number**.

An `8-bit unsigned` integer (`uint8`) has a range of **0–255**. Incrementing beyond **255** will <font color="#76923c">wrap back</font> to **0**. 
> wrap back is mechanisem for handling Overflow
```go
var a int8 = 3
a += 255 
```

----------
## lesson 8
`41.3e12` <font color="#e36c09">e12</font> means 12 zero, if you want to see the full number you should use *int64* as the variable type.

If a variable doesn’t have an **explicit type**, Go will infer `float64` for numbers contain-ing **exponents**.

The big package provides three types:
- `big.Int` is for big integers, when *18 quintillion* isn’t enough.
- `big.Float` is for arbitrary-precision floating-point numbers.
- `big.Rat` is for fractions like *⅓*.
> *big* types are also slower.

**<font color="#e36c09">Constants</font>** are different. Rather than infer a type, <u>constants can be *untyped*.</u>

<u>Calculations on <font color="#e36c09">constants</font> and literals are performed <font color="#e36c09">during compilation rather than while the program is running</font></u>. The Go compiler is written in Go. Under the hood, **untyped numeric constants are backed by the big package**
<hr>
## lesson 9

you can wrap text in *backticks* `'` instead of quotes `(")`, as shown in the following listing. *Backticks* indicate a **raw string literal**, raw string literals can *span multiple lines* of source code.
```go
rawString := `This is a raw string literal, and it will preserve the line breaks and all other special characters, including the backslash (\).`
```

Go provides *<font color="#ffff00">rune</font>*, which is an alias for the `int32` type.

A *<font color="#ffff00">byte</font>* is an alias for the `uint8` type

To display the **characters** rather than their **numeric values**, the `%c` format verb can be used Go provides a *character literal*. 

To represent a single character in Go, just enclose it in single quotes `'A'`. If no type is specified, Go will infer it as a `rune` type. Strings in Go are encoded using UTF-8.
<hr>
## lesson 10

The Go compiler rejects `"10" - 1` with a mismatched types error. In Go, you first need to convert `"10"` to an integer.

> Type conversions are **required** between *unsigned and signed integer types, and
between types of different sizes*.

To convert digits to a string, *each digit must be converted to a code point*, starting at 48 for the 0 character, through 57 for the 9 character. Thankfully, the **Itoa** function in the strconv (string conversion) package does this for you.
```go
package main

import (
	"fmt"
	"strconv"
)

func main() {
	digit := 7
	digitString := strconv.Itoa(digit)
	fmt.Printf("The digit %d as a string is: %s\n", digit, digitString)
}

```

The Go compiler will report an error if you attempt to convert a Boolean with
`string(false)`, `int(false),` or similar, and likewise for `bool(1)` or bool`("yes")`.
```go
bool(1) // error cannot convert 1 (untyped int constant) to type bool
string(false) // error cannot convert false (untyped bool constant) to type string
```
