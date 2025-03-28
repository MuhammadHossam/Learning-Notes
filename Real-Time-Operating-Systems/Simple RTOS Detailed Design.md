### *Core Design* 
* it will have a linked list for ready threads which is an array of number of possible priorities each index in the array will be another list for each priority since we can have different threads in the ready state with the same priority. 
* The linked list consists of the following attributes:
	1. Number of items in the list. 
	2. Pointer to the item with the highest priority to be easy fetched and accelerate the context switching time. 
	3. Each list should have a head for each list with two pointers; previous and next pointers to have more flexible linked list in adding and deleting new items. 
* The item should have the following attributes:
	1. Pointer to the next item.
	2. Pointer to the previous item.
	3. Pointer to the thread (TCB) of the item. 
	4. Pointer to the list where the item is pushed (Ready list or waiting list).
* Each thread have the following attributes: 
	1. Stack pointer.
	2. Priority.
	3. List item of the thread (Each thread may have multiple items). 
* The stack of each thread is an array of double word with configured size. 

### *SVC Call Routine*
* The RTOS can use the MSP or PSP in handling thread stacks, When an **exception (interrupt or fault) occurs**, the **LR (R14) register** holds a special **EXC_RETURN** value that tells the processor which stack was used before the exception. 
	1. Bit[2] = 0 --> Main stack pointer was used before the exception is triggered.
	2. Bit[2] = 1 --> Process stack pointer was used before the exception is triggered.
	NB: The MSP is used for both handler and thread modes by default, we can switch to the PSP in the thread mode but through assembly instruction. --> Check the manual.
* The user should request any functionality from the RTOS using SVC request, for example; the user can call the SVC_ThreadCreate funtion but inside it should request SVC call with the corresponding SVC request so internally in the SVC exception the RTOS responsible to call the create thread function with full privilege. 
* The arguments will be passed using the SVC_ThreadCreateFunction and will be pushed to R0-R3 and when we request SVC request they will be pushed to the stack and can be retrieved inside the handler easily or from the internal thread create function using argv array for the stack frame.  --> The function arguments are pushed to R0-R3 accordingly and these registers are the **first** 4 stack data to be retrieved using the stack pointer.
* List Initialization Sequence
	1. Initialize number of items to zero.
	2. initialize the end item to be pointing to itself through next and previous pointers to be pointing to the end item itself. 
	3. initialize the head item pointer to be pointing to the end item.
	



