---
title:  "Convolutional Neural Networks"
categories: 
    - programming
header:
    image: /images/2019-06-07-convolutional-neural-networks/header.jpg
---

## Table of Contents (unavailable)


## How to count the number of parameters to be trained ?

## Advanced Topics in Deep Convolutional Neural Networks

### Old techniques
Researchers built multiple computer vision techniques to deal with these issues: SIFT, FAST, SURF, BRIEF, etc. 
-> The detectors were either too general or too over-engineered.
-> Need Representation Learning (a technique that allows a system to automatically find relevant features for a given task).

Multilayer perceptron (MLP):
- Not suitable for image processing (too many weights, not translation invariant)

### CNNs
CNN được lấy cảm hứng từ cách thị giác hoạt động ở những loài động vật có vú (**Mammalian Visual Cortex**). Chúng nhìn nhận bằng các lớp neuron xếp trồng nên nhau. Từng khối như thế xử lý một mức feature, càng sâu thì càng được trừu tượng hóa. -> **A hierarchy of feautures** 

Kiến trúc của Deep CNN lấy cảm hứng từ các ý tưởng:
- Local Connections
- Layering
- Spatial invariance

CNN có thể chia làm 2 phần:
- Feature Learning (phần đầu)
- Classification (phần cuối)

Với phần Features Learning: nó bao gồm các **convolution block** nối tiếp nhau. Các block này được tạo bởi 3 phép tính Convolution, ReLU và Pooling

Với phần Classification: ma trận features sẽ được **flatten** thành vector và ném vào một lớp **fully connected**, và cuối cùng là thông qua một lớp **softmax** để có được xác suất cho classification. 

The general idea of CNN’s is to intelligently adapt to the properties of images:
- Pixel position and neighborhood have semantic meanings
- Elements of interest can appear anywhere in the image

#### Convolution layers
Convolutional layers are the layers where filters are applied to the original image, or to other feature maps in a deep CNN. This is where most of the user-specified parameters are in the network. The most important parameters are the number of kernels and the size of the kernels.

#### Padding
Padding makes the feature maps produced by the filter kernels the same size as the original image.

#### Pooling layers
Pooling layers are similar to convolutional layers, but they perform a specific function such as max pooling, which takes the maximum value in a certain filter region, or average pooling, which takes the average value in a filter region. These are typically used to reduce the dimensionality of the network.

#### Fully connected layers
Fully connected layers are placed before the classification output of a CNN and are used to flatten the results before classification. This is similar to the output layer of an MLP.


#### Receptive Field and Dilated Convolutions
The receptive field is defined as the region in the input space that a particular CNN’s feature is looking at

This dilated convolution works in a similar way to a normal convolution, the major difference being that the receptive field no longer consists of contiguous pixels, but of individual pixels separated by other pixels. The way in which a dilated convolutional layer is applied to an image is shown in the figure below.

![](/images/2019-06-07-convolutional-neural-networks/dilated_convolution.gif)
The motivation behind using dilated convolutions are:
- The detection of fine details by processing inputs in higher resolutions.
- A broader view of the input to capture more contextual information.
- Faster run-time with fewer parameters

#### Saliency Maps
Saliency maps are a useful technique that data scientists can use to examine convolutional networks. They can be used to study the activation patterns of neurons to see **which particular sections of an image are important for a particular feature.**

#### Transposed Convolution
Upscaling: make the input tensor larger.

### Classic Networks

#### AlexNet
- One of the most important architectures in deep learning. AlexNet destroyed the competition in the 2012 ImageNet Large Scale Visual Recognition Challenge (ILSVRC).

#### VGG16 & VGG19
- ImageNet Challenge 2014, 16 or 19 layers
- 138 million parameters (522MB)
- Convolutional layers use ‘same’ padding and stride s = 1.
- Max-pooling layers use a filter size f = 2 and stride s = 2.

#### GoogLeNet (Inception-v1)
- Introduce the concept of the inception module -parallel convolutional layers with different filter sizes.


### Residual Networks (ResNet)
- Introduce residual block. The network allows for the development of extremely deep neural networks, which contain 100 layers or more.
- Allow the network to become deeper without increasing the training complexity.

#### Dense Network (DenseNet)


#### Transfer Learning
Idea of transfer learning: to use pre-trained models, models with known weights, in order to apply them to a different machine learning problem. 

You must still train the network on your new data, but it is common to freeze the weights of the former layers as these are more generalized feautres that will likely be unchanged during training. You can think of this as an intelligent way of generating a pre-initialized network, as opposed to having a randomly initialized network.

Typically, smaller learning rates are used in transfer learning than in typical network training, as we are essentially tuning the network.

Transfer learning works best for problems that are fairly general and there are networks freely available online (such as image analysis) and when the user has a relatively small dataset available such that it is insufficient to train a neural network — this is a fairly common problem.

#### What do CNN layers learn?

Each CNN layer learns filters of increasing complexity.
The first layers learn basic feature detection filters: edges, corners, etc
The middle layers learn filters that detect parts of objects. For faces, they might learn to respond to eyes, noses, etc
The last layers have higher representations: they learn to recognize full objects, in different shapes and positions.

Reference: [https://towardsdatascience.com/advanced-topics-in-deep-convolutional-neural-networks-71ef1190522d](https://towardsdatascience.com/advanced-topics-in-deep-convolutional-neural-networks-71ef1190522d)