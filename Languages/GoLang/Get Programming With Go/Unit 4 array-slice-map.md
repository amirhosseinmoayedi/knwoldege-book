#golang #go #book_summery 
## unit 16

<font color="#e36c09">array</font> is **fixed lenght**, example of Array with 8 element in them: 
```go
 Var planets [8]string
```

Error happens at *<font color="#ffff00">complie time</font>* ,Panic happens at *<font color="#ffff00">runtime</font>*.

<u>Index at out of range is error at complie time</u>.

A **<font color="#e36c09">composite literal</font>** is a concise syntax to initialize any composite type **with the values you want**. Rather than declare an array and assign elements one by one:

```go
dwarfs := [5]string{"Ceres", "Pluto", "Haumea", "Makemake", "Eris"}
```

> With larger arrays, breaking the composite literal across multiple lines can be *more readable*.

as a convenience, you can ask the Go compiler to *count the number of elements in the composite literal* by specifying the *ellipsis `…`* instead of a number.

```go
planets := [...]string{
"Mercury",
"Venus",
"Earth",
"Mars",
"Jupiter",
"Saturn",
"Uranus",
"Neptune",
}
```

Iterate throw array:
```go
for i, dwarf := range dwarfs {
    fmt.Println(i, dwarf)
}
```

Assigning an array to a new variable or passing it to a function **makes a complete copy of its contents**.

it’s important to recognize that the **length of an array is part of its type**. The type `[8]string` and type `[5]string` are both collections of strings, but they’re two different types.

Array types are **one-dimensional**, but you can compose types to build **multi-dimensional** data structures.
An array of eight arrays of eight strings(2D):
```go
var board [8][8]string
```

----------

## unit 17

slicing an array produce an *<font color="#e36c09">slice</font>*.

Slice indices **may not be negative**.

The slicing syntax for arrays also works on *strings*

<u>Be aware that the indices indicate the number of bytes, <font color="#ffff00">not runes</font></u>:

```go
question := "¿Cómo estás?"
fmt.Println(question[:6]) // Prints '¿Cóm'
```

Many functions in Go operate *on slices rather than arrays*. If you need a slice that *reveals every element of the underlying array*, one option is to declare an array and then slice it with `[:]`

we can decleare slice directly, A slice of strings has the type `[]string`, with *no value between the brackets*. This differs from an array declaration, which always specifies a fixed length or ellipsis between the brackets.

```go
dwarfs := []string{"Ceres", "Pluto", "Haumea", "Makemake", "Eris"}
```

Slices are more versatile than arrays in other ways too. Slices have a length, but unlike arrays, *the length isn’t part of the type*.

----------

## lesson 18

The number of elements that are **visible through a slice determines its *length***. If a slice has an **underlying array that is larger, the slice may still have *capacity to grow***.

`len()` get the visible elements in slice, `cap()` get the capacity of the items.

if you append to an slice and it **does not have capacity** it would create **an new undelying array ,double size of the prevoious one** , copy the content of the previous one to the new one and then append to it.

*three-index* slicing to limit the capacity of the resulting slice.

<font color="#ffff00">three-index slicing</font> is used to extract a <u>sub-slice from an existing slice</u>. The syntax for three-index slicing in Go is `slice[start:end:capacity]`, where `start` is the starting index, `end` is the ending index, and `capacity` is the capacity of the new slice. **The three-index slicing is not commonly used in Go, and it is usually recommended to use two-index slicing `slice[start:end]` instead, as it is easier to understand and less prone to mistakes.** However, when you need to extract a sub-slice with a different capacity than the original slice, three-index slicing can be used.
```go
package main

import "fmt"

func main() {
	originalSlice := []int{1, 2, 3, 4, 5, 6, 7, 8, 9}

	// extract a sub-slice with a capacity of 4
	subSlice := originalSlice[2:5:4]
	fmt.Println("Original slice:", originalSlice)
	fmt.Println("Sub-slice:", subSlice)
	fmt.Println("Capacity of sub-slice:", cap(subSlice))
}

```

make slice of string preallocated with capacity of ten:
```go
dwarfs := make([]string, 0, 10)
```

decleare an variadic function:
```go
func terraform(prefix string, worlds ...string) []string{
    newWorlds := make([]string, len(worlds))
    for i := range worlds {
        newWorlds[i] = prefix + " " + worlds[i]
    }
    return newWorlds
}
```

The worlds parameter is a *slice of strings* that contains zero or more arguments passed to terraform function .

To pass a slice instead of multiple arguments, **expand** the slice with an *ellipsis*:
```go
planets := []string{"Venus", "Mars", "Jupiter"}
newPlanets := terraform("New", planets...)
fmt.Println(newPlanets)
```

----------
## lesson 19

```go
map[typeOfKey]typeOfValue
```

```go
//map composite literals
temperature := map[string]int{
"Earth": 15,
"Mars": -65,
}

temperature["Earth"] = 16 // accessing an element
```

If you access a key that doesn’t exist in the map, the result is the zero value for the type of map.

Go provides the ***comma, ok syntax***, which you can use to distinguish between the "Moon" not existing in the map versus being present in the map with a temperature of 0 C

```go
if moon, ok := temperature["Moon"]; ok {
    fmt.Printf("On average the moon is %vº C.\n", moon)
} else {
    fmt.Println("Where is the moon?")
}
```

Maps aren’t copied.

`delete()` built-in function removes an element from the map

If you pass a *map* to a function or method, it may alter the **contents of the map**. This behavior is similar to multiple *slices* that point to the same underlying array.

Maps are similar to slices in another way. Unless you initialize them with a composite literal, maps need to be allocated with the `make` built-in function

A map’s initial length will always be *zero* when using `make`

```go
temperature := make(map[float64]int, 8)
```

 Go doesn’t provide a set collection, some ways are around like too use map and set the value true.

