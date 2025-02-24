# HighOrderFunctions 👠

### A higher-order function is a function that takes functions as parameters, or returns a function.

```
// A simple high-order function that takes a function as a parameter
fun <T> printResult(input: T, transform: (T) -> String) {
    println(transform(input))
}

// A function that will be passed as a parameter
fun toUpperCase(input: String): String {
    return input.toUpperCase()
}

fun main() {
    // Using the high-order function and passing another function as an argument
    printResult("hello", ::toUpperCase) // Output: HELLO
    printResult("hello", { it.toUpperCase() }) // Output: HELLO
}
```

### Function types
 #### All function types have a parenthesized list of parameter types and a return type:
 *(A, B) -> C* : denotes a type that represents functions that take two arguments of types A and B and return a value of type C.

  The list of parameter types may be empty, as in *() -> A*. The Unit return type cannot be omitted.

  #### Function types can optionally have an additional receiver type, which is specified before the dot in the notation:
  
  *A.(B) -> C* represents functions that can be called on a receiver object A with a parameter B and return a value C.

  #### Suspending functions belong to a special kind of function type that have a suspend modifier in their notation, such as suspend () -> Unit or suspend A.(B) -> C.

The function type notation can optionally include *names* for the function parameters: (x: Int, y: Int) -> Point.

To specify that a function type is *nullable*, use parentheses as follows: ((Int, Int) -> Int)?

Function types can also be *combined* using parentheses: (Int) -> ((Int) -> Unit)

You can also give a function type an alternative name by using a *type alias*:
```
typealias ClickHandler = (Button, ClickEvent) -> Unit
```

### Instantiating a function type﻿
a lambda expression: { a, b -> a + b }

an anonymous function: fun(s: String): Int { return s.toIntOrNull() ?: 0 }

Use a *callable reference* to an existing declaration:

a top-level, local, member, or extension function: ::isOdd, String::toInt

a top-level, member, or extension property: List<Int>::size

a constructor: ::Regex

example of lambda function and function type:

```
 val sum: (Int, Int) -> Int = {x, y -> x + y}
    println(sum(2,7)) // out put = 9

   // val sum: (Int, Int) -> Int declares a function type.
   // { x, y -> x + y } is a lambda function, not just a regular variable.
 ```
