
![[Screenshot 2025-12-27 at 3.41.12 PM.png|500]]

Discriminative: from features make classes (from image: it's 0.8 cat and 0.2 dog)
Generative: from noise and class makes features --> model distribution of characteristics to reach a feature goal
Most of the time we want the model to generate a single class not multiple, specialized
noise is used to avoid repeating the same generation 
it's possible that the AI generates weird results, must be fixed

Taxonomy of generative models:
![[Screenshot 2025-12-27 at 3.45.10 PM.png|500]]

## Variational Auto-Encoders (VAE)

![[Screenshot 2025-12-27 at 3.45.45 PM.png|500]]

Train model to minimize loss, works at pixel level
After model is trained, decoder tested: test time = reconstruction from a random vector

Looks like a standard autoencoder, but it's a variational autoencoder:
- noise added in model, both in learning and testing
- encoder projects sample on a distribution, which is sampled (+noise) before being fed to the decoder
Model learns how to reconstruct the image from a neighborhood of points (because of the noise)
![[Screenshot 2025-12-27 at 4.00.11 PM.png]]

The domain we are trying to reconstruct is a manifold in the original feature space
manifolds = subspace embedded in the original space

Intrinsic Dimensionality (ID) = minimal number of variables needed to represent the data
ID identifies a manifold of smaller dimensions than that of the feature space
A size of the latent space much higher than the manifold ID complicates the life of the generative model

Which is the dimension for the latent space? difficult to calculate
- optimally, it's similar to the intrinsic dimensionality of the domain

- Too small --> model can't capture all the variability
- too large --> model may overfit

## Generative Adversarial Networks (GANs)

![[Screenshot 2025-12-27 at 4.13.16 PM.png]]

GAN doesn't have encoders

Generator makes images, while Discriminator tries to catch fake images that don't correspond to a match according to training set it was trained on
Each part is fighting each other, so they both get better because they are in competition

When Generator is good enough, freeze parameters, detach Discriminator and feed random noise to make different outputs
![[Screenshot 2025-12-27 at 4.17.39 PM.png|500]]

2 main issues: 
- generator only learns a (multi-mode and complex) distribution over the training set. Not a problem with small datasets and small variations, but issue with different classes and large variations
- generator has no way to determine the type/characteristic/class of the image to produce. Okay for generating new training data, but can be issue when samples for specific situations are needed

## GAN Anatomy

Discriminator: discriminates between real and fake samples. 
Classifier output: conditional probability \[real, fake]
![[Screenshot 2025-12-27 at 4.51.08 PM.png|500]]

Generator: samples of the class(es), it outputs sample features
![[Screenshot 2025-12-27 at 4.51.38 PM.png|500]]

How the generator learns:
- Generator: y_d as close to 1 (real) as possibile
- Discriminator: y_d as close to 0 (fake) as possible 
![[Screenshot 2025-12-27 at 4.53.23 PM.png|500]]

The discriminator should also improve its capabilities to tell real from fake samples
![[Screenshot 2025-12-27 at 4.56.09 PM.png|500]]

When generator produces good results: freeze parameters and feed noise to sample more outputs

Distribution approximations depends from the characteristics of the training set
- most common samples in training sets are more likely to be generated

GAN Architecture:
![[Screenshot 2025-12-27 at 4.59.23 PM.png|500]]

back propagation of both generator and determinator loss to update weights and upgrade the process (lock opposite NN while updating)

With alternate training of models, both models improve together
- perfect discriminator will label all fake images as fake --> generator has no ways to improve
- perfect generator will always fool the discriminator --> discriminator has no ways to improve

minimax game: 
- discriminator trying to maximize its reward
- generator is trying to minimize the discriminator reward (maximize its lost)
![[Screenshot 2025-12-27 at 5.18.57 PM.png|500]]
discriminator will not be able to distinguish real data from training data

Discriminator: needs to predict 1 for real and 0 for fake
maximize this loss function --> gradient ascent
![[Screenshot 2025-12-27 at 5.21.15 PM.png|300]]

Generator: needs to predict 1 for real and 0 for fake
minimize this loss function --> gradient descent
![[Screenshot 2025-12-27 at 5.21.57 PM.png|300]]

When D is too confident, the gradient for G goes to zero (vanishing gradient on generator)
![[Screenshot 2025-12-27 at 6.21.45 PM.png|500]]
Alternative formulation: generator can still learn even when the D is very successful
![[Screenshot 2025-12-27 at 6.21.59 PM.png|500]]

## Deep Convolutional GANs (DCGANs)

Vanilla GAN from Goodfellow papers mainly used fully connected models, not best choice for images
- 3 channels (RGB), input vector size: 196608 --> too many parameters

![[Screenshot 2025-12-27 at 6.23.50 PM.png|500]]

Key ideas:
- D: replace max pooling with convolutional stride
- D: Replace FC Layers with conv. layers
- G: use deconvolution (transposed convolutions) in generator for upsampling
- Use batch normalization after each layer (except the generator output)
- In the generator, use ReLU/LeakyReLU in hidden layers and tanh for output
- in discriminator use LeakyReLU

## GANs : Training Challenges

- non convergence
- mode collapse

### Non-Convergence
Deep learning models involve a single player:
- the player tries to maximize its reward (minimize its loss) using SGD (with backpropagation) to find the optimal parameters. SDG has convergence guaranteed
- with non-convexity, we might converge to a local optima $$min_G L(G)$$
GANs instead involve 2 players:
- Discriminator trying to maximize its reward
- Generator trying to minimize Discriminator's reward $$Min_G Max_D V(D,G)$$
SGD wasn't designed to find the Nash Equilibrium of a game: might not converte to the Nash Equilibrium at all

### Mode Collapse
With GANs, real data distributions are multi-modal
![[Screenshot 2025-12-27 at 6.30.36 PM.png|400]]
*any peak in the feature distribution is a mode*

the generator can get stuck in generating the two modes when stuck in a local optima, and may lose the ability to generate all other features
To solve this:
- use label multiclass classification: not just real, but guess which feature it is
- nobody knows why it works better, but empirically it does

## Improving GANs

so far
- **Unconditional Generation**: generator has no control over the generated data. Samples from random classes, training dataset doesn't need labels
  
- **Conditional Generation**: ask the generator to create samples of a specific class. Samples from the class you need, training dataset needs labels. Training set = the output we want to generate
  ![[Screenshot 2025-12-27 at 8.38.03 PM.png|500]]
  
- **Controllable Generation**: specify certain characteristics of the output without changing model



