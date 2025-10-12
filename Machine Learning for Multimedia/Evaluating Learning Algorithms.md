how to analyze the behavior of the algorithm and what to do to improve it

Diagnostic: a test that you can run to gain insights about what is/isn't working with an algorithm and gain audience as to how best to improve its performance
can take time to implement but doing so can be a very good use of your time

learn parameters from training data
compute dev/test set errors as value of the cost function on the dev/test set
cost function depends on model used
- regression error
- classification error
- (misclassification error)...

![[Screenshot 2025-10-12 at 10.17.40 PM.png|500]]

Given several models (different hyperparameters)
- fit each of them --> minimizing the cost function on the training set
- compute cross-validation error --> on cross-val set using param. found for given model
- choose model with lowest error and evaluate how it generalizes (on the test set)

![[Screenshot 2025-10-12 at 10.18.55 PM.png|500]]

Training vs cross-validation error:

- model complexity
![[Screenshot 2025-10-12 at 10.25.16 PM.png|500]]
- training set size
![[Screenshot 2025-10-12 at 10.25.47 PM.png|500]]

error with high bias:
![[Screenshot 2025-10-12 at 10.26.51 PM.png|500]]
at the end of the training, still a high error

error with high variance:
![[Screenshot 2025-10-12 at 10.27.18 PM.png|500]]
at the end of the training, still high variance


when evaluating an classification task, you must be given the human error reference.
- Training error: 8%
- Cross-val error: 11%
![[Screenshot 2025-10-12 at 10.43.37 PM.png|500]]
It can help in choosing what to focus onto

to reduce variance: regularization

chain of assumptions in Machine Learning:
- fits training set well on cost function
- fits cross-val set well on cost function
- fits test set well on cost function
- performs well in real world

Need to:
- find ways to deal with all of them
- possibly having them not influencing each other

