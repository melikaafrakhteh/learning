# Flow 🚰

#### A suspending function asynchronously returns a single value, but how can we return multiple asynchronously computed values? This is where Kotlin Flows come in.

Flow is essentially a reactive stream of data that can be emitted from a producer and collected by a consumer. 

Flows are part of the Kotlin Coroutines library.

#### Flows are `cold` streams similar to sequences — the code inside a flow builder does not run until the flow is collected. 

## Hot And Cold:
 `Hot data sources` are eager. They produce elements as soon as possible and store them. They create elements independently of their consumption. These are collections (List, Set) and Channel.
  Also, SharedFlow and StateFlow are hot.

  `Cold data sources` are lazy. They process elements on-demand on the terminal operation. All intermediate functions just define what should be done (most often using the Decorator pattern). They generally do not store elements and create them on demand. They do the minimal number of operations and can be infinite. Their creation and processing of elements is typically the same process as consumption. These elements are Sequence, Java Stream, Flow and RxJava streams (Observable, Single, etc).

  ![hot/cold](https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQB_sSVbHp_PobE7ggwcn3WxCiNe-lH7sf1xg&s)


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

`catch` method that we can chain to the flow to handle exceptions.

```
// Kotlin Coroutines Library
public fun <T> Flow<T>.catch(action: suspend FlowCollector<T>.(cause: Throwable) -> Unit): Flow<T>
```
example:
```
val spiderMenActorsFlowWithException_v3 =
        flow {
            emit(tobeyMaguire)
            emit(andrewGarfield)
            throw RuntimeException("An exception occurred")
            emit(tomHolland)
        }
        .catch { ex -> emit(tomHolland) }
        .onStart { println("The Spider Men flow is starting") }
        .onCompletion { println("The Spider Men flow is completed") }
        .collect { println(it) }
```

output:
```
The Spider Men flow is starting
Actor(id=Id(id=13), firstName=FirstName(firstName=Tobey), lastName=LastName(lastName=Maguire))
Actor(id=Id(id=14), firstName=FirstName(firstName=Andrew), lastName=LastName(lastName=Garfield))
Actor(id=Id(id=12), firstName=FirstName(firstName=Tom), lastName=LastName(lastName=Holland))
The Spider Men flow is completed
```
we can think about the catch function as a catch block that handles all the exceptions thrown before it in the chain. For this reason, the catch function can’t catch the exceptions thrown by the collect function .

example:
```
val spiderMenActorsFlowWithException =
    flow {
        emit(tobeyMaguire)
        emit(andrewGarfield)
        emit(tomHolland)
    }
    .catch { ex -> println("I caught an exception!") }
    .onStart { println("The Spider Men flow is starting") }
    .onCompletion { println("The Spider Men flow is completed") }
    .collect {
        if (true) throw RuntimeException("Oooops")
        println(it)
    }
```

output:
```
The Spider Men flow is starting
The Spider Men flow is completed
Exception in thread "main" java.lang.RuntimeException: Oooops
...
```

The only way we can prevent this case is to move the collect logic into a dedicated onEach function and put a catch in the chain after the onEach function. We can rewrite the above example as follows:

```
val spiderMenActorsFlowWithException =
    flow {
        emit(tobeyMaguire)
        emit(andrewGarfield)
        emit(tomHolland)
    }
    .onEach {
        if (true) throw RuntimeException("Oooops")
        println(it)
    }
    .catch { ex -> println("I caught an exception!") }
    .onStart { println("The Spider Men flow is starting") }
    .onCompletion { println("The Spider Men flow is completed") }
    .collect()
```

output:
```
The Spider Men flow is starting
I caught an exception!
The Spider Men flow is completed
```

The Kotlin Coroutines library provides a function to retry the execution of a flow in case of an exception: the `retry` function.

```
// Kotlin Coroutines Library
public fun <T> Flow<T>.retry(
    retries: Long = Long.MAX_VALUE,
    predicate: suspend (cause: Throwable) -> Boolean = { true } //The lambda decides whether the operation should be retried. 
): Flow<T>
```


### Flows are very similar to collections regarding the API available for transforming them. We can map, filter, and reduce them.

#### map:

```
fun <T, R> Flow<T>.map(transform: suspend (value: T) -> R): Flow<R> =
    flow {
        this@map.collect { value ->
            emit(transform(value))
        }
    }
```

example:
```
val lastNameOfJLActors: Flow<LastName> = zackSnyderJusticeLeague.map { it.lastName }
```

#### filter:

```
fun <T> Flow<T>.filter(predicate: suspend (value: T) -> Boolean): Flow<T> =
    flow {
        this@filter.collect { value ->
            if (predicate(value)) {
                emit(value)
            }
        }
    }
```

example:
```
val lastNameOfJLActors5CharsLong: Flow<LastName> =
    lastNameOfJLActors.filter { it.lastName.length == 5 }
```

#### fold:
the fold function is used to reduce the values of a flow to a single value. It’s a final operation, like the collect function, which suspends the current coroutine until the flow ends to emit values. 

```
// Kotlin Coroutines Library
public suspend inline fun <T, R> Flow<T>.fold(
    initial: R,
    crossinline operation: suspend (acc: R, value: T) -> R
): R {
    var accumulator = initial
    collect { value ->
        accumulator = operation(accumulator, value)
    }
    return accumulator
}
```

example:
```
val numberOfJlaActors: Int =
      zackSnyderJusticeLeague.fold(0) { currentNumOfActors, actor -> currentNumOfActors + 1 }
```

### Resource:
flow 101 [here](https://rockthejvm.com/articles/kotlin-101-flows#introduction)
hot and cold [here](https://kt.academy/article/cc-hot-cold)


