Machine Learning = field of study that gives computers the ability to learn without being explicitly programmed

A computer program is said to learn from experience E wrt some task T and some performance measure P, if its performance on T, as measured by P, improves with experience E
- E = experience of playing many games of checkers
- T = task of playing checkers
- P = probability that the program will win the next game

Supervised learning = human can act on the training, knows what's going on
Unsupervised learning = human cannot easily figure out the problem the machine is trying to solve

### Housing Price Prediction
![[Screenshot 2025-10-02 at 4.20.03 PM.png|500]]

How much money should i put down for 750 feet squared?

Curve can fit the data, approximately
![[Screenshot 2025-10-02 at 4.21.22 PM.png|500]]
it could be a solution, but this blue line is not the best representation of the data
![[Screenshot 2025-10-02 at 4.22.02 PM.png|500]]
polinomial curve is more fitting, different parameters will change the value on 750 feet squared
![[Screenshot 2025-10-02 at 4.22.42 PM.png|500]]
**Regression** problem, because we have a continuous set of values on the Y axis
supervised learning, because we are providing the valid data used for learning
![[Screenshot 2025-10-02 at 4.25.58 PM.png|500]]

### Malignant or Benign ?
![[Screenshot 2025-10-02 at 4.26.30 PM.png|600]]
given the size what is the chance for a tumor to be malignant/benign?

**Classification**: on Y axis you have a discrete set of values (0 and 1)
Not necessarily only one feature, but multiple
![[Screenshot 2025-10-02 at 5.09.28 PM.png|400]]
multi dimensional, hard do represent

### Unsupervised Learning
Clustering: grouping objects in the same group because they are similar in some sense
![[Screenshot 2025-10-02 at 5.14.02 PM.png|500]]

