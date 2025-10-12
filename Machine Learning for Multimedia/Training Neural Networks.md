![[Screenshot 2025-10-12 at 4.37.08 PM.png|500]]
![[Screenshot 2025-10-12 at 4.37.36 PM.png|500]]
![[Screenshot 2025-10-12 at 4.46.48 PM.png|500]]
![[Screenshot 2025-10-12 at 4.47.26 PM.png|500]]

![[Screenshot 2025-10-12 at 4.48.20 PM.png]]
in the linear part the neuron is computing the linear combination (z) and then it's applying the sigmoid (a) to give the output

And for now each unit is doing the same thing in the network
![[Screenshot 2025-10-12 at 4.48.58 PM.png]]

![[Screenshot 2025-10-12 at 4.49.56 PM.png|400]]
![[Screenshot 2025-10-12 at 4.50.19 PM.png|400]]

aˆ\[0]  is the input layer of activations, it propagates in the layers

for each training example we have to repeat the entire calculations
![[Screenshot 2025-10-12 at 4.55.29 PM.png|500]]

![[Screenshot 2025-10-12 at 4.56.14 PM.png|500]]

m images (number of examples)
n number of pixels (features)

![[Screenshot 2025-10-12 at 4.56.50 PM.png]]

often the only check needed is about the sizes of the matrices

Typically we use the sigmoid
![[Screenshot 2025-10-12 at 5.00.26 PM.png|500]]
which is always between 0 and 1, but sometimes it's a disadvantage 
Other activation functions can be used:

![[Screenshot 2025-10-12 at 5.01.09 PM.png|500]]

![[Screenshot 2025-10-12 at 5.01.15 PM.png|500]]

![[Screenshot 2025-10-12 at 5.01.23 PM.png|500]]

Notation:

![[Screenshot 2025-10-12 at 5.04.28 PM.png]]
each layer can have a different activation function

### Initialization

Problems with 0 initialization of parameters:
- gradient descent and other advanced optimization methods need initial value for W and b
- zero initialization could work for logistic regression but not for neural networks

![[Screenshot 2025-10-12 at 5.07.46 PM.png|500]]
