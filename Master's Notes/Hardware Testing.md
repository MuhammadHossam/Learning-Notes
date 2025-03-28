Bring-up Phase #3 (Current Controller testing)
* PI controller is working as expected.
* Modulated model predictive control isn't working on the HW for all phases. 

PIL Testing Test Steps
1. Develop the model needed to be run on the controller. 
2. Right click -> Deploy this subsystem to HW.
3. The generated model will be initiated in new simulink model. 
4. Open the APPS tab and chose SIL/PIL Manager.
5. Select SIL/PIL only. 
6. System under test -> Model blocks in SIL/PIL.
7. Run in Normal Mode. 
8. Run SIL/PIL 
9. if u faced any issues try to clear mex and delete the generated files. 

PIL Results 
* MPC and PI are working as expected in bring-up phase #3. 
* The concern now is the state flow generated code issues with model references (possible problem).
Next Steps
* Create a model for phase #3 only to run on the target on HW without any model references and see the results. ---> Done 

MOTOR TUNING
* Measure the motor phase resistance and inductance: 
	1. Apply constant current on the motor --> Vpowersupply = 1.86V at 0.5A consumed, Rtotal = 3.72ohm, Rtotal = Rphase/2, Rphase = 7.44ohm. 
	2. 0.1V --> 1A, ?? --> 0.3A
	3. R = 3.7ohm, L = 0.01 
	real32_T MCTL_tuneInducLoc = 0.0037;
	real32_T MCTL_tuneResLoc = 1.25;


Modulated Model Predictive Control Testing on HW 
* Phase currents at 60 rpm using sensor. 
![[Pasted image 20250323110437.png]]

![[Pasted image 20250323115749.png]]
PI Controller Testing on HW: 
* Phase currents at 60 rpm using Sensor.
![[Pasted image 20250323113353.png]]
* Motor Torque / Speed in RPM
![[Pasted image 20250323114128.png]]