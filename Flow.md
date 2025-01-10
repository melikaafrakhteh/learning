# Flow 🚰

A Flow<T> is a reactive data structure that emits a sequence of type T values. 

Flows are part of the Kotlin Coroutines library.

## Flow Builders:
### flowOf
 We can create a flow from a finite list of values using the flowOf function.

 example:
```
val numbers: Flow<Int> =
    flowOf(
       1,2,3,4,5
    )
```

 behind:
```
// Kotlin Coroutines Library
fun <T> flowOf(vararg values: T): Flow<T> = flow {
    for (value in values) {
        emit(value)
    }
}
```

### asFlow
We can even create a flow from a list, a set, and so on using the asFlow extension function.

example:
```
val numbers: Flow<Int> =
    listOf(
        1,2,3,4,5
    ).asFlow()
```

### flow
The flow function is the most general way to create a flow. It takes a lambda that can emit values using the emit function.

example:
```
val numbers: Flow<Int> = flow {
  emit(1)
  emit(2)
  emit(3)
  emit(4)
  emit(5)
}
```

### The `collect` function is used to consume the values emitted by the flow. It takes a lambda, which is called for each value the flow emits.

we called the collect function from the suspend main function. The collect method is a suspending function since it has to wait and suspend for consuming the emitted values without blocking a thread.

```
// Kotlin Coroutines Library
public interface Flow<out T> {
    public suspend fun collect(collector: FlowCollector<T>)
}
```
