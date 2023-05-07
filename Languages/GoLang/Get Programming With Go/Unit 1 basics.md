#go #golang #book_summery
## lesson 3 

In Go, the only **<font color="#92d050">true</font>** value is **<font color="#92d050">true</font>** and the only **<font color="#ffc000">false</font>** value is **<font color="#ffc000">false</font>**.

In php and javascript, `"1" == 1` is true (*lax*), but `"1" === 1` is false (*strict*).
Go only has a single equality operator( `==` ), **which doesn’t allow direct comparison of text with numbers**, so:
```go
1 == "1" // error invalid operation: 1 == "1" (mismatched types untyped int and untyped string)
```

Like with most programming languages, Go uses **short-circuit** logic. <u>If the first condition is <font color="#31859b">true</font>, there’s no need to evaluate what follows the operator, so it is <font color="#d99694">ignored</font></u>.
```go
a := 2
if (a == 2){
	fmt.Println("hi")
} else if (a == 3){
	fmt.Println("bye")
}
```

`fallthrough` is a keyword, which is used to execute the body of the next `case`.
```go
package main

import "fmt"

func main() {
	switch num := 4; num {
	case 4:
		fmt.Println("Number is 4")
		fallthrough
	case 3:
		fmt.Println("Number is 3")
	default:
		fmt.Println("Number is not 4 or 3")
	}
}

```

*Falling through* happens by default in **C, Java, and JavaScript**, whereas Go takes a
safer approach, requiring an explicit `fallthrough` keyword.

<hr>
## lesson 4

the short declaration operator `:=` cannot be used at the <font color="#e36c09">package level</font>.
```go
package main

import "fmt"

DOG := "SS" // cause error 
func main() {
	name := "John Doe"
	age := 30
	fmt.Println("Name:", name)
	fmt.Println("Age:", age)
}
```
