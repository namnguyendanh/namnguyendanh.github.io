---
title:  "Generative Deep Learning Note"
categories: 
    - AI
tags: 
    - AI 
    - GAN
---

Cuốn sách Generative Deep Learning mới ra cách đây được vài tháng. Có thể nói là công nghệ phát triển quá nhanh, cuốn sách tóm lược lại sự phát triển của mạng generative từ năm 2014 đến nay. 

Mình đã đăng ký ngay tài khoản O'Reilly để được đọc cuốn sách quý này. Dưới đây là những kiến thức mình tiếp nhận được.

Bài viết giới thiệu sách của chính tác giả: [đây](https://medium.com/applied-data-science/generative-deep-learning-the-parrot-has-landed-b291e6c254e)

Ngoài ra, có trang Applied Data Science trên Medium khá hay, mình nên ghé đọc sau này.

Giờ thì bắt đầu thôi !

> "Can we create something that is in itself creative" ?

Đây chính là câu hỏi mà generative modeling hướng đến trả lời.

## Chapter 1. Generative Modeling

> "A generative model describes how a dataset is generated, in terms of a probabilistic model. By sampling from this model, we are able to generate new data."

Giả sử ta có một bộ dữ liệu ảnh ngựa. Chúng ta muốn xây dựng một model có thể sinh ra ảnh ngựa (chưa từng tồn tại trước đó nhưng trông vẫn chân thực) bởi vì model đã học được general rules quyết định con ngựa trông ntn. Đây là một loại vấn đề có thể sử dụng generative modeling.

Data point được gọi là *observation*.

![](/images/2019-08-31-generative-deep-learning/1.png)

Mục tiêu là sinh ra data point chứa các features tương ứng với cách các features được tạo ra ở training set. 

> "A generative model must be *probabilistic* rather than *deterministic*"

Tức là ko thể dùng tính toán cố định như kiểu lấy avarage data points để sinh ra được, nó ko được coi là generative. Model cần có một yếu tố stochastic (random) ảnh hưởng đến sample được tạo ra.

### Generative vs Discriminative Modeling

![](/images/2019-08-31-generative-deep-learning/2.png)

Discriminative modeling cần data được gán nhãn 0-1 để phân biệt.

Vì vậy, D là *supervised learning*, còn G là *unsupervised learning*.

Theo cách hiểu toán học thì:
- D estimate $p(y|x)$ - the probability of a label y given observation x.
- G estimate $p(x)$ - the probability of observing observation x.

### Advances in Machine Learning
Về cả mặt học thuật và công nghiệp thì người ta chú trọng phát triển discriminative modeling.

### The Rise of Generative Modeling
Trong 3-5 năm trở lại đây thì những phát triển mới nhất đến từ các ứng dụng liên quan đến generative modeling.

![](/images/2019-08-31-generative-deep-learning/3.png)

Đoạn này khá dài, nói chung là để chỉ ra rằng nếu muốn tiến xa hơn nữa thì generative modeling là một hướng cực kỳ triển vọng.

### The Generative Modeling Framework

- We have a dataset of observations $\mathbf X$.
- We assume that the observations have been generated according to some unknown distribution, $p_{data}$
- A generative model $p_{model}$ tries to mimic $p_{data}$. If we achieve this goal, we can sample from $p_{model}$ to generate observations that appear to have been drawn from $p_{data}$
- We are impressed by $p_{model}$ if:
    - Rule 1: It can generate examples that appear to have been drawn from $p_{data}$
    - Rule 2: It can generate examples that are suitably different from the observations in $\mathbf X$. In other words, the model shouldn't simply reproduce things it has already seen.

### Probabilistic Generative Models
- **Sample Space**: the set of all values an observation $\mathbf x$ can take.

- **Probability Density Function**: the function that maps a point $x$ in the sample space to a number between 0 and 1.

- **Parametric Modeling**: $p_\theta(x)$ is a family of density functions that can be described using a finite number of parameters, $\theta$.

- **Likelihood**: $L(\theta|x)$ of a parameter set $\theta$ is a function that measure the plausibility of $\theta$ given some observed point $\mathbf x$.

$$l(\theta|X) = \sum_{x \in X}log p_\theta(x)$$

It therefore makes intuitive sense that the focus of parametric modeling should be to find the optimal value θ^ of the parameter set that maximizes the likelihood of observing the dataset X. This technique is quite appropriately called maximum likelihood estimation.

//TODO: Tìm hiểu thêm về Naive Bayes parametric model. Tại sao nó work trong trường hợp các features là độc lập, còn ko work trong trường hợp ngược lại (vd: pixels)

Naive Bayes models không hoạt động hiệu quả đối với dữ liệu ảnh thô.

### The Challenges of Generative Modeling
- How does the model cope with the high degree of conditional dependence between features ?
- How does the model find on of the tiny proportion of satisfying possible generated observations among a high-dimensional sample space ?

Deep learning là chìa khóa để giải quyết cả 2 vấn đề trên.

#### Representation learning 
Ý tưởng chính đằng sau representation learning đó là mô tả mỗi observation sử dụng low-dimensional latent space rồi học một mapping function. Mỗi một điểm trong latent space là đại diện (representation) của high-dimensional image.

![](/images/2019-08-31-generative-deep-learning/4.png)

Bằng cách giảm số lượng features, chúng ta có thể tạo ra các representation mà khi map lại domain ban đầu sẽ cho kết quả tốt hơn là làm việc trực tiếp với từng pixels.

## Chapter 2. Deep Learning
### Strutured and Unstructured Data
Structured Data: can be put in a table
Unstructured Data: image, audio, text

> "Deep learning can be applied to structured data, but its real power, especially with regard to generative modeling, comes from its ability to work with unstructured data"

### Deep Neural Networks
