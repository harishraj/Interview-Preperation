# Java Volatile Keyword
“… the volatile modifier guarantees that any thread that reads a field will see the most recently written value.” - Josh Bloch

"The Java language also provides an alternative, weaker form of synchronization, volatile variables, to ensure that updates to a variable are propagated predictably to other threads.When a field is declared volatile,the compiler and runtime are put on notice that this variable is shared and that operations on it should **not be reordered** with other memory operations. Volatile variables are **not cached** in registers or in caches where they are hidden from other processors,so a read of a volatile variable always returns the most recent write by any thread." - Java Concurrency In Practice(3.1.4 Volatile Variables)

[Good reference](http://tutorials.jenkov.com/java-concurrency/volatile.html)
