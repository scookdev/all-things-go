# Go Programming Language Cheatsheet

## Table of Contents

- [Basics](#basics)
- [Variables and Types](#variables-and-types)
- [Control Structures](#control-structures)
- [Functions](#functions)
- [Packages and Imports](#packages-and-imports)
- [Data Structures](#data-structures)
- [Methods and Interfaces](#methods-and-interfaces)
- [Concurrency](#concurrency)
- [Error Handling](#error-handling)
- [File I/O](#file-io)
- [Testing](#testing)
- [Best Practices](#best-practices)

## Basics

### Hello World

```go
package main

import "fmt"

func main() {
    fmt.Println("Hello, World!")
}
```

### Running a Go Program

```bash
# Run directly
go run main.go

# Build an executable
go build main.go

# Install a Go package
go install github.com/user/package
```

## Variables and Types

### Variable Declaration

```go
// Explicit type declaration
var name string = "John"

// Type inference
var age = 30

// Short declaration (only inside functions)
city := "New York"

// Multiple variable declaration
var a, b, c int = 1, 2, 3
x, y := 10, "hello"

// Constants
const Pi = 3.14159
```

### Basic Types

```go
// Boolean
var isTrue bool = true

// Numeric types
var i int = 42          // Platform dependent (int32 or int64)
var i8 int8 = 127       // -128 to 127
var i16 int16 = 32767   // -32768 to 32767
var i32 int32 = 2147483647  // -2147483648 to 2147483647
var i64 int64 = 9223372036854775807

var ui uint = 42        // Platform dependent
var ui8 uint8 = 255     // 0 to 255
var ui16 uint16 = 65535 // 0 to 65535

// Floating point
var f32 float32 = 3.14
var f64 float64 = 3.141592653589793

// Complex numbers
var c64 complex64 = 5 + 6i
var c128 complex128 = 5 + 6i

// String
var str string = "Hello, Go!"

// Rune (alias for int32, represents Unicode code point)
var r rune = 'A'

// Byte (alias for uint8)
var b byte = 'a'
```

### Type Conversion

```go
var i int = 42
var f float64 = float64(i)
var u uint = uint(f)

// String conversion
s1 := strconv.Itoa(i)     // Int to string
i2, err := strconv.Atoi("42")  // String to int

f2, err := strconv.ParseFloat("3.14", 64)  // String to float64
b2, err := strconv.ParseBool("true")       // String to bool
```

## Control Structures

### If-Else

```go
if x > 10 {
    fmt.Println("x is greater than 10")
} else if x > 5 {
    fmt.Println("x is between 6 and 10")
} else {
    fmt.Println("x is 5 or less")
}

// If with short statement
if value := getValue(); value > 10 {
    fmt.Println("Value is greater than 10")
}
```

### Switch

```go
switch day {
case "Monday":
    fmt.Println("Start of the work week")
case "Friday":
    fmt.Println("End of the work week")
case "Saturday", "Sunday":
    fmt.Println("Weekend")
default:
    fmt.Println("Midweek")
}

// Switch with no expression (alternative to if-else chains)
switch {
case hour < 12:
    fmt.Println("Good morning")
case hour < 17:
    fmt.Println("Good afternoon")
default:
    fmt.Println("Good evening")
}

// Switch with fallthrough
switch num {
case 1:
    fmt.Println("One")
    fallthrough
case 2:
    fmt.Println("Less than three")
}
```

### Loops

```go
// Basic for loop
for i := 0; i < 10; i++ {
    fmt.Println(i)
}

// For loop as a while loop
i := 0
for i < 10 {
    fmt.Println(i)
    i++
}

// Infinite loop
for {
    // Will run until a break statement
    if condition {
        break
    }
}

// For with range (arrays, slices, maps, strings)
nums := []int{1, 2, 3, 4, 5}
for index, value := range nums {
    fmt.Printf("Index: %d, Value: %d\n", index, value)
}

// Skipping index or value with _
for _, value := range nums {
    fmt.Println(value)
}

// Range over map
ages := map[string]int{"Alice": 25, "Bob": 30}
for name, age := range ages {
    fmt.Printf("%s is %d years old\n", name, age)
}

// Range over string (iterates over Unicode code points)
for i, char := range "Hello" {
    fmt.Printf("%d: %c\n", i, char)
}
```

## Functions

### Basic Function

```go
func add(a int, b int) int {
    return a + b
}

// Multiple parameters of the same type
func add(a, b int) int {
    return a + b
}
```

### Multiple Return Values

```go
func divide(a, b float64) (float64, error) {
    if b == 0 {
        return 0, errors.New("division by zero")
    }
    return a / b, nil
}

// Usage
result, err := divide(10, 2)
if err != nil {
    // Handle error
}
```

### Named Return Values

```go
func split(sum int) (x, y int) {
    x = sum * 4 / 9
    y = sum - x
    return // Returns x and y automatically
}
```

### Variadic Functions

```go
func sum(nums ...int) int {
    total := 0
    for _, num := range nums {
        total += num
    }
    return total
}

// Usage
sum(1, 2, 3)
nums := []int{1, 2, 3, 4}
sum(nums...)  // Spreading a slice
```

### Function as Value

```go
// Assigning to a variable
add := func(a, b int) int {
    return a + b
}

// Using as a callback
func compute(fn func(int, int) int) int {
    return fn(3, 4)
}
```

### Closures

```go
func adder() func(int) int {
    sum := 0
    return func(x int) int {
        sum += x
        return sum
    }
}

// Usage
pos := adder()
fmt.Println(pos(1)) // 1
fmt.Println(pos(2)) // 3
```

### Defer

```go
func example() {
    defer fmt.Println("This runs after the function completes")
    fmt.Println("This runs first")
}

// Multiple defers run in LIFO order
func example2() {
    defer fmt.Println("This runs third")
    defer fmt.Println("This runs second")
    fmt.Println("This runs first")
}

// Common use for file handling
func readFile(filename string) error {
    file, err := os.Open(filename)
    if err != nil {
        return err
    }
    defer file.Close() // Will run when function returns

    // Read from file
    return nil
}
```

## Packages and Imports

### Basic Structure

```go
// File: main.go
package main

// Multiple imports
import (
    "fmt"
    "math"
    "strings"
)

// Single import
import "time"

func main() {
    // Use imported packages
    fmt.Println(math.Pi)
}
```

### Exported Names

```go
// Capitalized names are exported (public)
func ExportedFunction() {}

// Lowercase names are unexported (private to the package)
func unexportedFunction() {}
```

### Import Aliases

```go
import (
    "fmt"
    m "math"  // Alias for math
    . "strings"  // Imports into current namespace (not recommended)
    _ "database/sql"  // Import for side effects only (init functions)
)

func main() {
    fmt.Println(m.Pi)  // Using alias
    fmt.Println(ToUpper("hello"))  // Using direct import
}
```

### Creating Packages

```go
// File: math/calculator.go
package math

// Exported function
func Add(a, b int) int {
    return a + b
}

// Unexported function
func multiply(a, b int) int {
    return a * b
}

// Package initialization
func init() {
    // Runs when package is imported
}
```

## Data Structures

### Arrays

```go
// Fixed size
var arr [5]int // Initialized with zero values
arr2 := [5]int{1, 2, 3, 4, 5}
arr3 := [...]int{1, 2, 3} // Size determined by number of elements

// Accessing elements
fmt.Println(arr2[1]) // 2

// Length
fmt.Println(len(arr)) // 5
```

### Slices

```go
// Declaration
var slice []int // nil slice
slice2 := []int{1, 2, 3, 4, 5}

// From array
arr := [5]int{1, 2, 3, 4, 5}
slice3 := arr[1:4] // [2, 3, 4]

// Make function
slice4 := make([]int, 5)      // len=5, cap=5
slice5 := make([]int, 3, 5)   // len=3, cap=5

// Append
slice6 := append(slice5, 4, 5, 6) // Automatically grows if needed

// Copy
slice7 := make([]int, len(slice2))
copy(slice7, slice2)

// Length and capacity
fmt.Println(len(slice6)) // Length
fmt.Println(cap(slice6)) // Capacity

// Slice tricks
// Remove from middle
slice = append(slice[:i], slice[i+1:]...)

// Delete without preserving order
slice[i] = slice[len(slice)-1]
slice = slice[:len(slice)-1]
```

### Maps

```go
// Declaration
var m map[string]int // nil map
m2 := map[string]int{"foo": 1, "bar": 2}
m3 := make(map[string]int) // Empty map

// Set values
m3["key"] = 42

// Get value
value := m2["foo"]
value2, exists := m2["baz"] // Check if key exists

// Delete key
delete(m2, "foo")

// Length
fmt.Println(len(m2))

// Iterate
for key, value := range m2 {
    fmt.Printf("%s: %d\n", key, value)
}
```

### Structs

```go
// Define a struct
type Person struct {
    Name    string
    Age     int
    Address Address
}

type Address struct {
    City  string
    State string
}

// Creating structs
var p1 Person // Zero value initialization
p2 := Person{Name: "Alice", Age: 30}
p3 := Person{"Bob", 25, Address{"New York", "NY"}} // Order matters

// Anonymous structs
point := struct {
    X, Y int
}{10, 20}

// Accessing fields
fmt.Println(p2.Name)
fmt.Println(p2.Address.City)

// Pointer to struct
p4 := &Person{Name: "Charlie", Age: 35}
fmt.Println(p4.Name) // Automatic dereferencing
```

## Methods and Interfaces

### Methods

```go
type Rectangle struct {
    Width, Height float64
}

// Method with value receiver
func (r Rectangle) Area() float64 {
    return r.Width * r.Height
}

// Method with pointer receiver (can modify the receiver)
func (r *Rectangle) Scale(factor float64) {
    r.Width *= factor
    r.Height *= factor
}

// Usage
r := Rectangle{Width: 10, Height: 5}
fmt.Println(r.Area())
r.Scale(2)
fmt.Println(r.Width) // 20
```

### Interfaces

```go
// Define an interface
type Shape interface {
    Area() float64
    Perimeter() float64
}

// Implement the interface
type Rectangle struct {
    Width, Height float64
}

func (r Rectangle) Area() float64 {
    return r.Width * r.Height
}

func (r Rectangle) Perimeter() float64 {
    return 2 * (r.Width + r.Height)
}

type Circle struct {
    Radius float64
}

func (c Circle) Area() float64 {
    return math.Pi * c.Radius * c.Radius
}

func (c Circle) Perimeter() float64 {
    return 2 * math.Pi * c.Radius
}

// Function using the interface
func PrintShapeInfo(s Shape) {
    fmt.Printf("Area: %f, Perimeter: %f\n", s.Area(), s.Perimeter())
}

// Usage
r := Rectangle{Width: 10, Height: 5}
c := Circle{Radius: 7}
PrintShapeInfo(r)
PrintShapeInfo(c)
```

### Empty Interface

```go
// Can hold any type
var i interface{}
i = 42
i = "hello"
i = struct{ name string }{"John"}

// Type assertions
str, ok := i.(string)
if !ok {
    fmt.Println("Not a string")
}

// Type switches
switch v := i.(type) {
case int:
    fmt.Printf("Integer: %d\n", v)
case string:
    fmt.Printf("String: %s\n", v)
default:
    fmt.Printf("Unknown type: %T\n", v)
}
```

## Concurrency

### Goroutines

```go
// Start a goroutine
go func() {
    // This runs concurrently
    fmt.Println("Hello from goroutine")
}()

// Wait for completion (simple example)
time.Sleep(100 * time.Millisecond)
```

### Channels

```go
// Unbuffered channel
ch := make(chan int)

// Send value (blocks until received)
go func() {
    ch <- 42
}()

// Receive value (blocks until sent)
value := <-ch

// Buffered channel
ch2 := make(chan int, 2)
ch2 <- 1  // Doesn't block
ch2 <- 2  // Doesn't block
// ch2 <- 3  // Would block until space available

// Closing channels
close(ch)

// Range over channel
go func() {
    for i := 0; i < 5; i++ {
        ch <- i
    }
    close(ch)
}()

for num := range ch {
    fmt.Println(num)
}

// Check if channel is closed
val, ok := <-ch
if !ok {
    fmt.Println("Channel closed")
}

// Select statement
select {
case val := <-ch1:
    fmt.Println("Received from ch1:", val)
case val := <-ch2:
    fmt.Println("Received from ch2:", val)
case ch3 <- 42:
    fmt.Println("Sent to ch3")
case <-time.After(1 * time.Second):
    fmt.Println("Timeout")
default:
    fmt.Println("No channel operations ready")
}
```

### WaitGroup

```go
import "sync"

func main() {
    var wg sync.WaitGroup

    // Add counters for each goroutine
    wg.Add(2)

    go func() {
        defer wg.Done() // Decrements counter
        fmt.Println("Goroutine 1")
    }()

    go func() {
        defer wg.Done() // Decrements counter
        fmt.Println("Goroutine 2")
    }()

    // Wait for all goroutines to finish
    wg.Wait()
    fmt.Println("All goroutines completed")
}
```

### Mutex

```go
import (
    "sync"
    "time"
)

func main() {
    var mutex sync.Mutex
    count := 0

    // Protect shared resource
    for i := 0; i < 10; i++ {
        go func() {
            mutex.Lock()
            defer mutex.Unlock()
            count++
        }()
    }

    time.Sleep(time.Second)
    fmt.Println(count)
}
```

### Once

```go
import "sync"

var once sync.Once

func initialize() {
    fmt.Println("Initialization done only once")
}

func main() {
    for i := 0; i < 10; i++ {
        once.Do(initialize) // Will only execute once
    }
}
```

## Error Handling

### Basic Error Handling

```go
import "errors"

// Return error
func divide(a, b float64) (float64, error) {
    if b == 0 {
        return 0, errors.New("division by zero")
    }
    return a / b, nil
}

// Check error
result, err := divide(10, 0)
if err != nil {
    fmt.Println("Error:", err)
    return
}
fmt.Println("Result:", result)
```

### Custom Errors

```go
// Defining custom error type
type MyError struct {
    Op  string
    Err error
}

func (e *MyError) Error() string {
    return fmt.Sprintf("%s failed: %v", e.Op, e.Err)
}

// Using custom error
func process() error {
    err := doSomething()
    if err != nil {
        return &MyError{
            Op:  "processing",
            Err: err,
        }
    }
    return nil
}
```

### Panic and Recover

```go
// Panic
func dangerous() {
    panic("something went terribly wrong")
}

// Recover with defer
func safeCaller() {
    defer func() {
        if r := recover(); r != nil {
            fmt.Println("Recovered from:", r)
        }
    }()

    dangerous()
    fmt.Println("This line won't execute")
}
```

## File I/O

### Reading Files

```go
// Read entire file
data, err := os.ReadFile("file.txt")
if err != nil {
    log.Fatal(err)
}
fmt.Println(string(data))

// Read with buffered reader
file, err := os.Open("file.txt")
if err != nil {
    log.Fatal(err)
}
defer file.Close()

scanner := bufio.NewScanner(file)
for scanner.Scan() {
    fmt.Println(scanner.Text())
}

if err := scanner.Err(); err != nil {
    log.Fatal(err)
}
```

### Writing Files

```go
// Write entire file
data := []byte("Hello, Go!")
err := os.WriteFile("output.txt", data, 0644)
if err != nil {
    log.Fatal(err)
}

// Write with buffered writer
file, err := os.Create("output.txt")
if err != nil {
    log.Fatal(err)
}
defer file.Close()

writer := bufio.NewWriter(file)
fmt.Fprintln(writer, "Hello, Go!")
writer.Flush()
```

### Directory Operations

```go
// Create directory
err := os.Mkdir("new_dir", 0755)
if err != nil {
    log.Fatal(err)
}

// Create directory with parents
err = os.MkdirAll("path/to/new_dir", 0755)
if err != nil {
    log.Fatal(err)
}

// List directory
files, err := os.ReadDir(".")
if err != nil {
    log.Fatal(err)
}

for _, file := range files {
    fmt.Println(file.Name(), file.IsDir())
}
```

## Testing

### Basic Test

```go
// File: math.go
package math

func Add(a, b int) int {
    return a + b
}

// File: math_test.go
package math

import "testing"

func TestAdd(t *testing.T) {
    got := Add(2, 3)
    want := 5

    if got != want {
        t.Errorf("Add(2, 3) = %d; want %d", got, want)
    }
}

// Run tests
// go test
// go test -v
// go test -cover
```

### Table-Driven Tests

```go
func TestAdd(t *testing.T) {
    tests := []struct {
        name     string
        a, b     int
        expected int
    }{
        {"positive", 2, 3, 5},
        {"negative", -2, -3, -5},
        {"mixed", 2, -3, -1},
    }

    for _, tc := range tests {
        t.Run(tc.name, func(t *testing.T) {
            got := Add(tc.a, tc.b)
            if got != tc.expected {
                t.Errorf("Add(%d, %d) = %d; want %d", tc.a, tc.b, got, tc.expected)
            }
        })
    }
}
```

### Benchmarks

```go
func BenchmarkAdd(b *testing.B) {
    for i := 0; i < b.N; i++ {
        Add(2, 3)
    }
}

// Run benchmarks
// go test -bench=.
```

### Test Main

```go
func TestMain(m *testing.M) {
    // Setup code
    fmt.Println("Starting tests")

    // Run tests
    code := m.Run()

    // Teardown code
    fmt.Println("Tests completed")

    // Exit with the code from the tests
    os.Exit(code)
}
```

## Best Practices

### Error Handling

```go
// Always check errors
file, err := os.Open("file.txt")
if err != nil {
    // Handle error
    return
}
defer file.Close()

// Wrap errors with context
import "fmt"

if err != nil {
    return fmt.Errorf("failed to process file: %w", err)
}
```

### Project Structure

```
myproject/
├── cmd/                  # Command applications
│   └── myapp/
│       └── main.go       # Main application entry point
├── internal/             # Private code
│   └── database/
│       └── db.go
├── pkg/                  # Public libraries
│   └── util/
│       └── helper.go
├── api/                  # API definitions
│   └── v1/
│       └── api.go
├── web/                  # Web assets
│   ├── templates/
│   └── static/
├── configs/              # Configuration files
│   └── config.yaml
├── test/                 # Additional test applications
├── scripts/              # Build and deployment scripts
├── vendor/               # Vendored dependencies
├── go.mod                # Module definition
└── go.sum                # Module checksums
```

### Go Modules

```bash
# Initialize a module
go mod init github.com/username/project

# Add dependencies
go get github.com/pkg/errors

# Update dependencies
go get -u

# Tidy dependencies
go mod tidy

# Vendor dependencies
go mod vendor
```

### Common Pitfalls

```go
// Loop variable capture in closures
for i := 0; i < 5; i++ {
    // Wrong: i is captured by reference
    go func() {
        fmt.Println(i) // May not print 0,1,2,3,4
    }()

    // Correct: create a copy of i
    go func(val int) {
        fmt.Println(val) // Will print 0,1,2,3,4
    }(i)
}

// Nil slice vs. empty slice
var s1 []int         // nil slice, len=0, cap=0
s2 := []int{}        // empty slice, len=0, cap=0
s3 := make([]int, 0) // empty slice, len=0, cap=0

fmt.Println(s1 == nil) // true
fmt.Println(s2 == nil) // false
```

### Context

```go
import "context"

// Create a context with cancellation
ctx, cancel := context.WithCancel(context.Background())
defer cancel() // Ensure resources are freed

// Create a context with timeout
ctx, cancel := context.WithTimeout(context.Background(), 2*time.Second)
defer cancel()

// Using context with channels
select {
case <-ctx.Done():
    fmt.Println("Operation cancelled:", ctx.Err())
case result := <-resultChan:
    fmt.Println("Result:", result)
}

// Passing context to functions
func processWithContext(ctx context.Context, data string) error {
    select {
    case <-ctx.Done():
        return ctx.Err()
    default:
        // Process data
        return nil
    }
}
```
