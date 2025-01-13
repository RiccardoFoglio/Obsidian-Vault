finding groups of objects such that the objects in a group will be similar to one another and different from other groups
![[Pasted image 20241106110208.png]]

Clusters used for:
- understanding data, group related documents etc...
- summarize large data sets

A clustering is a set of clusters
important distinction between:
- Partitional Clustering : divides data objects into non-overlapping subsets such that each data object is exactly one subset
- Hierarchical Clustering : set  of nested clusters organized as a hierarchical trees

Other distinctions between sets of clusters:
- Exclusive / Non-Exclusive : in non-ex, points can belong to multiple clusters
- Fuzzy / Non-Fuzzy : point belong to every cluster with a weight 0-1
- Partial / Complete : cluster only a part of the data
- Heterogeneous / Homogeneous : cluster of widely different sizes, shapes and densities

### Well Separated
cluster is a set of points such that any point in a cluster is closed to every other point in the cluster than to any point not in the cluster
![[Pasted image 20241106115209.png]]
### Center-Based
The center of a cluster is called centroid, the average of all the points in the cluster, or medoid, the most representative point of a cluster

![[Pasted image 20241106115306.png]]
### Contiguity-Based
![[Pasted image 20241106121005.png]]
### Density-Based
dense region of points, which is separated by low-density regions, from other regions of high density.
Used when the clusters are irregular or intertwined, and when noise and outliers are present.

![[Pasted image 20241106121018.png]]
### Conceptual Cluster
finds clusters that share some common property or represent a particular concept
![[Pasted image 20241106121130.png]]

# Clustering Algorithms

## K-Means Clustering
- partitional clustering approach
- each cluster is associated with a centroid (center point)
- each point is assigned to the cluster with the closest centroid
- number of clusters K must be specified

algorithm:
```
Select K points as initial centroids
repeat
	form K clusters by assigning all points to the closest centroid
	recompute centroid for each cluster
until : centroids don't change
```

- Initial centroids are often chosen randomly
- The centroid is (typically) the mean of the points in the cluster
- ‘Closeness’ is measured by Euclidean distance, cosine similarity, correlation, etc...
- K-means will converge for common similarity measures mentioned above.
- Most of the convergence happens in the first few iterations
- Complexity is $O( n * K * I * d )$
	- n = number of points
	- K = number of clusters
	- I = number of iterations
	- d = number of attributes
![[Pasted image 20241107110501.png]]

Most common measure is Sum of Squared Error (SSE)
$$
SSE = \sum^K_{i=1}\sum_{x\in C_i}dist^2(m_i, x)
$$
- For each point, the error is the distance to the nearest cluster
- x is a data point in cluster $C_i$ and $m_i$ is the representative point for cluster $C_i$
- Given two clusters, we can choose the one with the smallest error
- One easy way to reduce SSE is to increase K, the number of clusters

Initial Centroids Problem
- Multiple runs - helps but not on my side
- Sample and use hierarchical clustering to determine initial centroids
- select more than k initial centroids and then select the most widely separated
- postprocessing
- Bisecting K-means

Elbow Graph (Knee approach)
- plotting the quality measure trend (SSE) against K
![[Pasted image 20241107113724.png]]

Handling Empty Clusters
- Basic K-means algorithm can yield empty clusters
- several strategies:
	- Choose the point that contributes most to SSE
	- Choose a point from the cluster with the highest SSE
	- If there are several empty clusters, the above can be repeated several times.

Pre-processing : normalize the data, eliminate outliers
Post-processing : eliminate small clusters that may represent outliers, split "loose" clusters (high SSE), merge clusters that are close and have relatively low SSE

### Bisecting K-Means
variant of k-means algorithm, can produce a partial or hierarchical clustering
```
initialize the list of clusters to contain the cluster containing all points
repeat
	select a cluster from list of clusters
	for i = 1 to num_iterations do
		bisect the selected cluster using basic K-means
	end for
	add the 2 clusters from the bisection with the Lowest SSE to list of clusters
until : list of cluster contains K clusters
```

Limitation of K-means
- differing sizes
- differing densities
- differing non-globular shapes
- data outliers
## Hierarchical Clustering

Produces a set of nested clusters organized as a hierarchical tree
Can be visualized as a dendrogram
![[Pasted image 20241107114638.png]]

Strengths:
- Do not have to assume any particular number of clusters
- They may correspond to meaningful taxonomies

Two main types of hierarchical clustering:
- Agglomerative : Start with the points as individual clusters, At each step, merge the closest pair of clusters until only one cluster (or k clusters) left
- Divisive : Start with one, all-inclusive cluster, At each step, split a cluster until each cluster contains a point (or there are k clusters)
Traditional hierarchical algorithms use a similarity or distance matrix: Merge or split one cluster at a time
### Agglomerative Clustering Algorithm
More popular hierarchical clustering technique
```
Compute the proximity matrix
Let each data point be a cluster
repeat
	merge two closest clusters
	update the proximity matrix
until : only a single cluster remains
```
Different methods to define inter-cluster similarity:
- MIN : can handle non-elliptical shapes BUT is sensitive to noise and outliers
- MAX : less susceptible to noise and outliers BUT tends to break large clusters, biased towards globular clusters
- Group Average :  less susceptible to noise and outliers BUT biased towards globular clusters
- Distance between Centroids
- Other methods driven by an objective function

### Hierarchical Clustering : Group Average
Proximity of two clusters is the average of pairwise proximity between points in the two clusters
$$
proximity(Cluster_i, Cluster_j) = \frac{\sum proximity(p_i, p_j)}{|Cluster_i|\cdot|Cluster_j|}
$$
Need to use average connectivity for scalability since total proximity favors large clusters
![[Pasted image 20241108152141.png|500]]

Compromise between Single and Complete Link.
- Strengths : less susceptible to noise and outliers
- Limitations : biased towards globular clusters

### Cluster Similarity : Ward's Method
Similarity between two clusters is based on the increase in squared error when two clusters are merged. Similar to group average if distance between points is distance squared.

Less susceptible to noise and outliers
Biased towards globular clusters
Hierarchical analogue of K-Means --> can be used to initialize K-Means

![[Pasted image 20241108152441.png|500]]
### Time and Space Requirements

- $O(N^2)$ space since it uses the proximity matrix
- $O(N^3)$ time in many cases : there are $N$ steps and at each step the $N^2$ matrix must be updated 
## DBSCAN

DBSCAN is a density-based algorithm
- Density = number of points within a specific radius (Eps)
- a point is a **core point** if it has more than a specified number of points (MinPts) within Eps
- a **border point** has fewer than MinPts within Eps, but is in the neighborhood of a core point
- A noise point is any point that is not a core point or a border point 
![[Pasted image 20241108153149.png|500]]

Algorithm:
```
current_cluster_label = 1
for all core points
	if core point has no cluster label
		current_cluster_label = current_cluster_label + 1
		label current core point with cluster label
	for all points in eps-neighborhood except i-th (point itself)
		if point doesn't have cluster label
			label the point with current_cluster_label
```
![[Pasted image 20241108153355.png|500]]

Determining EPS and MinPts: idea is that for points in a cluster, their $k^{th}$ nearest neighbors are at roughly the same distance.
Noise points have the $k^{th}$ nearest neighbor at farther distance
Plot sorted distance of every point to its $k^{th}$ nearest neighbor
# Cluster Validity

For supervised classification we have a variety of measures to evaluate how good our model is.
For cluster analysis, the analogous question is how to evaluate the “goodness” of the resulting clusters?
We evaluate them:
- to avoid finding patterns in noise
- to compare clustering algorithms
- to compare two sets of clusters
- to compare two clusters

The aspects of cluster validation:
1. Determining the clustering tendency of a set of data, i.e., distinguishing whether non-random structure actually exists in the data.
2. Comparing the results of a cluster analysis to externally known results, e.g., to externally given class labels.
3. Evaluating how well the results of a cluster analysis fit the data without reference to external information. - Use only the data
4. Comparing the results of two different sets of cluster analyses to determine which is better.
5. Determining the ‘correct’ number of clusters.

For 2, 3, and 4, we can further distinguish whether we want to evaluate the entire clustering or just 
individual clusters.

Numerical measures are applied to judge various aspects of cluster validity
- External Index: Used to measure the extent to which cluster labels match externally supplied class labels
- Internal Index: Used to measure the goodness of a clustering structure without respect to external information
- Relative Index: Used to compare two different clusterings or clusters

**Cluster Cohesion**: Measures how closely related are objects in a cluster
$$
WSS=\sum_i \sum_{x\in C_i}(x-m_i)^2
$$
**Cluster Separation**: Measure how distinct or well separated a cluster is from other clusters – Separation is measured by the between cluster sum of squares
$$
BSS = \sum_i|C_i|(m-m_i)^2
$$
A proximity graph based approach can also be used for cohesion and separation.
- Cluster cohesion is the sum of the weight of all links within a cluster.
- Cluster separation is the sum of the weights between nodes in the cluster and nodes outside the cluster.
![[Pasted image 20241113114010.png]]
## Silhouette
A succinct measure to evaluate how well each object lies within its cluster
It is defined for single points
It considers both cohesion and separation
Can be computed for:
- individual points
- individual clusters
- clustering results

For each object i:
- a(i) : the average dissimilarity of i with all other objects within the same cluster (the smaller the value, the better the assignment)
- b(i) : min(average dissimilarity of i to any other cluster, of which i is not a member)
$$
s(i) = \frac{b(i)-a(i)}{\max\{a(i),b(i)\}}
$$
Ranges between -1 and +1, typically between 0 and 1, the closer to 1 the better

Silhouette for clusters and clusterings
- The average s(i) over all data of a cluster measures how tightly grouped all the data in the cluster are
- The average s(i) over all data of the dataset measures how appropriately the data has been clustered
## Rand Index
Any two objects that are in the same cluster should be in the same class and vice versa
given:
- f00 = number of pairs of objects having a different class and a different cluster
- f01 = number of pairs of objects having a different class and the same cluster
- f10 = number of pairs of objects having the same class and a different cluster
- f11 = number of pairs of objects having the same class and the same cluster

The Rand Index:
$$
Rand\ Index= \frac{f_{00}+f_{11}}{f_{00}+f_{01}+f_{10}+f_{11}}
$$

