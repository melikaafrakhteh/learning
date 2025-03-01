There's no garbage collector under iOS. Instead, simply use automatic reference counting (ARC). ARC will take care of most of the memory management for you without the runtime overhead of a garbage collector.

GC is like an invisible cleaner or somewhat like a robot vacuum cleaner that quietly monitors the memory used by your app and detects and dispose objects that are no longer referenced anywhere in the code.

# Garbage Collection 🗑️

### Basic:
A *Garbage Collector (GC)* is a system used in many programming languages to automatically manage memory. 
It identifies and releases memory that’s no longer needed by a program, preventing memory leaks and improving performance.
The GC monitors objects in memory, and when an object is no longer accessible or referenced, it deallocates that memory, making it available for other processes.
This automation helps developers avoid the pitfalls of manual memory management, which can often be complex and error-prone.


### Reference:
[first](https://medium.com/thejuniordeveloper/basics-of-android-garbage-collection-what-every-android-developer-should-know-e6c90e0dfc60)

[second](https://medium.com/@banerjee.s.sayans/android-garbage-collection-in-a-nutshell-e5c8acfa1538)

[third](https://fragmentedpodcast.com/episodes/064/)

[forth](https://www.droidcon.com/2024/09/20/garbage-collector-in-kmp-part-1/)


[fifth](https://proandroiddev.com/garbage-collector-in-kmp-part-2-b0577668ea16)

