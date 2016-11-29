---
layout: page
title:  "Efficiently Calculate the Second Order Derivatives in Theano"
categories: Python Theano DeepLearning
tags: Python Theaon DeepLearning
date:   2016-11-29 14:35:00 +08:00
---

Sometimes, it's not trivial to calculate the second order derivatives with Theano, becuase Theano did not well support calculating the derivatives w.r.t. subtensor.

Suppose a scalar $y$ derives from a vector $x$. To obtain the first order derivate $\frac{\delta y}{\delta x_i}$, we must calculate the `T.grad(y,x)[i]` instead of `T.grad(y,x[i])`. This is because Theano does NOT treat $x[i]$ as the input of $y$.  

With above, to obtain $\frac{\delta^2 y}{\delta x_i^2}$ for each $i$, we need use  `T.grad(T.grad(y,x)[i],x)[i]` together with high cost `T. scan` function to enumerate $i$. Hence, evaluating such a computational graph costs a lot. The diagonal of Hessian matrix also gives $\frac{\delta^2 y}{\delta x_i^2}$. But evaluating Hessian matrix with `theano.gradient.Hessian` also introduces significant cost.     

One possible solution is exploiting diagonal approximation, i.e. ignoring all non-diagonal elements in Hessian matrix. In this case, the second-order derivate for all input element can be approximately calculated by

````python
  first_derivative = T.grad(y,x)
  # the cross term partial derivatives are all zero
  second_derivate = T.grad(T.sum(first_derivative),x)
````
