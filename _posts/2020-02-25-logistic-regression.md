---
title:  "Logistic Regression"
categories: 
    - machinelearning
header:
    image: /images/2020-02-25-logistic-regression/header.png
---

## Introduction

**Logistic Regression** là một **parametric classification model**, dù rằng nó có cái tên "regression".

Ý tưởng đằng sau Logistic Regression khá giống Linear Regression. Nhưng thay vì fit một đường thẳng, thì ta sẽ fit một "S"-shaped curve, gọi là **Sigmoid**.

$$S(x) = \frac{1}{1+e^{-x}} = \frac{e^x}{e^x-1}$$

Vì sigmoid có range $[0, 1]$, vậy nên đầu ra của mô hình sẽ cho ra xác suất của input thuộc một trong hai classes.

## Training
Có một số cách để train:
- Iterative optimization algorithm: **Gradient Descent**
- Probabilistic method: **Maximum likelihood**

