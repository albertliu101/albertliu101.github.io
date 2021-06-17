---
layout: post
title:  Hypothesis Testing of Test Sets in Machine Learning
date:   2021-06-10 14:09:47 -0700
---

A common practice in supervised machine learning is to split your data into training, validation, and test sets. The reasoning/motivation for this is to evaluate the performance of your model on an independent set of data. For the classification problem, the distribution of the performance on the test set follows the binomial distribution.

## Preliminaries
The model to evaluate will be a binary classification model. Formally, I will define the model as a function that maps data from the input space to the class space:

$$ f_{model} : X \rightarrow \{0,1\} $$

For the truth data, I will define the following function:

$$ f_{truth} : X \rightarrow \{0,1\} $$

The data will be split into 2 disjoint sets: the training and test set.

$$ X_{train},X_{test} \subset X $$

The last assumption to make will be each training sample and test sample are i.i.d. random samples from the population X.

## Formulation
Suppose I want to predict the performance of my model on other samples drawn from the *same distribution* as the test set.
Performance can be defined an a few different measures. For this case I will define performance as accuracy:

$$ Performance  = \sum_{\forall x \in X_{test}} \frac{ \mathbb{1}_{ \{ f_{model}(x) = f_{truth}(x)\} }}{|X_{test}|} $$

Given the model I can map X into {0,1}:

$$f_{eval}(x) = 1_{\{f_{model}(x) = f_{truth}(x)\}}$$

If I apply $$f_{eval}$$ on the input space, the resulting random variable is bernoulli distributed. The sampling distribution of the test set is thus binomial distributed.

With the sampling distribution of the test set defined it becomes fairly straight forward to perform a 2 sample hypothesis test to check for model drift or assess confidence intervals on the model's performance. If you're feeling particularly bayesian, you can define a prior distribution on the bernoulli parameter.

