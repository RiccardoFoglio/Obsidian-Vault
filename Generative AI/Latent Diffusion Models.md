
Diffusion Model 
Deep Denoising Probabilistic Model - DDPM are the first steps towards diffusion models:
- Forward diffusion gradually perturbs the input data
- Reverse denoising process that learns to generate data through iterative denoising

![[Screenshot 2026-04-01 at 3.12.45 PM.png|500]]

Forward process gradually adds Gaussian noise over T Steps
![[Screenshot 2026-04-01 at 3.14.31 PM.png|500]]
![[Screenshot 2026-04-01 at 3.16.16 PM.png|500]]

Reverse Process: parameterized (learnable) function with weights learns to denoise
![[Screenshot 2026-04-01 at 3.18.50 PM.png|500]]
Our goal is to learn this function

DDPM From a Variational Prespective
![[Screenshot 2026-04-01 at 4.02.58 PM.png|500]]

Forward Process: sampling at time T (single pass, not incrementally, takes less time)
![[Screenshot 2026-04-01 at 4.04.38 PM.png|500]]

The expected value of a random variable is its weighted average, where each possible value is weighted by its probability of occurring.
It represents the "center of mass" of the probability distribution.
For a Gaussian distribution, it corresponds to the mean
![[Screenshot 2026-04-01 at 4.11.18 PM.png|500]]
![[Screenshot 2026-04-01 at 4.12.46 PM.png|500]]

- We don't need to iteratively add noise, we can directly sample the noisy image at each step of the forward process

Noise Schedule:
- Linear --> classic DDPM Default, image visually recognisable at low T, at T=0.5/0.7 most signal is gone --> destroys signal too fast
- Cosine --> Keeps more semantic structure at mid-noise levels, better gradient signal

![[Screenshot 2026-04-01 at 5.07.14 PM.png|500]]
eventually we'll get an image which is pure noise if T is large enough


The Reverse Process: goal is to approximate the true (unknown) reverse transition
![[Screenshot 2026-04-01 at 5.08.11 PM.png|500]]
This function is intractable, but we can make it tractable by conditioning on a clean data sample
![[Screenshot 2026-04-01 at 5.08.41 PM.png|500]]
This formulation is tractable and admits a closed form analytical solution

