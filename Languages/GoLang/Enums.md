#go #golang #idiomatic 
There are two way to define <font color="#76923c">enums</font> in golang:
1. **define the type our own**:
```go
type Season int64 

const (
	Summer Season = 0
	Autumn Season = 1
	Winter Season = 2
	Spring Season = 3
)
```
2. use `iota` (<font color="#ffff00">Recommended</font>)
```go
const (
	Summer Season = iota
	Autumn
	Winter
	Spring
)
```
Using `iota` instead of <u>manually assigning values also prevents you from inadvertently (without intention) assigning the same value to more than one constant</u>, which could otherwise <u>cause a conflict</u>. It also <u>carries over the type</u>, so we don’t have to declare `Season` every time.

### <font color="#92cddc">String</font>
define `String` method to the Enums
```go
func (s Season) String() string {
	switch s {
	case Summer:
		return "summer"
	case Autumn:
		return "autumn"
	case Winter:
		return "winter"
	case Spring:
		return "spring"
	}
	return "unknown"
}
```

### <font color="#92cddc">default</font>
In our previous examples, we defined our `Season` constants as integer values. **This means that `0` (which is Summer) would be the default value**.

If you want the **value to be set explicitly**, it’s better to add a custom “unknown” or “undefined”
```go
const (
	// since iota starts with 0, the first value
	// defined here will be the default
	Undefined Season = iota
	Summer
	Autumn
	Winter
	Spring
)

```

## <font color="#e36c09">Iota</font>
Used in go to represent number.
we can observe two properties of `iota`:
1.  <font color="#e36c09">The value of iota increments with each constant declaration</font>
2.  <font color="#e36c09">It starts from 0</font>

### Skipping Values
Remember, **iota values increment for every constant declaration**. So, if we want to skip a value, we can declare an empty constant value:
```go
const (
	SPADES = iota
	HEARTS
	_ // The value of iota will increase, since this is still a constant declaration
	DIAMONDS
	CLUBS
)
```


### When to Avoid Iota
Iota is best used when there **are arbitrary, or incremental values to be used as constants**.
In some cases, there<u> isn’t any sequential relationship between constants</u>. For example, time units like second, minute, hour and day <u>can be represented as non-sequential constants</u>:
```go
const (
	SECOND = 1
	MINUTE = 60 * SECOND
	HOUR   = 60 * MINUTE
	DAY    = 24 * HOUR
)
```
