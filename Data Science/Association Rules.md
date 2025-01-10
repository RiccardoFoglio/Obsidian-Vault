Extraction of frequent correlations or patterns from a transactional database

A collection of transactions is given, transaction = set of items not ordered.
A, B ==> C
A,B items in the rule body
C item in the rule head
the ==> means co-occurrence, not causality

- Itemset = set including one or more items
- k-itemset = contains k items
- support count = frequency of occurrence of an itemset
- support = fraction of transactions that contain an itemset
- frequent itemset = itemset whose support is greater than or equal to a minsup threshold

Given the association rule A ==> B
- Support : fraction of transactions containing both A and B
- Confidence : frequency of B in transactions containing A, represents the strength of the ==>

Example:
![[Pasted image 20241023102900.png]]

Given a set of transactions T, association rule mining is the extraction of the rules satisfying the constraints:
- support $\ge$ minsup threshold
- confidence $\ge$ mincoft threshold
result is : complete (all rules satisfy both constraints), correct (only the rules satisfying both constraints)

Brute-force approach: enumerate all possible permutations, compute support and confidence for each rule, prune the rules that don't satisfy the minsup and minconf constraints
Computationally unfeasible
Given an itemset, the extraction process may be split:
	first generate frequent itemset, most computationally expensive step
	next generate rules for each frequent itemset

frequent itemset generation:
Brute-force approach: each itemset in the lattice is a candidate frequent itemset. Scan the database to count the support of each candidate, match each transaction against every candidate.
  Complexity : $\approx O(|T|2^dw)$  where |T| number of transactions, d num. of items, w transaction length
Improve efficiency:
- reduce number of candidates
- reduce number of transactions
- reduce the number of comparisons

A Priori Principle : "if an itemset is frequent, then all of its subsets must also be frequent"
The support of an itemset can never exceed the support of any of its subsets
It holds due to the antimonotone property of the support measure
It reduces the number of candidates

Apriori Algorithm
Level-based approach, at each iteration extracts itemsets of a given length k
Two main steps for each level: 
1. candidate generation: 
	   Join Step: generate candidates of length k+1 by joining frequent itemsets of length k
	   Prune Step: apply Apriori principle, prune length k+1 candidate itemsets that contain at least one k-itemset that is not frequent
2. frequent itemset generation: scan DB to count support for K+1 candidates, prune candidates below minsup
![[Pasted image 20241023111310.png]]

Performance: extracting long frequent itemsets requires generating alla frequent subsets.
- minimum support threshold : increases number of frequent itemsets
- dimensionality : more space needed to support count of each item
- size of database : run time may increase with number of transactions
- average transaction width : transaction width increases in dense data sets

Improvements:
- Hash-Based itemset counting: k-itemset whose corresponding hashing bucket count below the threshold cannot be frequent
- Transaction reduction : transaction that doesn't contain any frequent k-itemset is useless in subsequent scans
- Partitioning: any itemset that is potentially frequent in DB must be frequent in at least one of the partitions of DB
- Sampling: mining on a subset of given data
- Dynamic Itemset Counting: add new candidate itemsets only when all of the subsets are estimated to be frequent

## FP-Growth Algorithm

Exploits a main memory compressed representation of the database, the FP-tree
high compression for dense data distributions
complete representation for frequent pattern mining
frequent pattern mining by means of FP-growth
- recursive visit of FP-tree
- applies divide-and-conquer approach
Only two database scans: count item supports + build FP-tree

Scan Header Table from lowest support item up
For each item i in header table extract frequent itemsets including item i and items preceding it in Header Table
1. build conditional pattern base for item i (i-CPB)
2. recursive invocation of FP-growth on i-CPB


An itemset is closed if none of its immediate supersets has the same support as the itemset

Selection of the appropriate minsup threshold is not obvious:
- too high: itemsets including rare but interesting items may be lost
- too low: computationally very expensive, large number of frequent itemsets

Large number of patterns may be extracted, rank them by level inf interestingness.
Objective Measures:
- rank patterns based on statistics computed from data
- initial framework only considered support and confidence
Subjective Measures:
- rank patterns according to user interpretation


Correlation: $\frac{P(A,B)}{P(A)P(B)}$ = $\frac{confr(r)}{sup(B)}$ 
- statistical independence: correlation =1
- positive correlation : correlation > 1
- negative correlation : correlation < 1


Weight could be considered
items or transactions may be weighted

Taxonomy: set of hierarchies that aggregate data items into higher-level concepts

Generalized Itemsets: sets of items at different generalization levels. May contain data items together with generalized items defined in the taxonomy. Summarize knowledge represented by a set of lower-level descendants
A generalized itemset covers a transaction when all:
- its generalized items are ancestors of items included in the transaction
- its data items are included in the transaction
Generalized itemset support ration between number of covered transactions and dataset cardinality
