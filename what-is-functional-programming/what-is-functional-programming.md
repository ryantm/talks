---
title: What is Functional Programming?
author: Ryan Mulligan
patat:
  wrap: true
  theme:
    code: [dullBlack,onDullWhite]
    codeBlock: [dullBlack,onDullWhite]
...

Functional Programming (FP) is using Math functions to program

---

# Math Functions

- Math: Domain -> Range; Programming: Value -> Result
- `f(x) =  m * x + b`
- `x = x + 1`
- `y = 5`

---

# What's not functional?

- no return value (methods)
- assignment
- effects are not treated specially

---

# Assignment

- mental overhead
- introduces the notion of state
- What is the program state?
- introduces the notion of time
- Where/when are we in the program?

---

# Assignment

How many states does the reader need to keep track of?

```javascript
x = 5        // x = 5
x = x * 10   // x = 50
x = x * 10   // x = 500
```

---

# Assignment refactoring

```javascript
x = 5        // x = 5
// e1...en
x = x * 10   // x = 50
// f1...fn
x = x * 10   // x = 500
// g1...gn
```

---

# Assignment refactoring

```javascript
y = 5            // y = 5
function e(x) {
  // e1...en
}
function f(x) {
  // f1...fn
}
function g(x) {
  // g1...gn
}
e(y)
f(y * 10)
g(y * 10 * 10)
```

---

# Computation by replacement

- whenever you see a function call, you can replace the call with the
  body of the function.
- nice way to understand programs
- "referential transparency", or "equational reasoning"

---

# Computation by replacement: Example 1

Consider this Javascript:

```javascript
const x = 4

function f(y) { return y + y; }
console.log(f(x))
console.log(f(x + 1))
```

---

# Computation by replacement: Example 1

Replace all calls to f with body of f:

```javascript
const x = 4

console.log(x + x)
console.log((x+1) + (x+1))
```

---

# Computation by replacement: Example 1

Replace all constants with values:

```javascript


console.log(4 + 4)         // 8
console.log((4+1) + (4+1)) // 10
```
---

# Computation by replacement: Example 2

Consider this Javascript, now with assignments:

```javascript
var x = 4
function f(y) { return y + y; }
x = x + 1
console.log(f(x))
x = x + 1
console.log(f(x))
```


---

# Computation by replacement: Example 2

Replace all calls to f with body of f:

```javascript
var x = 4

x = x + 1
console.log(x + x)
x = x + 1
console.log(x + x)
```

---

# Computation by replacement: Example 2

How do we proceed now? Track the value of x on every line.

```javascript
var x = 4                      // x = 4

x = x + 1                      // x = 5
console.log(x + x)             // 10
x = x + 1                      // x = 6
console.log(x + x)             // 12
```

---

# Computation by replacement: Example 2

Replace x at every line with x usages.

```javascript



console.log(5 + 5)             // 10

console.log(6 + 6)             // 12
```

---

# Computation by replacement: Example 3

What will happen in this example?

```javascript
var x = 4
function f(y) { return y + y; }
console.log(f(x++))
console.log(f(x++))
```

---

# Computation by replacement: Example 3

Total breakdown

```javascript
var x = 4

console.log((x++) + (x++))       // 9
console.log((x++) + (x++))       // 13
                                 // x = 8
```

---

# Debugging

- The current function context is defined solely by function arguments

---

# Side effects

- Disk
- Network
- Database
- Assignment

---

# Side effects

- Functional programming gives special treatment to side effects.
- Functions with special treatment to side effects are "pure"
- "purely functional programming"

---

# Side effects solution

- How do we keep functions pure while producing useful code?
- Use a domain specific langauge (DSL) for side effects
- Use a type system to enforce correct composition of functions that use this DSL.

---

# Haskell's side effect DSL

IO type marks functions that use the side effect DSL.

```haskell
putChar :: Char -> IO ()
getChar :: IO Char
```

---

# Haskell's side effect DSL

You don't have side effects in your program, your program builds up an
IO data type that gets passed to the Haskell runtime.


---

# Javascript IO side effect example



---

# Haskell IO example

- `main` hands an IO type to the runtime
- prints "haha" when executed by runtime

```haskell
import Control.Monad

main :: IO ()
main = replicateM_ 3 (putChar 'a')
```

---

# Computation by replacement holds for IO

Substitute `replicateM_` body:

```haskell
import Control.Monad

main :: IO ()
main = putChar 'a' >> replicateM_ 2 (putChar 'a')
```

---

# Computation by replacement holds for IO

Again:

```haskell
import Control.Monad

main :: IO ()
main = putChar 'a' >> putChar 'a' >> replicateM_ 1 (putChar 'a')
```

---

# Computation by replacement holds for IO

Again:

```haskell
import Control.Monad

main :: IO ()
main = putChar 'a' >> putChar 'a' >> putChar 'a' >> replicateM_ 0 (putChar 'a')
```

---

# Computation by replacement holds for IO

Again:

```haskell
import Control.Monad

main :: IO ()
main = putChar 'a' >> putChar 'a' >> putChar 'a' >> pure ()
```

---

# Replacement always safe

In a purely functional programming language you can always safely do
this mechanical substitution computation.

---

# Questions

---

# In Javascript

```
function f(x) { console.log(x); return x + 1; }
const array = [ f(1), f(2), f(3) ];             // prints to console unexpectedly
console.log(array)
```
