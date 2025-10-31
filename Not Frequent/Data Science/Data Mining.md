Data mining = algorithms to discover information hidden into a data collection. The extraction is automatic and is represented using patterns.

Data --> Selected data --> preprocessed data --> transformed data --> pattern --> knowledge

Techniques for: association rule extraction, classification, clustering

- Descriptive methods : extract interpretable models describing data
- Predictive methods : exploit some known variables to predict unknown or future values of other variables
# Data Processing

Data = collection of data objects and their attributes, property or characteristic of the object. 
A collection of attributes describe an object.

Types:
- Categorical: symbols
	- Nominal : distinctness --> eye color, id number etc...
	- Ordinal : distinctness & order --> rankings, grades etc...
- Numeric: numbers
	- Interval : distinctness & order & addition --> calendar dates
	- Ratio : all 4 --> temperature, length, time, counts etc...

Type of attribute depends on which of the following it possesses:
- Distinctness : either the same or different
- Order : can be ordered, so greater or lesser than
- Addition : add or subtract
- Multiplication : multiply or divide values

- Discrete Attribute
	- has only a finite or countably infinite set of values
	- often represented as integer variables
	- binary attributes are a special case of discrete
- Continuous Attribute
	- has real numbers as attribute values
	- finite number of digits to represent them
	- floating-point variables
### Data Set Types
- Record
	- Tables : each record has a fixed set of attributes
	- Document Data : semi-structured or unstructured, like Textual Data, still representable in a tabular way (words and frequency in document)
	- Transaction Data : each record is a set of items, ex: grocery store
- Graph
	- World Wide Web : information are nodes, and connections are links with weight
	- Molecular Structures
- Ordered
	- Spatial Data
	- Temporal Data : evolution of value over time
	- Sequential Data
	- Genetic Sequence Data
### Data Quality
Poor data quality negatively affects many data processing efforts
Data quality problems:
- Noise and Outliers
- Missing values
- Duplicate data
- Wrong data
#### Outliers
data objects with characteristics very different than most other data objects in data set
- noise that interferes with data analysis
- goal of the analysis: find the frauds, intrusions etc..
#### Missing Values
information not collected, attributes not applicable to all cases
#### Duplicate Data
Data set may include data objects that are duplicates, or almost duplicates of one another, major issue when merging data from heterogeneous sources
Data Cleaning : process of dealing with duplicates
# Data Preparation

### Aggregation
combining two or more attributes into a single one
- data reduction --> reduced representation of the dataset
	- sampling : reduce cardinality of set
	- feature selection : reduce number of attributes
	- discretization : reduce cardinality of attribute domain
- change of scale
- more stable data
#### Sampling
Main technique for data selection, obtaining and processing full data set is too expensive and time consuming.
Must represent the sample as a representative of the full dataset

- Simple Random Sampling : equal probability of selecting any particular item
	- with replacement : removes from the population
	- without replacement : just copy, no removal (items can be picked more than once)
- Stratified sampling : split data into several partitions, draw random samples from each partition
#### Dimensionality Reduction
avoid curse of dimensionality, reduce amount of time and memory required by data mining algorithms, allow data to be more easily visualized, may help eliminate irrelevant features and reduce noise.

Techniques:
- Principal Component Analysis (PCA) --> goal is to find a projection that captures the largest amount of variation in data
- Singular Value Decomposition
- Others: supervised and non-linear techniques

Curse of Dimensionality: When dimensionality increases, data becomes increasingly sparse in the space that it occupies.
#### Feature Subset Selection
Redundant features duplicate much of the info contained in one or more attributes
Irrelevant features contain no info that is useful for the data mining task at hand
- Brute force approach : try all possible feature subsets as input to data mining algorithm
- Embedded approaches : feature selection occurs naturally as part of data mining algorithm
- Filter approaches : features selected before data mining algorithm is run
- Wrapper approaches : use the data mining alg. as black-box to find best subset

Feature Creation: create new attributes that can capture the important information in a data set much more efficiently than the original attributes
#### Discretization
Process of converting a continuous attribute into an ordinal attribute.
Potentially infinite number of values mapped into a small number of categories.

Discretization is commonly used in classification: many classification algorithms work best if both the independent and dependent variables have only a few values

- N intervals with the same with width : $W = (V_{max}-V_{min}) / N$
- N intervals with the same cardinality : each interval contains the same n of objects
- clustering
- analysis of data distribution

Binarization : maps an attribute into one or more binary variables
- Continuous attribute : first map the attribute to a categorical one {low medium high}
- Categorical attribute : mapping to a set of binary attributes {high 1 0 0; medium 0 1 0; low 0 0 1}
one-hot encoding, only 1 bit takes the value 1
### Attribute Transformation

function that maps the entire set of values of a given attribute to a new set of replacement values such that old value can be identified with one of the new values

Normalization : type of data transformation.
Values of an attribute are scaled to fall into a small range \[-1, +1] or \[0, +1]
Techniques:
- min-max normalization : $v' = \frac{v-\min_A}{\max_A-\min_A}(new\_\max_A - new\_\min_A) + new\_\min_A$
- z-score normalization : $v' = \frac{v-\min_A}{stand\_dev_A}$
- decimal scaling : $v' = \frac{v}{10^j}$ where j is the smallest integer such that $\max(|v'|)<1$
## Similarity and Dissimilarity

Similarity = numerical measure of how alike two data objects are, it's higher when objects are more alive, often falls in the range \[0,1]

Dissimilarity = numerical measure of how different are two data objects. Minimum dissimilarity is often 0. Upper limit varies.

Proximity refers to a similarity or dissimilarity

![[Pasted image 20241022145559.png]]

#### Euclidian Distance

$$d(X,Y) = \sqrt{(\sum^{n}_{k=1}(x_k-y_k)^2}$$

where n is the number of dimensions (attributes) and $x_k$ and $y_k$ are the attributes of data object X and Y

#### Minkowski Distance
$$
d(X,Y) = (\sum^n_{k=1}|x_k-y_k|^r)^{1/r}
$$
where r is a parameter.

- R = 1 --> City Block distance : common example is the hamming distance, number of bits that are different between two binary vectors
- R = 2 --> Euclidean distance
- R = $\infty$ --> "supremum" distance : maximum difference between any component of the vectors

Common Properties of a Distance:
- $d(x,y) \ge 0$ Positive Definiteness
- $d(x,y) = d(y,x)$ Symmetry
- $d(x,z) \le d(x,y) +d(y,z)$ Triangle Inequality

Common Properties of a Similarity
- $s(x,y) = 1$ only if $x=y$
- $s(x,y) = s(y,x)$

SMC versus Jaccard
![[Pasted image 20241022153658.png]]
Cosine Similarity
![[Pasted image 20241022153707.png]]
## Correlation

Measure of the linear relationship between two data objects having binary or continuous variables.
Useful during the data exploration phase to be better aware of data properties
Analysis of feature correlation. Correlated features should be removed

Pearson's Correlation
![[Pasted image 20241022154501.png]]
![[Pasted image 20241022154606.png]]
