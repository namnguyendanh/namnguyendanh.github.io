---
title: "Linear Regression"
categories: 
    - MachineLearning
tags:
    - ML
---
## Lý thuyết
**Linear Regression** là một thuật toán thuộc *Supervised learning*.

### Mô tả bài toán
$$y \approx f(x) = \hat{y}$$
$$f(x) = w_1 x_1 + \ldots + w_n x_n + w_0$$

Công việc là đi tìm các hệ số tối ưu {$w_1$, $w_2$, $w_3$, $\ldots$, $w_0$} sao cho $\hat{y}$ gần với $y$(outcome thực) nhất.

### Hàm mất mát
$$
\begin{eqnarray}
L(\mathbf{w}) & = & \frac{1}{2} \sum_{i=1}^N (y_1 - \mathbf{\bar{x}_i w})^2\\
& = & \frac{1}{2}||\mathbf{y} - \mathbf{\bar{X} w}||^2_2 
\end{eqnarray}
$$

### Điểm tối ưu
Giá trị của $\mathbf{w}$ làm cho hàm mất mát đạt giá trị nhỏ nhất được gọi là *điểm tối ưu*(optimal point):
$$\mathbf{w^*} = arg \min_w L(\mathbf{w})$$

### Nghiệm bài toán
Cách phổ biến nhất để tìm nghiệm cho một bài toán tối ưu là **giải phương trình (gradient) = 0**

Đạo hàm theo $\mathbf{w}$ của hàm mất mát là:
$$\frac{\partial L(\mathbf{w})}{\partial \mathbf{w}} = \mathbf{\bar{X}^T} (\mathbf{\bar{X}^T w} - \mathbf{y})$$

Giải đạo hàm = 0 ta được nghiệm:
$$\mathbf{w} = (\mathbf{\bar{X}^T \bar{X}})^\dagger \mathbf{\bar{X}^T y}$$

## Ứng dụng trên Python
Ví dự đơn giản sử dụng đầu vào chỉ có 1 chiều.

Mô hình có dạng:
**(cân nặng) = w_1 * (chiều cao) + w_0**

### Hiển thị dữ liệu


```python
import numpy as np 
import matplotlib.pyplot as plt

# height (cm)
X = np.array([[147, 150, 153, 158, 163, 165, 168, 170, 173, 175, 178, 180, 183]]).T
# weight (kg)
y = np.array([[ 49, 50, 51,  54, 58, 59, 60, 62, 63, 64, 66, 67, 68]]).T
# Visualize data 
plt.plot(X, y, 'ro')
plt.axis([140, 190, 45, 75])
plt.xlabel('Height (cm)')
plt.ylabel('Weight (kg)')
plt.show()
```


![png][output_2_0.png]


### Nghiệm theo công thức


```python
# Building Xbar 
one = np.ones((X.shape[0], 1))
Xbar = np.concatenate((one, X), axis = 1)

# Calculating weights of the fitting line 
A = np.dot(Xbar.T, Xbar)
b = np.dot(Xbar.T, y)
w = np.dot(np.linalg.pinv(A), b)
print('w = ', w)
# Preparing the fitting line 
w_0 = w[0][0]
w_1 = w[1][0]
x0 = np.linspace(145, 185, 2)
y0 = w_0 + w_1*x0

# Drawing the fitting line 
plt.plot(X.T, y.T, 'ro')     # data 
plt.plot(x0, y0)               # the fitting line
plt.axis([140, 190, 45, 75])
plt.xlabel('Height (cm)')
plt.ylabel('Weight (kg)')
plt.show()
```

    w =  [[-33.73541021]
     [  0.55920496]]



![png][output_4_1.png]


### Nghiệm theo thư viện scikit-learn


```python
from sklearn import datasets, linear_model

# fit the model by Linear Regression
regr = linear_model.LinearRegression(fit_intercept=False) # fit_intercept = False for calculating the bias
regr.fit(Xbar, y)

# Compare two results
print( 'Solution found by scikit-learn  : ', regr.coef_ )
print( 'Solution found by (5): ', w.T)
```

    Solution found by scikit-learn  :  [[-33.73541021   0.55920496]]
    Solution found by (5):  [[-33.73541021   0.55920496]]


## Thảo luận

### Hạn chế của Linear Regression
1. **Nhạy cảm với nhiễu**(sensitive to noise). Vậy nên các nhiễu (outlier) cần phải loại bỏ trong bước tiền xử lý (pre-processing).

2. **Không biểu diễn được những mô hình phức tạp**

### Về bài viết này
Bài viết này được viết dựa trên bài [Linear Regression][fundaml_link] của anh Vũ Hữu Tiệp nhằm tóm gọn lại những ý chính cần thiết dưới góc độ quan sát của cá nhân tôi.


[fundaml_link]: https://machinelearningcoban.com/2016/12/28/linearregression/
[output_2_0.png]: /images/2018-06-05-linear-regression/output_2_0.png
[output_4_1.png]: /images/2018-06-05-linear-regression/output_4_1.png
