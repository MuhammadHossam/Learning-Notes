* Two or more threads may want to use non-sharable hardware (Serial port), or accessing shared memory simultaneously. 
* Atomic operation can solve the problem, but no processor can update a memory location with one instruction (it can only done for peripherals only in some controllers not all). 
* There are multiple methods for mutual exclusion: 
	1. Critical section by disable/enable interrupts but this will cause interrupt latency and affect the hard-real-time applications. 
	2. Disable the scheduler in the critical section but it will also cause the same issue, the controller will guarantee ISRs servicing but if higher priority thread arrived, it will not be handled and will affect the system responsiveness. 
	3. Using Mutex/Semaphores to manage the shared resource access between threads. 


Mutex 
* It is a flag that can be tested and **updated atomically** (should ensure critical section while updating).
* Mutex has two states: 
	1. Mutex = 1, No thread in the critical section and any thread can access this resource. 
	2. Mutex = 0, Another thread in the critical section and no other thread can access this resource.
* Each thread checks the mutex before entering the critical section, if the mutex is 1, the thread sets the mutex to 0 (Mutex Lock) and enters the critical section and reset the mutex back to 1 (Mutex release) after leaving. 

Semaphore
* Resources can be shared between limited number of threads. 
* Mutex is a binary flag (binary semaphore) and can be extended to a counter to support access from multiple threads.
* Semaphore is functionally similar to the mutex but with counter.
* If thread tries to access empty semaphore counter, it will be pushed to the waiting/blocked list.

MAILBOX
* Used for inter-thread communication, unlike semaphores, which provide only synchronization with information exchange. 
* Using shared buffers, one thread can fill the buffer (Producer), and the other empties it (Consumer).
* The consumer needs to wait when the buffer is empty and the producer also needs to wait when the buffer is full.
* Shared buffer needs to be updated by the producer and consumer mutually exclusively, so mutex is needed for mutual exclusive access to the buffer and semaphore for counting number of message in the buffer.
* Buffer has limited size, producer can't fill the buffer infinitely, so another semaphore is needed for counting the empty slots in the buffer.
* There are two indices one for write position and another one for read position. 
![[Pasted image 20250311162551.png]]

