Polling vs Interrupt

It is usually preferred to use Interrupt functionalities to be noticed about external events 
1. It is more **timing** efficient because the event is handled as soon as required, without any latency (this fundamental for safety) 
2. It is more **power efficient** because the system sleeps between requests (this is fundamental for mobile) 

Unfortunately, not all events are triggering interrupt
- Peripheral cores, inside the SoC, which are not connected to the Interrupt Controller (quite unusual)
- External devices connected to pins that cannot be configured as external interrupt sources (case of study: the joystick).

![[Pasted image 20241209161102.png]]
![[Pasted image 20241209161140.png]]

Polling implementation:

Software approach : `while(1) poll(register);`
--> Low latency Power inefficient

Timer based approach :
- To trigger an interruption at regular intervals
- The system sleeps while the timer is counting
--> Trading-off Latency and Power efficiency


The LandTiger LPC17XX board features a 5 way digital joystick (SW5). 
The joystick may be used for example to select options in a menu shown on the LCD. 
Each direction (up, down, left, right) and the Select function are connected to a dedicated digital input pin on the LPC1768.
Multiple keys can be pressed at the same time (e.g. up and right). 
Input pins are active low when a key is pressed. 
The input pins are hardware debounced.

Limitations:
The joystick is connect to pins not owning external interrupt capabilities 
Therefore the only way to read its value is to 
	Setup the pins connected to the 5-way joystick actuators as 
		GPIO 
		in input direction 
	Poll the GPIO register value
		Retrieving a full port value
		Then making the proper bits to selectively notice a change of status

![[Pasted image 20241209161737.png]]

The functionalities of the RIT (Repetitive Interrupt Timer) are used to implement the polling functionalities 
- Every time (50ms) the RIT triggers an interrupt 
- This timing looks fine for interfacing human a behavior (finger pressure) 
- Importantly, the input pins are hardware debounced

![[Pasted image 20241209161830.png]]

In the RIT Handler 
A. The value of the port GPIO1 is read 
B. If the value of bit 25 is 0 then 
1. If it is the first time: an action is performed 
2. If it is a pressure repetition, no action is performed (push button functionality).

![[Pasted image 20241209161855.png]]

![[Pasted image 20241209162014.png]]

![[Pasted image 20241209161906.png]]

## Button and joystick by using the RIT only
![[Pasted image 20241209162105.png]]