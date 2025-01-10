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

### A flow is started when a terminal operation is called on it

 The `collect` function is used to consume the values emitted by the flow. It takes a lambda, which is called for each value the flow emits.(a terminal operation)

we called the collect function from the suspend main function. The collect method is a suspending function since it has to wait and suspend for consuming the emitted values without blocking a thread.

```
// Kotlin Coroutines Library
public interface Flow<out T> {
    public suspend fun collect(collector: FlowCollector<T>)
}
```

## Flows Lifecycle

The `onStart` function lets us add operations to be executed when the flow is started. 

example:
```
val spiderMenWithLatency: Flow<Actor> = flow {
    delay(1000)
    emit(tobeyMaguire)
    emit(andrewGarfield)
    emit(tomHolland)
}
spiderMenWithLatency
    .onStart { println("Starting Spider-Men flow") }
    .collect { println(it) }
```

output:
```
Starting Spider-Men flow
(1 sec.)
Actor(id=Id(id=13), firstName=FirstName(firstName=Tobey), lastName=LastName(lastName=Maguire))
Actor(id=Id(id=14), firstName=FirstName(firstName=Andrew), lastName=LastName(lastName=Garfield))
Actor(id=Id(id=12), firstName=FirstName(firstName=Tom), lastName=LastName(lastName=Holland))
```

behind:
```
// Kotlin Coroutines Library
public fun <T> Flow<T>.onStart(
    action: suspend FlowCollector<T>.() -> Unit
): Flow<T>
```

the `onEach` function is used to apply a lambda to each value emitted by the flow. 

example:
```
spiderMen
    .onEach { delay(1000) }
    .collect { println(it) }
```

We can use the `onCompletion` function to add a lambda to be executed when the flow is completed.

example:
```
spiderMen
    .onEach {
      println(it)
    }
    .onCompletion { println("End of the Spider Men flow") }
    .collect()
```

output:
```
Actor(id=Id(id=13), firstName=FirstName(firstName=Tobey), lastName=LastName(lastName=Maguire))
Actor(id=Id(id=14), firstName=FirstName(firstName=Andrew), lastName=LastName(lastName=Garfield))
Actor(id=Id(id=12), firstName=FirstName(firstName=Tom), lastName=LastName(lastName=Holland))
End of the Spider Men flow`
```

## Flows Error Handling
The Kotlin Coroutines library provides a function to handle the case of an empty flow: the `onEmpty` function

example:
```
val actorsEmptyFlow =
    flow<Actor> { delay(1000) }
        .onEmpty {
            println("The flow is empty, adding some actors")
            emit(henryCavill)
            emit(benAffleck)
        }
        .collect { println(it) }
```
output:

```
(1 sec.)
The flow is empty, adding some actors
Actor(id=Id(id=1), firstName=FirstName(firstName=Henry), lastName=LastName(lastName=Cavill))
Actor(id=Id(id=4), firstName=FirstName(firstName=Ben), lastName=LastName(lastName=Affleck))
```

Creating an empty flow using a dedicated builder called `emptyFlow` is possible.

Another typical case is when exceptions arise during the execution of a flow. 

example:

```
val spiderMenActorsFlowWithException =
    flow {
        emit(tobeyMaguire)
        emit(andrewGarfield)
        throw RuntimeException("An exception occurred")
        emit(tomHolland)
    }
    .onStart { println("The Spider Men flow is starting") }
    .onCompletion { println("The Spider Men flow is completed") }
    .collect { println(it) }
```

output:
```
The Spider Men flow is starting
Actor(id=Id(id=13), firstName=FirstName(firstName=Tobey), lastName=LastName(lastName=Maguire))
Actor(id=Id(id=14), firstName=FirstName(firstName=Andrew), lastName=LastName(lastName=Garfield))
The Spider Men flow is completed
Exception in thread "main" java.lang.RuntimeException: An exception occurred
...
```

