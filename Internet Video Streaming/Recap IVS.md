
# Audio
## Audio and Voice Signals
### The Physics of Sound and Human Hearing
The document starts by defining sound in the physical world and how humans perceive it.
- Acoustic waves are simply variations in air pressure over time.
- The amplitude of a sound is measured as the difference between the average local atmospheric pressure and the specific pressure caused by the sound wave.
- The human ear can only perceive a subset of these waves, meaning we cannot hear ultra-sounds or infra-sounds.
- The human voice system can only reproduce a subset of the sounds that the human ear is capable of perceiving.
- Sound pressure level (SPL) is measured in decibels (dB) using the formula $SPL=10~log_{10}P/P_{0}$, where $P_{0}$ is the minimum perceivable intensity.
- Everyday sounds range widely: a whisper is about **20 dB**, normal conversation falls around **40 to 60 dB**, music ranges from **60 to 70 dB**, and a jet taking off reaches a damaging **130 dB**.
### The Shift from Analog to Digital
To process sound with a computer, continuous real-world analog signals (measuring time and intensity) must be converted into digital numbers (like 0s and 1s).
- This conversion process relies on A/D (Analog-to-Digital) and D/A (Digital-to-Analog) converters, also known as transducers.
- Digital signals offer far better noise immunity than analog signals, allowing them to be restored without distortion.
- Digital signals do not degrade over time and can always be reconstructed perfectly from scratch.
- Working in the digital domain is cheaper for storage, allows for faster processing and transmission, and enables complex operations like encryption.
### Sampling: Discretizing Time
Sampling is the process of converting a continuous-time signal into discrete time chunks.
- Under specific conditions, a discrete-time signal can carry the exact same information as a continuous-time signal.
- The sampling frequency is calculated as $f_{C}=1/T_{C}$.
- According to the Nyquist theorem, the sampling frequency must be at least double the maximum frequency present in the sound to properly reconstruct the signal, expressed as $F_{c}>2 F_{0}$.
- To reconstruct the continuous signal from discrete samples, the system uses interpolation via a low-pass filter.
- An ideal low-pass filter has an infinite slope and an impulse response defined by $Sinc(x)=sin(x)/x$.
- Because an ideal filter is anti-causal and impossible to achieve perfectly in reality, real filters approximate it, resulting in some ripple and a finite slope.
### Quantization and Pulse Code Modulation (PCM)
While sampling handles time, quantization handles the intensity (amplitude) of the signal.
- Quantization limits the representation of continuous intensity to a finite set of values, which inevitably causes some information loss.    
- This limitation creates a built-in "quantization error" when mapping a continuous value to the nearest discrete level.
- A uniform quantizer spaces its adjacent voltage levels equally.
- Linear PCM (Pulse Code Modulation) is the standard format for representing these signals, assigning a specific number of bits ($N$) per sample.
- Audio CDs use **16 bits**, telephone voice uses **12 bits**, and SuperAudioCD uses **24 bits**.
- The coding rate is calculated as the sampling frequency multiplied by the number of bits (Rate = Fc * N bit/s).
- For example, an audio CD's rate is calculated as 44100 Hz * 16 bits * 2 channels, resulting in 1,411,200 bit/s.
### Signal-to-Noise Ratio (SNR)
The presentation concludes by treating the unavoidable quantization error as "noise" to evaluate signal quality.
- The maximum quantization error is half of the quantization interval, or $\Delta/2$.    
- By treating quantization error as noise, we can calculate the Signal-to-Noise Ratio (SNR).
- SNR is calculated by comparing signal variance ($\sigma_{v}^{2}$) to noise variance ($\sigma_{e}^{2}$) using the formula $SNR=10~log_{10}({\sigma_{v}}^{2}/{\sigma_{e}}^{2})$ in decibels.
- Assuming a uniform distribution of noise, the SNR can be estimated as $SNR\sim6^{*}N-f({\sigma_{v}}^{2}/{X_{max}}^{2})$, where $X_{max}$ is the quantizer range.    
- A 16-bit CD audio track has a theoretical maximum SNR of 96 dB.
- However, if the audio does not use the full quantizer range (like very low-volume music), the actual SNR will be significantly lower than the standard $6N$ prediction.

## Voice Coding
### The Challenges of Telephone Voice
- The human voice covers a frequency range from roughly 100 Hz to between 5000 and 7000 Hz.
- To save bandwidth, telephone systems restrict this to a narrower window of 300 Hz to 3400 Hz.
- This specific bandwidth guarantees that the speech remains intelligible, though it sacrifices naturalness compared to the original voice.
- A basic digital telephone voice uses a sampling frequency ($F_{c}$) of 8000 Hz and 12 bits per sample, which yields a bit rate of 96 kbit/s.
### Uniform vs. Non-Uniform Quantization
- A uniform quantizer spaces its intervals equally, making it optimal only for signals with a uniform probability density function.
- Multimedia signals, including voice, do not exhibit uniform distributions; lower intensity values are much more probable, forming a Gamma distribution.    
- Non-uniform quantizers improve efficiency by placing more intervals where signal intensities are most probable.
- The Max-Lloyd theorem is utilized to compute the specific level distribution that will yield the maximum Signal-to-Noise Ratio (SNR).
- By using non-uniform quantization, 256 carefully distributed levels (8 bits) can deliver the same voice quality as 4096 uniform levels (12 bits).
### The ITU-T G.711 Standard (Log-PCM)
- Established in 1988, the ITU-T G.711 standard defines the Pulse Code Modulation (PCM) of voice frequencies at a rate of 64 kbit/s.
- It utilizes a semi-logarithmic quantizer (Log-PCM) to convert a 12-bit or 16-bit linear PCM signal into an 8-bit representation.
- The standard has two regional variants: A-law for Europe and µ-law for the USA.
- While G.711 features ultra-low complexity and virtually no delay (just 1 sample), it does not offer the lower bitrates required for modern applications like wireless GSM, which targeted 12 kbit/s.
### Differential PCM (DPCM) and Linear Prediction
- Voice signals typically feature a strong correlation between subsequent samples.
- Differential PCM (DPCM) exploits this by encoding only the difference ($d[n]$) between a current sample and the previous one.
- Because the variance of this difference is smaller than the variance of the absolute values, fewer bits are needed to achieve the same SNR.
- To maximize this gain, a prediction coefficient ($\alpha$) is introduced to better estimate the next value.
- The optimal $\alpha$ is the correlation coefficient between samples, which sits at about 0.9 for telephone speech.
- N-th order DPCM extends this by using a linear combination of $N$ past samples (typically between 8 and 12 for voice) to predict the future signal.
- The optimal coefficients for N-th order DPCM are calculated to minimize the variance of the prediction error using the Levinson-Durbin algorithm.
### Adaptive Approaches (APCM and ADPCM)
- Because media signals are highly non-stationary, using static, pre-fixed parameters over time is inadequate.
- However, speech can be considered stationary over short time windows of 5 to 20 ms, equating to about 160 samples.
- Energy-Adaptive PCM (APCM) dynamically changes the full-scale value of the uniform quantizer based on the local energy of the signal to save bits.
- ITU-T G.726 Adaptive DPCM (ADPCM), standardized in 1990, combines DPCM and APCM to compute locally optimal prediction coefficients.
- ADPCM drops the bit rate to 32 kbit/s but comes with high computational complexity and significant overhead (about 8% of the bitrate) to transmit the quantized coefficients to the receiver.
- To achieve good quality at bitrates below 32 kbit/s, pure waveform approaches like PCM and ADPCM hit their limit, and parametric approaches become necessary.
