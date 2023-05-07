#go #golang #generics
# Go:<font color="#31859b"> write Go programs by writing code, not by defining types</font>.
When it comes to <font color="#76923c">generics</font>, <u>if you start writing your program by defining type parameter constraints, you are probably on the wrong path</u>. **Start by writing functions. It’s easy to add type parameters later when it’s clear that they will be useful**.

When to use go ?
> 1. if he have repeated code with the difference only in data type
> 2. if the code you are writing is independent of data type you work with
> 3. in the functions not the methodes


`any` keyword is the same as the `{}interface`.

## <font color="#e36c09">Type Parameters</font>
Type parameters are **denoted using a capital letter** (i.e. `T`) **wrapped in square brackets** `[]` **immediately following the function or struct name** (i.e. `func f[T any](t T)`). The type parameter is then used in the <u>execution of the function, or instantiation of the struct</u>, to indicate the type of the parameter.

this an example, this works only for string keys:
```go
func MapKeys(m map[string]int) []string{
	var s []string
	for k := range m {
		s = append(s, k)
	} 

	return s
}
```
but this works for all:
```go
func MapKeys[K comparable, V any](m map[K]V) []K{
	var s []K
	for k := range m {
		s = append(s, k)
	} 

	return s
}
...
MapKeys[string, int](map[string]int{...})
```

## <font color="#ffc000">Type Constraints</font>
Constraints are used to create a <font color="#ffc000">union</font> (denoted by `|`) of allowed types.
```go
type MyConstraint interface {
  int | int8 | int16 | int32 | int64
}
```
One of the benefits of using constraints is they allow you to define a set of allowed types represented by the type parameter. <font color="#ffc000">Constraint enforcement helps reduce possible misuse</font>.

### <font color="#ffff00">comparable</font>
The new `comparable` keyword, in Go 1.18, was added for specifying types that can be compared with the `==` and `!=` operators.


comparable is an<u> interface that is implemented by all comparable types</u> (booleans, numbers, strings, pointers, channels, interfaces, arrays of comparable types, structs whose fields are all comparable types). The comparable interface may only be used as a type parameter constraint, not as the type of a variable.

### <font color="#ffff00">constraints with methods</font> 
```go
type MyConstraint interface {
  Integer | Float | ~string
  String() string
}
```
When creating a constraint, that adds a <u>method to the interface with</u> `builtin` types, ensure the constraint specifies any `builtin` using the `~` token. If the `~` is missing the constraint can _**never**_ be satisfied since Go `builtin` types do not have methods.





