classification in different classes
![[Screenshot 2025-10-04 at 4.43.00 PM.png|500]]

Dataset:
![[Screenshot 2025-10-04 at 4.45.08 PM.png|500]]
![[Screenshot 2025-10-04 at 4.47.01 PM.png|500]]

after adding a new data in the dataset
![[Screenshot 2025-10-04 at 4.47.33 PM.png|500]]
 the linear approach doesn't seem to be working anymore
![[Screenshot 2025-10-04 at 4.48.07 PM.png|500]]

Cannot and shouldn't use linear hypothesis for classification problems, we use

**Logistic Regression (Logit)**

![[Screenshot 2025-10-04 at 4.56.03 PM.png|500]]

example with dataset with 2 features: tumor size and age of patient
![[Screenshot 2025-10-04 at 5.02.49 PM.png|500]]
I ran some algorithms and got the parameters that will minimize the hypothesis
![[Screenshot 2025-10-04 at 5.04.00 PM.png|500]]

![[Screenshot 2025-10-04 at 5.04.51 PM.png|500]]
![[Screenshot 2025-10-04 at 5.05.43 PM.png|500]]


![[Screenshot 2025-10-04 at 5.08.05 PM.png]]
gradient descent is no more convex, so you need a log to get rid of the exponential part of the regression
![[Screenshot 2025-10-04 at 5.09.38 PM.png|400]]![[Screenshot 2025-10-04 at 5.10.31 PM.png|400]]

BINARY CROSS-ENTROPY COST FUNCTION
![[Screenshot 2025-10-04 at 5.11.36 PM.png]]

now using the gradient descent:
![[Screenshot 2025-10-04 at 5.12.55 PM.png]]

Looks the same to linear regression but it's the definition of $h_\theta(x)$ that changed

### Multi Class
![[Screenshot 2025-10-04 at 6.46.02 PM.png|500]]
applying to each class the classification will lead to multiple grouping
![[Screenshot 2025-10-04 at 6.46.29 PM.png|500]]

**underfitting** = we don't have enough features, the hypothesis may not fit the training set well, the model has not learned enough and makes unreliable predictions --> high bias

**overfitting** = if we have too many features the hypothesis may fit the training set very well but fail to generalize to new examples (predict new values...) --> high variance

![[Screenshot 2025-10-04 at 6.49.52 PM.png|500]]

To solve overfitting:
- reduce number of features: 
	- manually select which features to keep
	- model selection algorithms
- regularization
	- keep all features but reduce magnitude
	- works well when we have a lot of features, each of which contributes a bit to predicting y

![[Screenshot 2025-10-04 at 6.53.44 PM.png]]
$\lambda$ = regularization parameter
need to update the optimization algorithm accordingly

