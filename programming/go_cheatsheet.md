## Go Cheatsheet
Go's mascot is the adorable gopher. [@egonelbre](https://github.com/egonelbre/gophers)

![](https://raw.githubusercontent.com/egonelbre/gophers/master/.thumb/vector/friends/heart-hug.png) 
![](https://raw.githubusercontent.com/egonelbre/gophers/master/.thumb/vector/fairy-tale/witch-learning.png)

# Table of contents

- [Hello World](#hello-world)
- [Variable Declaration and Assignment](#variable-declaration-and-assignment)
- [Basic Data types](#basic-data-types)
  - [Integer](#integer)
  - [Byte, Rune, Characters](#byte-rune-characters)
  - [Float](#float)
  - [Complex](#complex)
  - [Boolean](#boolean)
- [Arrays](#arrays)
- [Slices](#slices)
- [Maps](#maps)
- [Pointers](#pointers)
- [Times and Date](#times-and-date)
- [Structs](#structs)
- [Interfaces](#interfaces)
- [Flow Control](#flow-control)
  - [if condition](#if-condition)
  - [for loop and How to while](#for-loop-and-how-to-while)
  - [switch](#switch)
- [Functions](#functions)
  - [normal func](#normal-func)
  - [anonymous functions](#anonymous-functions)
  - [multiple return values](#multiple-return-values)
  - [function as return values and parameters](#function-as-return-values-and-parameters)
  - [variadic functions](#variadic-functions)
  - [scope](#scope)
  - [init() function](#init-function)
  - [type method](#type-method)
- [Errors, Panic and error handling](#errors-panic-and-error-handling)
- [Concurrency](#concurrency)
  - [go routines](#go-routines)
  - [wait groups](#wait-groups)
  - [channels](#channels)
- [Reflection](#reflection)
- [Testing](#testing)
- [Snippets](#snippets)
  - [Accepting input from cli](#accepting-input-from-cli)
  - [Regex and Pattern matching](#regex-and-pattern-matching)
  - [HTTP Programming](#http-programming)
  - [File Handling](#file-handling)
- [Commands](#commands)
- [Good To Know](#good-to-know)
- [References](#references)


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

[_time_date.go_](https://play.golang.org/p/V3qqkoqN5Ps)

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

### Structs
When you need to group various types of variables and create a
new handy type, you can use a structure. The various elements of a structure are called the
fields of the structure. Struct zero value is an initialized struct with fields of zero values.

[_structs_demo.go_](https://play.golang.org/p/H4ywnmO9VUV)

Defining Structs : 
```go
// BMI : Body mass index struct with json tags (talked about later)
type BMI struct {
	Person  string
	Height  int         // cms
	Weight  int         // kgs
	Contact Contact     // nested struct
}

// Contact : struct with contact information (but with no tags)
type Contact struct {
	Phone string
	Email string
}
```
Initializing and Accessing Struct:

```go
func main() {

	// literals
	person1 := BMI{Person: "John Doe", Height: 178, Weight: 80, Contact: Contact{Phone: "1234555", Email: "a@a.com"}}
	person2 := BMI{"Lorem Ipsum", 168, 75, Contact{}}

	// accessing and setting fields. Structs are mutable.
	person2.Weight = 77

	// ommitted fields are of zero value
	person3 := BMI{Person: "John Doe", Height: 178}
	fmt.Println("Weight of person3 :", person3.Weight) // 0 int zero value

	// pointer to struct
	person4 := &BMI{Person: "John Doe", Height: 178, Weight: 80, Contact: Contact{Phone: "1234555", Email: "a@a.com"}}
	fmt.Println("Person :", person4)
	// access values in pointers
	// Both ways are correct, the pointers are automatically dereferenced.
	fmt.Println((*person4).Contact.Phone, person4.Contact.Phone)
}
```

#### Tags
Go struct tags are annotations that appear after the type in a Go struct declaration.
A struct tag looks like this, with the tag offset with backtick <code>`</code> characters:
```go
// BMI : Body mass index struct with json tags (talked about later)
type BMI struct {
	Person  string  `json:"person"`
	Height  int     `json:"hgt"`         // cms
	Weight  int     `json:"wgt"`         // kgs
	Contact Contact `json:"contactInfo"` // nested struct
}

// Contact : struct with contact information (but with no tags)
type Contact struct {
	Phone string
	Email string
}

func main() {
	// We use json tags here for specifying the name of fields when struct is converted to json.
	// We can also use omitempty with json tag to omit zero-valued fields.
	out, err := json.MarshalIndent(person1, "", "  ")
	if err != nil {
		log.Println(err)
	}

	fmt.Println(string(out))
	/** Response :
		{
	      "person": "John Doe",
	      "hgt": 178,
	      "wgt": 80,
	      "contactInfo": {
	        "Phone": "1234555", // same because no json tag
	        "Email": "a@a.com" // same because no json tag
	      }
	    }
	    **/
}
```

### Interfaces
Go interface type defines the behavior of other types by specifying a set
of methods that need to be implemented.

Simply, interfaces are abstract types that define a set of functions that need to be
implemented so that a type can be considered an instance of the interface.



### Flow Control

#### if condition
[_if_demo.go_](https://play.golang.org/p/CFhTrRXBlJU) : There are two types of if loops here, 
1. One is your standard, run-of-the-mill conditional if
1. The second is an if loop with a statement. 

```go
	// boilerplate
	mapLiteral := map[string]int{
		"January":  1,
		"February": 2,
	}
	month := "February"
	monthNum, ok := mapLiteral[month]

	// Conditional if #1
	if ok {
		fmt.Println("Map contains : ", month)
	} else if monthNum != 1 {
		fmt.Println("Month is not January")
	} else {
		fmt.Println("Month Num : ", month, monthNum)
	}

	// if with statements #1
	if _, ok := mapLiteral[month]; ok {
		fmt.Println("Month exists in the map")
	}
	// if with statements #2
	if monthNum, ok = mapLiteral["January"]; monthNum == 1 {
		fmt.Println("Month is January")
	}
```

#### for loop and How to while 
[_for_loops.go_](https://play.golang.org/p/VUu-qx4LCEC) : For loop has three sections : Initialization, Condition, Afterthought (increment/decrement),
in Go, all three are optional.

```go
    // simple
    for i := 0; i < 3; i++ {
        fmt.Println("simple for : i is", i)
    }
    
    i := 0
    // while(true)
    for {
        fmt.Println("while : i is", i)
        i++
        if i > 5 {
            break
        }
    }
    
    i = 0
    // do...while(expression)
    for ok := true; ok; ok = i < 1 {
        fmt.Println("do-while : i is", i)
        i++
    }
```

#### switch
[_switch.go_](https://play.golang.org/p/0OFs5hWpKTz) : 
In go, switch cases do not need break statements because they do not fallthrough
For explicit fallthrough,  the *fallthrough* keyword tells Go to execute the branch that follows the current one.

```go
	asString := "1"

	switch asString {
	case "1":
		fmt.Println("One!")
		fallthrough // will call Zero! as well.
	case "0":
		fmt.Println("Zero!")
	default:
		fmt.Println("Do not care!")
	}
```

### Functions
[_func_demo.go_](https://play.golang.org/p/q88v7UuPIKM)
#### normal func

```go
func simpleAdd(i, j int) int {
	return i + j
}
func main() {
	k := simpleAdd(5, 7)
}
```

#### anonymous functions
Anonymous functions can be defined inline without the need for a name and they are
usually used for implementing things that require a small amount of code. They can also be called as *closures*.
```go
func main() {
	// anonymous function
	sqr := func(i int) int {
		return i * i
	}
}
```
#### multiple return values
```go
// multiple return values
func doubleSquare(i int) (int, int) {
	return i * i, i * 2
}

// named return values
func namedMinMax(x, y int) (min, max int) {
	if x > y {
		min = y
		max = x
	} else {
		min = x
		max = y
	}
	return
}

func main() {
	k, sqr := doubleSquare(3)
	min, max := namedMinMax(4,6)
}
```

#### function as return values and parameters

Function Returning functions :

```go
	// func returning func
	// i and j here are completely independent and they maintain their values
	i := funReturnFun()
	j := funReturnFun()
	fmt.Println("1:", i()) // 1: 1
	fmt.Println("2:", i()) // 2: 4
	fmt.Println("j1:", j())// j1: 1
	fmt.Println("j2:", j())// j2: 4 
	fmt.Println("3:", i()) // 3: 9 
```
Function as parameters : 
```go
// function as parameter
func funFun(f func(int) int, v int) int {
	return f(v)
}
// plain old function
func simpleSquare(i int) int {
	return i * i
}

func main() {
	k = funFun(simpleSquare, 5)
}

```
#### variadic functions
Go also supports variadic functions, which are functions that accept a variable number of
arguments.
```go
// variadic function
func varFunc(input ...string) {
    fmt.Println(input)
}

func main() {
	// calling variadic functions
	varFunc("Hello", "World!")
	varFunc([]string{"This", "Go Playground", "Is", "EPIC AF"}...)
}
```

#### scope
Go follows a simple rule that states that **functions**, **variables**, **types** and so forth that begin
with an *Uppercase* letter are public, whereas functions, variables, types, and so on that
begin with a lowercase letter are private which means they can  be strictly used and called internally in a package.

#### init() function
Every Go package can optionally have a private function named init() that is
automatically executed at the beginning of the execution time.
```go
func init() {
    fmt.Println("init is auto-executed at the beginning of execution time")
}
func main() {
	fmt.Println("main function gets called")
}
```
The console output will look like :
```
init is auto-executed at the beginning of execution time
main function gets called
```
#### type method
Methods defined on a type. A type method is a function with a special receiver argument.
The receiver appears in its own argument list between the func keyword and the method name.
In this example, the Abs method has a receiver of type Vertex named v.

```go
// type and type method
type Vertex struct {
	X, Y float64
}
// type method with receiver
func (v Vertex) Abs() float64 {
	return math.Sqrt(v.X*v.X + v.Y*v.Y)
}

func main() {
	// type method,calling method Abs of type Vertex
	v := Vertex{3, 4}
	fmt.Println(v.Abs())
}
```

### Errors, Panic and error handling

### Concurrency

#### go routines

#### wait groups

#### channels

### Reflection

### Testing

### Snippets

#### Accepting input from cli

#### Regex and Pattern matching

#### HTTP Programming

#### File Handling

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
1. Go works using the **m:n scheduler** (or M:N scheduler). 
   It schedules goroutines, which are lighter than OS threads, using OS threads.

### References
1. Use [Better Go Playground](https://chrome.google.com/webstore/detail/better-go-playground/odfhkelcmblecfdnboahphiafolojmpl?hl=en)
on go playground for more features.