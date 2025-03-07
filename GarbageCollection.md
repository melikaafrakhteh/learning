There's no garbage collector under iOS. Instead, simply use automatic reference counting (ARC). ARC will take care of most of the memory management for you without the runtime overhead of a garbage collector.

GC is like an invisible cleaner or somewhat like a robot vacuum cleaner that quietly monitors the memory used by your app and detects and dispose objects that are no longer referenced anywhere in the code.

# Garbage Collection 🗑️

### Basic:
A *Garbage Collector (GC)* is a system used in many programming languages to automatically manage memory. 
It identifies and releases memory that’s no longer needed by a program, preventing *memory leaks* and improving performance.
The GC monitors objects in memory, and when an object is no longer accessible or referenced, it deallocates that memory, making it available for other processes.
This automation helps developers avoid the pitfalls of manual memory management, which can often be complex and error-prone.

### Memory Leaks:
#### Failure to release unused objects from the memory.
Failure to release unused objects from the memory means that there are unused objects in the application that the GC cannot clear from memory.

Leaks can be divided into two categories: *permanent*, *temporary*

**permanent:** Those that occupy the memory unit until the application terminates. when an app is terminated, the GC releases all of the memory that the app has used.

**temporary:** Those that occupy the memory unit until the method terminates

#### What is RAM (Memory)?
Random access memory, is the memory in android devices or computers that is used to store current running applications and their data.

Two main characters in the RAM: *Heap*, *Stack*

**Stack:** used for *static* memory allocation.
The primitive variable will be created and stored in the stack memory.
The objects in the stack are only there for a limited time. Once the method is complete, the objects will be released and reclaimed.

**Heap:** used for *dynamic* memory allocation.

Whenever you create an object, it’s always created in the heap.
object heap memory address is stored on the stack.

For releasing and reclaiming the objects from heap memory, we need help.

To provide a smooth user experience, Android sets a hard limit on the heap size for each running application.
If your app hits this heap limit and tries to allocate more memory, it will receive an *OutOfMemoryError* and will terminate.


#### A memory leak can easily occur in Android when AsyncTasks, Handlers, Singletons, Threads, and other components are used incorrectly.




### Reference:
[first](https://medium.com/thejuniordeveloper/basics-of-android-garbage-collection-what-every-android-developer-should-know-e6c90e0dfc60)

[second](https://medium.com/@banerjee.s.sayans/android-garbage-collection-in-a-nutshell-e5c8acfa1538)

[third](https://fragmentedpodcast.com/episodes/064/)

[forth](https://www.droidcon.com/2024/09/20/garbage-collector-in-kmp-part-1/)


[fifth](https://proandroiddev.com/garbage-collector-in-kmp-part-2-b0577668ea16)

[memory leaks](https://proandroiddev.com/everything-you-need-to-know-about-memory-leaks-in-android-d7a59faaf46a)

[memory leasks persian1](https://virgool.io/MobileLab/%D9%81%DB%8C-%D9%85%D8%B5%D8%A7%D8%A6%D8%A8-%D9%86%D8%B4%D8%AA-%D8%AD%D8%A7%D9%81%D8%B8%D9%87-%D8%AF%D8%B1-%D8%A7%D9%86%D8%AF%D8%B1%D9%88%DB%8C%D8%AF-%DB%8C%D8%A7-%D9%85%D9%85%D9%88%D8%B1%DB%8C-%D9%84%DB%8C%DA%A9-%DA%86%DB%8C%D8%B3%D8%AA%D8%A8%D8%AE%D8%B4-%D8%A7%D9%88%D9%84-s9b0knq5bemn)

[memory leaks persian2](https://virgool.io/MobileLab/%D9%81%DB%8C-%D9%85%D8%B5%D8%A7%D8%A6%D8%A8-%D9%86%D8%B4%D8%AA-%D8%AD%D8%A7%D9%81%D8%B8%D9%87-%D8%AF%D8%B1-%D8%A7%D9%86%D8%AF%D8%B1%D9%88%DB%8C%D8%AF-%DB%8C%D8%A7-%D9%81%D8%B1%D8%A7%D8%B1-%D8%A7%D8%B2-%D8%AA%D9%84%D9%87-%D9%85%D9%85%D9%88%D8%B1%DB%8C-%D9%84%DB%8C%DA%A9%D8%A8%D8%AE%D8%B4-%D8%AF%D9%88%D9%85-tpvyhymwpxns)

