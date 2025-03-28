* The scheduler is a part of the kernel responsible for determining which thread will run next. Most real-time kernels are priority-based. 
* Types of the priority-based kernels:
	1. Non-preemptive Kernel.
		* Each thread should explicitly give up control of the CPU. 
		* Low interrupt latency (no preemption will occur).
		* Non-reentrant functions can be used without any fear since the shared resources between threads will not affected due to non-preemption methodology. 
		* Need to only guard shared resources between interrupts and tasks. 
		* Lower responsiveness and not recommended for real-time applications especially hard real time ones. 
	2. Preemptive Kernel.
		* very high system responsiveness. 
		* Highest priority thread ready to run is always given control of the CPU.
		* Current thread is preempted when a higher priority thread is ready instantly. 
		* Non-reentrant functions shall not used without exclusive access. 
		* Lower priority threads may never be run (Starvation).
* The scheduler can use the round robin scheduling allowing equal time slices share of the CPU in case of many ready threads have the same priority. 
