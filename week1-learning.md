# 🚀 Week 1 of Learning Go: From First Principles to Core Syntax

## 📘 Why I'm Learning Go

Go is good for designing and developing software backend systems. As a DevOps engineer working with infrastructure automation, I keep encountering tools built in Go, for example Kubernetes, Docker, and Terraform. It became clear: **Go isn’t just a language, it’s a foundational pillar in modern cloud-native systems.**

But instead of jumping into frameworks, I chose to learn **from first principles**—understanding Go’s syntax, memory model, and built-in capabilities **before** building REST APIs or services.

This blog captures my **Week 1 journey**, where I focused on understanding Go's:

- Basic structure
- Type system
- Control flow
- Functions
- Memory and pointers
- Arrays and slices
- Structs and pointers to structs

---

## 🧱 Packages, Imports & Program Structure

- Go programs start with a `main` package.
- Execution begins in `func main()`.
- You import external packages like this:

```go
import (
  "fmt"
  "math/rand"
)
```

> ✅ **Note:** Factored import statements are a good practice for clean and readable code.

---

## 🔤 Exported vs Unexported Names

- Identifiers that begin with **uppercase letters** are **exported** (public).
- Lowercase identifiers are private to their package.

```go
import "math"

fmt.Println(math.Pi) // ✅ works: Pi is exported
fmt.Println(math.pi) // ❌ compile error: pi is unexported
```

---

## 🧮 Functions and Return Values

### ➤ Single and Multiple Arguments

```go
func add(x, y, c int) int {
  return x + y + c
}

func main() {
  fmt.Println(add(40, 10, 45)) // Output: 95
}
```

### ➤ Multiple Return Values

```go
func swap(x, y, z string) (string, string, string) {
  return x, y, z
}

func main() {
  a, b, c := swap("hello", "world", "learning go")
  fmt.Println(a, b, c) // Output: hello world learning go
}
```

### ➤ Named Return Values & Naked Return

```go
func split(sum int) (x, y int) {
  x = sum * 4 / 9
  y = sum - x
  return // naked return
}

func main() {
  fmt.Println(split(17)) // Output: 7 10
}
```

---

## 🧠 Variables, Constants & Type Inference

### ➤ Variable Declarations

```go
var i int
j := 3.2
fmt.Println(i, j) // Output: 0 3.2
```

Uninitialized variables get **zero values** based on their types.

### ➤ Constants

```go
const Pi = 3.14
const Truth = true

func main() {
  fmt.Println("Happy", Pi, "Day")      // Output: Happy 3.14 Day
  fmt.Println("Go rules?", Truth)      // Output: Go rules? true
}
```

### ➤ Numeric Constants & Bit Manipulation

```go
const (
  Big = 1 << 100
  Small = Big >> 99
)

fmt.Println(needInt(Small))     // 21
fmt.Println(needFloat(Small))   // 0.2
fmt.Println(needFloat(Big))     // 1.2676506002282295e+29
```

---

## 📐 Type System & Type Inference

Go automatically assigns types to values using inference:

```go
v := 42         // int
x := 34.567     // float64
s := "GoLang"   // string

fmt.Printf("%T %T %T", v, x, s) // Output: int float64 string
```

---

## 🔁 Control Structures – `for`, `if`, `defer`

### ➤ Traditional For Loop

```go
for i := 0; i < 10; i++ {
  fmt.Println(i)
}
```

### ➤ While-Style For Loop

```go
sum := 1
for sum < 1000 {
  sum += sum
}
```

### ➤ If With Short Statement

```go
if v := math.Pow(2, 3); v < 10 {
  fmt.Println(v) // Output: 8
}
```

### ➤ Defer Statement

```go
defer fmt.Println("world")
fmt.Println("hello")
// Output:
// hello
// world
```

`defer` is used to delay execution—commonly for resource cleanup.

---

## 🧷 Pointers: Referencing & Dereferencing

In Go, a pointer holds the **memory address** of a value.

### ➤ Example

```go
i := 42
p := &i         // pointer to i
*p = 21         // update i through the pointer
fmt.Println(i)  // Output: 21
```

### 🔍 How It Works:
- `&i` gives the memory address of `i`.
- `*p` accesses (dereferences) the value at that address.
- Go allows **implicit dereferencing** in many cases for readability.

> Pointers are **safe** in Go (no pointer arithmetic), but powerful for mutability and memory efficiency.

---

## 🧩 Structs and Pointers to Structs

### ➤ Basic Struct and Field Access

```go
type Vertex struct {
  X, Y int
}

v := Vertex{1, 2}
fmt.Println(v.X) // Output: 1
```

### ➤ Pointer to Struct

```go
p := &v
p.X = 1000
fmt.Println(v) // Output: {1000 2}
```

Go allows `p.X` instead of `(*p).X`—this is called **implicit pointer dereferencing**.

### ✅ Practical Use Case

```go
type User struct {
  Name string
  Age  int
}

func updateAge(u *User, age int) {
  u.Age = age
}

user := User{"Alice", 25}
updateAge(&user, 30)
fmt.Println(user) // Output: {Alice 30}
```

---

## 🧮 Arrays and Slices

### ➤ Array

```go
primes := [6]int{2, 3, 5, 7, 11, 13}
```

### ➤ Slicing

```go
slice := primes[1:4] // Output: [3 5 7]
```

> ✅ **Important**: 
- The **low index** defaults to `0` if omitted.
- The **high index** is **exclusive**—up to `n-1`.

```go
s = s[:2]  // [3 5]
s = s[1:]  // [5]
```

### ➤ Shared Underlying Array

```go
a := names[0:2]
b := names[1:3]
b[0] = "XXX"
fmt.Println(a, b)      // [John XXX] [XXX George]
fmt.Println(names)     // [John XXX George Ringo]
```

### ➤ Slice Literals

```go
q := []int{2, 3, 5}
r := []bool{true, false, true}
```

---

## 🧭 Summary of Key Takeaways

| Concept          | What I Learned                                             |
|------------------|------------------------------------------------------------|
| Packages         | `main` is entry point; imports use factored style          |
| Variables        | Declared with `var` or `:=`; zero-values are default       |
| Constants        | Typed/untyped; bit shifts can generate large numbers       |
| Functions        | Support multiple/named return values                       |
| Type Inference   | Automatically inferred; explicit when needed               |
| Control Flow     | `for`, `if`, `defer` handle looping, branching, and cleanup|
| Pointers         | Simple, safe referencing/dereferencing                     |
| Structs          | Composite types; pointer access is clean and readable      |
| Arrays/Slices    | Slices are views on arrays; slicing uses [low:high)        |

---

## ⏭️ What’s Next – Week 2 Goals

Next week, I’ll continue strengthening my basics. If all goes well:

- Start exploring **API development using `net/http`**
- Learn **Test-Driven Development (TDD)** with Go’s `testing` package
- Build something minimal—but meaningful

> 🧠 The best part so far? Learning Go by **understanding why** it’s designed this way—not just how to use it.

Let me know if you're also learning Go—I'd love to exchange tips, projects, or simply geek out over clean syntax.

---
