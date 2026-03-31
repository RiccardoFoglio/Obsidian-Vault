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
![[Screenshot 2026-03-31 at 7.21.43 PM.png|600]]

