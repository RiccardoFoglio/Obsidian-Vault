Algorithms that try to mimic the brain
very widely used in 80s and early 90s, then diminished
recent resurgence: state-of-the-art technique for many applications

very hard to find this hypothesis, and also have many features
![[Screenshot 2025-10-04 at 7.09.53 PM.png|500]]

when you want to recognize an image, you don't feed a single pixel but the whole image as pixel values, so a matrix of 3 layers (RGB) --> 64x64 images means 3 matrices of 4096 values = 12288 features
![[Screenshot 2025-10-04 at 7.17.49 PM.png|500]]

Brain has one algorithm to learn

our neuron:
![[Screenshot 2025-10-04 at 7.20.36 PM.png|500]]

artificial neuron model:
![[Screenshot 2025-10-04 at 7.21.22 PM.png|500]]
Sigmoid (logistic) also used as activation function a --> classification

Artificial Neural Network:
![[Screenshot 2025-10-04 at 7.23.12 PM.png|500]]
2 layer network because you don't count the input layer (Layer 0)
the intermediate layers are called Hidden Layers

![[Screenshot 2025-10-04 at 7.27.47 PM.png|500]]
![[Screenshot 2025-10-04 at 7.28.02 PM.png|500]]
![[Screenshot 2025-10-04 at 7.28.37 PM.png|500]]
we rename input layers from x to $aˆ{[0]}$
![[Screenshot 2025-10-04 at 7.30.21 PM.png|500]]

Binary vs Multiclass
![[Screenshot 2025-10-04 at 7.31.22 PM.png|500]]

Often neural networks are black box models, because the hidden layers do a lot of the work but only the output is shown

Architecture : Shallow vs Deep: deep neural network = lots of layers 
it's chosen by the developer

Car recognition:
![[Screenshot 2025-10-04 at 7.36.24 PM.png|500]]
m = complete data set, lots of cars and lots of non car to train the network
we need labels (y)

weights and biases
![[Screenshot 2025-10-04 at 7.38.32 PM.png|500]]

Representation of logistic regression as a neural network

![[Screenshot 2025-10-04 at 9.35.04 PM.png|500]]
![[Screenshot 2025-10-04 at 9.35.41 PM.png|500]]
we want to find w and b to minimize J(w,b) using gradient descent
![[Screenshot 2025-10-04 at 9.36.23 PM.png|500]]
![[Screenshot 2025-10-04 at 9.36.51 PM.png|500]]

