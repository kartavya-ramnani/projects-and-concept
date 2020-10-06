## Go Cheatsheet
Go's mascot is the adorable gopher. [@egonelbre](https://github.com/egonelbre/gophers)

![](https://raw.githubusercontent.com/egonelbre/gophers/master/.thumb/vector/friends/heart-hug.png) 
![](https://raw.githubusercontent.com/egonelbre/gophers/master/.thumb/vector/fairy-tale/witch-learning.png)

### Hello World
File names typically use snake_case. 

[_hello_world.go_](https://play.golang.org/p/cXkEy9zNB9j)

```go
package main

// import is used to import dependencies.
// go does not allow unused imports (except for side-effects, discussed later)
import (
	"fmt" // fmt package used for formatted I/O operations.
)

// Hello World
func main() { // curly brace needs to be here. It can not be on the next line
	fmt.Println("Hello World !")
}
```

### Variable Declaration and Assignment
[_variable_declaration_assignment.go_](https://play.golang.org/p/_ZKmdBUxzZz)
```go
    var greet string
    greet = "Hello World!"
```
[Short Assignment Statement](https://play.golang.org/p/_ZKmdBUxzZz)
```go
    // short assignment statement
    greetShortAssignment := "Hello World!"
    
    // this short assignment also works
    greetHello, greetWorld := "Hello", "World!"
```

### Basic Data types
[_basic_data_types.go_](https://play.golang.org/p/_ldzJN9Ze1c)

[General operators work.](https://yourbasic.org/golang/operators/)
#### Integer
Zero value is 0
```go
    // signed int8 (-128 , 127),int16 (-2^15, 2^15 - 1) 
    // int32 (-2^31, 2^31 - 1), int64 (-2^63, 2^63 - 1), int (platform dependent)
    var num1 int8 = 125
    num5 := -100 // type inferred is int
    
    // unsigned uint8 (0, 255), uint16 (0, 2^16 - 1),
    // uint32 (0, 2^32 - 1), uint64 (0, 2^64 - 1), uint (platform dependent)
    var num3 uint = 100
```

#### Byte, Rune, Characters
Go does not have character type as such, and we use <code>byte, rune</code> for characters.
Zero value of byte and rune is 0.
```go
        var myByte byte = 'a'
	var myRune rune = '♥'
	fmt.Printf("%c = %d and %c = %U\n", myByte, myByte, myRune, myRune)
        // output : a = 97 and ♥ = U+2665
```

#### Float
Zero value is 0.
```go
	// floating point : 
	// float32, float64 (6 and 15 decimal digits precision resp.)
	float1 := 9715.635   // Type inferred as `float64`
	var float2 float32 = 10.4563
```

#### Complex
Zero value is 0 + 0i.
```go
	// complex64: real and imaginary float32
	// complex128 : real and imaginary float64
	complexNum := 5 + 7i  // Type inferred as `complex128`
	var complexNum2 complex128 = 10 + 14i
	complexNum3 := complexNum + complexNum2
	// use complex function to initialize a complex number with floats 
	overallComplex := complex(float1, float2)
```

#### Boolean
Zero value is false.
```go
	var isData bool = true
	isDataNot := false
```

### Arrays
[_arrays.go_](https://play.golang.org/p/Hoqv1RlwiOm)

The size of the array is stated before its type, which is defined before its elements.
Go arrays have many disadvantages :
1. Go arrays are not dynamic and size cannot be changed.
1. Passed as value in functions, which can be slow for a big array
1. Arrays are rarely used and slices are preferred over arrays.

```go
    // one dimensional array
    anArray := [4]int{1, 2, 4, -4}
    // length of array
    fmt.Println("length: ", len(anArray))

    // looping through an array (loops covered later)
    for i := 0; i < len(anArray); i++ {
        fmt.Println("element at position", i, "is", anArray[i])
    }
    // using range keyword
    for index, value := range(anArray) {
        fmt.Println("element at position", index, "is", anArray[i])
    }
```

### Slices

[_slices.go_](https://play.golang.org/p/ZxDP0kFcFE8)

Slices are dynamic, powerful and passed by reference. Slices are implemented using arrays internally. Zero value is nil.

Same looping like in arrays apply.
```go
	// slice literal
	aSlice := []int{1, 2, 3, 4, 5}

	// slice initialization using make(type, capacity) keyword
	bSlice := make([]int, 0) // []

	// appending to a slice
	bSlice = append(bSlice, 5) // [5]

	// reslicing using [:] notation, [1:3] -> index 1 is included while 3 is excluded
	bSlice = aSlice[1:3]

	// Beware, reslicing a slice does not create a copy but references the parent slice. 
	// any changes you make in the reslice will also show in the parent slice.

	// To Make Copies use copy() function :
	// there is a catch here : Copy returns the number of elements copied,
	// which will be the minimum of len(src) and len(dst).
	newSlice := make([]int, len(bSlice))
	copy(newSlice, bSlice)
```
### Maps

[_maps.go_](https://play.golang.org/p/JHCE2ln1j_B)

Go map is equivalent to the well-known hash table found in many other programming
languages. Zero value is nil.
```go
	// map literal
	mapLiteral := map[string]int{
		"January":  1,
		"February": 2,
	}
	// initializing a map with make()
	aMap := make(map[string]int)
	aMap["March"] = 3
	aMap["April"] = 4

	// fetching a map key, ok below is used to check if map contains the key.
	// use _ in place of monthNum when only need to check contains.
	monthNum, ok := aMap["March"]     // ok will be true, monthNum will be 3
	monthNum2, ok := aMap["December"] // ok will be false, monthNum will be zero value

	// deleting a key
	delete(aMap, "April")

	// iterating a map :
	for key, value := range mapLiteral {
		fmt.Println("key, value:", key, value)
	}
```

### Pointers

[_pointers.go_](https://play.golang.org/p/E06Cje-E8_5)

Go supports pointers, which are memory addresses that offer improved speed in exchange
for difficult-to-debug code and nasty bugs. 
Use <code>*</code> to get the value of a pointer, which is called dereferencing the pointer, and <code>&</code> to get the memory address of a non-pointer variable.

```go
    i := -10
    j := 25
    // pointer to i and j using & keyword
    pI := &i
    pJ := &j
    // dereference it to get the value
    fmt.Println("pointer values pI, pJ", *pI, *pJ)

    // use pointers to change values of fields in functions without return.
    getIncrement(pI)

//getIncrement : increments the value by 1 (covered func later)
func getIncrement(a *int) {
	*a = *a + 1
}
```
Why use pointers ?
1. Pointers allow you to share data, especially between Go functions.
1. Pointers can be extremely useful when you want to differentiate between a zero
   value and a value that is not set.

### Times and Date
Go doesn’t use yyyy-mm-dd layout to format a time. Instead, you format a special layout parameter the same way as the time or date should be formatted.
Please find formats [here](https://golang.org/pkg/time/#pkg-constants).

```go
    fmt.Println("Epoch time:", time.Now().Unix())
    t := time.Now()
    fmt.Println(t.Weekday(), t.Day(), t.Month(), t.Year())
    
    // sleep to get time difference
    time.Sleep(time.Second)
    t1 := time.Now()
    // use .Sub method to calculate time difference.
    fmt.Println("Time difference:", t1.Sub(t))
    
    // location time
    loc, _ := time.LoadLocation("Europe/Paris")
    londonTime := t.In(loc)
    
    // parse time Parse(layout, value string)
    parsedTime, _ := time.Parse("2006-01-02", "1999-12-31")
    // format time to any other layout Format(layout)
    fmt.Println(parsedTime.Format("January 2, 2006"))
```

### Flow Control


### Commands
1. <code>go build hello_world.go</code> -> builds and generates executable file.
<code> ./hello_world </code> to run the executable file.
1. <code>go run hello_world.go</code>code> -> builds and runs the go file. (generates and deletes the executable file)
1. <code>go tool compile hello_world.go</code> -> compiles and generates the object file (.o extension)
1. <code>GODEBUG=gctrace=1 go run hello_world.go</code> -> print analytical data about the operation of the garbage collector.

### Good To Know
1. Go uses concurrent tricolor mark-and-sweep algorithm for garbage collection.
1. Unsafe code is Go code that bypasses the type safety and the memory security of Go. Usually, something which happens in
real time and uses pointers.

### References
1. Use [Better Go Playground](https://chrome.google.com/webstore/detail/better-go-playground/odfhkelcmblecfdnboahphiafolojmpl?hl=en)
on go playground for more features.