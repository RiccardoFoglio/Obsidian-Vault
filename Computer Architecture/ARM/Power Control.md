avoid infinite while loop, it's expensive
3 methods:
- Reduce CPU clock frequency : system goes slower, so saves power
- shut down some peripherals
- Power Control Mode : sleep mode, deep sleep mode etc...

Registers
PCON :  set the Power Control Mode
PCONP : shut down peripherals

Sleep Mode:
- The clock to the core is stopped. no more power used by the processor, memory systems and its controllers, and internal buses.
- The SMFLAG bit in PCON is set
- Peripheral functions continue operation. they may generate interrupts to cause the processor to resume execution.
- Wake-up: when any enabled interrupt occurs

Deep Sleep Mode:
- All clocks are stopped, except IRC and RTC. all dynamic operations of the chip are suspended.
- The DSFLAG bit in PCON is set.
- Wake-up: when some interrupts occur, like
	- external interrupts EINT0-EINT3 
	- watchdog timer (driven by IRC clock) 
	- RTC alarm interrupt 
	- GPIO interrupts
	- NMI.

Power-Down Mode
- Same as deep sleep mode; in addition IRC is stopped and flash memory is turned off : higher power savings, but waking-up takes more time: flash operations must be resumed before a code or data access in the flash memory.
- The PDFLAG bit in PCON is set.
- Wake-up: same kinds of interrupt as deep sleep mode.

Deep Power-Down Mode
- The entire chip is powered off, except: 
	- RTC (real-time clock) and its backup registers 
	- RESET pin
	- Wake-up Interrupt Controller
- The DPDFLAG bit in PCON is set.
- Wake-up: 
	- external reset 
	- RTC alarm interrupt.


Two instructions suspend execution and enter a low-power state: 
- WFI: Wait For Interrupt 
- WFE: Wait For Event
With both instructions, the processor remains in low-power state until it detects an interrupt.
In a multiprocessor system, the low-power state after WFE can be exited also when another process executes a SEV instruction.

![[Pasted image 20241208201721.png]]

## Enter Sleep Mode
```C
int main (void) { 
	LED_init(); 
	BUTTON_init(); 
	LPC_SC->PCON &= 0xFFFFFFFFC; 
	SCB->SCR |= 0x2; /*set SLEEPONEXIT*/ 
	__ASM("wfi"); 
}
```

## Enter Power-Down Mode
```C
int main (void) { 
	LED_init(); 
	BUTTON_init(); 
	LPC_SC->PCON |= 0x1; 
	LPC_SC->PCON &= 0xFFFFFFFFD; 
	SCB->SCR |= 0x2; /*set SLEEPONEXIT*/ 
	/* SCB->SCR |= 0x4; not required */ 
	__ASM("wfi"); 
}
```