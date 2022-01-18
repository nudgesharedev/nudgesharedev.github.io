# 9. Machine Learning Research Part 2

## Introduction

In the last post we explained the basics of machine learning, as result of our research. With the foundations in place, we began to explore specific models which may be applicable to our project — inferring likely causes of symptoms.

## Linear Classifiers

Linear classifier models make the assumption that labels are predicted by a linear combination of features. Their general form is:

![Untitled](/assets/9%20Machine%20Learning%20Research%20Part%202%20497fbe7c89f34f968ed4cb1ad7026c47/Untitled.png)

Using a model with $n$ features and $m$ labels:

- $\vec{x}$ is an $n$-dimensional feature vector (transposed)
- $w$ is an $n$ x $m$ weight matrix
- $\vec{c}$ is an $m$-dimensional bias vector
- $f$ is an arbitrary function

## Perceptron

Perceptron is a simple linear model for binary classification. It can be used to decide whether something is a member of a class or not.

## $f(x)  =\begin{cases}1 & \text{if }\ x > 0,\\0 & \text{otherwise}\end{cases}$

The model will output a 1 (a yes answer) if and only if the linear combination of features is greater than zero.

## Logistic Regression

Logistic regression approximates the probability of an event.

## $f{\left(x\right)} = \sigma{\left( x\right)} = \left(1 + e^{-x}\right )^{-1}$

## Linear Regression

Linear regression approximates a simple linear combination using the identity function.

## $f(x) = x$

## Using Gradient Descent

Gradient descent is a general method for finding local minima of a function. It works by travelling in the opposite direction to the gradient at a given point. 

![Untitled](/assets/9%20Machine%20Learning%20Research%20Part%202%20497fbe7c89f34f968ed4cb1ad7026c47/Untitled%201.png)

Written as an iterative formula,

## $x_{n+1} =-\alpha  \cdot f'{\left(x_{n}\right)}$

The learning rate $\alpha \in (0, 1)$ controls the speed of movement. Small values offer high precision but lengthen execution time, large values do the opposite.

In supervised learning, we're trying to minimise a model's inaccuracy. If we can express this as a differentiable function, gradient descent can be used to find locally optimal parameters.

### Updating Parameters

Updating parameters with gradient descent is simple but the derivation does require a bit of calculus.

Start by expressing the problem in terms of gradient descent.

## $\theta_{n+1} =-\alpha  \cdot e'{\left(\theta_{n}\right)}$

You're trying to find which parameters $\theta$, minimise the model's error. Typically the mean-squared error function is used:

## $e(\theta) = \sum_{i=0}^k\frac{1}{2} {\left(\vec{y}_i - p_\theta{\left(\vec{x}_i\right)}\right)^2}$

You can omit the sum and subscripts for now

## $e{\left(\theta\right)}=\frac{1}{2}{\left(\vec{y} - p{\left(\vec{x}\right)}\right)^2}$

The model's predictor function can be defined as

## $p{\left(\vec{x}\right)} = f{\left(\vec{x} w\right)}$

Notice that the bias terms $\vec{c}$ have been included in $w$. 

## $\theta = w \implies e'{\left( \theta \right)} = e'{\left( w \right)}$

Now use partial differentiation

## $a=w\vec{x}$

# $\frac{\partial e}{\partial w} = \frac{\partial e}{\partial a} \cdot \frac{\partial a}{\partial w}$

# $\frac{\partial a}{\partial w} = \frac{\partial}{\partial w} \small{\vec{x}w}$

# $= \small{\vec{x}}$

# $\frac{\partial e}{\partial a} = \frac{\partial}{\partial a}{\frac{1}{2}{\left(\small{\vec{y}  - p{\left(\vec{x}\right)}}\right)}}^2$

# $\small{=} \left(\small{\vec{y} - p{\left(\vec{x}\right)}}\right) \cdot \frac{\partial}{\partial a} \left(\small{\vec{y} - p{\left(\vec{x}\right)}}\right)$

# $\small{=} \left(\small{\vec{y} - p{\left(\vec{x}\right)}}\right) \cdot \frac{\partial}{\partial a} \left(\small{\vec{y} - f{\left(a\right)}}\right)$

# $\small{=} \left(\small{\vec{y} - p{\left(\vec{x}\right)}}\right) \cdot \small{f'{\left(a\right)}}$

Combine the results

## $e'{\left(\theta\right)}= \left(\vec{y} - p{\left(\vec{x}\right)}\right) \cdot f'{\left(a\right)}\cdot \vec{x}$

Finally, reintroduce the sum

## $e'(\theta) = \sum_{i=0}^k \left(\vec{y}_i - p{\left(\vec{x}_i\right)}\right) \cdot f'{\left(a\right)}\cdot \vec{x}_i$

## Limitations

Many relationships are non-linear such as XOR. In these cases, linear models will fail to make accurate predictions.
