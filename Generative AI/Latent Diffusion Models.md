
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
This function is intractable (can't compute it), but we can make it tractable by conditioning on a clean data sample
![[Screenshot 2026-04-01 at 5.08.41 PM.png|500]]
This formulation is tractable and admits a closed form analytical solution

Assuming the reverse transition is Gaussian
![[Screenshot 2026-04-01 at 5.19.36 PM.png|500]]

Diffusion Model often use U-NET architectures with residual blocks, skip-connections and self-attention layers
![[Screenshot 2026-04-01 at 5.51.33 PM.png|500]]
The time step is given as input, the reverse function depends on the step

U-NET works well for denoising:
- multi-scale feature fusion
- respects spatial structure

Bottleneck: lowest resolution (8x8): self-attention for global coherence


Training the Reverse Transition
![[Screenshot 2026-04-01 at 5.53.52 PM.png|500]]
Calculate noise over samples and take the average
To calculate the Loss:
![[Screenshot 2026-04-01 at 5.54.13 PM.png|500]]


Training Loop in practice:
1. Sample noise to ad to each image in the batch
2. Sample a random timestep for each image t
3. Add noise to the clean images according to the noise magnitude
4. Predict the noise residual
5. Compute the MSE loss
6. Update the gradients
(No need to compute the entire chain for each image -- we can predict the noisy image at an arbitrary step)
![[Screenshot 2026-04-01 at 5.59.38 PM.png|500]]

At inference time
![[Screenshot 2026-04-01 at 6.00.33 PM.png|500]]

# Why Latent Diffusion Models?

cost for a sample of an image 1024x1024x1000 timesteps?

Computational cost scales with resolution per each timestep t
DDPM originally worked at 32x32 or 64x64, so scaling to high resolution is extremely expensive
Each denoising step requires a full forward pass of large U-NET over the full spatial resolution
1000 sequential steps: real-time generation at high-res is infeasible on a single GPU

Realism = Structure + Detail

![[Screenshot 2026-04-01 at 6.14.51 PM.png|500]]
![[Screenshot 2026-04-01 at 6.15.38 PM.png|500]]

Latent Diffusion Model (LDM): first compress image to latent z with a perceptual VAE then run diffusion entirely in latent space
![[Screenshot 2026-04-01 at 6.19.03 PM.png|500]]
Model doesn't work in pixel space (very large costs) but in a compressed latent space (not vectors but feature maps!)

From pixel to latent space:
- 2-stage training: original model
	1. Train a perceptual autoencoder to compress images to z and reconstruct faithfully
	2. Train a DDPM entirely in the low-dimensional latent space z
- One-stage training: possible in more recent models

- Reduce computational load: 64x64 latent vs 512x512 pixel --> 64x fewer operations per diffusion step!
- Flexible method: change domain by changing the VAE Block

The VAE Component: Adversarial Discriminator (improve fidelity)
![[Screenshot 2026-04-01 at 6.25.42 PM.png|500]]
Regularize latent space:
1. Use KL-divergence w.r.t. to prior 
2. Discretize encoding as in VQ-VAE

# Putting Everything Together

Original Architecture
![[Screenshot 2026-04-01 at 6.28.10 PM.png|500]]

![[Screenshot 2026-04-01 at 6.47.46 PM.png|500]]

**Cross-Attention Mechanism** (for text/class inputs) : text converted into embeddings (vectors) via a language model (like CLIP) and injected into the denoising U-NET through cross-attention layers

**Concatenation Mechanism** (for Spatial Inputs) : When the input is spatially aligned (image-to-image translation, inpainting, or semantic maps) the switch directs the input to be concatenated directly to the noisy latent input of the U-NET

Semantic Synthesis
![[Screenshot 2026-04-01 at 6.51.56 PM.png|500]]

Training at scale: Exponential Moving Average

Latent diffusion models are heavy model, small batch sizes create instabilities during training
During training, 2 copies of the U-NET are maintained in parallel
- $\theta$ (optimizer weights) : update at every gradient step via Adam, noisy, can fluctuate
- $\theta_{EMA}$ (EMA weights) : updated as a running average, smooth stable trajectory through parameter space
![[Screenshot 2026-04-01 at 6.55.42 PM.png|500]]

Diffusion models are successful because:
- **Sample Quality** : progressive denoising yields sharp, high-fidelity images. Early steps determine global structure, later steps add fine detail
- **Diversity** : stochastic reverse process explores the full data distribution. No mode collapse unlike GANs
- **Training Stability** : fixed encoder, only the denoising decoder is learned. Well-conditioned denoising subproblems at each noise level

