---
title:  "Linear Regression"
categories: 
    - machinelearning
header:
    image: /images/2020-02-25-linear-regression/header.png
---

## Introduction
**Linear Regression** là một **linear model** xử lý vấn đề về regression. Trong ML, nó là một thuật toán *supervised learning*.

$$y = LR(x)$$

Cụ thể hơn, $y$ có thể tính toán bằng tổ hợp tuyến tính của $x$. Với x đa chiều, mô hình được gọi là **multiple linear regression**. Trong không gian đa chiều, mô hình sẽ fit một hyperplane.

## Training
### Least Squares Error (LSE)
$$J(w) = \sum^n_{i=1}(y(x^i)-y^i_{true})^2$$

LSE is a method that builds a model and MSE is a metric that evaluate your model's performances.

[Proof of convextity of LSE](https://math.stackexchange.com/questions/483339/proof-of-convexity-of-linear-least-squares)

Sử dụng **Gradient Descent** để tối ưu.