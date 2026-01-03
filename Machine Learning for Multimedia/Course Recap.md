
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
For example, you can create a third-order polynomial hypothesis for house prices based on size by using $x_1 = (\text{size})$, $x_2 = (\text{size})^2$, and $x_3 = (\text{size})^3$ .