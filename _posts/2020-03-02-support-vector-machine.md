---
title:  "Support Vector Machine"
toc: true
categories:
    - Technology
tags:
    - machinelearning
header:
    image: /images/2020-03-02-support-vector-machine/header.png
---

## Giới thiệu
Support Vector Machine là thuật toán classification 

### Khoảng cách từ một điểm tới một siêu mặt phẳng
$$\frac{|\mathbf w^T\mathbf x_0 + b|}{\|\mathbf w\|_2}$$

Bài toán tối ưu trong SVM là bài toán đi tìm đường phân chia sao cho *margin* là lớn nhất. Đây cũng chính là lý do vì sao SVM còn được gọi là *Maximum Margin Classifier*.

Với cặp dữ liệu $(\mathbf x_n, y_n)$ bất kỳ, khoảng cách từ điểm đó tới mặt phân chia là:

$$\frac{y_n(\mathbf w^T\mathbf x_n + b)}{\|\mathbf w\|_2}$$

Như vậy, ta có margin:

$$\text{margin} = \text{min}_n\frac{y_n(\mathbf w^T\mathbf x_n+b)}{\|\mathbf w\|_2}$$

Tối ưu trong SVM chính là tìm $(\mathbf w, b)$ sao cho:

$$(\mathbf w, b) = \text{argmax}_{\mathbf w, b}\{\text{min}_n\frac{y_n(\mathbf w^T\mathbf x_n+b)}{\|\mathbf w\|_2}\}$$

Tiếp tục biến đổi để đưa về bài toán tối ưu lồi (đọc thêm ở phần tham khảo).
## References
- https://machinelearningcoban.com/2017/04/09/smv/