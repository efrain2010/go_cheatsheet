# Golang cheatsheet and study

## Table of Contents

- [Golang cheatsheet and study](#golang-cheatsheet-and-study)
  - [Table of Contents](#table-of-contents)
  - [Data types](#data-types)
  - [Constants](#constants)
  - [Variables and data types](#variables-and-data-types)
    - [Strings](#strings)
    - [Math operators](#math-operators)
      - [Importing parts of the Math package](#importing-parts-of-the-math-package)
    - [Date & Time](#date--time)
  - [Data collections](#data-collections)
    - [Arrays](#arrays)
      - [Array literal](#array-literal)
    - [Slices](#slices)
    - [Make & New functions](#make--new-functions)
      - [New fuction](#new-fuction)
      - [Make fuction](#make-fuction)
    - [Sorting](#sorting)
    - [Maps](#maps)
      - [Add](#add)
      - [Get](#get)
      - [Remove](#remove)
      - [Iterate](#iterate)
    - [Structs](#structs)
      - [Structs for data](#structs-for-data)
  - [Language organization](#language-organization)
    - [Logic operators](#logic-operators)
      - [If statement](#if-statement)
      - [Switch statement](#switch-statement)
      - [For loop](#for-loop)
      - [While loop](#while-loop)
      - [break and continue](#break-and-continue)
      - [Goto statement](#goto-statement)
    - [Functions](#functions)
      - [Functions with N numbers of parameters](#functions-with-n-numbers-of-parameters)
      - [Returning multiple values](#returning-multiple-values)
      - [naked return](#naked-return)
    - [Methods](#methods)
    - [Interfaces](#interfaces)
    - [Errors](#errors)
    - [Defered](#defered)
    - [Channels](#channels)
  - [Data management](#data-management)
    - [Pointers](#pointers)

## Data types

* Boolean types
  * bool
* String type
  * string
* Integer types
  * unit8 unit16 unit32 unit64
  * int8 int16 int32 int64
  * byte uint int uintptr
* Floating types
  * float32 float64
* Complex numbers
  * complex64 complex128

## Constants

Constants also have **explicit** and **implicit** typing, both need to have the reserved word **const** at the beginning of the declaration, the only difference is for the implicit type where the constant type (int | string | bool) is not required. For example:

Explicit typing:
```go
const anInteger int = 42
```

Implicit typing:
```go
const anInteger = 42
```

## Variables and data types

GO provides **exlpicit** and **implicit** typing. 

**Explicit** typing refers to declaring the type of the variable. To declare an explicit type, the reserved word **var** must be placed before the name of the variable, and the **type** of the variable after (int, string, boolean). For example:

```go
var aString string = "Efra"
var anIntegeer int = 42
var aBoolean bool = true
```

**Implicit** typing refer to when the variable types are not writen and Go infers them from the data stored. To declare an implicit variable a **colon** must be placed right before the equals symbol. For example:

```go
aString := "Efra"
anIntegeer := 42
aBoolean := true
```

**Note:** The colon before the equals symbol must be used ONLY when the variable is declared, if the variable is assign with other value later on, only the equals symbol will be needed.

### Strings

String is a data type are included in the go library, however, more complex functions for strings can be used by importing the "strings" package within the go file, complete documentation in the Go [String package documentation](https://golang.org/pkg/strings/)

Example:
```go
package main

import (
  "fmt"
  "string"
)

func main() {
  str1 := "Efra"
  str2 := "efra"

  fmt.Println(strings.ToUpper(str1))
  fmt.Println(strings.Title(str2))
  fmt.Println("Check, is it case sensitive?", strings.EqualFold(str1, str2))
  // Check, is it case sensitive? false
}
```

Go offers the function len to count the number of characters in a string

```go
str1 := "Efra"
fmt.Println("Length of String", len(str1))
// Length of String 4
```

### Math operators

Go provides the Math operators available in C and Java. The basic math operators are listed in the table bellow, for more complex operations, use the [math package](https://golang.org/pkg/math/).

| Operator | Name       | Operator  | Name        |
| -------- | ---------- | --------- | ----------- |
|    +     | Sum        |     &     | AND         |
|    -     | Difference |   &#124;  | OR          |
|    *     | Product    |     ^     | XOR         |
|    /     | Division   |     &^    | clear       |
|    %     | Mod        |     <<    | Left shift  |
|          |            |     >>    | Right shift |

<br/>

**Note:** Go is strict with when working with data types, therefore it don't implicitly convert data types.

<br/>

#### Importing parts of the Math package

<br/>

To use some part of the Math package we can import only that part to the code, for example:

```go
package main

import (
  "fmt"
  "math/big"
)

func main(a int, b int) (int) {
  var b1, b2, b3, bigSum big.Float
	b1.SetFloat64(23.53)
	b2.SetFloat64(56.12)
	b3.SetFloat64(76.31)

	bigSum.Add(&b1, &b2).Add(&bigSum, &b3)
  fmt.Printf("BigSum = %.10g\n", &bigSum)
  // BigSum = 155.96
}
```

**Note:** The ampersand "&" is used to declare a pointer, more on [Pointers](#pointers).

### Date & Time

GO allows to easly manipulate date and time using the [time package](https://golang.org/pkg/time/). The package allow you to set the date and time format, add or substract dates, and also get months, days, week days very very easy, for example:

```go
package main

import (
	"fmt"
	"time"
)

func main() {
	t := time.Date(2009, time.November, 10, 23, 0,0,0, time.UTC)	
  fmt.Printf("Go lanched at %s\n", t)
  // Go lanched at 2009-11-10 23:00:00 +0000 UTC
	
	now := time.Now()
  fmt.Printf("The time now is %s\n", now)
  // The time now is 2020-12-27 12:22:50.402278 +0000 GMT m=+0.000176536

  fmt.Println("The month is:", t.Month())
  // The month is: November
	fmt.Println("The day is:", t.Day())
  // The day is: 10
	fmt.Println("The week day is:", t.Weekday())
  // The week day is: Tuesday

	tomorrow := t.AddDate(0,0,1)
  fmt.Printf("Tomorrow is: %v, %v, %v, %v\n", tomorrow.Weekday(), tomorrow.Month(), tomorrow.Day(), tomorrow.Year())
  // Tomorrow is: Wednesday, November, 11, 2009

	longFormat := "Monday, January 2, 2006"
  fmt.Println("Tomorrow is:", tomorrow.Format(longFormat))
  // Tomorrow is: Wednesday, November 11, 2009
  
  shortFormat := "1/2/06"
	fmt.Println("Tomorrow is:", tomorrow.Format(shortFormat))
  // Tomorrow is: 11/11/09
}
```

## Data collections

### Arrays

Like in java Arrays help to store a list of values, in go an array can be declared implicitely of explicitely, however, all values in array must be of the same type, like in Java, and array must have a fixed length. To access values in arrays index is used, for example

```go
var array1 [3]string
array1[0] = "Red"
array1[1] = "Green"
array1[2] = "Blue"

fmt.Println(array1)
// [Red Green Blue]

fmt.Println(array1[1]) // <-- Printing the second value of the array
// Green
```

To obtain the length of the array the function "len" can be used, which is also used to count the number of characters in a string.

```go
var array1 [3]string
array1[0] = "Red"
array1[1] = "Green"
array1[2] = "Blue"

fmt.Println("Length of", len(array1))
// Length of 3
```

#### Array literal

We can use array literals to assign values to the array when it's first created, Go will infer the type of the values and all must be of the same type.

```go
var numbers = [5]{5, 3, 1, 2, 3}
fmt.Println(numbers)
// [5 3 1 2 3]

fmt.Println(numbers[2])
// 1
```

**Note:** An array is an object, is it is passed to  a function, a copy will be passed, rather than a reference. Can't add or remove items at run time, for those features slices should be used.

### Slices

It is recommended to use slices rather than arrays in Go. A slice works like an array, however, slices are dynamically sized, they can be declared with `var a []int{1, 2, 3}` or without `var a []int` a set of values.

To add new values the function **append** is used `a = append(a, 4)`, it creates a new slice and stores it in the variable, it is also possible to add a new set of values from another array or slice `a = append(otherSlice(1:4))`.

Additionally slices can be created using the [make](#make--new-functions) function which creates a slice with a given length and capacity, allocates the memory and initializes it, the make function accepts 3 parameter, which the first if the type, the second the length, and the third (optional) the capacity `s := make(int[], 3, 5)`.

### Make & New functions

These functions are used to create instances of maps, slices and channels. 

#### New fuction

Allocates but doesn't initialize memory

```Go
// Example to be assigned
```

#### Make fuction

Allocates and initializes memory.

```Go
m := make(map[string]int)
m["key"] = 42
fmt.Println(m)
// map[key:42]
```

### Sorting

Go offers an extense library for sorting, a slice can be order by using the built-in [order package](https://golang.org/pkg/sort/). For example:

```Go
numbers := make(int[], 5, 5)
numbers[0] = 134
numbers[1] = 22
numbers[2] = 5
numbers[3] = 4
numbers[4] = 51

fmt.Println(numbers)
// [134, 22, 5, 4, 51]

sort.Ints(numbers)
fmt.Println(numbers)
// [4, 5, 22, 51, 134]
```

### Maps

Unorder collection of key value pairs (hash table), the type of the key can be of any value, but it is common to use a string as a key and any other type as a value.

```Go
states := make(map[string]string)
```

#### Add

To add a value to the map a new key of the assigned type must be used and then store the value. Go will automaitically allocate memory for each new value.

```Go
states["WA"] = "Washington"
states["OR"] = "Oregon"
states["CA"] = "California"

fmt.Println(states)
// [WA:Washington OR:Oregon CA:California]
```

#### Get

Geting a value from a map is very similar to getting a value from a slice or array, the key of the value must be used.

```Go
california := states["CA"]
fot.Println(california);
// California
```

#### Remove

To delete a value from the map the function **delete** is used, the first parameter of the function of the map and the second the key which is going to be deleted.

```Go
delete(states, "OR")
// [WA:Washington CA:California]
```

#### Iterate

To iterate through a map the for statemate can be used with the function range, this returns the index and the value in each iteration.

```Go
for key, value range states {
  fmt.Println(key, value)
  // WA Washington 
  // CA California
}
```

### Structs

Data structures, they encapsulate data and methods, however it does not have inheritance model.

#### Structs for data

To defined a struct the reserved word **type** must be used followed by the name of the struct with a capital letter and at last the keyword **struct**, i.e. `type Dog struct`. To declare the data types, they must be placed in new lines, first the name of the field with capital letter, then the type `Breed string`. Creating an instance of a sctruc is very similar to creating an object, and that is because when creating a sintax a object of the struct is being defined `poodle := Dog{"Poodle", 21}`

```Go
type Dog struct {
  Breed string
  Weight int
}

func main() {
  poodle := Dog{"Poodle", 21}
  fmt.Println(poodle)
  // {Poodle, 21}
}
```


## Language organization

### Logic operators

#### If statement

Go like python does not need parenthesis for the if statement, however, else if and else need to be on the same line where the previous if closes.

```Go
x := 42
if x < 0 {
} else if x < 0 {
} else {
}
```

#### Switch statement

```Go
variable := "first"
s := ""

switch variable {
  case "first":
    s = "@ first"
  case "second":
    s = "@ second"
  case "third":
    s = "@ third"
}
fmt.Println(s)
// @ first
```

#### For loop

Go offers different way of creating for loops, which are listed bellow.

Regular for Loop

```Go
sum := 0
for i := 0; i < 10; i++ {
  sum += i
}
fmt.Prinln(sum)
// 45
```

For loop using slice length

```Go
colors := []string{"red", "blue", "yellow"}
for i := 0; i < len(colors); i++ {
  fmt.Println(colors[i])
  // red
  // blue
  // yellow
}
```

For loop using the range key word

```Go
colors := []string{"red", "blue", "yellow"}
for i := range colors {
  fmt.Println(colors[i])
  // red
  // blue
  // yellow
}
```

#### While loop

Go does not support traditional while loops, but we can create a for loop using the for loop. In the following example the for loop will be executed while sum is less than 1000.

```Go
sum := 1
for sum < 1000 {
  sum += sum
}
```

#### break and continue

Go allow programmers using break and continue statement the same way as they work in Java.

```Go
sum := 1
for sum < 1000 {
  sum += sum
  if sum > 500 {
    fmt.Println("Greater than 500")
    break;
  }
  if sum == 256 {
    fmt.Println("This will not be printed")
    continue;
  }
}

```

#### Goto statement

Like in C, Go allows you to write goto statements, which helps to jump over a specific line of code. To specify the line where the code will jump to the keywork need to be writen followed by a colon and the action which will be executed `endofprogram : fmt.Println("End of program")`, then to go to that line the keyword **goto** must be writen with the name of the goto line.

```Go
sum := 1
for sum < 1000 {
  sum += sum
  if sum > 500 {
    goto endofprogram
  }
  fmt.Println(sum)
}

endofprogram : fmt.Println("End of program")
```

### Functions

Declaring a function in Go is very similar to declaring a function in Python, simply use the reserved word **func** followed by the name of the function in lower case and parenthesis `func main()`.

To declare parameters in a function the name of the values must be writen followed by the type of the value `func sum (a int, b int)`. The value types to be return must be inside a parenthesis after the parameters `func sum (a int, b int) (int)`, example:

```go
func sum (a int, b int) (int) {
  return a + b
}
```

**Note:** Functions that are not being exported must be in lower case, if the function is going to be exported the first letter must be in Upper case.

**Note 2:** Go does not allow function overloading.

#### Functions with N numbers of parameters

To create a function with N number of parameters, the parameter should be of the same type, first the name of values which will be recieved, then three dots, then the type of the parameters.

```Go
func main() {
  result := addValues(23, 54)
  fmt.Println(result)
  // 77
  result := addValues(12, 54, 79)
  fmt.Println(result)
  // 145
}

func addValues(values ...int) int {
  sum := 0
  for i := range values {
    sum += values[i]
  }
  fmt.Println("%T\n", values)
  // []int
  return sum
}
```

#### Returning multiple values

To return multiple values from a function the types will be returned must be declared after the closing parenthesis of the parameters, and before the opening brackets `func example(name, lastname string) (string, int) {}`. 

#### naked return

The naked declaration allows return the values of the function by writing the names of the variables in the signature of the function, go will recognize the value asignations inside the function and return then all by default.

```Go
func example(name, lastname string) (fullname string, length int) {
  fullname := name + " " + lastname
  length := len(fullname)
  return
}
```

### Methods

In Go a method instead of being a function inside a class, a method is a function of a type. To declare a method, after creating a type with its values, a function miust be created and pass inside parenthesis the type to which is being attached and then the method name.

```Go
type Dog struct {
  Breed string
  Weight int
  Sound string
}

func (d Dog) Speak() {
  fmt.Println(d.Sound)
}

func (d Dog) SpeakThreeTimes() {
  d.Sound = fmt.Sprintf("%v! %v! %v!\n", d.Sound, d.Sound, d.Sound)
  fmt.Println(d.Sound)
}

func main() {
  poodle := Dog("Poodle", 37, "Woof")
  fmt.Println(poodle)
  poodle.Speak()
  // Woof
  poodle.Sound = "Arf"
  poodle.Speak()
  // Arf
  poodle.SpeakThreeTimes()
  // Arf! Arf! Arf!
}
```

### Interfaces

Go supports interfaces implicitly, this means that the methods will not use the keyword **implements** to actually implement an intereface, however, to implement an interface the **cast** syntax need to be used, this means that to each method needs to be casted to the interface so this interface can be implemented.

```Go
type Animal interface {
  Speak() string
}

type Dog struct {
}

func (d Dog) Speak() string {
  return "Woof!"
}

func main() {
  poodle := Animal(Dog{}) // Casting Doog to the interface Animal
  fmt.Println(poodle)
}
```

### Errors

Go does not support classic error handling syntax like try and catch, instead an error in Go is an instance of an interface, that defines a single method error.

```Go
import (
  "fmt"
  "os"
  "errors"
)

func main() {
  f, err = os.Open("filename.txt")
  if err == nil {
    fmt.Println(f)
  } else {
    fmt.Prinln(err.Error()) // Printing error message
  }

  myError := errors.New("My error string") // custom error
  fmt.Prinln(myError)

  attendance := map[string]bool {
    "Ann": true,
    "Mike": true}
  attended, ok := attendance["Mike"] // comma ok syntax for errors
  if ok {
    fmt.Println("Mike attended?", attended)
  } else {
    fmt.Println("No info for Mike")
  }
}
```

### Defered

helps to defer an execution of a line of code until everything else inside that function is finished, last in first on (LIFO). To use defers the keyword **defer** must be placed before the line of code.

```Go
func main() {
  defer fmt.Prinln("Close the file!")
  fmt.Println("Open the file!")

  defer fmt.Prinln("Statement 1")
  defer fmt.Prinln("Statement 2")
  defer fmt.Prinln("Statement 3")
  defer fmt.Prinln("Statement 4")
  fmt.Prinln("Undeferred statement")
}

// Open the file!
// Undeferred statement
// Statement 4
// Statement 3
// Statement 2
// Statement 1
// Close the file!
```

### Channels

## Data management

### Pointers

Pointers are variables that holds the address in memory of another value. A pointer can be declared with a specific type writing an asteristc to the left of the value type `var p *int`, then to link the pointer to a value the ampersand (&) symbol must be used, for example:

```go
var p *int

var v int = 42
p = &v
```

In this example first a int pointer is declared, then after declaring the variable "v" the ampersand (&) symbol is used to get access to the address of the value of v and set it to the pointer p.

To obtain and modify the value of the pointer the name of the pointer must be used with an asterisc (*) to the left of the name, for example:

```go
  var value1 float64 = 42.13
	pointer1 := &value1 // <-- Link the value address tot he pointer
	fmt.Println("Value 1: ", *pointer1) // <-- Print the pointer value
  // Value 1: 42.13
  
	*pointer1 = *pointer1 / 31 // <-- Dividing the pointer value by 31
  fmt.Println("Value 1: " , *pointer1)
  // Value 1:  1.3590322580645162 <-- pointer value

	fmt.Println("Value 1: " , value1)
  // Value 1:  1.3590322580645162 <-- variable value
```