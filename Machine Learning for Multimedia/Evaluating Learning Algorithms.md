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

## Error with skewed classes

Cancer classification example
- train classifier: Y=1 cancer, Y=0 no cancer
- threshold: set to 0.5 --> greater than 0.5 means y=1
- I find that i'm correct 99%, so 1% error on validation/test set

what if only a small fraction have cancer? like 0.5%
if i have a function that ignores input and always gives 0 (no cancer)

![[Screenshot 2025-11-04 at 10.37.58 PM.png|500]]

Precision: of all patients where we predicted y=1, how many really have cancer?
$$
precision = \frac{true\_pos}{true\_pos + false\_pos}
$$
Recall: of all patients that actually have cancer, how many did we correctly detect?
$$
recall = \frac{true\_pos}{true\_pos + false\_neg}
$$

We want to predict y=1 only if very confident
- threshold set to 0.7 or 0.9 (very high)
High precision, but low recall

We want to avoid missing too many cases of cancer (avoid false negatives)
- threshold set to 0.4 or 0.2
Lower precision, but higher recall

Comparison between precision (P) and recall (R)
![[Screenshot 2025-11-04 at 10.50.43 PM.png|500]]

When we have multiple metrics to classify an algorithm, we can have max 1 optimizing metrics and then satisficing metrics:

![[Screenshot 2025-11-04 at 11.14.46 PM.png|400]]

if we have enough time, the peak accuracy is limited by the human-level performance. Past it, machines are needed to improve accuracy but take a long time

![[Screenshot 2025-11-04 at 11.17.03 PM.png|500]]

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


