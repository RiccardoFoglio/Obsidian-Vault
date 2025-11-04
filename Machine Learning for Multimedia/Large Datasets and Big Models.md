"It's not who has the best algorithms that wins, but it's who has the most data"

![[Screenshot 2025-11-04 at 11.23.13 PM.png|500]]

Assume feature x has sufficient info to predict y accurately
useful test: given input x, can a human expert confidently predict y?
	can a human expert predict housing price from only size and no other feature?

- Using a learning algorithm with many parameters a **big model** would mean low bias
- Using a **large training set model** would unlikely overfit, which would mean low variance

Idea: combine them

![[Screenshot 2025-11-04 at 11.29.41 PM.png|500]]

## Regularizing neural networks:

Linear Regression:
![[Screenshot 2025-11-04 at 11.31.59 PM.png]]
![[Screenshot 2025-11-04 at 11.32.21 PM.png]]
![[Screenshot 2025-11-04 at 11.32.41 PM.png]]
![[Screenshot 2025-11-04 at 11.32.59 PM.png]]
tell pytorch to apply a L2 regularization

in a neural networks (L Layers)
![[Screenshot 2025-11-04 at 11.35.40 PM.png]]
the goal is to make this smoother so closer to 0

update step/equations --> without regularization:
![[Screenshot 2025-11-04 at 11.38.35 PM.png]]

update step/equations --> with regularization:
![[Screenshot 2025-11-04 at 11.39.04 PM.png]]

Regularization goal: make contribution of weights smoother to make the hypothesis less complex and reduce variance

## Dropout
another regulation technique

![[Screenshot 2025-11-04 at 11.42.51 PM.png]]![[Screenshot 2025-11-04 at 11.43.10 PM.png]]
randomly turn off some neurons --> train a simpler network in a random way

## Data Agumentation
another way to try to reduce overfitting
![[Screenshot 2025-11-04 at 11.44.45 PM.png|500]]
distorted images, rotated, scaled, flipped --> changed in a controlled way

## Batch vs Mini-batch gradient descent

Batch could be very big, would take a long time and make process very slow

splitting batch in mini batches is commonly used

Epoch: one pass through the entire training set
- batch gradient descent: 1 epoch = 1 step
- mini-batch gradient descent: 1 epoch = 5000 steps (if 5000 mini-batches)

![[Screenshot 2025-11-05 at 12.13.37 AM.png|500]]
not a steady monotonic descent, some variability

![[Screenshot 2025-11-05 at 12.15.14 AM.png|500]]

small training (ex: 2000 samples) use batch gradient descent
with larger training set: use mini-batch gradient descent (typical size: 64,128,256...1024)

## The problem of local optima in neural networks
![[Screenshot 2025-11-05 at 12.18.38 AM.png]]
Multiple parameters in a NN, so it's unlikely to get stuck in a local minima
Other problems:
![[Screenshot 2025-11-05 at 12.20.18 AM.png]]
very slow, not steep

consider a very deep NN
![[Screenshot 2025-11-05 at 12.20.43 AM.png]]