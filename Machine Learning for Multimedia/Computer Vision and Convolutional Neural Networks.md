images as input: big

1000x1000 image --> 3M Features
![[Screenshot 2025-11-05 at 12.36.14 AM.png]]

Convolution:
![[Screenshot 2025-11-05 at 12.37.05 AM.png]]

Vertical Edge Detection : matrix convoluted by a filter (kernel)
![[Screenshot 2025-11-05 at 12.37.42 AM.png|500]]
![[Screenshot 2025-11-05 at 12.38.08 AM.png|500]]

Vertical Result
![[Screenshot 2025-11-05 at 12.40.44 AM.png|500]]

![[Screenshot 2025-11-05 at 12.41.10 AM.png|400]]

Learning to detect edges?
![[Screenshot 2025-11-05 at 12.41.46 AM.png|500]]

Shrinking output: n x n input, f x f filter --> n-f+1 x n-f+1 output
Problem: throwing away info at the edges

Solution: padding --> output is the same size as the real input
![[Screenshot 2025-11-05 at 12.44.40 AM.png]]

- Valid Convolution: No Padding
- "Same" Convolution: pad so that output size is the same as input size --> p = (f-1)/2

Strided Convolutions: stride by a diff value then 1 --> smaller matrix --> smaller output (when we need to shrink on purpose we can use strided convolutions)
![[Screenshot 2025-11-05 at 12.46.52 AM.png|500]]

Our inputs are RGB --> more channels --> but output is still 2D
![[Screenshot 2025-11-05 at 12.48.39 AM.png]]
different filters per channel or the same one repeated, are all possible

We can have different set of filters on the same image to have a 3D output
![[Screenshot 2025-11-05 at 12.49.42 AM.png]]

Output: n-f+1 x n-f+1 x n_c' (number of filter matrices)

Applying non linear function to a linear combination (z) --> a = g(z)
![[Screenshot 2025-11-05 at 12.52.54 AM.png]]
convolutional layer: yellow and orange = weights, the singular results are neurons, and the result of the calculation is the activation a

The parameters are inside the boxes --> 16 w + 1 b


Example: 10 filters 3x3x3 --> 1 layer of convolutional NN, how many parameters?
- (3x3x3 + 1) x 10 = 280, not many



Notation (61/109)

