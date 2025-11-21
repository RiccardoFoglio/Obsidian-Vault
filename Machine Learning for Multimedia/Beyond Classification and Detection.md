![[Screenshot 2025-11-20 at 5.51.19 PM.png|500]]

semantic segmentation: where are some pixels with respect to others, and if they are part of the same group.
In Instance segmentation we have objects not pixels

## Semantic Segmentation

Label each pixel in the image with a category label, don't differentiate instances, only pixels

 ![[Screenshot 2025-11-20 at 6.59.11 PM.png]]

sliding window approach can already take care of this (brute force, not efficient)
![[Screenshot 2025-11-20 at 7.00.10 PM.png]]
- we are not reusing shared features between overlapping patches
- the size of the patch may be problematic, too small = cant recognize what it is, too big = large computations

Fully convolutional approach is better
Design network as a bunch of convolutional layers to make predictions for pixels all at once
![[Screenshot 2025-11-20 at 7.02.01 PM.png]]
same spatial size of the input kept for all convolutions so it doesn't scale the input size

Design network as bunch of convolutional layer with downsampling and upsampling inside the network
![[Screenshot 2025-11-20 at 8.40.47 PM.png]]
After having reduced the spatial resolution in the first half of the network, it's increased again in the second part to match input size

to do downsampling: pooling, strided convolutions
to do upsampling ???

in-network upsampling: unpooling
![[Screenshot 2025-11-20 at 8.45.11 PM.png|500]]
![[Screenshot 2025-11-20 at 8.45.38 PM.png|500]]
need to remember the position of the max before the shrink, because it might be relevant. 
pooling works without parameters, anything learnable is missing.

Learnable upsampling: transpose convolution
![[Screenshot 2025-11-20 at 8.47.51 PM.png|500]]
![[Screenshot 2025-11-20 at 8.48.51 PM.png|500]]

## Instance Segmentation

Label each pixel in the image with a category label, and differentiate instances
![[Screenshot 2025-11-21 at 2.12.16 PM.png]]

### Mask R-CNN
![[Screenshot 2025-11-21 at 2.32.35 PM.png|500]]

Network that can detect that a bounding box is containing something 
- convolutional network applied to image extracts features
- regional interest extraction on the feature map
- classification of the regions
- furthermore classify the pixels/elements of each bounding box

Fully convolutional segmentation added to the R-CNN

Outputs:
![[Screenshot 2025-11-21 at 4.02.18 PM.png]]

we can modify the network and have it extract also the joint positions of the skeletons of the elements:
![[Screenshot 2025-11-21 at 4.03.47 PM.png]]

## Face Verification and Recognition

Verification: input image, name/ID, output whether that input is that of the claimed person

Recognition: given a database of X people, get an input image and output the ID if the image is any of the X people (or not recognized)

one-shot learning = Learning from one example to recognize the person again

learning with a similarity function --> d(img1, img2) degree of difference between two images,

VERIFICATION:  if the difference between the two is below a threshold they are classified as the same

RECOGNITION: compute similarity between input image and database

when an image passes a conv. network, we have a smaller encoded function, so we can define the similarity with the values of the function

![[Screenshot 2025-11-21 at 4.34.54 PM.png]]

parameters of the network define an encoding of f(xi)
learn parameters so that:
- if xi, xj, are the same person, then the difference d(xi, xj) is small

A = anchor, P = positive, N = negative
![[Screenshot 2025-11-21 at 4.36.21 PM.png]]
the easiest path for the network is to have everything = 0, which satisfies
![[Screenshot 2025-11-21 at 4.37.43 PM.png]]
too easy, so we add a parameter to avoid this
![[Screenshot 2025-11-21 at 4.37.56 PM.png]]![[Screenshot 2025-11-21 at 4.38.03 PM.png]]

our goal is to minimize this value, which is done by finding the max between the value of the expression we found before and 0
![[Screenshot 2025-11-21 at 4.43.27 PM.png]]

training set needs a lot of images of the same person to learn the subtle differences
triple loss is difficult to manage because of this requirement


Learning with binary classification: two inputs in the network, at the end join them with a logistic regression which will do a 0/1 classification: are the two inputs the same person or not?
--> SIAMESE NETWORK
Train the network with pairs of images
![[Screenshot 2025-11-21 at 4.50.44 PM.png]]

# Neural Style Transfer

![[Screenshot 2025-11-21 at 4.51.21 PM.png]]

