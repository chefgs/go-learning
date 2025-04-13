# Week 1 of Learning Go: From First Principles to Core Syntax

## 📚 Why I'm Learning Go

As someone working in DevOps and automation, Go keeps showing up in tools I use daily (Terraform, Kubernetes, Docker). It’s fast, efficient, and increasingly the language of systems engineering. My goal? Learn it the right way: **from first principles**.

I dedicated this first week to mastering the **core syntax**, understanding memory handling, types, control structures, and common patterns that make Go elegant and powerful.

---

## 🔹 Packages and Imports

- Every Go program starts with a \`\` declaration.
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

func main() {
  fmt.Println(add(40, 10, 45)) // Output: 95
}
```

### Function with multiple return values:

```go
func swap(x, y, z string) (string, string, string) {
  return x, y, z
}

func main() {
  a, b, c := swap("hello", "world", "learning go")
  fmt.Println(a, b, c) // Output: hello world learning go
}
```

### Named return values (naked return):

```go
func split(sum int) (x, y int) {
  x = sum * 4 / 9
  y = sum - x
  return
}

func main() {
  fmt.Println(split(17)) // Output: 7 10
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

func main() {
  fmt.Println("Happy", Pi, "Day") // Output: Happy 3.14 Day
  fmt.Println("Go rules?", Truth) // Output: Go rules? true
}
```

### Big/Small Constants:

```go
const (
  Big = 1 << 100
  Small = Big >> 99
)

func needInt(x int) int { return x*10 + 1 }
func needFloat(x float64) float64 {
  return x * 0.1
}

func main() {
  fmt.Println(needInt(Small))     // Output: 21
  fmt.Println(needFloat(Small))   // Output: 0.2
  fmt.Println(needFloat(Big))     // Output: 1.2676506002282295e+29
}
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

v := Vertex{1, 2}      // struct instance
p := &v                // pointer to struct
p.X = 1000             // updating field via pointer (implicit dereferencing)
```

In Go, a `struct` is a composite type that groups variables (fields) under a single name. It’s often used to represent data models like coordinates, configurations, or database entities.

You can access struct fields using dot notation (e.g., `v.X`). When working with a pointer to a struct, Go allows you to access fields using the same syntax: `p.X` is interpreted as `(*p).X`. This makes pointer dereferencing seamless and readable.

### 🧠 Why use pointers with structs?

- **Memory efficiency**: Pass references instead of copying the entire struct
- **Mutability**: Update struct fields directly via pointers
- **Useful in APIs**: Modify request/response data within handler functions

### ✅ Example in practice:

```go
type User struct {
  Name string
  Age  int
}

func updateAge(u *User, newAge int) {
  u.Age = newAge
}

func main() {
  user := User{"Alice", 25}
  updateAge(&user, 30)
  fmt.Println(user) // Output: {Alice 30}
}
```

This approach is especially powerful when working with REST APIs or in-memory stateful services where struct manipulation is common.

---

## 🔹 Arrays and Slices

In Go, slices provide a flexible way to work with sequences. They are built on top of arrays, but unlike arrays, slices are dynamic and can grow or shrink.

👉 **Slicing Basics:**

- A slice is created using a colon `:` operator: `array[low:high]`.
- The \*\*low index defaults to \*\***`0`** if not provided.
- The **high index represents the position just beyond the last element**, so slicing **goes up to \*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*****`n-1`**.

For example:

```go
arr := [5]int{10, 20, 30, 40, 50}
slice := arr[1:4] // includes elements at index 1, 2, 3 => [20 30 40]
```

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
s := []int{2, 3, 5, 7, 11, 13}
```

### Slice defaults:

```go
s = s[1:4]
s = s[:2] // s gets the result of previous assignment
s = s[1:]  // s gets the result of previous assignment
```

In the slicing example above, result is printed as below

```
Reset
[3 5 7]
[3 5]
[5]
```

---

## 🚀 Final Thoughts

This week, I went from zero to solid on Go fundamentals:

- How Go handles variables, memory, and structures
- Writing functions with multiple returns
- Using slices effectively
- Understanding package structure and scoping rules

In **Week 2**, I’ll continue building my core knowledge and, if all goes well, start exploring \*\*API development using \*\*, followed by **TDD practices** in Go.

> Learning from first principles has been the most rewarding part. Instead of skipping ahead to frameworks, I'm understanding the "why" behind the language.

Let me know if you’re also learning Go — happy to exchange notes!

