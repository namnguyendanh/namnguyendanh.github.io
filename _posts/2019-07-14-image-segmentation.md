---
title:  "Image Segmentation"
---
# Image Segmentation

![instance_segmentation_example](/images/2019-07-14-image-segmentation/instance_segmentation_example.jpg)

## Tại sao cần Images Segmentation
Sử dụng trong y học, hình dáng của các tế bào ung thư đóng vai trò quan trọng trong việc xác định mức độ nguy hiểm của ung thư.

Ngoài ra, còn được ứng dụng trong:
- Traffic Control Systems
- Self Driving Cars
- Locating objects in satellite images

## Phân loại 
![Semantic vs Instance](/images/2019-07-14-image-segmentation/semantic-vs-instance.png)

## Region-based Segmentation
Một cách đơn giản để phân đoạn ảnh đó là lợi dụng các giá trị của pixel, những vật thể khác nhau có độ tương phản tốt với nhau.
-> **Threshold Segmentation**

- **Global threshold**: chia ảnh là 2 vùng: object và background
- **Local threshold**: chia ảnh theo nhiều vùng khi có nhiều object.

## Edge Detection Segmentation
Sử dụng convolution filter để detect cạnh.

Sobel filter (horizontal)

$$
\begin{bmatrix}
1 & 2 & 1 \\
0 & 0 & 0 \\
-1& -2& -1\\
\end{bmatrix}
$$

Sobel filter (vertical)
$$
\begin{bmatrix}
-1 & 0 & 1 \\
-2 & 0 & 2 \\
-1 & 0 & 1 \\
\end{bmatrix}
$$

Laplace operator (both horizontal and vertical)
$$
\begin{bmatrix}
1 & 1 & 1 \\
1 & -8 & 1 \\
1 & 1 & 1\\
\end{bmatrix}
$$

## Image Segmentation based on Clustering
Sử dụng thuật toán clustering để phân đoạn các điểm ảnh, trong đó có *k-means*.

## Mask R-CNN
Mask R-CNN là một mạng mở rộng của Faster R-CNN 

![Mask R-CNN](/images/2019-07-14-image-segmentation/mask-r-cnn.png)

> "Mask R-CNN is the current SOTA for image segmentation and runs at 5 fps."

|Algortihm|Description|Advantages|Limitations|
|---|---|---|---|
|Region-Based Segmentation|Separates the objects into different regions based on some threshold value(s).| a. Simple calculations <br/>b. Fast operation speed <br/>c. When the object and background have high contrast, this method performs really well|When there is no significant grayscale difference or an overlap of the grayscale pixel values, it becomes very difficult to get accurate segments.|
|Edge Detection Segmentation|Makes use of discontinuous local features of an image to detect edges and hence define a boundary of the object.|It is good for images having better contrast between objects.|Not suitable when there are too many edges in the image and if there is less contrast between objects.|
|Segmentation based on Clustering|Divides the pixels of the image into homogeneous clusters.|Works really well on small datasets and generates excellent clusters.|a. Computation time is too large and expensive. <br/>b. k-means is a distance-based algorithm. It is not suitable for clustering non-convex clusters.|
|Mask R-CNN|Gives three outputs for each object in the image: its class, bounding box coordinates, and object mask|a. Simple, flexible and general approach <br/>b. It is also the current state-of-the-art for image segmentation|High training time|

## Tham khảo
- https://www.analyticsvidhya.com/blog/2019/04/introduction-image-segmentation-techniques-python/