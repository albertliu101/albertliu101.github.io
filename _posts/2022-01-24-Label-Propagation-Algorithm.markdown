---
layout: post
title: The Label Propagation Algorithm  
date:   2022-01-24
---
## Semisupervised learning overview
Machine learning has the fairly unique ability to solve problems across many disciplines with data. These solutions often belong to the supervised learning domain; the data needed is labeled data. There is an issue - labeled data can be hard to come by. Unlabeled data on the other hand is far easier to come by. The field of semi-supervised learning is a solution that merges both unlabeled and labeled data into the solution of the problem.

The field of semi-supervised learning can be split into the 2 major categories: inductive and transductive[1]. In an inductive learning setting, data is trained from a dataset with the goal of predicting teh test data samples correctly. In a tranductive setting, the algorithm has access to both the training data as well as the test data. The algorithm that is the subject of this post - the label propagation algorithm - falls into the transductive setting [2].

## Algorithm
The label propagation as the name suggests, propagates the labels of the labeled data to the unlabled data. It does this in a graph-based framework. The first step of the label propagation algorithm is to construct a graph with the dataset's samples as the nodes. The weights of the connections between the nodes can be constructed from a measure of pairwise similarity of the two nodes. By computing the pairwise similarity of each node with every other node, the resulting graph becomes fully connected (if the metric is not 0). With pairwise similarities, a probability measure can be constructed by normalizing the weights between each node to all other nodes to equal 1. The probability that each node "transitions" to the each other node is determined by how "similar" the other node is[2].

We can construct a matrix representing the similarities of the nodes of the graph using the following similarity metric where $$d_{ij}$$ is the local Euclidean distance between the 2 nodes:

$$w_{ij} = e^{\frac{-d_{ij}^2}{\sigma^2}}$$

In matrix form we can represent the transitions probabilities from each node to each other node:

$$T_{ij} = P(j \rightarrow i) = \frac{w_{ij}}{\sum w_{kj}}$$

This transition matrix serves as a linear map that maps the labels of our dataset to every element.

We can construct the initial state of our nodes with the following matrix:

$$Y_{ij} = 1$$ if sample i has label j \
$$Y_{ij} = 0$$ if sample i does not have label j \
$$Y_{ij} = 0$$ if sample i is unlabeled

$$Y_{(i,)}$$ represents the probability over the labels for each node (set to 1 since we know the true label of the sample).

After a single interation of propagation: \
$$Y_{n+1} = TY_{n}$$

Since we know the labeled datapoints already, we do not update the distribution of those points. To address this, the transition matrix T as well as the distribution matrix Y can be partitioned as follows:

$$Y_{l} = $$ distribution of labeled points \
$$Y_{u} = $$ distribution of unlabeled points; we can set these to 0 or a uniform distribution as the initial condition

$$ \begin{bmatrix} Y_{l}\\Y_{u} \end{bmatrix} 
\leftarrow \begin{bmatrix} T_{ll} & T_{lu} \\ T_{ul} & T_{uu} \end{bmatrix} \begin{bmatrix} Y_{l}\\Y_{u} \end{bmatrix}$$

Since we already know the labels for the labeled points, we can keep them constant and only update the submatrix $$Y_{u}$$. \
The update equation becomes: \
$$Y_{u} \leftarrow T_{ul} Y_{l} + T_{uu} Y_{u}$$ 

To maintain a probability measure on the points we need to make sure that the rows of $$Y_{u}$$ sum up to 1. 
To do this we can row normalize the transition matrix T to get matrix $$\bar{T}$$.

The update equation is thus: \
$$ Y_{u} \leftarrow \bar{T}_{ul} Y_{l} + \bar{T}_{uu} Y_{u}$$

This equation is the core of the algorithm. We can get the result after iterating a finite amount of times. However, this formulation has an additional nicety - the limit as the iterations increase converges.

The equation for $$Y_{u}$$ as the number of iterations approaches infinity is:

$$ {Y_{u}}^{(\infty)} = {T_{uu}}^{\infty} + \sum_{n=0}^\infty {T_{uu}}^n T_{ul}Y_{l}$$ \
where:
$${X}^{(n)}$$ is $$X$$ at the $$n^{th}$$ iteration \
$${X}^n$$ is $$X$$ to the $$n^{th}$$ power.

Since $${T_{uu}}^\infty = 0$$ the solution that follows is a geometric series: \
$${Y_{u}}^{(\infty)} = \sum_{n=0}^\infty {T_{uu}}^n T_{ul}Y_{l} $$ \
$$ T_{uu} {Y_{u}}^{(\infty)} = {Y_{u}}^{(\infty)} - T_{ul}Y_{l} $$ \
$$ {Y_{u}}^{(\infty)} = {( I - {T}_{uu} )}^{-1} T_{ul}Y_{l}$$

The result is a probability over the set of labels for each of the unlabled data points.

## References
[1] Van Engelen, Jesper E., and Holger H. Hoos. "A survey on semi-supervised learning." Machine Learning 109.2 (2020): 373-440.

[2] Xiaojin Zhu and Zoubin Ghahramani. "Learning from Labeled and Unlabeled Data with Label Propagation".(2002).
