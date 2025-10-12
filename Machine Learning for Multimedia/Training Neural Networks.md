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

Initialization

Problems with 0 initialization of parameters:
- gradient descent and other advanced optimization methods need initial value for W and b
- zero initialization could work for logistic regression but not for neural networks

![[Screenshot 2025-10-12 at 5.07.46 PM.png|500]]


When training a neural network

choose a network architecture:
- no. if inputs
- no. of outputs
- hidden layers: one ore more, with a given number of hidden units (the more unites the better but higher computational costs)

1. randomly initialize weights
2. Implement forward propagation to get outputs for any input
3. implement code to compute cost function J
4. implement backward propagation to compute partial derivatives
5. use gradient descent or advanced optimization method with backward propagation to try to minimize J


Prediction of house prices: non linear problem

![[Screenshot 2025-10-12 at 8.32.40 PM.png|500]]

traditional NN can be used for some tasks on tabular data, 
but for other types of data (images, audio etc...) other models works better.
- Images --> lots of pixels --> CNNs (convolutional)
- Audio --> values that change over time --> RNNs (recurrent)


NN trained for face recognition:
![[Screenshot 2025-10-12 at 8.40.25 PM.png|500]]
this is what we see inside the neurons
- each layer activates when more and more details get recognized, starting from edges, then features like nose & mouth, then face shape etc... the more layers the inputs go thru

Scaling a NN:
![[Screenshot 2025-10-12 at 8.43.53 PM.png|500]]
- traditional: has a cap that limits the performance, even after adding data
- the bigger NN the more accurate they can get with more data

to scale we need:
- data
- computation power
- algorithms

hyperparameters: need to find the optimal configuration
- learning rate
- n. iterations
- n. hidden layers
- n. hidden units per layer
- activation functions
- ...

iterative process, it takes a long time

![[Screenshot 2025-10-12 at 10.08.16 PM.png]]

not having a test set can be okay, and only having a development set
avoid mismatched distribution: make sure dev/test sets come from same distribution of training set