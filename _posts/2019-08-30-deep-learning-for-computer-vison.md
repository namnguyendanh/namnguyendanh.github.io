---
title:  "DL4CV Note"
categories: 
    - AI
---

Hôm qua, mình mới tải được bộ sách DL4CV của Adrian Rosebrook do một giảng viên chia sẻ lại. Mình đã mong được đọc bộ này từ lâu rồi, thật may mắn ghê :D

Đây là bài viết note lại những kiến thức học thêm được từ cuốn sách.

# Starter Bundle

## 1. Introduction
> "The secret of getting ahead is to get started" - Mark Twain

> "We need to be practitioners in our respective fields as well".

Cuốn này dạy sử dụng Deep Learning trong lĩnh vực Computer Vision theo kiểu ứng dụng. 
Sử dụng 2 thư viện: keras + mxnet, và tất nhiên là code bằng ngôn ngữ Python. 

**Questions**:
- Tại sao lại dạy kiểu ứng dụng ?. 
    - Vì hiểu lý thuyết với cài đặt trong thực tế là hai quá trình khác nhau. Biết 1 trong 2 là chưa đủ.
- Tại sao không học Tensorflow ?
    - Vì Keras (và mxnet) là một thư viện dành riêng cho Deep Learning. Còn Tensorflow dành riêng cho computational graph. Giống kiểu tại sao code linear regression bằng sklearn mà không bằng numpy ấy. 
- Nền tảng ban đầu ?
    - Để đọc được cuốn này (Starter Bundle) thì chưa cần hiểu trước về Deep Learning hay OpenCV vẫn có thể bắt đầu được.
- Tác giả cuốn này là ai ?
    - Là tác giả của blog nổi tiếng về ứng dụng Computer Vision và xử lý ảnh: https://www.pyimagesearch.com/.

## 2. What is Deep Learning ?
Do mình đã hiểu cơ bản về DL rồi nên bỏ qua phần này. Giới thiệu về DL thì mình recommend là đọc chương đầu tiên trong cuốn Deep Learning Book. 

## 3. Image Fundamentals
**Questions**:
- Tại sao khi load ảnh với OpenCV thì lại sử dụng hệ màu BGR thay vì RGB ?

- Hệ màu RGB
- Tọa độ ảnh: O nằm ở góc trên cùng bên trái, Ox hướng bên phải, Oy hướng xuống dưới.
- Image as Numpy array sẽ có shape: height, width, channels (or depths). Do matrix thường quy ước shape là rows * columns nên height đứng trước width.
- OpenCV sử dụng BGR thay vì RGB (yếu tố lịch sử)

## 4. Image Classification
- Định nghĩa về Image Classification, Dataset, Feature extraction.
- Semantic gap
- Challenges: viewpoint variation, scale variation, deformation, occlusions, illumination, background clutter, intra-class variation, 
- Types of Learning: 
    - Supervised Learning
    - Unsupervised Learning
    - Semi-supervised Learning
- DL Classification Pipeline: 
    1. Gather your dataset
    2. Split your dataset into training and testing set, Ex: 1/3, 1/4, 1/10. And set aside 10-20% of training set for validation.
    3. Train your network
    4. Evaluate
- CNN is *end-to-end* models, so we don't need features extraction (hand-crafted).

## 5. Datasets for Image Classification

### 5.1. MNIST
10 classes: digits

### 5.2. Animals: Dogs, Cats, and Pandas
3 classes: dog, cat, panda

### 5.3. CIFAR-10
10 classes (but harder than MNIST)

### 5.4. SMILES
2 classes: smiling or not smiling

### 5.5. Kaggle: Dogs vs Cats

### 5.6. Flowers-17
17 classes

### 5.7. CALTECH-101
A popular benchmark dataset for object detection.

### 5.8. Tiny ImageNet 200
Used in CS231n

### 5.9. Adience
Used for age and gender recognition.

### 5.10. ImageNet
Nearly 22000 categories (synset)

#### ImageNet Large Scale Visual Recognition Challenge (ILSVRC)
1000 catogories, 1.2 million images.

### 5.11. Kaggle: Facial Expression Recognition Challenge
7 emotions

### 5.12. Indoor CVPR

### 5.13. Standford Cars
196 classes of cars, 16.195 images.
class imbalances.

### 5.14. LISA Traffic Signs

### 5.15. Front/Rear View Vehicle

## 6. Configuring Your Development Environment
Technologies change so fast, so check up this [link](http://dl4cv.pyimagesearch.com)
- Python, Keras, Mxnet
- OpenCV, scikit-image, scikit-learn,...

## 7. Your First Image Classifier
Có một bộ dữ liệu gồm 3000 ảnh chia đều cho 3 class: dog, cat, panda (trích từ Kaggle và ImageNet)

Mình thấy cách xây dựng quy trình project trong sách rất đáng để học hỏi. Mình đã ghi lại trong repo [minhdq99hp/sample-code](www.github.io/minhdq99hp/sample-code). Có thể mình sẽ viết riêng về phần này trong một post khác.

### k-NN: A simple classifier
Thuật toán k-NN mình đã biết nên bỏ qua. Bạn có thể đọc thêm tại trang machinelearningcoban. k-NN classifier là một *lazy learner*, tức là ko học gì hết, training set sẽ dùng trong quá trình predict luôn.

Trong sách sử dụng `KNeighborsClassifier` từ thư viện `sklearn`. Thường thì chọn k là số lẻ để tránh việc số lượng vote ngang nhau. Và distance là L2, có thể thay bằng L1 (Manhattan/city-block distance)

Điểm mạnh của k-NN:
- Đơn giản, dễ hiểu, dễ cài đặt

Điểm yếu của k-NN:
- Tốn thời gian trong công đoạn phân loại, do phải so sánh khoảng cách với toàn bộ data points, độ phức tạp là O(N), ko phù hợp với dataset lớn.

Có một cách khác là dùng Approximate Nearest Neighbor (ANN), FLANN, Annoy để khắc phục về mặt thời gian, tuy nhiên sẽ bị giảm độ chính xác.

## 8. Parameterized Learning 
> “A learning model that summarizes data with a set of parameters of fixed size
(independent of the number of training examples) is called a parametric model. No
matter how much data you throw at the parametric model, it won’t change its mind
about how many parameters it needs.” – Russell and Norvig (2009)

### Linear Classifier
Scoring Function: $s = f(x_i, \mathbf W, \mathbf b)$

Hinge Loss Function: $L_i = \sum_{j \neq y_i} max(0, s_j-s_{y_i} + 1)$

hoặc Squared Hinge Loss Function.

### Cross-entropy Loss and Softmax Classifiers
> "Softmax classifiers give you probabilities for each class label while hinge loss gives you the
margin."

// TODO: tìm hiểu thêm về sự khác biệt giữa Cross-entropy loss và hinge loss.

Đến bước này thì mọi thứ trở lên quen thuộc hơn nhiều. 


# Practitioner Bundle
## 1. Introduction
## 2. Data Augmentation
### 2.1 What is Data Augmentation ?
Data Augmentation (tăng cường dữ liệu) là một phương pháp regularization.

Data augmentation kết hợp nhiều kỹ thuật (thêm nhiễu) để có thể sinh ra training samples mới từ bộ training samples gốc.

Mục tiêu của Data Augmentation là tăng tính generalizability của model. -> learn more robust features. 

Trong computer vision, data augmentation khá dễ để hình dung. Chúng ta có thể tạo ra dữ liệu mới chỉ bằng những phép biến đổi hình học cơ bản:
- Translations
- Rotations
- Changes in scale
- Shearing
- Horizontal flips

### 2.2 Visualizing Data Augmentation
