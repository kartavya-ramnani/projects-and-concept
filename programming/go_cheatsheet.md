## Go Cheatsheet
![](https://raw.githubusercontent.com/egonelbre/gophers/10cc13c5e29555ec23f689dc985c157a8d4692ab/vector/fairy-tale/witch-learning.svg)

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
[variable_declaration_assignment.go](https://play.golang.org/p/_ZKmdBUxzZz)
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

### Data types
[data_types.go]()

[General operators work.](https://yourbasic.org/golang/operators/)
#### Integer
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
```go
        var myByte byte = 'a'
	var myRune rune = '♥'
	fmt.Printf("%c = %d and %c = %U\n", myByte, myByte, myRune, myRune)
        // output : a = 97 and ♥ = U+2665
```

#### Float
```go
	// floating point : 
	// float32, float64 (6 and 15 decimal digits precision resp.)
	float1 := 9715.635   // Type inferred as `float64`
	var float2 float32 = 10.4563
```

#### Complex
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
```go
	var isData bool = true
	isDataNot := false
```

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