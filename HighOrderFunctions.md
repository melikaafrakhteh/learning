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
}
```

