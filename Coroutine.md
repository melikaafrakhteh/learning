# Coroutine :shipit:

### a powerful tool for asynchronous programming.
### threads are heavy:
#### For every thread, the OS must allocate a lot of context information on the stack. Moreover, every time a computation reaches a blocking operation, the underneath thread is paused, and the JVM must load the context of another thread. The context switch is costly, so we should avoid blocking operations in our code.

### coroutines are very lightweight:
#### They are not mapped directly on OS threads but at the user level, with simple objects called continuations. Switching between coroutines does not require the OS to load another thread’s context but to switch the reference to the continuation object.

#### a coroutine is not bound to any particular thread. It may suspend its execution in one thread and resume in another one.

> [!NOTE]
> Making a network request on the main thread causes it to wait, or block, until it receives a response. Since the thread is blocked, the OS isn't able to call onDraw(), which causes your app to freeze and potentially leads to an Application Not Responding (ANR) dialog. For a better user experience, let's run this operation on a background thread.

> Note that all coroutines must run in a scope. A CoroutineScope manages one or more related coroutines.

> Since this coroutine is started with viewModelScope, it is executed in the scope of the ViewModel. If the ViewModel is destroyed because the user is navigating away from the screen, viewModelScope is automatically cancelled, and all running coroutines are canceled as well

```
sealed class Result<out R> {
    data class Success<out T>(val data: T) : Result<T>()
    data class Error(val exception: Exception) : Result<Nothing>()
}
```
```
class LoginViewModel(
    private val loginRepository: LoginRepository
): ViewModel() {

    fun login(username: String, token: String) {

        // Create a new coroutine on the UI thread
        viewModelScope.launch {
            val jsonBody = "{ username: \"$username\", token: \"$token\"}"

            // Make the network call and suspend execution until it finishes
            val result = loginRepository.makeLoginRequest(jsonBody)

            // Display result of the network request to the user
            when (result) {
                is Result.Success<LoginResponse> -> // Happy path
                else -> // Show error in UI
            }
        }
    }
}
```

```
class LoginRepository(...) {
    ...
    suspend fun makeLoginRequest(
        jsonBody: String
    ): Result<LoginResponse> {

        // Move the execution of the coroutine to the I/O dispatcher
        return withContext(Dispatchers.IO) {
            // Blocking network request code
        }
    }
}
```

> [!NOTE]
> When you don't pass a Dispatcher to launch, any coroutines launched from viewModelScope run in the `main thread`.

> all suspend functions must be executed in a coroutine.

## Suspend Function
regular function + suspend/resume concept

In one line: Suspending a coroutine means stopping it in the middle.

#### example:
Let’s start playing a video game:

You are playing well(Suppose).

You reach a checkpoint.

You save the current position.

You turn off the game and both you and your computer focus on doing different things now.

You are done with your tasks and you come back and resume from where you saved.

A coroutine can start executing the function, it can suspend(save the state and leave the thread for others), and later resume, once any suspending task(like a network call) is done executing.

```
suspend fun myFunction() {
  println("Before")
  delay(1000) // suspending
  println("After")
}
```

under the hood:
```
// A simplified picture of how myFunction looks under the hood
fun myFunction(continuation: Continuation<Unit>): Any {
  if (continuation.label == 0) { //Starting point, label stores the point where the execution was suspended.
    println("Before")
    continuation.label = 1 //Update just before suspension
    if (delay(1000, continuation) == COROUTINE_SUSPENDED){
      return COROUTINE_SUSPENDED
    }
  }
  //Point after suspension
  if (continuation.label == 1) {
    println("After")
    return Unit
  }
  error("Impossible")
}
```

This function needs its continuation to remember its state.
> [!IMPORTANT]
> Using suspend doesn't tell Kotlin to run a function on a background thread. It's normal for suspend functions to operate on the main thread. It's also common to launch coroutines on the main thread. You should always use withContext() inside a suspend function when you need main-safety, such as when reading from or writing to disk, performing network operations, or running CPU-intensive operations.
```
suspend fun fetchDocs() {                      // Dispatchers.Main
    val result = get("developer.android.com")  // Dispatchers.Main
    show(result)                               // Dispatchers.Main
}

suspend fun get(url: String) =                 // Dispatchers.Main
    withContext(Dispatchers.IO) {              // Dispatchers.IO (main-safety block)
        /* perform network IO here */          // Dispatchers.IO (main-safety block)
    }                                          // Dispatchers.Main
}
```



## coroutine builder

### launch:

#### it starts a new coroutine that is “fire and forget” — that means it won’t return the result to the caller

```
fun CoroutineScope.launch(
    context: CoroutineContext = EmptyCoroutineContext, 
    start: CoroutineStart = CoroutineStart.DEFAULT, // the builder’s default behavior is to immediately start the new coroutine 
    block: suspend CoroutineScope.() -> Unit
): Job
```

#### Job
Controls the lifecycle of the coroutine

#### CoroutineContext
The coroutine context is a set of various elements. The main elements are the Job of the coroutine and its dispatcher

***Dispatchers:***
Determines what thread or threads the corresponding coroutine uses for its execution.

**Dispatchers.Default:** CPU-intensive work, such as sorting large lists, doing complex calculations, and similar. A shared pool of threads on the JVM backs it. It uses a thread pool with a number of threads equal to the number of available processors.

**Dispatchers.IO:** networking or reading and writing from files. In short — any input and output, as the name states and limited to 64 threads or the number of cores by default

**Dispatchers.Main:** recommended dispatcher for performing UI-related events.

**Dispatchers.Unconfined:** This dispatcher doesn’t confine coroutines to any specific thread. The coroutine starts the execution in the inherited CoroutineDispatcher that called it. But after a suspension ends, it may resume in any other thread.

#### example:

```
suspend fun concurrentMorningRoutine() {
    coroutineScope {
        launch {
            bathTime()
        }
        launch {
            boilingWater()
        }
    }
}

suspend fun boilingWater() {
    logger.info("Boiling water")
    delay(1000L)
    logger.info("Water boiled")
}

suspend fun bathTime() {
  logger.info("Going to the bathroom")
  delay(500L)
  logger.info("Exiting the bathroom")
}
```

The log of the above code is something similar to the following:

```
09:09:44.817 [main] INFO CoroutinesPlayground - Starting the morning routine
09:09:44.870 [DefaultDispatcher-worker-1 @coroutine#1] INFO CoroutinesPlayground - Going to the bathroom
09:09:44.871 [DefaultDispatcher-worker-2 @coroutine#2] INFO CoroutinesPlayground - Boiling water
09:09:45.380 [DefaultDispatcher-worker-2 @coroutine#1] INFO CoroutinesPlayground - Exiting the bathroom
09:09:45.875 [DefaultDispatcher-worker-2 @coroutine#2] INFO CoroutinesPlayground - Water boiled
09:09:45.876 [DefaultDispatcher-worker-2 @coroutine#2] INFO CoroutinesPlayground - Ending the morning routine
```

***This model of concurrency is cooperative***
The coroutine#2 had a chance to execute only when coroutine#1 reached the execution of a suspending function, i.e., the delay function.
when suspended, the coroutine#1 was running on thread DefaultDispatcher-worker-1. Whereas, when resumed, it ran on thread DefaultDispatcher-worker-2. Coroutines run on configurable thread pools. As the log suggested, the default thread pool is called Dispatchers.Default 


### async
We can use the async builder to create a coroutine that returns a value.

```
public fun <T> CoroutineScope.async(
  context: CoroutineContext = EmptyCoroutineContext,
  start: CoroutineStart = CoroutineStart.DEFAULT,
  block: suspend CoroutineScope.() -> T
): Deferred<T>
```

On the object of type Deferred<T>, we can call the `await` method to wait for the coroutine’s completion and get the returned value.

#### example:
```
suspend fun breakfastPreparation() {
    coroutineScope {
        val coffee: Deferred<String> = async {
            preparingJavaCoffee()
        }
        val toast: Deferred<String> = async {
            toastingBread()
        }
        logger.info("I'm eating ${coffee.await()} and ${toast.await()}")
    }
}

suspend fun preparingJavaCoffee(): String {
  logger.info("Preparing coffee")
  delay(500L)
  logger.info("Coffee prepared")
  return "Java coffee"
}

suspend fun toastingBread(): String {
  logger.info("Toasting bread")
  delay(1000L)
  logger.info("Bread toasted")
  return "Toasted bread"
}
```

```
21:56:46.091 [main] INFO CoroutinesPlayground - Starting the morning routine
21:56:46.253 [DefaultDispatcher-worker-1 @coroutine#1] INFO CoroutinesPlayground - Preparing coffee
21:56:46.258 [DefaultDispatcher-worker-2 @coroutine#2] INFO CoroutinesPlayground - Toasting bread
21:56:46.758 [DefaultDispatcher-worker-1 @coroutine#1] INFO CoroutinesPlayground - Coffee prepared
21:56:47.263 [DefaultDispatcher-worker-1 @coroutine#2] INFO CoroutinesPlayground - Bread toasted
21:56:47.263 [DefaultDispatcher-worker-1 @coroutine#2] INFO CoroutinesPlayground - I'm eating Java coffee and Toasted bread
21:56:47.263 [DefaultDispatcher-worker-1 @coroutine#2] INFO CoroutinesPlayground - Ending the morning routine
```






### Resource:
kotlin 101 [here](https://rockthejvm.com/articles/kotlin-101-coroutines)






 
