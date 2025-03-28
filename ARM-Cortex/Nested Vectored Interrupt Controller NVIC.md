Exceptions and Interrupts: 
* Exceptions are events that cause changes to program flow.
* Exception Handler is executed when exception happen.
* Interrupts in ARM architecture are one type of exception generated from peripheral, external inputs, or triggered by software while the other exceptions are generated from the CPU itself.
* Interrupt Service Routines (ISR) is executed when interrupt happen. 
* Interrupt can be disabled completely or masked (be in pending state till to be un-masked by SW).
* The non-maskable exceptions are some exceptions that can't be masked and should be active if they have the highest priority. 
![[Pasted image 20250306000628.png]]
Nested Vectored Interrupt Controller (NVIC):
* Provide flexible exception and interrupt management.
* Nested exception/interrupt support. 
* Vectored exception/interrupt entry.
* Interrupt masking.
* Exception escalation if one of the exceptions isn't enabled or masked, the fault will be escalated to hard fault exception.
* The interrupt/Exception active state can be read in the SW. 
* If interrupt is interrupted by another higher priority interrupt, the lower one will be in active state but it isn't being serviced.  

Vector Table: 
* It is an array of word data (32 bit address aligned since most of ARM cortex M4 based controller are 32 bit based architecture) inside the system memory, each representing the starting address of one exception handler.
* Relocatable through the vector table offset register.
* The order of exceptions in the interrupt vector table should be fixed across all arm-cortex based controller while the interrupts are listed according to the micro-controller manufacturer (HW connection of the interrupts to the NVIC). 
* The base address of the new vector table must be aligned to the size of the vector table extended to the next larger power of 2 (minimum alignment is 128 bytes since first 7 bits are reserved).
![[Pasted image 20250306001318.png]]
Priority:
* The higher priority exception can pre-empt a lower priority exception.
* Cortex M4 processors support three fixed highest priority levels and up to 256 levels of programmable priority (with a maximum of 128 levels of pre-emption 8-bits).
* The actual number of available programmable priority levels is decided by the micro-controller manufacturer (in STM32F4, only 4 bits are used for pre-emption priority levels and it doesn't have sub-priority fields). 

CORE Registers for Exception or Interrupt Masking
* PRIMASK is a register used to mask all interrupts/exceptions except the reset, hard fault, and NMI exceptions.
* FAULTMASK is a register used to mask all interrupts/exceptions except the reset and hard fault exceptions. 
* BASEPRI is a register that can be used to set the minimum priority needs to be masked, so if any lower priority than this register value will be masked.

Interrupt/Exception Workflow Configuration
1. Set up the priority group setting.
2. Set up the priority level of the interrupt.
3. Enable the interrupt at the NVIC and at the peripheral.
4. Configure the interrupts at MCU peripheral.
5. Interrupt shall be triggered by peripheral or from the software.