Objectives:
- prediction of a class label
- definition of an interpretable model of a given phenomenon
![[Pasted image 20241029154408.png]]

Give a collection of class labels and a collection of data objects labelled with a class label:
find a descriptive profile of each class, which will allow the assignment of unlabeled objects to the appropriate class

- Training set: collection of labeled data objects used to learn the classification model
- Test set: collection of labeled data objects used to validate the classification model

Evaluation of classification techniques:
- Accuracy : quality of prediction
- Interpretability : model interpretability and compactness
- Incrementality : model update in presence of newly labeled record
- Efficiency : model building time and classification time
- Scalability : training set size, attribute number
- Robustness : noise, missing data
## Decision Tree

![[Pasted image 20241029163744.png]]
### Hunt's Algorithm
- If $D_t$ contains record that belongs to more than one class:
	- select the "best" attribute A on which to split $D_t$ and label node t as A
	- split $D_t$ into smaller subsets and recursively apply the procedure to each subset
- If $D_t$ contains records that belong to the same class $y_t$ then t is a leaf node labeled $y_t$
- if $D_t$ is an empty set then t is a leaf node labeled as the default class

Adopts a greedy strategy: best attribute for the split is selected locally at each step (Not a global optimum)

Issues: 
- structure of test condition
- selection of best attribute for the split
- stopping condition for the algorithm

Structure of test condition depends on attribute type: nominal ordinal or continuous. It also depends on the number of outgoing edges:
- Multi-way split: as many partitions as distinct values
- Binary split: divides values into two subsets, need to find optimal partitioning

Splitting on continuous attributes: different techniques
- Discretization : to form ordinal categorical attribute (static: discretize once at beginning) (dynamic: discretize during tree induction)
- Binary decision (A $<$ v or A $\ge$ v)

Selection of best attribute
Attributes with homogeneous class distribution are preferred. need a measure of node impurity.
Many different measures available
- GINI index
- Entropy
- Misclassification error
Different algorithms rely on different measures
#### GINI Impurity Measure
$$
GINI(t) = 1-\sum_{j}[p(j|t)]^2
$$
- Max value: $1-1/n_c$ when records are equally distributed among all classes
![[Pasted image 20241030112637.png]]
![[Pasted image 20241030112716.png]]

Split based on GINI:
$$
GINI_{split} = \sum^k_{i=1}\frac{n_i}{n}GINI(i)
$$
where $n_i$ = number of records at child *i*
*n* = number of records at node *p*

#### Entropy Impurity Measure (INFO)
$$
Entropy(t) = -\sum_jp(j|t)\log_2p(j|t)
$$
- Max: $\log(n_c)$ when records are equally distributed
![[Pasted image 20241030113937.png]]

Split based on INFO
$$
GAIN_{split} = Entropy(p)-(\sum^k_{i=1}\frac{n_i}{n}Entropy(i))
$$
$$
GainRATIO_{split} = \frac{GAIN_{split}}{SplitINFO}
$$
$$
SplitINFO = -\sum^k_{i=1}\frac{n_i}{n}\log\frac{n_i}{n}
$$
### Stopping Criteria for Tree Induction

Stop expanding a node when all the records belong to the same class
Stop expanding a node when all the records have similar attribute values

Underfitting : model is too simple, both training and test errors are large
Overfitting : decision boundary is distorted due to noise point

- Pre-Pruning: stop algorithm before it becomes a fully-grown tree. Typical stopping condition for a node: all instances in the same class or all attribute values are the same
   
- Post-Pruning: grow decision tree to its entirety, then trim the nodes in a bottom-up fashion. If generalization error improves after trimming, replace sub-tree by a leaf node. Class label of leaf node is determined from majority class of instances in the sub-tree.

Number of instances gets smaller as u traverse down the tree.
Number of instances at the leaf nodes could be too small to make any statistically significant decision
Missing values affect decision tree construction in 3 ways:
- affect how impurity measures are computed
- affect how to distribute instances with missing value to child nodes
- affect how a test instance with missing value is classified

**Accuracy** : For simple datasets, comparable to other classification techniques 
**Interpretability** : Model is interpretable for small trees, Single predictions are interpretable
**Incrementality** : Not incremental
**Efficiency** : Fast model building, very fast classification
**Scalability** : Scalable both in training set size and attribute number
**Robustness** : Difficult management of missing data

## Random Forest

Ensemble learning technique: multiple base models combined to improve accuracy and stability and to avoid overfitting.
Random forest = set of decision trees: a number of decision trees are built at training time, the class is assigned by majority voting

![[Pasted image 20241031112310.png]]

Bootstrap aggregation: given a training set of instances, it selects B times a random sample with replacement from D and trains trees on these dataset samples. 
For each candidate split in the learning process, a random subset of the features is selected.
Trees are decorrelated: feature subset are sampled randomly, hence different features can be selected as best attributes for the split.

Algorithm
- given a training set D of n instances with p features
- b = 1...B
	- Sample randomly with replacement n' training examples. A subset $D_b$ is generated
	- Train a classification tree on $D_b$
		- during tree construction, for each candidate split
			- $m<<p$ random features are selected ($m \sim \sqrt(p)$)
			- the best split is computed among these m features
- Class is assigned by majority voting among the B predictions
### Evaluation of random forests

- **Accuracy** : Higher than decision trees
- **Interpretability** : 
	- Model and prediction are not interpretable. 	A prediction may be given by hundreds of trees. 
	- Provide global feature importance : an estimate of which features are important in the classification
- **Incrementality** : Not incremental Evaluation of random forests
- **Efficiency** : Fast model building, very fast classification
- **Scalability** : Scalable both in training set size and attribute number
- **Robustness** : Robust to noise and outliers
## Rule-Based Classification

Classify records by using a collection of "if then" rules : $(Condition) \rightarrow y$ 
Examples:
- (Blood Type=Warm) ^ (Lay Eggs=Yes) → Birds

A rule r covers an instance X if the attributes of the instance satisfy the condition of the rule

- Mutually exclusive rules: two conditions can't be true at the same time
- Exhaustive rules: classifier rules account for every possible combination of attribute values

Rules can be simplified, but become no longer mutually exclusive or exhaustive.

Ordered rule set: rules are rank ordered according to their priority (an ordered rule set is known as a decision list.) When a test record is presented to the classifier, it's assigned to the class label of the highest ranked rule it has triggered. If none of the rules fired, it's assigned to the default class

Building classification rules:
- Direct method : extract rules from data
- Indirect method : extract rules from other classification models (decision trees, neural networks...)

- Accuracy : Higher than decision trees
- Interpretability : Model and prediction are interpretable
- Incrementality : Not incremental Evaluation of rule based classifiers
- Efficiency : Fast model building, very fast classification 
- Scalability : Scalable both in training set size and attribute number
- Robustness : Robust to outliers

## K-Nearest Neighbor


## Bayesian Classification

Bayes Theorem: let C and X be random variables 

$P(C,X) = P(C|X) P(X)$ and $P(X,C) = P(X|C) P(C)$

Hence $$P(C|X) P(X) = P(X|C) P(C)$$ 
and also $$P(C|X) = P(X|C) P(C) / P(X)$$
How the classification works:
- let the class attribute and all data attributes be random variables. C = any class label, X = records to be classified
- Compute $P(C|X)$ for all classes
- Assign X to the class with Maximal $P(C|X)$
- Applying Bayes Theorem: $P(C|X) = P(X|C)\cdot P(C)/P(X)$

How to estimate $P(X|C)$ ?
- Naive Hypothesis : $P(x_1,...,x_k|C) = P(x_1|C)P(x_2|C)...P(x_k|C)$, not always true
- Bayesian networks : allow specifying a subset of dependencies among attributes

Example:
![[Pasted image 20241031121337.png|400]]
![[Pasted image 20241031121357.png|400]]
![[Pasted image 20241031121415.png|400]]

- Accuracy : Similar or lower than decision trees, naïve hypothesis simplifies model
- Interpretability : Model and prediction are not interpretable, the weights of contributions in a single prediction may be used to explain 
- Incrementality : Fully incremental, does not require availability of training data  
- Efficiency : Fast model building, very fast classification
- Scalability : Scalable both in training set size and attribute number
- Robustness : Affected by attribute correlation

## Support Vector Machines

find a linear hyperplane that separates the data

- Accuracy : Among best performers 
- Interpretability : Model and prediction are not interpretable, **Black box model**
- Incrementality : Not incremental Evaluation of Support Vector Machines 
- Efficiency : Model building requires significant parameter tuning, very fast classification 
- Scalability : Medium scalable both in training set size and attribute number
- Robustness : Robust to noise and outliers

## Artificial Neural Networks

inspired to the structure of the human brain
- neurons are elaboration units
- synapses as connection network
Different tasks --> different architectures

![[Pasted image 20241105152609.png|500]]
![[Pasted image 20241105152625.png|500]]

Activation: stimulates biological activation to input stimuli, provides non-linearity to the computation, may help saturate neuron outputs in fixed ranges
Different functions:
- Sigmond ( $\tanh$ ) : saturate input value in a fixed range, non linear for all the input scale, typically used by FFNNs for both hidden and output layers
![[Pasted image 20241108143642.png|500]]

- Binary Step : outputs 1 when input is non-zero. Useful for binary outputs. Issues: not appropriate for gradient descent (derivative not defined in 0, and =0 in every other position

- ReLU (Rectified Linear Unit) : used in deep networks (CNNs), avoids vanishing gradient, doesn't saturate. Neurons activate linearity only for positive input

- SoftMax : differently to other activation functions it's applied only to the output layer. After SoftMax, the output vector can be interpreted as a discrete **distribution of probabilities**
- ![[Pasted image 20241108144044.png|500]]
### Building a FFNN
for each node: define weights and offset value
providing the highest accuracy on the training data
Iterative approach on training data instances
algorithm:
```
assign random values to weights and offsets
process instances in the training set one at a time
for each neuron
	compute result when applying weights, offset and activation function
forward propagation until the output is computed
compare the output with expected output and evaluate error
backpropagation of the error, by updating weights and offset
```
Process ends when:
- % accuracy above given threshold
- % of parameter variation below given threshold
- max number of epochs is reached


- Accuracy : among best performers 
- Interpretability : Model and prediction are not interpretable, Black box model 
- Incrementality : Not incremental Evaluation of Feed Forward NN 
- Efficiency : Model building requires very complex parameter tuning, it requires significant time, very fast classification
- Scalability : Medium scalable both in training set size and attribute number 
- Robustness : Robust to noise and outliers, requires large training set, otherwise unstable when tuning parameters
### Convolutional Neural Networks (CNN)

Allow automatically extracting features from images and performing classification
![[Pasted image 20241108144847.png|500]]

Typical convolutional layer:
- convolution stage - feature extraction by means of sliding filters
- sliding filters activation - apply activation functions to input tensor
- pooling  - tensor downsampling 
![[Pasted image 20241108145049.png|500]]

**Tensors** : data flowing through CNN layers is represented in the form of tensors, N-dimensional vectors.
**Rank** = number of dimensions (0 : scalar, 1 : 1D vector, 2 : 2D vector (matrix))
**Shape** = number of elements for each dimension (ex: vector of length 5 has shape 5)

Images = rank-3 tensors with shape \[depth, height, width]
![[Pasted image 20241108145355.png]]

**Convolution** : processes data in form of tensors.
Input : input image or intermediate features (tensors)
Output : tensor with extracted features
Sliding filter produces the values of the output tensor. contain the trainable weights of the NN. Each convolutional layer contains many (hundreds) filters
after convolving tensor with N filters we obtain:
- rank-3 tensor with shape \[N,h,w]
- each filter generates a layer in the depth of the output tensor

**Activation** : ReLU is typically used for CNNs (faster training, doesnt saturate, faster computation of derivatives)

**Pooling** : performs tensor down-sampling. sliding filter which replaces sensor with a summary statistic of nearby outputs. maxpool is the most common : computes the max value as statistic
![[Pasted image 20241108150030.png|500]]

Convolutional layers training : during training each sliding filter learns to recognize a particular pattern in the input tensor.
Filters in shallow layers recognize textures and edges
Filters in deeper layers recognize objects and parts

Sematic segmentation CNNs : allow assigning a class to each pixel of the input image.
Composed of 2 parts:
- encoder network : convolutional layers to extract abstract features
- decoder network : deconvolutional layers to obtain the output image from the extracted features

### Recurrent Neural Networks (RNN)
allow processing sequential data $x(t)$. Differently from normal FFNN they are also able to keep a state which evolves during time.
Applications : machine translation, time series prediction, speech recognition, part of speech tagging

A RNN receives as input a vector $x(t)$ and the state at previous time step $s(t-1)$ 
A RNN typically contains many neurons organized in different layers
![[Pasted image 20241108150941.png|500]]

Training performed with **Backpropagation Through Time**.
Given a pair training sequence $x(t)$ and expected output $y(t)$ : error is propagated through time, weights are updated to minimize the error across all the time steps

Issues
- vanishing gradient : errore gradient decreases rapidly over time, weights are not properly updated. This makes harder having RNN with long-term memories
Solution : LSTM (Long Short Term Memories)
- RNN with gates which encourage the state information to flow through long time intervals

Autoencoders will allow compressing input data by means of compact representations and from them reconstruct the initial input. 
- for feature extraction : the compressed representation can be used as significant set of features representing unput data
- for image (or signal) denoising: the image reconstructed from the abstract representation is denoised with respect to the original one
![[Pasted image 20241108151301.png|500]]

### Word Embeddings (Word2Vec)
word embeddings associate words to n-dimensional vectors
- trained on big text collections to model the word distributions in different sentences and contexts.
- able to capture the semantic information of each word
- words with similar meaning share vectors with similar characteristics
![[Pasted image 20241108151708.png|500]]

Each word is represented with a vector, so operations among words are allowed
![[Pasted image 20241108151737.png|500]]

Semantic relationships among words are captured by vector positions
![[Pasted image 20241108151759.png|500]]






# Model Evaluation

Methods for performance evaluation : partitioning techniques for training and test sets
Metrics for performance evaluation: accuracy, other measures
Techniques for model comparison: ROC curve
Objective: reliable estimate of performance
Performance of a model may depend on other factors besides the learning algorithm: class distribution, cost of misclassification, size of training and test sets...

Learning curve shows how accuracy changes with varying training sample size.
Requires a sampling schedule for creating learning curve:
- arithmetic sampling
- geometric sampling
Effect of small sample size: bias in the estimate, variance of estimate

Methods of estimation:
- Partitioning labeled data for training, validation and test
- several partitioning techniques: holdout, cross validation
- Stratified sampling to generate partitions, without replacement
- Bootstrap, sampling with replacement

Holdout: fixed partitioning (80% training, 20% test). Appropriate for large datasets. May be repeated several times

Cross Validation: 
	partition data into k disjoint subsets
	k-fold: train on k-1 partitions, test on the remaining one
	reliable accuracy estimation, not appropriate fore very large datasets
Leave-One-Out:
	cross validation for k=n
	only appropriate for very small datasets

Model performance estimation:
- Model training step : building a new model
- Model validation step : hyperparameter tuning, algorithm selection
- Model test step : estimation of model performance

typical dataset size:
- training set : 60%
- validation set : 20%
- test set : 20%

Splitting labeled data: use hold-out to split in: training+validation, test
Use cross validation to split in training and validation

Binary classifier:
![[Pasted image 20241105145505.png]]

Accuracy = Num. of correctly classified objects / Num. of classified objects = $\frac{a+d}{a+b+c+d}$

Recall (r) = Num. of objects correctly assigned to C / Num. of objects belonging to C = $\frac{a}{a+c}$

Precision (p) = Num. of objects correctly assigned to C / Num. of objects assigned to C = $\frac{a}{a+b}$

F - measure (F) = $\frac{2rp}{r+p}$ = $\frac{2a}{2a+b+c}$

Receiver Operating Characteristic (ROC) : characterizes the trade-off between positive hits and false alarms
- TPR : True Positive Rate : TPR = TP / (TP + FN)
- FPR : False Positive Rate : FPR = FP / (FP + FN)

![[Pasted image 20241105151540.png]]
(FPR, TPR)
(0,0) : declare everything to be negative class
(1,1) : declare everything to be positive class
(0,1) : ideal

To build ROC Curve:
- use classifier that produces posterior probability for each test instance P(+|A)
- sort the instances according to P(+|A) in decreasing order
- Apply threshold at each unique value of P(+|A)
- count the number of TP, FP, TN, FN and find TPR, FPR

No model consistently outperforms the other
Ideally area under ROC is 1.0, with random guess is 0.5