# 10. Machine Learning Research Part 3

This is the final post regarding our initial research into machine learning. In this post we discuss naive Bayes classifiers and decision tree models. We conclude that a decision tree is the most readily applicable model class to our project.

## Naive Bayes

Naive Bayes classifiers use Bayes' theorem to predict the most likely class of an observation. They're called "naive" because they assume features are statistically independent. NBCs are popular for their simplicity and scalability.

### Conditional Probability

Conditional probability describes the likelihood of an event given that another event has happened.   

## $P{\left(A\ |\ B\right)} = \cfrac{P{\left(A\ \cap \ B\right)}}{P{\left(B\right)}}$

The probability of A given B is exactly the probability of A and B, over the probability of B. What happens if we swap the events:

## $P{\left(B\ |\ A\right)} = \cfrac{P{\left(B\ \cap \ A\right)}}{P{\left(A\right)}}$

## $= \cfrac{P{\left(A\ \cap \ B\right)}}{P{\left(A\right)}}$

## $= \cfrac{P{\left(A\ |\ B\right)\cdot P{\left(B\right)}}}{P{\left(A\right)}}$

This is called Bayes' theorem: the probability of B given A is exactly the probability of A given B times the probability of B, over the probability of A. We can extend this to the problem of classification.

### Derivation

## $P{\left(y\ |\ \vec{x}\right)}=\cfrac{P{\left(\vec{x}\ |\ y\right)}\cdot P{\left(y\right)}}{P{\left(\vec{x}\right)}}$

(What is the probability of label/output $y$ given some feature vector $\vec{x}$?)

Now since we're given a feature *vector,* we need to separate its components. This can be done using the chain rule of probability and exploiting conditional independence:

## $P{\left(y\ |\ \vec{x}\right)}=P{\left( y\right)} \cdot  P{\left(x_0\ |\ {y}\right)} \cdot P{\left(x_1\ |\ {y}\right)} \cdot\ .\ .\ .\ \cdot P{\left(x_n\ |\ y\right)}$

This equation is "naive" because it assumes the components of $\vec x$ (i.e. the features) are statistically independent. If this isn't actually the case, the model can become inaccurate.

The denominator stays constant, so we're left with:

## $P{\left(y\ |\ \vec{x}\right)} =\frac{1}{k} \cdot P{\left( y\right)} \cdot \prod_{i=0}^nP{\left(x_i\ | \ y\right)}$

To convert this to a classifier, we just need to select the label which maximizes this value:

## $p{\left(\vec{x}\right)}=\text{argmax}_{y\in Y}\left(P{\left( y\right)} \cdot \prod_{i=0}^nP{\left(x_i\ | \ y\right)}\right)$

### Laplace Smoothing

If any term in the product is zero, the entire product will be zero. For example, if the feature vector represents word frequencies, there may be words which do not appear in the dataset and have a likelihood of zero.

To solve this problem, we increase each value in the feature vector by one. This will remove any zero values without effecting the results of argmax.

## Decision Tree Models

Tree models divide up a dataset by making a series of explicit decisions depending on feature values. 

![Untitled](10%20Machine%20Learning%20Research%20Part%203%2046c0bdc6fcfb4ebeb89501efbec389e7/Untitled.png)

In a decision tree classifier, leaf nodes (shown in colour), represent class predictions, i.e. "x is a cat." Nodes with children represent binary questions, i.e. "is x covered in fur?" 

### Advantages

- can be applied to numeric and categorical data without needing to use dummy variables
- low pre-processing requirements
- easy to visualise, interpret and understand (this is especially relevant to NugeShare)
- high performance, compared to larger models like neural networks
- can approximate any Boolean function including XOR

### Disadvantages

- small changes in training data can result in vastly different trees
- trees tend to easily become overfit (can be alleviated with pruning)
- greedy algorithms (which are most often used) usually find only local minima

### Usage in NugeShare

Using a decision tree model, we can have our chatbot ask explicit questions about a user’s symptoms. Then using responses, it can follow the decision tree until it has one or several probable causes of the symptoms. Our primary task will be in collecting a high quality dataset for the model, preparing it correctly and finally, ensuring the model generalises well to unseen examples.