
Reverb: 

Audio signal has mean = 0, processed in floating point between -1 and 1, but file in 16 bit so goes from 0 to 2ˆ16 = 65536 mapped to another scale -1 to 1

audio = variation of pressure in the air, tends to come back to 0, if not it has an offset (can be removed to clean audio)

normalization can peak maximum of signal to 1 --> everything amplified and rescaled to use all range

Piano  ??  (Impulse Response) == Reverberant Piano
	we use a convolution as operation


2 applications:
- Discriminative: classification and tagging --> done with CNNs
- Generative: source separation (generative bc you create new audio files)

2 approaches: 
- with domain knowledge: spectrogram based
- without domain knowledge: raw audio (end-to-end)

Images = 2D signal (x,y)
Audio = 1D signal --> huge, raw audio 16-bit sample, sampling rate: 44.1kHz = 704k bit/s

from 20Hz to 20kHz (= Nyquist frequency SR/2 ) is the range --> sample to 44.1kHz
able to preserve frequency up to 20kHz if we sample up to twice this value to avoid aliasing.

2D representation of audio, tried to reduce input size --> reduce sample rate and resample to 22kHz or 8kHz for voice
with 2D representation = spectrogram
![[Screenshot 2025-11-25 at 7.14.21 PM.png]]

waveform converted to spectrogram = compact matrix that is an image (colors are an effect, it's grey scale)
Time sampled in portions, and height = frequency (Hz)

difference between image and spectrogram:
- 1 channel (S) instead of 3(I)
- in spectrogram: 2 different dimensions, one time and one frequency, 2 different meanings
  in image: all spatial
- Perceptual scaling: Images are linear, spectrogram is log-scaled

1 octave = 12 semitones, linear scale, but in frequency these are logarithmic

STFT (short time fourier transform) analyzes short period of time of the signal --> better than analyzing the whole signal --> putting them all together will analyze the whole signal

$\Delta t$ = length of window --> M_TIME is the discrete steps of each time slot
$\Delta f$ = Sampling rate / NFFT (number of divisions of the frequency range that we have values for)

Data Augmentation:
- translate in time
- use different "resolutions" in time, frequency, sampling frequency
- add random or sampled noice or reverb
- change pitch and time
- AVOID flipping, rotation, blur (?)

Time-shift and time-stretch are different:
- **time-shift** is just a translation, speak at a different time (duration doesn't change)
- **time-stretch** is a transposition, speak slower or faster, **only duration changes not pitch**

**Pitch-shift**: sound pitch is shifted up or down **without changing tempo** 
(chipmunk effect) speed change is not useful


(jump all slides to 83)
## Classification and Tagging

1-D Approach: convolution matrix that expands all the frequencies moved in time, avoid invariance in the frequency

2-D Approaches: 12kHz of frequencies divided in 256 points, 21ms as delta T, then decide how many levels etc...
Mel-Spectogram can reduce to 96 fro 256 because we can have less notes in octaves than 256
MFCC is even more condensed, but more work beforehand is required

Music Tagging : uses RNN, time is the dimension not space

## End-to-End learning: Raw Audio

learn from waveform: very small CNN 1 dimensional blocks, frame size of 3 samples, stride of 1 sample


some applications in audio classification used the same network from videos but adapted spectrogram, like VGG

Sound source separation: similar to semantic segmentation, here we separate sources of sound from the sound itself
when mix sounds --> they add and mix together, sum of N sources

![[Screenshot 2025-11-25 at 10.54.00 PM.png]]

can be used to denoise, isolate vocal and instrumental...

challenges:
- image object are separated, occupy different pixels, but in sound they overlap --> soft mask
- separation must maintain temporal coherence
- spectrogram based approach needs a resynthesis step


Wave --> STFT --> spectrogram --> magnitude to DNN -->  time frequency masking + Phase Spectrogram --> Inverse STFT --> separated sources

Time-frequency mask: grey scaled, regression used, pixel by pixel multiply  

audio is very sensible, so mask needs to be good

Wave-U-Net is the adaptation to use waveform instead of spectrogram