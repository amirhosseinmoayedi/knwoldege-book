#go #golang 
In Go, the predefined `init()` function sets off a piece of code to **run before any other part of your package**. This code will execute as soon as the <font color="#e36c09">package is imported</font>, and can be used when you need your application to initialize in a specific state, such as when you have a specific configuration or set of resources with which your application needs to start.

Unlike the `main()` function that can only be declared once, the `init()` function can be declared **multiple times throughout a package**. However, multiple `init()`s can <u>make it difficult to know which one has priority over the others</u>. 
In most cases, `init()` functions will execute in the order that you encounter them. Let’s take the following code as an example:
```go
package main

import "fmt"

func init() {
	fmt.Println("First init")
}

func init() {
	fmt.Println("Second init")
}

func init() {
	fmt.Println("Third init")
}

func init() {
	fmt.Println("Fourth init")
}

func main() {}
```
Notice that each `init()` is run in the order in <font color="#31859b">which the compiler encounters it</font>. However, it may not always be so easy to determine the order in which the `init()` function will be called.

Per the Go language specification for <font color="#fac08f">Package Initialization</font>, when multiple files are encountered in a package, they are <u>processed alphabetically</u>.

## Using `init()` for <font color="#ffc000">Side Effects</font>
In Go, it is sometimes desirable to import a package **not for its content**, <u>but for the side effects that occur upon importing the package</u>.This often means that there is an `init()` statement in the imported code that executes before any of the other code, allowing for the developer to manipulate the state in which their program is starting. This technique is called _<font color="#00b0f0">importing for a side effect</font>_.
```go
...
import _ "image/png"
...
```
You may have noticed the blank identifier) (`_`) before `"image/png"`. This is needed because *Go does not allow you to import packages that are not used throughout the program*. By including the blank identifier, the value of the import itself is discarded, so that <u>only the side effect of the import comes through</u>. This means that, even though we never call the `image/png` package in our code, we can still import it for the side effect.