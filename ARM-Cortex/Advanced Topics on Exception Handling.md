![[Pasted image 20250324233034.png]]*Procedure Call Standard for ARM Architecture (AAPCS)*
* Any C Function can modify R0 to R3, R12, R14(LR), and PSR which are called as caller saved registers. (No need to store them in case of the caller or callee function). --> NB: Caller function calls the callee function. ( We should take care that any function can update them)
* C function shall save R4 to R11 (in case of usage) to the stack before use and restore them back after use (callee-saved registers).
* Any C function shall use R0 to R3 for the function attributes, and use R0 for return in case of 32-bit variable and use R0 and R1 in case of 64 bit variable return. 
* While stacking, the CPU push the function arguments to R0-R3 then trigger SVC call which push these registers to the stack, so the stack frame can be used internally in the handler without any issue, just keep in mind the order of the stack frame.
 
![[Pasted image 20250306204632.png]]


*Exception Handlers
* Exception handlers and ISRs are normal C functions.
* Processor automatically stack R0 to R3, R12, LR, and PSR at exception entrance and unstack them at exception exit (The exception may interrupt a normal C-function while doing an operation on any register).
* In normal C function the return address (PC) is stored in (LR) before function call, but in case of exception this not happening, so the processor shall store the EXC_RETURN in (LR) automatically at exception entry.

*Stacking 
* Once the interrupt request arrives, the processor should finalize the instruction being executed then start do stacking sequence then fetch the corresponding vector for the requested interrupt then start executing the ISR.
* The interrupt latency is the time between the moment of the interrupt request arrival and the starting of ISR execution.

*Tail Chaining
* When a lower interrupt was being set to pending state due to pre-emption from higher priority interrupt, the processor will not execute the unstacking since it knows that there is a pending interrupt so it will do tail chaining to avoid redundant unstacking then stacking sequence.
![[Pasted image 20250306212111.png]]

*Late Arrival 
* The NVIC is able to continue stacking process in case of late arrival for higher priority interrupt to avoid redundant unstacking-stacking sequence but it is smart enough to fetch the vector of the higher priority interrupt.
![[Pasted image 20250306212436.png]]

*Pop Preemption
* If a new interrupt is arrived while unstacking sequence is on going, the NVIC is smart to stop the unstacking sequence and fetch the vector of the arrived interrupt.
![[Pasted image 20250306212545.png]]

*Supervisory Call  Exception (SVC) 
Step#1
* When an SVC instruction is executed, it generates an **exception** that transitions execution from **Thread Mode** to **Handler Mode**. The return from the **SVC handler** follows the same **EXC_RETURN** mechanism as other exceptions. 

*Step#2 
* The processor saves the core registers automatically and the following registers are pushed onto the active stack (PSP or MSP, depending on EXC_RETURN). the stacking happens automatically before entering the SVC handler (We can check the data of these registers after stacking by going to memory stack and check the address of the used stack pointer before interruption and navigate to it in the memory section while debugging then u can see the values of the following registers).
	1. R0-R3, R12.
	2. LR (link register)
	3. PC
	4. xPSR
* Processor fetches the exception vector and load it to PC to begin execution in Handler Mode.
* The xPSR is pushed first then PC --> LR --> R12 --> R3-R0 (in reverse order while stacking)

*Step#3
* Reads the link register (EXC_RETURN it is a part of the link register which is a value stored in the LR after stacking and no more contains the return address for the interrupted code since the return address is pushed from LR to the stack while stacking) to determine the previous mode, that indicates whether the exception was triggered from Thread mode using PSP or MSP.
* Extract the SVC Number since each number can be mapped to a specific functionality (request type) needed from the SVC call.
	 svc_number is get by adding the SP address by 24 byte to get the stacked program counter value and then subtract it by #2 to get the svc request number// Fetch SVC number from stack this is done by reading the PC address that is the address of the next instruction to be called after svc #0, after subtraction by #2 we should fetch this instruction from the program memory and we can get the value. 


NB: if a normal function is executing in thread mode it will always have a return address to the caller function (which called it) so in case of exception is triggered, the processor should stack the PC for the next instruction to be executed in the normal function after return from the handler ISR and also the LR should be stacked and pop again after return from the ISR so the normal function can return to the caller (parent) function which called it. 

*Step #4 
* We should branch back from the SVC handler using LR which contains the EXC_RETURN value.


*C-Function Call in Exception 
* When we try to call a normal function inside the SVC exception handler, the function should have pointer to a word as an attribute, so the function will be passed by the stack pointer address (Before stacking to the function) to can access the exception stack frame.  
* The CPU push the value of the PC at the moment of function call to the LR, so the LR now is corrupted if it was called from exception --> We need to store the EXC_RETURN first before calling any function in the exception handler. 


*Memory Alignment*
* The arm cortex-M4 mandated the two word alignment in case of task stacking and context switching to ensure higher efficiency. 
* It needs two word alignment for exception entry since it is mandated by the ARM architecture procedure call standard (*APPCS*) where the SP must be aligned to two word before entering an exception, in case of not alignment, the bus exception fault may be generated and will be escalated to hard fault.
* It provides efficient context switching and compliance with ARM architecture.