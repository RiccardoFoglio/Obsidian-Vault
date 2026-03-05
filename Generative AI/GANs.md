![[Screenshot 2026-03-05 at 9.41.51 PM.png]]

Discriminative: probability that the class label is correct given a feature
Generative: random input and class generates a feature

**Manifold Hypothesis**: Natural (high-dimensional) data actually lies in a low-dimensional space
![[Screenshot 2026-03-05 at 9.43.50 PM.png]]
Goal: find a representation that concentrates only on natural images (data) and captures semantic similarity

The domain is a manifold in the original feature space:
- Manifold = subspace embedded in the original space
- Example: space of faces is a manifold of the space of possible RGB images

Manifolds have a smaller intrinsic dimensionality than the input space:
- can be presented by a compact feature vector
- these features can be complicated functions of the original input space

![[Screenshot 2026-03-05 at 9.46.58 PM.png]]

How to train a GAN: the two models compete in a aminimax game: 
- discriminator tries to maximize its reward --> correctly classify real vs fake images
- generator seeks to minimize the discriminator's reward --> fool the discriminator
backprop error to update discriminator weights

Notation
![[Screenshot 2026-03-05 at 9.50.28 PM.png]]
Sampling
![[Screenshot 2026-03-05 at 9.51.05 PM.png]]
The distribution approximation depends from the characteristics of the training set: most common samples in the training sets are more likely to be generated

Latent Space Interpolation
Interpolating in latent spaze (z space → noise vector space) yields meaningful outputs

sample morphs from one sample to another by interpolating
![[Screenshot 2026-03-05 at 9.53.28 PM.png]]

![[Screenshot 2026-03-05 at 9.56.10 PM.png]]![[Screenshot 2026-03-05 at 9.58.23 PM.png]]

Training the D
![[Screenshot 2026-03-05 at 9.59.30 PM.png]]
![[Screenshot 2026-03-05 at 9.59.39 PM.png]]
Training the G
![[Screenshot 2026-03-05 at 10.02.10 PM.png]]
![[Screenshot 2026-03-05 at 10.02.21 PM.png]]

Training is unstable
both models should improve together, if D is too good or bad, training collapses
D-s accuracy should stay around 50% (guessing)

when D is too confident the gradient for G goes to zero
![[Screenshot 2026-03-05 at 10.07.32 PM.png]]
alternative formulation --> generator maximizes the log-probability of the discriminator being mistaken
![[Screenshot 2026-03-05 at 10.08.03 PM.png]]

Mode Collapse
![[Screenshot 2026-03-05 at 10.10.25 PM.png]]
Generator outputs only a possible mode and neglect the others, happens when the discriminator doesnt perform well on a specific mode

Non-saturating loss
![[Screenshot 2026-03-05 at 10.12.50 PM.png]]

Wasserstein GAN (WGAN): state of the art GANs don't use the original formulation
Wasserstein GAN is based on the concept of Wasserstein or Earth mover distance between the real and fake distribution
![[Screenshot 2026-03-05 at 10.15.16 PM.png]]
![[Screenshot 2026-03-05 at 10.15.24 PM.png]]

The Wasserstein distance admits a dual formulation which is tractable
![[Screenshot 2026-03-05 at 10.17.05 PM.png]]
where the supremum is over al Lipschitz functions f, meaning:
![[Screenshot 2026-03-05 at 10.17.35 PM.png]]

Intuition: the function f cannot "grow too fast"

We use this to swap the Discriminator with a Critic
![[Screenshot 2026-03-05 at 10.18.26 PM.png]]
Critic doesn't produce a binary classification but a real value which is unbounded

Loss of the critic:
![[Screenshot 2026-03-05 at 10.19.27 PM.png]]

Problem: the critic must be a Lipschitz function
• simple solution: **clip the weights** between -1 and 1
• more elegant solution: Lipschitz constraint is enforced by promoting
**critic gradients to be small** in the loss function (penalty)
![[Screenshot 2026-03-05 at 10.21.00 PM.png]]


The generator wants to **minimize** the Wasserstein distance or, conversely, to **maximize** the critic's output on fake samples, i.e. maximize 𝔼 𝐷 𝐺 𝑧 ,which is equivalent to minimizing the negative:
![[Screenshot 2026-03-05 at 10.23.28 PM.png]]
This is fundamentally different from the original GAN — the critic 𝐷is **not** a classifier outputting probabilities and the gradient signal remains **non-saturating** throughout training.

```python
for epoch in range(EPOCHS):
	for i, (real_images, labels) in enumerate(data_loader):
		# Sample noise as generator input
		z = torch.randn(BATCH_SIZE, LATENT_DIM, device=DEVICE)
		# Sample fake images
		fake_images = generator(z)
		# Compute the output of the critic
		real_out= critic(real_images)
		fake_out= critic(fake_images)
		gradient_penalty= compute_gradient_penalty(..)
		d_loss= torch.mean(fake_out)-torch.mean(real_out)+lambda*gradient_penalty
		
def compute_gradient_penalty(critic, real_samples, fake_samples):
	# Random weight term for interpolation between real and fake samples
	alpha = torch.rand(BATCH_SIZE, 1, 1, 1).to(DEVICE)
	# Get random interpolation between real and fake samples
	interpolated = ….
	output = critic (interpolated)
	# Get gradient w.r.t. interpolates
	gradients = torch.autograd.grad(outputs=logits, inputs=interpolated,  grad_outputs=torch.ones_like(logits), create_graph=True)[0]
	gradients = gradients.view(BATCH_SIZE, -1)
	# Compute the norm
	gradient_penalty= ((gradients.norm(2, dim=1) - 1) * 2).mean()
	return gradient_penalty
```

Wasserstein GAN is well-defined and non-vanishing gradients even when the distributions are supported on compact and disjoint manifolds
![[Screenshot 2026-03-05 at 10.30.37 PM.png]]

DCGAN Architecture
![[Screenshot 2026-03-05 at 10.31.38 PM.png]]
Training DCGANs: the breakthrough of the paper was putting forth the main key ideas used in all following works:
- replace max pooling with convolutional stride
- replace FC Layers with conv layers
- use deconvolution in generator for upsampling
- use batch normalization after each layer
- in the generator use ReLU in hidden layers and tanh for the output
- in the discriminator use LeakyReLU

## Conditional Generation

Conditional GAN: combine in the same generative process the randomness (input noise) and control (input label)
![[Screenshot 2026-03-05 at 10.35.20 PM.png]]

Pix2Pix: generator takes an image as input (instead of label), generator aims at producing a specific, consistent output. Trained on paied image sets
![[Screenshot 2026-03-05 at 10.36.06 PM.png]]
Pix2Pix Module Architecture
![[Screenshot 2026-03-05 at 10.36.40 PM.png]]

CycleGAN: Find reciprocal mapping between images with similar content, but different style from unpaired image sets
![[Screenshot 2026-03-05 at 10.39.38 PM.png]]

Cycle Consistency: from zebra to horse and back to zebra (cycle)
Real and fake zebra MUST match (cycle consistency)
Cycle consistency --> preserve contents
Discriminator loss --> style transfer
![[Screenshot 2026-03-05 at 10.41.52 PM.png]]

Loss based on pixel differences for both loop directions
![[Screenshot 2026-03-05 at 10.42.14 PM.png]]

Cycle consistency loss is the sum of pixel losses in both cycle directions
![[Screenshot 2026-03-05 at 10.42.54 PM.png]]

Adversarial loss: uses least square loss (helps with vanishing gradient and mode collapse)
![[Screenshot 2026-03-05 at 10.46.26 PM.png]]

Identity loss: extra loss term to help color preservation in the output. Enforces identity at pixel level (discourages color distortion)

![[Screenshot 2026-03-05 at 10.49.22 PM.png]]

Image super-resolution
![[Screenshot 2026-03-05 at 10.50.05 PM.png]]

SRResNet: DCGAN like architecture, during training HR (high-res) images are downsampled to LR (low-res) and fed to a generator that creates a SR (super-res) of LR
![[Screenshot 2026-03-05 at 10.50.35 PM.png]]