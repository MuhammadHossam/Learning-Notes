* The Cortex-M4 processor provides **exclusive access instructions** (`LDREX` and `STREX`) to implement **atomic operations** in multi-threaded environments. These instructions help in handling **critical sections, mutexes, and semaphores** efficiently without using full interrupt disabling.
### **Exclusive Access Mechanism**

Exclusive access ensures that **a memory location is not modified by another execution context** between a **read-modify-write** sequence.

1. **`LDREX` (Load Exclusive)**:
    - Loads a value from memory **and marks it as exclusive** in a special internal monitor.
    - Other accesses to the memory by another core or thread **invalidate the exclusive monitor**.
2. **`STREX` (Store Exclusive)**:
    - Stores a new value **only if the exclusive monitor is still valid**.
    - Returns **0 if successful** (store completed), **1 if failed** (memory changed externally).

Summary: 
* If thread A load exclusive from the memory and was preempted by thread B that tries to load the same memory and for example acquire the mutex, then Thread A now can't store what it loaded before and it needs to re-load the new value from the memory. 
* Thread A now needs a retry sequence to access the memory. 

![[Pasted image 20250311154624.png]]
