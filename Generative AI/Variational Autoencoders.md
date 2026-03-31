Nowadays not used by itself, but as part of more complex pipelines

Encoder-Decoder Architecture: design a network as a bunch of conv layers with downsampling and upsampling inside the network
after having reduced the spatial resolution in the first half of the network, it's increased again in the second part to match input size

In-network upsampling: Unpooling
![[Screenshot 2026-03-31 at 7.13.27 PM.png|500]]

Architectural Variants: U-NET
![[Screenshot 2026-03-31 at 7.14.38 PM.png|500]]

From encoder-decoder to auto-encoder
![[Screenshot 2026-03-31 at 7.15.03 PM.png|500]]
what loss function can be used to train an autoencoder for image reconstruction? 
- L2 loss Function $||x-\hat x||^2$ 
- Alternatives:
	- Mean Squared Error (MSE) : averate squared pixel-wise difference between input and reconstruction, implicitly assumes the pixel errors follow a Gaussian distribution. Tends to produce slightly blurry outputs because it penalizes large deviations
	- Binary Cross-Entropy (BCE) : Used when pixel values are normalized to \[0,1]\[0,1]\[0,1] and treated as Bernoulli probabilities. More appropriate for binary or near-binary images (MNIST)

Transformer Decoder vs VAE Decoder
![[Screenshot 2026-03-31 at 7.21.43 PM.png|700]]

Can you sample new images from a trained classic autoencoder by randomly picking a point in the latent space? Why or why not?
NO, no reconstruction from "random" vector
- If we choose an arbitrary latent vector, we may not be close to any points in the training set and the reconstruction is garbage
- We need to ensure that the latent space has a proper structure and maps valid output
![[Screenshot 2026-03-31 at 7.26.02 PM.png]]

# From Autoencoders to Variational Autoencoders

The key is to map the complicated data distribution to a simpler distribution (encoder) we can sample from
Distributions rather than deterministic value ensure that close points in latent space lead to similar reconstructions, giving meaning to the latent space
![[Screenshot 2026-03-31 at 7.30.32 PM.png|500]]

Our goal is to learn the data distribution
![[Screenshot 2026-03-31 at 7.32.31 PM.png|500]]
The integral over all latent codes is intractable for non-linear decoders --> need a variational approach

We introduce the variational distribution:
![[Screenshot 2026-03-31 at 7.34.44 PM.png|500]]
![[Screenshot 2026-03-31 at 7.35.07 PM.png|500]]
![[Screenshot 2026-03-31 at 7.35.30 PM.png|500]]

main difference with autoregressive model is that 
	When training a Variational Autoencoder are are not computing the exact probability of the data (impossible) but we are calculating an approximation 

The loss becomes the sum of the Reconstruction and Latent regularization terms
- **Reconstruction term**: encourages the decoder to faithfully recover x from a sampled latent z
- **Latent Regularization term**: penalizes the encoder for straying from the standard Gaussian prior, and shapes the latent space into a smooth, sampleable manifold
![[Screenshot 2026-03-31 at 10.11.42 PM.png|500]]

![[Screenshot 2026-03-31 at 10.13.45 PM.png|500]]

Usually we force both the Encoder and Decoder to use Gaussian Distribution (helps with calculations) --> reconstruction error reduces to a mean-squared error loss
![[Screenshot 2026-03-31 at 10.17.41 PM.png|500]]

Sampling is not a deterministic or dfferentiable process --> can't backpropagate
Solution: put the random process outside the network by adding noise
![[Screenshot 2026-03-31 at 10.21.02 PM.png|500]]

Issues with Variational Auto-Encoders
- Decoder must output a single mean --> output of VAEs is blurry, cannot be fixed by making the networks deeper, it's a fundamental consequence of the Gaussian decoder objective, rooted in the least-squares nature of the reconstruction loss
- Latent Space collapse --> the decoder is often too powerful, making it easier to generate output without using latent codes, latent space becomes indistinguishable from the prior, causing independent and identically distributed samples

Possible solutions: Hierarchical VAEs, or Vector Quantized VAEs

VAE-GAN
![[Screenshot 2026-03-31 at 10.24.10 PM.png|500]]
Vector Quantized VAE
![[Screenshot 2026-03-31 at 10.24.25 PM.png|500]]
