# Week 1 of Learning Go: From First Principles to Core Syntax

## 📚 Why I'm Learning Go

As someone working in DevOps and automation, Go keeps showing up in tools I use daily (Terraform, Kubernetes, Docker). It’s fast, efficient, and increasingly the language of systems engineering. My goal? Learn it the right way: **from first principles**.

I dedicated this first week to mastering the **core syntax**, understanding memory handling, types, control structures, and common patterns that make Go elegant and powerful.

---

## 🔹 Packages and Imports

- Every Go program starts with a `` declaration.
- Imports can be **factored** like this:

```go
import (
  "fmt"
  "math/rand"
)
```

- Go enforces good style: exported identifiers begin with **uppercase letters** (e.g., `Pi` from `math`), while lowercase identifiers are unexported.

---

## 🔹 Functions and Return Values

### Function with multiple arguments:

```go
func add(x, y, c int) int {
  return x + y + c
}
```

### Function with multiple return values:

```go
func swap(x, y, z string) (string, string, string) {
  return x, y, z
}
```

### Named return values (naked return):

```go
func split(sum int) (x, y int) {
  x = sum * 4 / 9
  y = sum - x
  return
}
```

---

## 🔹 Variables and Constants

- Variables can be declared with `var`, or using short declarations `:=` inside functions.
- Uninitialized variables get **zero values**.

```go
var i int
j := 3.2
```

### Constants:

```go
const Pi = 3.14
const Truth = true
```

### Big/Small Constants:

```go
const (
  Big = 1 << 100
  Small = Big >> 99
)
```

---

## 🔹 Type Inference and Basic Types

- Go supports strong **type inference**:

```go
v := 42 // inferred as int
```

- Basic types include: `int`, `float64`, `bool`, `string`, `complex128`, `rune`, and `byte`

```go
var (
  ToBe bool = false
  z complex128 = cmplx.Sqrt(-5 + 12i)
  s string = "hello"
)
```

---

## 🔹 Control Structures: for, if, defer

### For loop:

```go
for i := 0; i < 10; i++ {
  fmt.Println(i)
}
```

### While-like for loop:

```go
for sum < 1000 {
  sum += sum
}
```

### If with short statement:

```go
if v := math.Pow(2, 3); v < 10 {
  return v
}
```

### Defer:

```go
defer fmt.Println("world")
fmt.Println("hello")
```

---

## 🔹 Pointers

```go
p := &i   // assign pointer
*p = 21   // dereference to change value
```

In Go, a pointer is a variable that **stores the memory address of another variable**. You create a pointer using the `&` (address-of) operator, and access or modify the value it points to using the `*` (dereference) operator.

- `p := &i` means pointer `p` now holds the address of variable `i`.
- `*p = 21` modifies the value stored at that address, effectively updating `i`.

Go supports safe and simple pointer usage without pointer arithmetic (like in C), making memory management more controlled while still powerful.

Pointers are especially useful when:

- Passing large structs or data to functions without copying
- Modifying values inside functions
- Sharing data across function calls or components

---

## 🔹 Structs and Pointer to Structs

```go
type Vertex struct {
  X, Y int
}

v := Vertex{1, 2}
p := &v
p.X = 1000
```

> Accessing struct fields via pointer doesn't require explicit dereferencing.

---

## 🔹 Arrays and Slices

### Array:

```go
var a [2]string
primes := [6]int{2, 3, 5, 7, 11, 13}
```

### Slices:

```go
primes := [6]int{2, 3, 5, 7, 11, 13}
s := primes[1:4] // slice with elements 3, 5, 7
```

- Slices reference the underlying array. Mutating one slice affects others sharing the same data.

### Slice literals:

```go
q := []int{2, 3, 5, 7, 11, 13}
r := []bool{true, false, true}
```

### Slice defaults:

```go
s = s[1:4]
s = s[:2]
s = s[1:]
```

---

## 🚀 Final Thoughts

This week, I went from zero to solid on Go fundamentals:

- How Go handles variables, memory, and structures
- Writing functions with multiple returns
- Using slices effectively
- Understanding package structure and scoping rules

In **Week 2**, I’ll continue building my core knowledge and, if all goes well, start exploring **API development using **``, followed by **TDD practices** in Go.

> Learning from first principles has been the most rewarding part. Instead of skipping ahead to frameworks, I'm understanding the "why" behind the language.

Let me know if you’re also learning Go — happy to exchange notes!

