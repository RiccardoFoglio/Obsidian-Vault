![[Screenshot 2025-10-02 at 5.37.30 PM.png|500]]
this is the possible shape of the J function

the goal is to minimize it --> find the minimum by calculating the steepest slope
![[Screenshot 2025-10-02 at 5.39.07 PM.png|500]]
![[Screenshot 2025-10-02 at 5.39.17 PM.png|500]]
different starting points and local minimums can be a problem

repeat until convergence (very bottom)

![[Screenshot 2025-10-02 at 5.40.29 PM.png]]

- := simultaneous update, save and update
- $\alpha$ learning rate = determines the step size of each iteration
- derivative = partial derivative on the J function on j
- j=0 and j=1 --> simultaneous update implementation

if learning rate is too small: very slow
if learning rate is too large: might overshoot the minimum and miss it
if Theta_1 is alreay at a local minimum, no udate
it can converge to a local minimum even with the learning rate fixed
As we approach a local minimum, gradient descent will automatically take smaller steps

**batch** gradient descent: each step of gradient descent uses all training examples

everything can be transformed into vectors with multivariate linear regression

![[Screenshot 2025-10-02 at 5.54.55 PM.png|500]]

Feature scaling: make sure features are on a similar scale
![[Screenshot 2025-10-02 at 5.56.36 PM.png]]

Given an approximation of every feature in the range -1, 1
![[Screenshot 2025-10-02 at 5.56.59 PM.png|500]]

Mean Normalization: replace $x_i$ with ($x_i$- $\mu_i$) to make features have approximately zero mean
$\mu_i$ = average value if $x_i$ in training set
$$x_i := \frac{x_i - \mu_i}{s_i}$$
$s_i$ being the range


J should always decrease after every iteration
if it doesn't: use smaller learning rate