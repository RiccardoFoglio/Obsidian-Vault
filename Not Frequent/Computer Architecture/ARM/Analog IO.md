Analog and Digital conversion

When dealing with an analog value that needs to be processed by a digital system, an analog-to-digital converter (ADC) will be imperative

The opposite can also be done, using a digital-to-analog converter (DAC)


The digitization of analog signals involves the rounding off of the values which are approximately equal to the analog values.
The method of sampling chooses a few points on the analog signal and then these points are joined to round off the value to a near stabilized value.
Such a process is called as **Time and Amplitude Quantization**.

Time Quantification is obtained by sampling the analog signal at discrete points in time
These points are usually evenly spaced in time, with the time between being usually referred to as the sampling interval
The minimum sample interval or maximum conversion rate depends on the specific ADC circuit.

The amplitude quantization of an analog signal is done by discretizing the signal with a number of quantization levels 
The spacing between the two adjacent representation levels is called a quantum or step-size 
The resolution of an A/D converter (ADC) is specified in bits and determines how many distinct output codes (2n ) the converter is capable of producing


A 12-bit ADC is on-chip in the LPC1768 device 
- 12-bit successive approximation analog to digital converter 
- Input multiplexing among 8 pins 
- Power-down mode 
- Measurement range VREFN to VREFP (typically 3 V) 
- 12-bit conversion rate of 200 kHz 
- Some advanced usage mode like 
	- Burst conversion mode for single or multiple inputs.
	- Optional conversion on transition on input pin or Timer Match signal.

![[Pasted image 20241209184903.png]]

## ADC connection on board
![[Pasted image 20241209185154.png]]
Adjustable potentiometer VR1 is connected to analog channel P1.31 (AD0.5). 
JP12 jumper is used to enable the potentiometer input.
VR1 setting provides input voltages between 0V and 3V3 to the ADC.
![[Pasted image 20241209185724.png]]
![[Pasted image 20241209190534.png]]
![[Pasted image 20241209190748.png]]

ADC_IRQHandler()
![[Pasted image 20241209190804.png]]

![[Pasted image 20241209191003.png]]

## Digital to Analog Converter

Similarly to A/D Conversion, a Digital value can be converted into an analog one using a DAC
Range :0 to 3.3V 
Resolution : Depends of the number of bits of the DAC, 3.3V/15 = 0.22V 
Precision : n bits • $2^n$ alternative --> $2^4 = 16$ alternatives

DAC connection on board : External speaker circuit is connected to DAC output pin P0.26.
The DAC output is enabled by JP2 jumper.z
![[Pasted image 20241209200202.png]]

Sound can be obtained by repeatedly feeding the loudspeaker with a sampled sinusoid.
![[Pasted image 20241209200217.png]]

Loudness and pitch : Controlled by amplitude and frequency
Humans can hear from about 25 to 20,000 Hz
A musical tone (note) produced by a sinusoid of a particular frequency
![[Pasted image 20241209200254.png]]

What is a musical tone? A sinusoid of a particular frequency. Notes vary by twelfth root of 2 ~ 1.059
What would the samples be? Fixed point numbers
How do we generate a sinusoid? Output appropriate digital values via a resistor network that effectively produces an pseudo-analog signal
What about frequency? Employ a programmable timer to tell us when to output the next value
Tempo defines note duration
Quarter note = 1 beat
120 beats/min => ½ s duration
![[Pasted image 20241209200417.png]]Chord: Two notes at the same time = Superimposed waveforms, 262 Hz (low C) and a 392 Hz (G)
![[Pasted image 20241209200444.png]]

![[Pasted image 20241209200511.png]]