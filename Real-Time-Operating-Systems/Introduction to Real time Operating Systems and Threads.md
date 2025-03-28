Why we need to use RTOS? 
* RTOS is useful for application having varying execution order (execution threads) with different priority levels. 
* Minimizing waiting time by executing other tasks instead of just waiting. 
* Managing system resources and shared hardware.
* Manage threads by handling the running, waiting, and blocked threads. 
* Responsible for context switching between threads.

RTOS Design Considerations
* Extra memory overhead.
* Execution time overhead for context switching as well as task scheduling.
* Porting RTOS isn't easy, some assembly codes needs hard porting according to the microcontroller specific architecture and instruction set.

Foreground and Background systems
* The RTOS is a golden example for the foreground and background systems since the running thread will be in the foreground and the waiting tasks will be in the background. 

Execution Thread 
* A sequence of instructions to be executed for doing specific job.
* It has its own program start address, program counter, stack pointer, CPU flags, and CPU control registers.
* The operating system should store the CPU status, flags, and registers in the currently context-switched task in its corresponding thread control block TCB to keep the thread status.
* Each thread has its own stack space.
*  It has three states: ready state, waiting state, and running state.
*  It goes to the waiting state when it asks for not ready resources.
*  Its state can be changed by an IRQ that triggers some events then the RTOS can move it from waiting to ready state.
* Its state is switched from running to ready state when an IRQ triggers another high priority thread into the ready state.

Thread Creation
* It should allocate and initialize the thread control block, program counter pointing to the thread function (pointer to function), thread priority, amount of stack space, and return address.

Thread Lists
* All active threads (waiting or ready) except running thread have their thread object in one linked list (Waiting list or ready list).
* The list shall copy only the address to the thread object not the whole thread to save execution time and memory.
* The list shall be ordered according to the thread’s priorities, so the scheduler can access the highest priority thread fast without consuming overhead execution time.
* The first node is a dummy head that points to the highest priority node.
* Each state will have array of lists, since each list in the array defines a list of threads of certain priority, for example the index 0 in the array of ready lists will be a list that contains all threads with the priority zero in the ready state to be executed.

ARM Cortex M4 OS Support Features
* It has two stack pointers, MSP which is used for the OS kernel (used only in privileged mode and exception handling) and the interrupt handlers, and PSP which is used by application (threads execution) tasks.
* System tick timer (SYSTICK) which provides the RTOS with the system tick.
* SVC and Pend-SV exceptions are exceptions used for OS kernel.
* Unprivileged execution level only used for the application tasks, while the execution of the kernel should be done in privileged mode.
* We should use exclusive accesses for semaphore and mutual exclusive.
* SVC (Supervisory Call) interrupt handler should have the highest priority, since this handler is responsible for OS kernel handling.
* Any task can request a service like requesting a privileged access from the OS kernel through SVC SW interrupt request.
* Pend-SV(Pend-able service call) exception can be triggered by software but not precise like SVC since it shouldn’t be handled immediately. It has the lowest priority.
* Pend-SV is used for tasks context switching.
* SYSTICK timer is responsible for context switching trigger through Pend-SV ISR.
* Pend-SV shouldn’t interrupt any other interrupts, since it will delay the other interrupt execution which may lead to usage fault.
* Pend-SV is a popular interrupt which is used for low critical tasks.
* Application task shouldn’t do any critical section, it should ask the kernel through SVC call.
* We can do interrupt partitioning by executing the critical part in the interrupt and then execute the parts with no time restrictions in the pend-SV.
*  It is recommended to use a double-word alignment data memory (64 bit in ARM cortex) to accelerate memory operations.
* If there are two threads with same priority in the ready state. The RTOS should share the CPU across both threads using round-robin technique.
![[Pasted image 20250306234657.png]]

