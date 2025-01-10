Project Functionality
- button INT0 -> switch off all leds
- button KEY1 -> switch on LD4 – LD7
- button KEY2 -> switch off LD8 – LD11

![[Pasted image 20241208181841.png]]

- sample.c : stores the main function, which calls LED_init() and BUTTON_init()

- lib_button.c: inizialitation of buttons and NVIC 

INT0: p2.10 
KEY1: p2.11 
KEY2: p2.12

released = 1 
pressed = 0

For the selected pins, you have to indicate: 
- pin functionality: Pin Connect Block (PINCON) -> Pin Function Select Register (PINSEL) 
- the pin direction (input or output): GPIO -> Fast GPIO Port Direction control register (FIODIR)
![[Pasted image 20241208185918.png]]

- IRQ_button.c: handlers for external interrupts
![[Pasted image 20241208190930.png]]
![[Pasted image 20241208190944.png]]![[Pasted image 20241208191019.png]]

# Switch (de)Bouncing

• When you push a button, press a micro switch or flip a toggleswitch, two metal parts come together. 
• For the user, it might seem that the contact is made instantly. 
• That is not quite correct.

• Inside the switch there are moving parts.
• When you push the switch, it initially makes contact with the other metal part, but just in a brief split of a microsecond.
• Then it makes contact a little longer, and then again a little longer.
• In the end the switch is fully closed.
• The switch is bouncing between in-contact, and not in-contact

![[Pasted image 20241208192320.png]]![[Pasted image 20241208192329.png]]