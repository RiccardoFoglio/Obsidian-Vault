
# Unit 1 : Machine Learning Foundations

Machine Learning represents a new capability for computers to solve problems that cannot be easily programmed by hand:

- **Arthur Samuel (1959)**: ML is the field giving computers the ability to learn without being explicitly programmed.
- **Tom Mitchell (1998)**: A program learns from experience **E** with respect to task **T** and performance measure **P**, if performance on **T**, measured by **P**, improves with **E**.

Example:
- T = playing checkers
- P = probability the program will win the next game
- E = Overcoming the experience of playing tens of thousands of games against itself

**Major Paradigms**:
- **Supervised Learning**: The algorithm is given "right answers" (labels) for training.
    - **Regression**: Predicts continuous valued output (e.g., housing prices).        
    - **Classification**: Predicts discrete valued output (e.g., malignant vs. benign tumor).

- **Unsupervised Learning**: The algorithm finds structure in unlabeled data.
	- **Clustering**: Grouping similar objects (e.g., Google News grouping stories, social network analysis).
# Unit 2 : Model & Cost Function (Linear Regression)

Linear Regression is used to predict a continuous "target" variable based on one or more "input" variables. 

- $x$ : input variable or "feature" (size of a house in squared feet)
- $y$ : output variable or "target" variable (price of house)
- $(x,y)$ : A single training example
- $(x^{(i)},y^{(i)})$ : the i-th training example in the dataset
- $m$ : total number of training examples in the set

Hypothesis Function ($h$) : This function maps input x to the estimated output y. For univariate linear regression (regression with one variable) the hypothesis is defined as:
$$h_\theta(x) - \theta_0 + \theta_1x$$
where the variables $\theta_0$ and $\theta_1$ are parameters of the model (weights $w$ and bias $b$)

Cost Function measures the accuracy of the hypothesis function by calculating how close the predicted values are to the actual values in the training set

Square Error Cost Function: most common for linear regression
$$J(\theta_0, \theta_1) = \frac{1}{2m} \sum^{m}_{i=1}(h_\theta(x^{(i)})-y^{(i)})$$

Simplified model: often the hypothesis is used as: $h_\theta = \theta_1 x$ (setting $\theta_0 = 0$), resulting in a cost function that only depends one a single parameter $J(\theta_1)$

The Goal of Linear Regression is to choose parameters $\theta_0$ and $\theta_1$ that **minimize** the cost function. By minimizing the squared difference between predictions and actual data we find the "line of best fit"

# Unit 3 : Parameter Learning (Gradient Descent)

**Gradient Descent**: iterative optimization algorithm used to minimize the cost function
$$\text{repeat until convergence: } \theta_j := \theta_j - \alpha \frac{\partial}{\partial \theta_j} J(\theta_0, \theta_1) \quad (\text{for } j=0 \text{ and } j=1)$$
**Simultaneous Update**: it's critical to compute the updates for all parameters simultaneously. calculate "temporary" values for all $\theta_j$ using the current parameters before updating the actual $\theta$ values.

Imagine standing on a hill in 3D landscape representing the cost function: gradient descent takes steps in the direction of the steepest descent until it reaches a local minimum. 
Depending on where you start, you may end up at different local minima.

**Learning Rate** ($\alpha$) is an hyperparameter that determines the size of the steps taken during each iteration
- if too small: gradient descent is very slow because it takes tiny steps
- if too large: gradient descent can overshoot the minimum, failing to converge or even diverging (cost increases)

fixed $\alpha$ : don't need to decrease it over time, as the algorithm approaches a local minimum the derivative (slope) naturally becomes smaller, meaning the algorithm takes smaller steps

**Multivariate Linear Regression**: when dealing with multiple features the model becomes more complex:
- **Hypothesis**: $h_{\theta}(x) = \theta_0 + \theta_1x_1 + \theta_2x_2 + \dots + \theta_nx_n$ 
- **Vectorized Form**: By defining $x_0 = 1$, we can write the hypothesis compactly as $h_{\theta}(x) = \theta^T x$.
- **Batch Gradient Descent**: Each step of the algorithm uses **all** training examples to compute the gradients.

Feature Scaling and Mean Normalization: gradient descent converges much faster if features are on a similar scale
- **Feature Scaling**: Adjusting features to have a similar range, typically approximately $[-1, 1]$.
- **Mean Normalization**: Subtracting the average value ($\mu_i$) from each feature to give them approximately zero mean.
- **Combined Formula**: $x_i := \frac{x_i - \mu_i}{s_i}$ (where $s_i$ is the range or standard deviation).

**Debugging and Alternatives**:
- **Convergence Check**: Plot the cost function $J(\theta)$ against the number of iterations.
  $J(\theta)$ should decrease after every single iteration. 
  An automatic test can declare convergence if $J(\theta)$ decreases by less than a small threshold (e.g., $10^{-3}$) in one step.

- **Normal Equation**: An analytical alternative to gradient descent that solves for $\theta$ directly: $$\theta = (X^T X)^{-1} X^T y$$
	- **Pros**: No need to choose $\alpha$, no iterations, and no feature scaling required.
    - **Cons**: Computationally expensive ($O(n^3)$) if the number of features $n$ is very large.

- **Advanced Optimizers**: Algorithms like Conjugate Gradient, BFGS, and L-BFGS are often faster than gradient descent and don't require manual $\alpha$ selection, though they are more complex.

**Polynomial Regression**: Linear regression can be adapted to fit non-linear data by creating "new" features. 
For example, you can create a third-order polynomial hypothesis for house prices based on size by using $x_1 = (\text{size})$, $x_2 = (\text{size})^2$, and $x_3 = (\text{size})^3$

# Unit 4 : Model & Cost Function (Logistic Regression)

Classification vs Regression:
- Linear regression predicts a continuous number
- classification deals with a discrete target variable ($y$)

**Binary Classification**: target $y$ can only take two values: negative and positive

Using Linear Regression for classification often leads to poor performance because a single outlier can significantly shift the regression line, and hypothesis can output values much larger than 1 or smaller than 0, which are hard to interpret as probabilities

To ensure output always between 0 and 1, we use the **Sigmoid (or Logistic) function**
$$g(z) = \frac{1}{1 + e^{-z}}$$
Hypothesis: $h_\theta(x)$:  $h_\theta(x) = g(\theta^T x) = \frac{1}{1 + e^{-\theta^T x}}$.

The output $h_\theta(x)$ is interpreted as the probability that $y=1$ for a given input $x$.
For example, $h_\theta(x) = 0.7$ means there is a $70\%$ chance the tumor is malignant.
![[Pasted image 20260104143737.png|200]]

Decision Boundaries: line (or surface) that separates the area where we predict $y=1$ from where we predict $y=0$ 
- Predict $y=1$ if $h_\theta(x) \ge 0.5$ (which occurs when $\theta^T x \ge 0$).
- Predict $y=0$ if $h_\theta(x) < 0.5$ (which occurs when $\theta^T x < 0$).

Non-linear Boundaries: we can add polynomial features to create complex, non-linear decision boundaries like circles or ellipses.

Can't use the Squared Error cost function from linear regression because the resulting function would be **non-convex**, meaning the gradient descent could get stuck in many local minima

**Logistic Regression Cost Function**:
- $Cost(h_\theta(x), y) = -\log(h_\theta(x))$ if $y=1$.    
- $Cost(h_\theta(x), y) = -\log(1 - h_\theta(x))$ if $y=0$.

- Simplified Formula (Binary Cross-Entropy): 
  $$J(\theta) = -\frac{1}{m} \sum_{i=1}^m [y^{(i)} \log(h_\theta(x^{(i)})) + (1 - y^{(i)}) \log(1 - h_\theta(x^{(i)}))]$$

- **Gradient Descent**: To minimize this cost, we use the same update rule as linear regression, though the definition of $h_\theta(x)$ has changed.

**Multi-class Classification** (One vs All) : train a separate logistic regression classifier for each class $i$ to predict the probability that $y=i$
To make a new prediction, run all classifiers and pick the class $i$ that gives the highest probability

- **Underfitting** (High Bias): model is too simple to capture the underlying trend of the data
- **Overfitting** (High Variance): The model fits the training data too perfectly (including its noise), causing it to fail on new, unseen data

- **Regularization**: technique used to solve overfitting by keeping all features but reducing the magnitude/values of the parameters $\theta_j$
	- Regularized Cost Function: add a penalty term to the cost function $$J(\theta) = [\text{Original Cost}] + \frac{\lambda}{2m} \sum_{j=1}^n \theta_j^2$$
	- $\lambda$ (regularization parameter) : controls the trade-off between fitting the training data and keeping the parameters small. if $\lambda$ is too large, the model will underfit, if it's 0 the model will overfit
		![[Pasted image 20260104144720.png|500]]

# Unit 5 : Neural Networks

NN are algorithms designed to mimic human brain. Popular in 80s and 90s, but lately massive resurgence as a state-of-the-art technique for many applications
- **Non-Linear Problem** : traditional methods like linear or logistic regression struggle with complex classification boundaries. To capture non-linear trends one might add polynomial features but leads to a feature explosion
- **Computer Vision** :  64x64 RGB image = 12288 raw features, scale too big for computing a simple regression technique
- **"One Learning Algorithm" Hypothesis** : brain uses a general-purpose learning mechanism, experiments suggest same algorithm can handle diverse types of data

**Neuron Model**:
- Biological Anatomy:
	- Dendrites = input wires that receive the signals
	- Nucleus / Cell Body = computation unit
	- Axon = output wire that transmits the signal to other neurons
- Artificial Model:
	- inputs ($x_1, x_2, x_3...$) enter a computation unit ($a$) which applies an activation function to produce an output ($h_\theta(x)$)

**Activation Functions**: determines whether a neuron fires based on the weighted sum of its inputs
- **Sigmoid (Logistic)** : outputs a value between 0 and 1, defined as $\sigma(z) = \frac{1}{1+e^{-z}}$
- **ReLU (Rectified Linear Unit)** : outputs 0 for negative inputs and input itself for positive values, most popular choice for deep networks

**Architecture**
- Layer 0 = input layer, contains the raw input features
- Hidden layers = intermediate layers between input and output, where network learns internal representations
- Output Layer = produces the final prediction

- **Key Notation**:
    - $a_i^{[j]}$: The activation of unit $i$ in layer $j$.
    - $\Theta^{[j]}$: The matrix of weights controlling the mapping from layer $j$ to layer $j+1$.

- **Forward Propagation**: The process of calculating activations layer by layer. For example, $a_1^{[1]} = g(\Theta_{10}^{[1]}x_0 + \Theta_{11}^{[1]}x_1 + \Theta_{12}^{[1]}x_2 + \Theta_{13}^{[1]}x_3)$.

To train these networks efficiently we need some mathematical optimizations
- **Vectorization** = instead of using explicit for-loops to iterate over training examples fore features, we use matrix operations. This allows the computer to process data much faster

- **Computation Graphs** = Visual way to represent complex functions. By breaking a function into simple steps, we can use the chain rule to calculate derivatives efficiently (basis of back-propagation)

- **Loss Functions**
	- **Regression Loss** = Often uses squared error
	- **Classification Loss** = uses the convex log-loss function : $Loss(\hat{y},y) = -y \log(\hat{y}) - (1-y)\log(1-\hat{y})$. 




# Unit 6 : Training Neural Networks

Neural Networks can be viewed as multiple logistic regression units stacked together
- Logistic Regression = uses single layer to map inputs to an output
- Neural Network = introduces hidden layers between input and final output. Each layer computes a linear transformation ($z$) followed by a non-linear activation ($a$)

![[Screenshot 2025-10-12 at 4.48.20 PM.png|400]]

Forward Propagation is the process of calculating predictions by passing data through the network.
- **Layer-wise Equations** for any layer $l$ : 
	- $Z^{[l]} = W^{[l]}A^{[l-1]} + b^{[l]}$.
	- $A^{[l]} = g^{[l]}(Z^{[l]})$ (where $g$ is the activation function).

- **Vectorization**: instead of processing examples one by one in a for-loop, entire training set $X$ is processed as a Matrix $A^[0]$. 
  This allows $Z^[l]$ and $A^[l]$ to be computed for all $m$ training examples simultaneously

Choosing the right **activation function** is critical for network performance
- **Sigmoid**: $\sigma(z) = \frac{1}{1+e^{-z}}$. Outputs range $(0, 1)$.
- **Tanh (Hyperbolic Tangent)**: $tanh(z) = \frac{e^z - e^{-z}}{e^z + e^{-z}}$. Outputs range $(-1, 1)$. It is usually superior to Sigmoid for hidden layers because it centers data around zero.
- **ReLU (Rectified Linear Unit)**: $ReLU(z) = \max(0, z)$. The most common choice for hidden layers because it speeds up training by reducing the "vanishing gradient" problem.
- **Leaky ReLU**: $max(0.01z, z)$. Ensures that even negative inputs have a small, non-zero gradient.

**Backward Propagation** uses the chain rule to calculate the gradient of the cost function with respect to every weight ($W$) and bias ($b$) in the network

The **Gradient Path** starts at the output layer by calculating $dZ^{[L]} = A^{[L]} - Y$ and works backward toward the input.

**Key Formulas**:
- $dW^{[l]} = \frac{1}{m}(dZ^{[l]}A^{[l-1]T})$.
- $db^{[l]} = \frac{1}{m} \text{sum}(dZ^{[l]})$.
- $dA^{[l-1]} = W^{[l]T} dZ^{[l]}$.

How you start training matters
- **Problem with Zero Initialization** : if all weights are initialized to zero, every neuron in a hidden layer will perform the same calculation and learn the same features, making the hidden layer redundant
- Solution : initialize weights randomly to small values (range $[-\epsilon, \epsilon]$). Biases can be 0

**Hyperparameters** are set by the developer and control the training process:
- learning rage ($\alpha$)
- number of iterations
- Number of hidden layers and units per layer
- Choice of activation functions

**Dataset Management**: in big data era, the way we split data has changed
- **Traditional Split** : 60% Training / 20% Dev / 20% Test
- **Big Data Era** : 98% Training / 1% Dev / 1% Test (large dataset, 1% is enough to test)

Mismatched Distributions: it's vital that Dev and Test sets come from the same distribution even if the training set is different

# Unit 7 : Evaluating Learning Algorithms

When a learning algorithm makes unacceptably large errors on new data, developers often waste time on strategies that don't help.

- To fix High Variance (Overfitting) : get more training examples or try a smaller set of features
- To fix High Bias (Underfitting) : try getting additional features, handcrafting new features, or increasing the learning rate

Machine Learning Diagnostics: tests that provide insight into what is or isn't working. guiding how to best improve performance. While they take time to implement they are very efficient use of time in the long run.

A standard workflow involves splitting the dataset into 3 parts: Training Cross-Validation and Test sets.
1. Fit Models : learn parameters for several models (with different hyperparameters) by minimizing the cost function on the training set
2. Select Model : compute the error for each model on the cross-validation set. Choose the model with the lowest cross-validation error
3. Evaluate Generalization : use the test set to see how the chosen model performs on unseen data

Understanding why a model is failing is critical for improvement:
- High Bias (Underfitting) = model is too simple. Both Training error and Cross-Validation error are high and close to each other
- High Variance (Overfitting) = model is too complex. Training Error is very low, but CV Error is much higher, creating a large gap

Learning curves plot error as a function of the training set size ($m$):
- In High Bias : increasing $m$ doesn't help much, both $J_{train}$ and $J_{CV}$ flatten out at a high value early on
- In High Variance : there is usually a large gap between $J_{train}$ and $J_{CV}$. In this scenario getting more training data is likely to help close the gap and improve performance

Standard accuracy is misleading if classes are skewed (only 0.5% of patients have cancer).
Using a Confusion Matrix is better to derive:
- Precision : fraction of predicted positives that are actually positive 
  $$\frac{True Pos}{True Pos + False Pos}$$
- Recall : fraction of actual positives that were correctly detected 
  $$\frac{True Pos}{True Pos + False Neg}$$
- Trade-Off : raising the classification threshold increases precision but lowers recall, lowering does the opposite
- F1 Score : single number evaluation metric that combines both using their harmonic mean
  $$F_1 = 2 \frac{P \times R}{P + R}$$

- **Human-level performance** : serves as proxy for Bayer (optimal) error. The difference between human error and training error defines "Avoidable Bias"

- **Optimizing vs Satisfying Metrics** : when balancing multiple goals pick one to optimize (maximize/minimize) and set thresholds for the others to satisfy (running time $\le 100ms$)

- **Orthogonalization** : principle of having controls that affect only one component of the system at a time, making it easier to diagnose and fix specific objectives

# Unit 8 : Large Datasets and Big Models

The Role of Data and the "Big Model" Rationale: for many complex tasks, the amount of data is more important than the specific algorithm used.
- **The Power of Data**: as the training set size increases (e.g., millions of words for text classification), various algorithms (Naive Bayes, Perceptron, etc.) tend to converge toward similarly high performance.
- **Large Data Rationale**: To successfully use a massive dataset, you need a "big model" (a neural network with many hidden units) to achieve **low bias**. The large training set then ensures **low variance** by preventing the model from overfitting the data.
- **Human Expert Test**: A useful test for whether a model _should_ be able to predict $y$ from $x$ is whether a human expert could confidently do so given the same input.

Advanced Regularization Techniques: to combat **high variance** (overfitting) in big models, several techniques are used:
- **L2 Regularization (Weight Decay)**: Adds a penalty term to the cost function ($\frac{\lambda}{2m}\sum w^2$) to keep weight values small. In gradient descent, this manifests as "weight decay," where weights are slightly reduced in every update step.
- **Dropout**: nodes in the network are randomly "dropped" (set to zero) during training with a specific probability (e.g., 0.5). This prevents neurons from becoming overly dependent on specific inputs. **Important**: Dropout must be disabled during test time.
- **Data Augmentation**: Artificially increases the training set size by applying transformations to existing data, such as flipping, rotating, or distorting images of cats or digits.

Optimization: Mini-batch Gradient Descent
Training on millions of examples with standard "Batch" gradient descent is too slow because one step requires processing the entire dataset.
- **Mini-batches**: The training set is split into smaller subsets called mini-batches (e.g., 1,000 examples each).
- **The Process**: The algorithm performs a gradient descent step after each mini-batch rather than waiting for the whole dataset.
- **Epoch**: One pass through the entire training set. In mini-batch descent, one epoch consists of many individual steps (e.g., 5,000 steps if there are 5,000 mini-batches).
- **Stochastic Gradient Descent (SGD)**: A special case where the mini-batch size is 1. While fast, it loses the speed benefits of **vectorization**.
- **Choosing Size**: Typical mini-batch sizes are powers of 2 (64, 128, 256, 1024) to optimize CPU/GPU memory usage.

Vanishing and Exploding Gradients : In very deep networks, gradients can grow or shrink exponentially as they are multiplied through many layers.
- **Exploding Gradients**: If weights are $>1$, activations and gradients grow exponentially, causing training to diverge.
- **Vanishing Gradients**: If weights are $<1$, they decrease exponentially, making the network stop learning.
- **Mitigation (Initialization)**: This is managed by careful weight initialization. A common strategy is to set the variance of the weights $Var(w_i) = \frac{1}{n}$ (where $n$ is the number of input features) to keep the signal strength consistent across layers.

Normalization Strategies
- **Input Normalization**: Scaling features to have a mean of 0 and a variance of 1 to speed up gradient descent.
- **Batch Normalization**: A critical technique where the **activations ($Z$)** of internal layers are normalized during training.
    - **Benefits**: It allows much faster training, makes the network more robust to weight changes in earlier layers, and has a slight regularization effect similar to dropout.

Softmax and Transfer Learning
- **Softmax Layer**: A generalization of logistic regression for **multi-class classification**. 
  It outputs a probability distribution over $C$ classes where all probabilities sum to 1
- **Transfer Learning**: The process of taking a network trained on a large task (Task A, e.g., general image recognition) and repurposing it for a different task (Task B, e.g., radiology images).
    - **When it works**: It is most effective when Task A has much more data than Task B and the low-level features (edges, shapes) from Task A are useful for Task B.

# Unit 9 : Computer Vision and Convolutional NN

Computer vision involves various tasks such as **Image Classification** (e.g., is it a cat?), **Object Detection** (identifying where objects are), and **Neural Style Transfer** (merging the content of one image with the style of another).
- **The Dimensionality Problem**: A small $64 \times 64$ RGB image has $12,288$ features ($64 \times 64 \times 3$). However, a $1000 \times 1000$ image has **3 million features**.
- **Why Standard NNs Fail**: If the first hidden layer of a standard network has 1,000 units, the weight matrix $W^{[1]}$ would have **3 billion parameters** for a $1MP$ image, making it computationally impossible to train without overfitting.

Convolutions allow the network to detect low-level features like edges using a small **filter** (or kernel).
- **Edge Detection**: By convolving a $6 \times 6$ image with a $3 \times 3$ filter, you produce a $4 \times 4$ output.
    - **Vertical Filter**: A filter like `[[1, 0, -1], [1, 0, -1], [1, 0, -1]]` detects vertical transitions in brightness.
    - **Horizontal Filter**: Detects horizontal edges.
    - ![[Screenshot 2025-11-05 at 12.41.10 AM.png|400]]
- **Learning Features**: Instead of using hand-coded filters (like Sobel or Scharr), deep learning allows the network to **learn the filter weights** ($w_1, \dots, w_9$) that are most useful for the task.
  ![[Screenshot 2025-11-05 at 12.41.46 AM.png|500]]

- **Padding ($p$)**: Standard convolutions "shrink" the image and "throw away" information from the edges.    
    - **Valid Convolution**: No padding ($p=0$).
    - **Same Convolution**: Padding pixels are added so the output size matches the input size. Formula: $p = (f-1)/2$.
    ![[Screenshot 2025-11-05 at 12.44.40 AM.png|400]]
- **Stride ($s$)**: The number of pixels the filter jumps at each step.
  ![[Screenshot 2025-11-05 at 12.46.52 AM.png|500]]
- Output Size Formula: For an $n \times n$ input and $f \times f$ filter:
  $$\text{Output Size} = \lfloor \frac{n + 2p - f}{s} + 1 \rfloor \times \lfloor \frac{n + 2p - f}{s} + 1 \rfloor \text{}$$

Convolutions on 3D (RGB) Images
- **Multiple Channels**: For an RGB image ($6 \times 6 \times 3$), the filter must also be 3D ($3 \times 3 \times 3$). The output is still a 2D map because values across all channels are summed.    
- **Multiple Filters**: If you apply $n_C$ different filters (e.g., 10 filters), the output will have $n_C$ channels (e.g., $4 \times 4 \times 10$).

Pooling layers reduce the spatial size (width and height) of the representation to speed up computation and make features more robust.
- **Max Pooling**: Takes the maximum value in a window (e.g., $2 \times 2$). It is the most common type and helps detect if a feature is present anywhere in the region.
  ![[Screenshot 2025-11-05 at 10.25.37 PM.png|300]]
- **Average Pooling**: Takes the average value.
  ![[Screenshot 2025-11-05 at 10.27.27 PM.png|300]]
- **Key Property**: Pooling has **no learnable parameters**; it only has hyperparameters like filter size ($f$) and stride ($s$).

Case Study: LeNet-5 Architecture : a classic CNN for digit recognition typically follows this pattern:
1. **Input**: $32 \times 32 \times 3$ image.
2. **CONV1**: 6 filters ($5 \times 5$), stride 1 $\rightarrow$ $28 \times 28 \times 6$.
3. **POOL1**: Max pool ($2 \times 2$), stride 2 $\rightarrow$ $14 \times 14 \times 6$.
4. **CONV2**: 16 filters ($5 \times 5$), stride 1 $\rightarrow$ $10 \times 10 \times 16$.
5. **POOL2**: Max pool ($2 \times 2$), stride 2 $\rightarrow$ $5 \times 5 \times 16$.
6. **FC3, FC4, Softmax**: Fully connected layers followed by a 10-class output.
![[Screenshot 2025-11-05 at 10.29.27 PM.png|500]]

Why Convolutions Work : CNNs are much more efficient than fully connected (FC) networks because of two main principles:
- **Parameter Sharing**: A feature detector (like an edge detector) useful in one part of the image is likely useful in another part.
- **Sparsity of Connections**: Each output value depends only on a small number of local inputs, not the entire image.
**Result**: For a layer where a standard network might need **14 million parameters**, a ConvNet might only need **456 parameters**, making it less prone to overfitting.
# Unit 10 : CNN - Case Studies and Tasks

### **1. Classic Networks**

Three foundational architectures that defined the early eras of deep learning.

- **LeNet-5 (1998)**: Designed for document recognition (digit classification).
    - **Architecture**: Follows a sequence of 
      **Input (32x32x1) → CONV (5x5, s=1) → AVG-POOL (2x2, s=2) → CONV (5x5, s=1) → AVG-POOL (2x2, s=2) → FC (120 units) → FC (84 units) → Output**
    - **Key Insight**: It established the standard pattern of alternating convolutional and pooling layers.
		![[Screenshot 2025-11-05 at 10.54.53 PM.png|400]]

- **AlexNet (2012)**: The model that triggered the deep learning revolution by winning the ImageNet challenge.
    - **Architecture**: Significantly larger than LeNet, it processes $227 \times 227 \times 3$ images. It uses large filters ($11 \times 11$) in early layers and Max-Pooling.
    - **Structure**: Consists of 5 Convolutional layers and 3 Fully Connected layers, ending with a 1000-class Softmax.
		![[Screenshot 2025-11-05 at 10.55.31 PM.png|500]]

- **VGG-16 (2015)**: Notable for its extreme simplicity and uniformity.
    - **Design Rule**: Uses exclusively **$3 \times 3$ convolutions** (stride 1, same padding) and **$2 \times 2$ Max-Pooling** (stride 2).
    - **Scaling**: The number of filters doubles after every pooling layer (64 → 128 → 256 → 512), while the spatial dimensions ($H, W$) are halved.
		![[Screenshot 2025-11-05 at 11.07.11 PM.png|500]]

Residual Networks (ResNet): As networks grew deeper, they encountered the "vanishing gradient" problem, where training error actually _increased_ for very deep "plain" networks.
![[Screenshot 2025-11-06 at 12.06.36 AM.png|500]]
- **The Residual Block**: Introduces a **"shortcut" or "skip connection"** that bypasses one or more layers.
    - **Mathematical Formula**: Instead of just calculating $a^{[l+2]} = g(z^{[l+2]})$, ResNet calculates $a^{[l+2]} = g(z^{[l+2]} + a^{[l]})$.
- **Performance**: This shortcut allows the gradient to flow through the network more easily, enabling the training of extremely deep networks (e.g., **ResNet-152**) without the degradation in performance seen in plain networks.
![[Screenshot 2025-11-06 at 12.06.59 AM.png|500]]

1x1 Convolutions and the Inception Module:
- **1x1 Convolutions ("Network in Network")**: Used to change the number of channels (dimensionality reduction/increase) while keeping height and width constant.
    - **Example**: A $28 \times 28 \times 192$ input can be shrunk to $28 \times 28 \times 32$ using thirty-two $1 \times 1$ filters, significantly reducing computational cost for subsequent layers.
- **Inception Module**: Instead of choosing one filter size, the Inception module applies $1 \times 1, 3 \times 3,$ and $5 \times 5$ convolutions, along with Max-Pooling, **all in parallel**.
    - **Concatenation**: The outputs of all these operations are concatenated along the channel dimension.
    - **Efficiency**: To avoid computational explosion, $1 \times 1$ "bottleneck" layers are used before the $3 \times 3$ and $5 \times 5$ convolutions to reduce the number of input channels.

Practical Strategy - Data vs. Hand-Engineering: 
- **Small Datasets (e.g., Object Detection)**: Require more **hand-engineering**, sophisticated algorithms, and complex network architectures to achieve good results.
- **Large Datasets (e.g., Speech Recognition)**: Can rely on **simpler algorithms** and less manual architecture design, as the sheer volume of data provides the necessary knowledge.
- **Case Study Recommendations**:
    - Use published architectures from the literature.
    - Leverage open-source implementations.
    - Use **pre-trained models** and perform **fine-tuning** on your specific dataset/task.
# Unit 11 : Object Detection




# Unit 12 : Beyond Classification and Detection
# Unit 13 : Sequence Models
# Unit 14 : Machine and Deep Learning for Audio
# Unit 15 : Generative Adversarial Networks (GANs)
# Unit 16 : Human Action Recognition

