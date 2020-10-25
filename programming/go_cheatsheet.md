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
  - [Tags](#tags)
- [Interfaces](#interfaces)
  - [Type Assertion](#type-assertion)
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
    - [go routines vs threads](#go-routines-vs-threads)
  - [wait groups](#wait-groups)
  - [channels](#channels)
- [Reflection](#reflection)
  - [Pitfalls of reflection](#pitfalls-of-reflection)
- [Testing](#testing)
- [Snippets](#snippets)
  - [Accepting input from cli](#accepting-input-from-cli)
  - [Regex and Pattern matching](#regex-and-pattern-matching)
  - [HTTP Programming](#http-programming)
  - [File Handling](#file-handling)
- [Commands](#commands)
- [Good To Know](#good-to-know)
- [References](#references)
- [Conclusion](#conclusion)

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

The biggest advantage you get from having and using an interface is that you can pass a
variable of a type that implements that particular interface to any function that expects a
parameter of that specific interface. 


Under the hood, an interface value can be thought of as a tuple consisting of a value and a concrete type

#### Type Assertion
Type assertions help you to do two things. 
1. Checking whether an interface value keeps a particular type. 
2. When used this way, a type assertion returns two values: the
underlying value and a bool value. The Boolean value tells you whether the type assertion was successful or not.
```go
	var myInt interface{} = 123
	k, ok := myInt.(int) // type assertion
	if ok {
		fmt.Println("Success:", k)
	}
	v, ok := myInt.(float64)
	if ok {
		fmt.Println(v) // will not come here
	} else {
		fmt.Println("Failed without panicking!")
	}
```

[_interface_demo.go_](https://play.golang.org/p/C28mR-CeQK_M)
```go
// Go Interface - `Shape`
type Shape interface {
	Area() float64
	Perimeter() float64
}

// Struct type `Rectangle` - implements the `Shape` interface by implementing all its methods.
type Rectangle struct {
	Length, Width float64
}

func (r Rectangle) Area() float64 {
	return r.Length * r.Width
}

func (r Rectangle) Perimeter() float64 {
	return 2 * (r.Length + r.Width)
}

// Struct type `Circle` - implements the `Shape` interface by implementing all its methods.
type Circle struct {
	Radius float64
}

func (c Circle) Area() float64 {
	return math.Pi * c.Radius * c.Radius
}

func (c Circle) Perimeter() float64 {
	return 2 * math.Pi * c.Radius
}

func (c Circle) Diameter() float64 {
	return 2 * c.Radius
}

// interfaces
func main() {

	var s Shape = Circle{5.0}
	Calculate(s)

	s = Rectangle{4.0, 6.0}
	Calculate(s)

}

func Calculate(x Shape) {
	// type assertion
	_, ok := x.(Circle)
	if ok {
		fmt.Println("Is a Circle!")
	}
	v, ok := x.(Rectangle)
	if ok {
		fmt.Println("Is a Rectangle:", v)
	}
	fmt.Println(x.Area())
	fmt.Println(x.Perimeter())
}
```

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
Go offers its own unique and innovative way of achieving concurrency, which comes in the
form of goroutines and channels.

Concurrency and Parallelism are **NOT** the same. Parallelism is the simultaneous execution of multiple entities of some kind, whereas concurrency is a way of structuring your components so that they can be executed independently when possible.

#### go routines
Goroutines are the smallest Go entities that can be executed on their own in a Go program.

##### go routines vs threads
A goroutine is the minimum Go entity that can be executed concurrently. Goroutines live in UNIX threads that live in UNIX processes.
Goroutines are lighter than threads, which, in turn, are lighter than processes.

[_goroutines_demo.go_](https://play.golang.org/p/r82JlnsHSLW) : The following code launches 10 goroutines.
```go
	count := 10
	// please do not assume that goroutines will be created in order.
	for i := 0; i < count; i++ {
		go func(x int) {
			fmt.Printf("%d ", x)
		}(i)
	}

	// this is used to wait for the goroutines to be finished before closing the program.
	// otherwise the program would have closed without all the goroutines completed.
	// better way to implement this is using a WaitGroup.
	time.Sleep(time.Second)
```

#### wait groups
A WaitGroup is used to wait for a collection of Goroutines to finish executing. The control is blocked until all Goroutines finish executing.

[_waitgroup_demo.go_](https://play.golang.org/p/lwdVcsiUAgi)

```go
func main() {
	no := 3
	var wg sync.WaitGroup
	for i := 0; i < no; i++ {
		wg.Add(1)
		go process(i, &wg)
	}
	wg.Wait()
	fmt.Println("All go routines finished executing")
}

func process(i int, wg *sync.WaitGroup) {
	defer wg.Done()
	fmt.Println("started Goroutine ", i)
	time.Sleep(2 * time.Second)
	fmt.Printf("Goroutine %d ended\n", i)
}
```

When the number of sync.Add() calls and sync.Done() calls are equal, everything will
be fine in your programs. 

However, when you call <code>sync.Add(1)</code> function n+1 times while sync.Done() only n times, this leads to <code>fatal error: all goroutines are asleep - deadlock!</code>

When you call <code>sync.Add(1)</code> function n times while you call sync.Done() n+1 times, this leads to <code>panic: sync: negative WaitGroup counter.</code>


#### channels
Channels can get data from goroutines in a concurrent and efficient way. 
It is a communication mechanism that allows goroutines to exchange data, among other things.

### Reflection
Reflection is an advanced Go feature that allows you to dynamically learn the type of an
arbitrary object, as well as information about its structure. Go offers the <code>reflect</code> package
for working with reflection.

The concrete type of interface{} is represented by <code>reflect.Type</code> and the underlying value is represented by <code>reflect.Value</code>. 
There are two functions <code>reflect.TypeOf()</code> and <code>reflect.ValueOf()</code> which return the <code>reflect.Type</code> and <code>reflect.Value</code> respectively.

The <code>NumField()</code> method returns the number of fields in a struct and the <code>Field(i int)</code> method returns the <code>reflect.Value</code> of the ith field.

[_reflection_demo.go_](https://play.golang.org/p/mTgTVVOb6R4) :
```go
type A struct {
	X int
	Y float64
	Z string
}

func main() {

	a := A{100, 200.12, "Struct a"}
	// A type, value, kind :  main.A {100 200.12 Struct a} struct
	fmt.Println("A type, value, kind : ", reflect.TypeOf(a), reflect.ValueOf(a), reflect.TypeOf(a).Kind())

        // real-life example
	PrintFieldsIfStruct(a)

}

// PrintFieldsIfStruct prints fields of a struct if the input kind is a struct
func PrintFieldsIfStruct(q interface{}) {
	if reflect.ValueOf(q).Kind() == reflect.Struct {
		v := reflect.ValueOf(q)
		// for a, will print : Number of fields 3
		fmt.Println("Number of fields", v.NumField())
		for i := 0; i < v.NumField(); i++ {
			f := v.Field(i)
			n := v.Type().Field(i).Name
			t := f.Type().String()
			fmt.Printf("Name: %s  Kind: %s  Type: %s\n", n, f.Kind(), t)
			/**
			    Name: X  Kind: int  Type: int
			    Name: Y  Kind: float64  Type: float64
			    Name: Z  Kind: string  Type: string
			 **/

		}
	}
}
```

#### Pitfalls of reflection
1. Extensive use of reflection will make your programs hard to read and maintain.
1. Reflection can make your program slower because it is a dynamic code.
1. Reflection errors cannot be caught at build time and are reported at
   runtime as panics, which means that reflection errors can potentially crash your programs.

### Testing

### Snippets

#### Accepting input from cli

#### Regex and Pattern matching

#### HTTP Programming

#### File Handling

### Commands
1. <code>go build hello_world.go</code> -> builds and generates executable file.
<code> ./hello_world </code> to run the executable file.
1. <code>go run hello_world.go</code> -> builds and runs the go file. (generates and deletes the executable file)
1. <code>go tool compile hello_world.go</code> -> compiles and generates the object file (.o extension)
1. <code>GODEBUG=gctrace=1 go run hello_world.go</code> -> print analytical data about the operation of the garbage collector.

### Good To Know
1. Go uses concurrent tricolor mark-and-sweep algorithm for garbage collection.
1. Unsafe code is Go code that bypasses the type safety and the memory security of Go. Usually, something which happens in
real time and uses pointers.
1. Go works using the **m:n scheduler** (or M:N scheduler). 
   It schedules goroutines, which are lighter than OS threads, using OS threads.

### References
1. [A Tour of Go](https://tour.golang.org/welcome/1) is a good starting point.
1. [Go Documentation](https://golang.org/ref/spec), [Effective Go](https://golang.org/doc/effective_go.html), [Go Blog](https://blog.golang.org/).
1. Mastering Go by Mihalis Tsoukalos is an excellent read.
1. golangbot.com has excellent reference blogs.
1. Use [Better Go Playground](https://chrome.google.com/webstore/detail/better-go-playground/odfhkelcmblecfdnboahphiafolojmpl?hl=en)
on go playground for more features.
1. Excellent talks : 
    1. [Go Workshop by Dave Cheney](https://www.youtube.com/watch?v=gi7t6Pl9rxE)
    1. [Go Best Practises by Brian Ketelsen](https://www.youtube.com/watch?v=MzTcsI6tn-0)

### Conclusion
Made with ♡ by [Kartavya Ramnani](https://github.com/kartavya-ramnani). 

If you found this useful, please ☆ the repository to encourage me to keep updating this.

You can find me on [Linkedin](https://www.linkedin.com/in/kartavya9/), [Twitter](https://twitter.com/KartavyaRamnani), [StackOverflow](https://stackoverflow.com/story/kartavya9). 