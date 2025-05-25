# Go Programming Language (Golang)

## Introduction

Go (or Golang) is a statically typed, compiled programming language designed at Google by Robert Griesemer, Rob Pike, and Ken Thompson. Go was designed to be efficient, readable, and safe, with features like garbage collection, structural typing, and CSP-style concurrency.

## Installation

### macOS

```bash
# Using Homebrew
brew install go
```

### Linux

```bash
# Download the latest Go release
wget https://golang.org/dl/go1.21.0.linux-amd64.tar.gz

# Extract the archive
sudo tar -C /usr/local -xzf go1.21.0.linux-amd64.tar.gz

# Add Go to PATH
echo 'export PATH=$PATH:/usr/local/go/bin' >> ~/.profile
source ~/.profile
```

### Windows

1. Download the Windows installer from the [official Go website](https://golang.org/dl/)
2. Run the installer and follow the prompts
3. Verify installation by running `go version` in Command Prompt

## Basic Syntax

### Hello World

```go
package main

import "fmt"

func main() {
    fmt.Println("Hello, World!")
}
```

### Variables

```go
var name string = "John"      // Explicit type
var age = 30                 // Type inferred
salary := 5000               // Short declaration (only inside functions)

// Multiple declarations
var (
    firstName, lastName string
    isEmployed         bool
    yearOfBirth        int
)
```

### Constants

```go
const Pi = 3.14
const (
    StatusOK       = 200
    StatusNotFound = 404
)
```

## Data Types

### Basic Types

- `bool`: true/false
- `string`: UTF-8 strings
- `int`, `int8`, `int16`, `int32`, `int64`: Integer numbers
- `uint`, `uint8`, `uint16`, `uint32`, `uint64`: Unsigned integers
- `float32`, `float64`: Floating point numbers
- `complex64`, `complex128`: Complex numbers
- `byte`: Alias for uint8
- `rune`: Alias for int32 (represents a Unicode code point)

### Composite Types

- Arrays: `var arr [5]int`
- Slices: `slice := []int{1, 2, 3}`
- Maps: `m := map[string]int{"foo": 1, "bar": 2}`
- Structs:
  ```go
  type Person struct {
      Name string
      Age  int
  }
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

// If with a short statement
if err := someFunction(); err != nil {
    // Handle error
}
```

### For Loops

```go
// Basic for loop
for i := 0; i < 10; i++ {
    fmt.Println(i)
}

// While-like loop
i := 0
for i < 10 {
    fmt.Println(i)
    i++
}

// Infinite loop
for {
    // Do something forever
    break  // Exit the loop
}

// Iterating over arrays, slices, maps, etc.
for index, value := range mySlice {
    fmt.Println(index, value)
}
```

### Switch

```go
switch os := runtime.GOOS; os {
case "darwin":
    fmt.Println("macOS")
case "linux":
    fmt.Println("Linux")
default:
    fmt.Printf("%s\n", os)
}
```

## Functions

### Basic Function

```go
func add(a int, b int) int {
    return a + b
}

// Multiple return values
func divide(a, b float64) (float64, error) {
    if b == 0 {
        return 0, errors.New("division by zero")
    }
    return a / b, nil
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
```

### Closures

```go
func counter() func() int {
    count := 0
    return func() int {
        count++
        return count
    }
}
```

## Error Handling

```go
// Error handling with if
f, err := os.Open("file.txt")
if err != nil {
    log.Fatal(err)
}

// Defer closing the file
defer f.Close()

// Multiple error handling
if _, err := io.Copy(dst, src); err != nil {
    return err
}
```

## Concurrency

### Goroutines

```go
// Start a goroutine
go func() {
    fmt.Println("Running in a goroutine")
}()
```

### Channels

```go
// Create a channel
ch := make(chan int)

// Send value
go func() {
    ch <- 42  // Send value to channel
}()

// Receive value
value := <-ch  // Receive value from channel

// Buffered channels
bufferedCh := make(chan string, 2)
```

### Select

```go
select {
case msg1 := <-ch1:
    fmt.Println("Received from ch1:", msg1)
case msg2 := <-ch2:
    fmt.Println("Received from ch2:", msg2)
case ch3 <- 42:
    fmt.Println("Sent to ch3")
case <-time.After(1 * time.Second):
    fmt.Println("Timeout")
default:
    fmt.Println("No communication")
}
```

## Packages and Imports

```go
// Single import
import "fmt"

// Multiple imports
import (
    "fmt"
    "os"
    "io"
    "log"
)

// Import with alias
import (
    "fmt"
    mrand "math/rand"
)
```

## Testing

```go
// In file named something_test.go
package main

import "testing"

func TestAdd(t *testing.T) {
    got := add(2, 3)
    want := 5
    if got != want {
        t.Errorf("add(2, 3) = %d; want %d", got, want)
    }
}
```

## Popular Go Libraries and Frameworks

### Web Frameworks

- Gin: Fast HTTP web framework
- Echo: High performance, minimalist web framework
- Beego: Full-featured MVC framework

### Database

- GORM: ORM library for Go
- sqlx: Extensions to database/sql
- pgx: PostgreSQL driver and toolkit

### API Development

- gRPC: High performance RPC framework
- Gorilla Mux: Powerful HTTP router and URL matcher
- Fiber: Express-inspired web framework

### Utilities

- Viper: Complete configuration solution
- Cobra: CLI application library
- zap/logrus: Structured logging packages

## Best Practices

1. **Error Handling**: Always check and handle errors
2. **Code Organization**: Follow standard project layout
3. **Documentation**: Write godoc for exported functions and types
4. **Testing**: Write tests for important functionality
5. **Interfaces**: Keep interfaces small and focused
6. **Concurrency**: Be careful with shared memory, prefer channels
7. **Naming**: Use camelCase for variable names, PascalCase for exported names

## Further Learning Resources

- [Go Tour](https://tour.golang.org/)
- [Effective Go](https://golang.org/doc/effective_go)
- [Go by Example](https://gobyexample.com/)
- [Go Documentation](https://golang.org/doc/)
- [Go Playground](https://play.golang.org/)
