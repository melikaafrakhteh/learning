# Infix function 🖇️

### These functions are omitting the dot and the parentheses for the call

## rules:
#### They must be member functions or extension functions.

#### They must have a single parameter.

#### The parameter must not accept variable number of arguments and must have no default value.


example:

```
infix fun Int.shl(x: Int): Int { ... }

// calling the function using the infix notation
1 shl 2

// is the same as
1.shl(2)
```

>[!NOTE]
> Note that infix functions always require both the receiver and the parameter to be specified.

```
class MyStringCollection {
    infix fun add(s: String) { /*...*/ } //member function

    fun build() {
        this add "abc"   // Correct
        add("abc")       // Correct
        //add "abc"        // Incorrect: the receiver must be specified
    }
}
```

### Let’s see some of the examples of user defined infix function:

#### Checking if a Value is Within a Range
```
fun main() {
    val result = 5 isInRange 1..10
    println(result)
    // Result: true
}

infix fun Int.isInRange(range: IntRange): Boolean {
    return this in range
}
```

#### Appending an Item to a List
```
fun main() {
    val list = listOf(1, 2, 3)
    val newList = list append 4
    println(newList)
    // Result: [1, 2, 3, 4]
}

infix fun <T> List<T>.append(item: T): List<T> {
    return this + item
}
```

#### Custom equals Check
```
fun main() {
    val isEqual = "Hello" customEquals "Hello"
    println(isEqual)
    // Result: true
}

infix fun <T> T.customEquals(other: T): Boolean {
    return this == other
}
```

Happy Infixing !

