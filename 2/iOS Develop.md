# iOS Develop

#### [Transitioning to ARC Release Notes](https://developer.apple.com/library/archive/releasenotes/ObjectiveC/RN-TransitioningToARC/Introduction/Introduction.html#//apple_ref/doc/uid/TP40011226)

Automatic Reference Counting (ARC) is a compiler feature that provides automatic memory management of Objective-C objects. Rather than having to think about retain and release operations, ARC allows you to concentrate on the interesting code, the object graphs, and the relationships between objects in your application.

#### [Does iOS 5 have garbage collection?](https://stackoverflow.com/a/6577366)

You appear to be talking about Automatic Reference Counting, mentioned in other answers. ARC is a kind of GC in that it automates memory freeing, but has a number of differences from a good garbage collector.

Firstly, it's mainly a compiler technology. The compiler knows about Cocoa's reference-counting guidelines, so it inserts retains and releases where they should be according to the rules. It works just like if you'd written the retains and releases yourself — it simply inserts them for you. Normal garbage collectors keep track of your program's memory while it is running.

Second, since it is just like retain and release, it can't catch retain cycles (if Object A retains Object B and Object B retains Object A, and nothing else references either of them, they both become immortal). You need to take the same precautions to prevent them.

It also uses resources differently from an automatic garbage collector. The garbage collectors used with Objective-C have to scan for unreferenced memory and collect it — which is expensive, and can lead to "stuttering" on slower systems — but they only have to do this occasionally, and in theory can even fine-tune their collection cycles to match how a program actually uses its memory. In general, a GC program will use more memory than a non-GC program and will slow down significantly when the GC decides to collect. ARC, on the other hand, moves the "scanning" to compile-time and frees memory as soon as it's available, but it has to constantly update object reference counts instead of waiting for garbage to build up like a collector.

#### [Why doesn't iOS have automatic garbage collection?](https://stackoverflow.com/a/6385380)

The problem with garbage collection is that memory usage grows until it's collected, so there might be more memory allocated than necessary. That's bad for devices with restricted memory and no option to swap.

When the garbage collector runs, it scans the heap to find memory that's no longer being used, and that's an expensive process, that will slow down your device until is has completed.

#### [iOS: Garbage collection](https://stackoverflow.com/questions/12811767/ios-garbage-collection)

iOS has no method of Garbage Collection. Even so, Garbage Collection is entirely unnecessary (for all practical purposes) when ARC is used. ARC works its magic at compile time to do the reference counting for you thereby making it unnecessary (and actually non-allowed) to use any other sort of memory management.

**Edit:**

To clarify, OS X does currently support garbage collection, but it is deprecated (unbeknown to me until a few minutes ago) as of OS X Mountain Lion. iOS never has and never will support garbage collection. Any advantages of garbage collection are moot when compared to those of ARC, and Apple has made the move to (forcefully) nudge us in the right direction.

