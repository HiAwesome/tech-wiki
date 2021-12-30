# iOS Develop

#### [Why doesn't iOS have automatic garbage collection?](https://stackoverflow.com/a/6385380)

The problem with garbage collection is that memory usage grows until it's collected, so there might be more memory allocated than necessary. That's bad for devices with restricted memory and no option to swap.

When the garbage collector runs, it scans the heap to find memory that's no longer being used, and that's an expensive process, that will slow down your device until is has completed.

#### [iOS: Garbage collection](https://stackoverflow.com/questions/12811767/ios-garbage-collection)

iOS has no method of Garbage Collection. Even so, Garbage Collection is entirely unnecessary (for all practical purposes) when ARC is used. ARC works its magic at compile time to do the reference counting for you thereby making it unnecessary (and actually non-allowed) to use any other sort of memory management.

**Edit:**

To clarify, OS X does currently support garbage collection, but it is deprecated (unbeknown to me until a few minutes ago) as of OS X Mountain Lion. iOS never has and never will support garbage collection. Any advantages of garbage collection are moot when compared to those of ARC, and Apple has made the move to (forcefully) nudge us in the right direction.

