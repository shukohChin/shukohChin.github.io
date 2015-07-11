---
layout: post
title: Machine Learning
category: DataScience
tags: [machine_learning, coursera]
---

### 目录
- [1.Lenear Regression]（#1.Lenear Regression）
- [2.Logistic Regression]（#2.Logistic Regression）

# 1.Lenear Regression
### Machine learning定義
* Field of study that gives computers the ability to learn without being explicitly programmed.
* A computer program is said to _learn_ from experience E with respect to some task T and some performance measure P, if its performance on T, as measured by P, improves with experience E.

### Machine learning种类
* Supervised Learning - "right answers" given  
	+ regression problem(continuous)  
		例：housing price prediction
	+ classification problem(discrete)  
		例：mail spam
* Unsupervised Learning  
		例1：group google news  
		例2：discover market segments and group customers into different market segments

### Linear regression with one variable
* 基本数学表达式  
	Hypothesis:  
		$$
		h_{\theta}(x) = \theta_{0} + \theta_{1}x 
		$$
	Parameter:
		$$
		\theta_{0}, \theta_{1}
		$$
	Cost Function:
		$$
		J(\theta_{0}, \theta_{1})=\frac{1}{2m}\sum_{i=1}^m(h_{\theta}(x^{(i)})-y^{(i)})^2
		$$
	Goal:
		$$
		minimize_{\theta_{0},\theta_{1}} J(\theta_{0},\theta_{1})
		$$

### Gradient descent algorithm 梯度下降算法
* repeat until convergence {
		$$
		\theta_{j} := \theta_{j} - \alpha\frac{\partial}{\partial\theta_{j}}J(\theta_{0}, \theta_{1})
		$$
} (simultaneously update j = 0 and j = 1)  
	- gradient descent will automatically take smaller steps

### Gradient descent for linear regression
* Gradient descent algorithm
	- repeat until convergence {
		$$
		\theta_{0} := \theta_{0} - \alpha\frac{1}{m}\sum_{i=1}^m(h_{\theta}(x^{(i)})-y^{(i)}))
		$$
		$$
		\theta_{1} := \theta_{1} - \alpha\frac{1}{m}\sum_{i=1}^m(h_{\theta}(x^{(i)})-y^{(i)}))x^{(i)}
		$$
}
* The cost function $$J(\theta_{0}, \theta_{1})$$ for linear regression has __no local optima__(other than the globle minimun), so gradient descent will __not__ get stuck at a bad local minimun
* "Batch": Each step of gradient descent uses all the training examples

#### Multiple features
* Notation:
	+ n = number of features
	+ $x^{(i)}$ = input (features) of $i^{th}$ training example
	+ $x_{j}^{(i)}$ = value of feature $j$ in $i^{th}$ training example 
	+ $h_{\theta}(x) = \theta_{0} + \theta_{1}x_{1} + \theta_{2}x_{2} + ... + \theta_{n}x_{n}$ => $\theta^Tx$

### Gradient Descent for Multiple Variables 多维梯度下降算法
* 基本数学式
	Hypothesis:
	$$
	h_{\theta}(x) = \theta^Tx = \theta_{0}x_{}0 + \theta_{1}x_{1} + ... + \theta_{n}x_{n} 
	$$
	Parameters: __($\theta$: n+1 dimensioned vector)__
	$$
	\theta_{0},\theta_{1},...,\theta_{n} 
	$$
	Cost function:
	$$
	J(\theta_{0}, \theta_{1},...\theta_{n})=\frac{1}{2m}\sum_{i=1}^m(h_{\theta}(x^{(i)})-y^{(i)})^2
	$$
* Gradient descent:
	- Repeat {
	$$
		\theta_{j} := \theta_{j} - \alpha\frac{\partial}{\partial\theta_{j}}J(\theta_{0},...,\theta_{n})
	$$	}(simultaneously update every j = 0,...,n)
* New algorithm
	- Repeat {
	$$\theta_{0} := \theta_{0} - \alpha\frac{1}{m}\sum_{i=1}^m(h_{\theta}(x^{(i)})-y^{(i)}))x_{0}^{(i)}$$
	$$\theta_{1} := \theta_{1} - \alpha\frac{1}{m}\sum_{i=1}^m(h_{\theta}(x^{(i)})-y^{(i)}))x_{1}^{(i)}$$
	$$\theta_{2} := \theta_{2} - \alpha\frac{1}{m}\sum_{i=1}^m(h_{\theta}(x^{(i)})-y^{(i)}))x_{2}^{(i)}$$
	}  

### Feature Scalling



# 2.Logistic Regression
### Classfication
* 基本概念
	Email: Spam/Not Spam
	Tumor: Malignant/Benign  
	$y\in${0, 1}  
	- 0: "Negative Class"(benign tumor)
	- 1: "Positive Class"(malignant tumor)
* Logistic Regression - Sigmoid function
	$$h_{\theta}(x) = g(\theta^Tx)$$
	$$g(z) = \frac{1}{1+e^{-z}}$$
	- Suppose predict "y=1" if $h_\theta(x) >= 0.5$
	$$\theta^Tx >= 0$$
	- predict "y=0" if $h_\theta(x) < 0.5$
	$$\theta^Tx < 0$$
* Logistic regression cost function  
	$$Cost(h_\theta(x), y) = \cases{
							  -log(h_\theta(x)) & \text{if }y = 1\cr
							  -log(1-h_\theta(x)) & \text{if }y = 0} $$

	or  
	$$Cost(h_\theta(x), y) = -ylog(h_\theta(x)) - (1-y)log(1-h_\theta(x))$$
* Gradient Descent
	$$J(\theta) = -\frac{1}{m}[\sum_{i=1}^my^{(i)}logh_\theta(x^{(i)})+(1-y^{(i)})log(1-h_\theta(x^{(i)}))]$$
	
	Want $min_{\theta}J(\theta)$:
	Repeat {
	$$\theta_j := \theta_j - \alpha\sum_{i=1}^m(h_\theta(x^{(i)}) - y^{(i)})x_j^{(i)}$$
	}(simultaneously update all $\theta_j$)
* Optimization algorithms 优化
	- Gradient descent  
	(No need to manually pick \alpha, faster than gradient descent, but more complex):
	- Conjugate gradient
	- BFGS
	- L-BFGS