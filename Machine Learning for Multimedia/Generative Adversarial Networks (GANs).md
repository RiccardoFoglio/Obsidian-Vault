
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
Differences in the output features can be obtained acting on the input noise vector
How?
- Interpolating in latent space (z-space --> noise vector space) it's possible to interpolate between generated outputs
![[Screenshot 2025-12-28 at 5.42.11 PM.png|500]]
issues:
- output feature correlation : z-space and output features are strongly correlated, difficult to isolate individual affects
- z-space entanglement : each noise component acts on multiple features, everything tied together
  ![[Screenshot 2025-12-28 at 5.44.11 PM.png|400]]
  need a z-space large enough
  regularizer encourages disentanglement: penalizes entanglement and can be cast as a supervised/unsupervised method
  ![[Screenshot 2025-12-28 at 5.45.01 PM.png|400]]

Possible regularizers:
- mutual information (estimates the statistical dependence between 2 variables): maximize MI between latent variables $z_i$ and data generated as G($z_i$)
- total correlation : minimize total correlation between latent variables to achieve their statistical independence
- Geometric regularization : idea is to penalize the discrepancies between data generated along the interpolating line in latent space. Requires choice of a discrepancy function

## GAN Applications

Image to Image translation: transferring style from one image to another
(day to night, sketch to photo, BW to Color etc...)

can be paired or unpaired
![[Screenshot 2025-12-28 at 5.49.44 PM.png|500]]
### Paired i2i : Pix2Pix 
type of conditional GAN for paired image 2 image translation
![[Screenshot 2025-12-28 at 5.50.40 PM.png|500]]
Generator takes a real image as input and aims at producing a specific output (the paired image)
![[Screenshot 2025-12-28 at 5.52.12 PM.png|500]]
input discriminator is a real input concatenated with EITHER the generated output OR the ground truth
![[Screenshot 2025-12-28 at 5.52.57 PM.png|500]]

Pix2Pix Architecture:
![[Screenshot 2025-12-28 at 5.53.16 PM.png|500]]

Pix2Pix Loss : composed by adversarial loss (any) plus a regularization term
pixel loss, aimed at providing generator with more information about the real target to generate
![[Screenshot 2025-12-28 at 5.55.19 PM.png|500]]

### Unpaired i2i : CycleGAN
Find reciprocal mapping between two sets of images (with different styles)
aims at finding commonalities and differences

![[Screenshot 2025-12-28 at 5.56.50 PM.png|500]]

From zebra to horse and back to zebra (cycle)
real and fake zebra MUST match --> *cycle consistency* --> preserve contents
![[Screenshot 2025-12-28 at 5.57.45 PM.png|500]]

Discriminator loss --> style transfer
![[Screenshot 2025-12-28 at 5.58.38 PM.png|500]]

Module Architecture
![[Screenshot 2025-12-28 at 5.59.06 PM.png|500]]

Loss: Cycle consistency loss --> based on pixel differences for both loop directions
![[Screenshot 2025-12-28 at 6.00.22 PM.png|500]]
![[Screenshot 2025-12-28 at 6.00.54 PM.png|500]]
cycle consistency loss is the sum of pixel losses in both cycle directions
sum of adversarial loss using a weight (as before)

Adversarial Loss : uses least square loss (helps vanishing gradient and mode collapse), output is patch-based (consider a sum over all patches)

Identity loss : extra loss term (optional) to help color preservation in the output. Enforces identity at pixel level (discourages color distortion). Two identity losses, one for each domain

CycleGAN Loss
![[Screenshot 2025-12-28 at 6.05.14 PM.png|500]]

- Work well for transferring styles
- Work bad when the transformation involves GEOMETRIC transformations

### Image Super-Resolution
Increase image resolution creating "new" pixel contents
SRResNet : DCGAN like architecture
During training high resolution images (HR) are downsampled to low-resolution (LR) and fed to a generator that creates a Super-resolution (SR) of LR
![[Screenshot 2025-12-28 at 6.07.36 PM.png|500]]

SR Loss: final loss function is based on the VGG network
![[Screenshot 2025-12-28 at 6.08.42 PM.png|500]]

Discriminator Loss: combination of losses, Adversarial loss for discriminator
Generator Loss : Adversarial loss + content loss

### 3D - GAN
Generating 3D instead of 2D images. 
Approach is the same: 
- generator --> 3D shapes
- discriminator --> tells real from fake 3D models

High unbalance between generator and discriminator: generating 3D objects is much more complex than classifying them
3 training tricks:
- learning rate for D much smaller than learning rate for G
- Batch size is very large (100)
- Discriminator is updated only if the accuracy in the last batch is < 80%

Combines VAE and GAN to produce a latent vector for 3D generation from image space
![[Screenshot 2025-12-28 at 6.17.15 PM.png|500]]
