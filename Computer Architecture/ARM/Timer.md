Timers are peripherals for synchronizing the system, based on counting
- to wait for a delay time 
- to perform operations at regular time

Usually, when a timer ends counting, the system needs to react
- Typically an interrupt handler is entered.
- While timer counts, the CPU can enter a low power mode

![[Pasted image 20241209153816.png]]

$$count = time [s] * frequency [1/s]$$
$$count = 10 [s] * 25*10^6 [1/s]  = 25*10^7 = 0x0EE6B280$$

If a timing request is large, the count value could not fit in the timer register.
Hardware and software features can be used to address this issue
- HW – Cascade of counters
- HW – Prescalers
- SW – Handler software count of HW events

Types of Timers:
- System Tick Timer: basic timer with 24-bit counter. If the CPU clock is set to 100 MHz, the default interrupt rate is 10 millisecond.
- Capture/Compare timers 0-3: standard timers with 32-bit counter. A prescale counter can change the interrupt rate
- Repetitive Interrupt Timer: it generates repeating interrupts at specified time intervals, without using a standard timer

- PWM: it extends the standard timers for digital signal modulation. 
- Motor Control PWM: optimized timer for three-phase AC and DC motor control applications.
- Watchdog timer: it resets the microcontroller within a reasonable amount of time if it enters an erroneous state.
## Standard Timer

The Timer/Counter is designed to count cycles of the peripheral clock (PCLK) or an externally-supplied clock.
It can optionally generate interrupts or perform other actions at specified timer values, based on four match registers. 
It also includes four capture inputs to trap the timer value when an input signal transitions, optionally generating an interrupt.
## Timer Counter (TC)

The Timer Counter counts clock cycles. it is incremented when the prescale counter reaches its terminal count. 
It counts up through the value 0xFFFF FFFF and then wrap back to 0x0000 0000.
When it reaches the upper limit, it does not generate an interrupt:
- a match register should be configured. 
- It can be reset before reaching its upper limit.

## Timer Control Register (TCR)

The lowest two bits of TCR control the operation of the Timer/Counter:
- Bit 0 = 1 -> enable timer for counting.
- Bit 1 = 1 -> reset counter.

## Match Registers and Match Control Register

The Match register values are continuously compared to the Timer Counter value.
When the two values are equal, actions can be triggered automatically:
- to generate an interrupt
- to reset the Timer Counter
- to stop the timer

Actions are controlled by the settings in the Match Control Register
![[Pasted image 20241209155142.png]]
Bits 3-5, 6-8, and 9-11 behave in the same way for MR1, MR2, and MR3 respectively

3 possible behaviors:
- Continuous operation with optional interrupt generation on match
- Stop timer on match with optional interrupt generation
- Reset timer on match with optional interrupt generation

## Interrupt Register

The Interrupt Register consists of
- 4 bits for the match interrupts
- 2 bits for the capture interrupts.
If an interrupt is generated, the corresponding bit in the IR is high, otherwise the bit is low
Writing 1 to the corresponding IR bit will reset the interrupt, writing 0 has no effect

### Prescaling

The Prescale register stores a 32-bit value
The Prescale Counter is incremented on every PCLK

When the Prescale Counter reaches the value stored in the Prescale register:
- the Timer Counter is incremented
- the Prescale Counter is reset on the next PCLK

Example: Prescale register is set to 1, the Timer Counter is incremented every 2 PCLKs

## External Match Register

For every match register, the external match register has
- an output bit (status of match)
- a pair of configuration bits (functionality control).

When a match register equals timer counter, the related bit in the external match register changes according to the configuration bits

The 4 least significant bits of external match register are sent to MAT output

### Registers for capturing an event

- 2 Capture Registers: CR0 and CR1.
- 4 Capture Control Registers (one for each pin of CAP input).

Transition of single CAP pin can be monitored
- rising edge and/or
- falling edge

In such a case, two actions are supported
- Timer Counter value is stored in a capture register
- an interrupt is generated

Bits in Capture Control Register:
- Bit 0 = 1: store TC on CR0 on rising edge of CAP pin
- Bit 1 = 1: store TC on CR0 on falling edge of CAP pin
- Bit 2 = 1: generate an interrupt with CR0 load
- Bit 3-5 are the same for CR1

## Count Control Register

Selects between Timer and Counter Mode
- Timer mode
	- Prescale Counter is incremented on every PCLK
	- when the Prescale Counter reaches the value of Prescale register, Timer Counter is incremented
- Counter mode
	- a selected bit of CAP is sampled on every PCLK
	- is the expected transition of CAP bit is recognized (rising and/or falling edge or no change), then the Timer Counter is incremented

# Library

```C
int main (void){
	LED_init();
	BUTTON_init();
	init_timer(0, 0x017D7840);
	emable_timer(0);

	LPC_SC->PCON |= 0x1;
	LPC_SC->PCON &= 0xFFFFFFFD;
	__ASM("wfi");
}
```

Configuring MCR
![[Pasted image 20241209160641.png]]
![[Pasted image 20241209160823.png]]

