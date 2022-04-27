---
title:  "Machine Learning"
categories: 
    - machinelearning
header:
    image: /images/2020-02-25-machine-learning/header.png
---

Machine Learning là lĩnh vực nghiên cứu về các thuật toán, model nhằm giải quyết một vấn đề nào đó mà không cần phải lập trình nó explicit.

ML giải quyết các vấn đề

## Table of Contents
- Online learning system: 
- Out-of-core learning
- [Linear Regression](https://minhdq99hp.github.io/machinelearning/2020/02/25/linear-regression/)
- [Logistic Regression](https://minhdq99hp.github.io/machinelearning/2020/02/25/logistic-regression/)
- [Support Vector Machine](https://minhdq99hp.github.io/machinelearning/2020/03/02/support-vector-machine/)



## Mean Square Error (MSE)
$$MSE = \frac{1}{n}\sum^n_{i=1}(Y_i-\hat {Y_i})^2$$

The MSE is the mean ($\frac{1}{n}\sum^n_{i=1}$) of the squares of the errors $(Y_i-\hat {Y_i})^2$.

## Ý tưởng trích lọc từ cuốn Rules of Machine Learning: Best Practices for ML Engineering

To make great products:
**"do machine learning like the great engineer you are, not like the great machine learning expert you aren’t."**

Hầu hết vấn đề bạn sẽ đối diện là về kỹ thuật. Những cải thiện lớn sẽ đến từ những features tốt, chứ không phải thuật toán khủng.

Thế nên cách tiếp cận tốt nhất là:
1. Đảm bảo pipeline giữ vững từ đầu đến cuối
2. Bắt đầu với một mục tiêu hợp lý, khả thi
3. Thêm những feature đơn giản một cách đơn giản
4. Đảm bảo pipeline vẫn được giữ vững

## Before ML
### Rule #1: Có thể thử nghiệm những thuật toán khác trước khi động đến ML
ML thì hay đấy, cơ mà nó cần nhiều data. Hãy tìm các heuristic để giải quyết vấn đề của bạn. Khi nào có data hẵng đưa ML vào trong sản phẩm.

### Rule #2: Thiết kế và áp dụng các metrics
Trước khi sử dụng ML, thì hãy track những gì có thể để so sánh với hệ thống cũ.

### Rule #3: Chọn ML thay vì heuristic phức tạp
A complex heuristic is unmaintainable. Khi bạn đã có đủ data thì hãy chuyển sang ML.


## ML Phase 1: Your First Pipeline
### Rule #4: Giữ model đầu tiên đơn giản và ổn định

Đảm bảo rằng:
1. Đầu vào của model đúng
2. Model fit.
3. Đầu vào server đúng.

### Rule #5: Test the infrastructure independently from the ML
ML có yếu tố bất lường, thế nên đảm bảo là hệ thống ML có thể test được.

## References:
- [Dive Into Deep Learning](d2l.ai)
- [Best ML Books in 2020](https://blog.floydhub.com/best-machine-learning-books/?fbclid=IwAR2vThdUCo-IeilT3tqj_0AEwULNyzE9S2uLFBmYg16sQivtdWLnnV7kPmI)