GPIO pins

- led.c: functions to initialize and turn on/off leds
- led.h: prototypes

Perpheral group name -> specific register of peripheral     elaboration
`LPC_PINCON->PINSEL4 &= 0xFFFF0000;`

The user manual of the SoC does not describe:
- the list of components in the board 
- how the board components are connected to the SoC.

This information can be obtained only by reading the board schematic.
![[Pasted image 20241208175323.png]]

After discovering the interested CPU pins, you have to provide additional information to the SoC: 
- the pin functionality 
- the pin direction (input or output)

![[Pasted image 20241208175539.png]]
![[Pasted image 20241208175707.png]]![[Pasted image 20241208175725.png]]
![[Pasted image 20241208175735.png]]

```C
void LED_init(void) { 
	// PIN mode GPIO: 
	// P2.0-P2.7 are set to zero 
	LPC_PINCON->PINSEL4 &= 0xFFFF0000; 
	// P2.0-P2.7 on PORT2 defined as 
	// Output 
	LPC_GPIO2->FIODIR |= 0x000000FF; 
}

void all_LED_on(void) { LPC_GPIO2->FIOSET = 0x000000FF; } 

void all_LED_off(void) { LPC_GPIO2->FIOCLR = 0x000000FF; }
```