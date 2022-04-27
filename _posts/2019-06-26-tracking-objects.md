---
title:  "Tracking Objects on Video"
categories: 
    - technology
tags: 
    - computervision
header:
    image: /images/2019-06-26-tracking-objects/header.png
---

Object tracking là quá trình định vị một vật thể chuyển trong trong video (camera tĩnh). Real-time object tracking là một phần quan trọng trong nhiều ứng dụng Computer vision như là camera giám sát, nhận diện cử chỉ, AR, nén video và hỗ trợ tài xế lái xe. 

 

Tracking object có thể đạt được với nhiều cách, hầu hết phụ thuộc vào yêu cầu bài toán đặt ra. 

 

## Nhận diện vật thể chuyển động  

 

Để mà track được bất kỳ thứ gì trong video thì đầu tiên chúng ta phải xác định được vùng chứa những vật thể chuyển động. Có nhiều cách để track vật thể trong video. Ví dụ: vật thể di chuyển vị trí -> so sánh sự khác nhau giữa các frame, track chuyển động của tay -> meanshift based on the màu da, những vật thể biết trước -> sử dụng các thủ thuật như template-matching. 

 

## Nhận diện chuyển động (cách cơ bản nhất) (Frame difference method) 

 

Cách đơn giản và dễ hiểu nhát là tính toán độ khác nhau giữa các frames hay là giữa "background" frame và các frame khác. Trước khi tính toán độ khác nhau giữa các frame thì chuyển ảnh về gray scale và dùng GaussianBlur để lọc nhiễu (nhiễu này là nhiễu tự nhiên xảy ra do rung động hay thay đổi ánh sáng,...). Ngoài ra, sử dụng thêm eroding và dilating (tham khảo cuối bài) cũng có tác dụng lọc nhiễu. Cuối cùng là sử dụng threshold để tách được foreground mask (việc chọn lựa ngưỡng threshold rất quan trọng). Việc chọn threshold value có thể được chọn thủ công hoặc tự động bằng thuật toán.  

 

Cách này khá chính xác nhưng có một điểm yếu lớn đó là phải có "background" frame ban đầu. Điều này khiến cách tiếp cận này không phù hợp với điều kiện môi trường thay đổi nhiều (ánh sáng). Chúng ta cần một cách tiếp cận khác tốt hơn. 

 

-> robust background subtraction algorithms có thể giải quyết được: 

- thay đổi ánh sáng 

- những chuyển động non-stationary (mưa, tuyết, lá cây,...) 

- thay đổi cảnh dài hạn  

 

 

## Background subtractors - KNN, MOG2 and GMG 

 

Có 3 background subtractors có trong OpenCV3: K-Nearest Neighbors (KNN), Mixture of Gaussians (MOG2), and Geometric Multigrid (GMG) tương đương với 3 thuật toán dùng để tính background subtraction. 

 

Mấy cái khác gì thuật toán GrabCut và Watershed (đã giới thiệu ở seminar trước) ? 

BackgroundSubtractor được xây dựng cho video analysis, nó sẽ "học" điều gì đấy thông qua mỗi frame. VD: với GMG, có thể specify 120 frames để dành cho video analysis, những frame này được lưu trữ lại để giúp cái thiện khả năng nhận dạng chuyển động tốt hơn qua thời gian. 

 

Một feature khác của BackgroundSubtractor đó là khả năng nhận diện được shadows. Từ đó chúng ta có thể loại bỏ shadows bằng threshold. 

 

Background modling gồm 2 bước chính: 

- Background Initialization 

- Background Update 

 

 

 

current frame 
ba ckground model 
THRESHOLD 
foreground mask 
 

 

Ngoài ra, còn có các hướng tiếp cận khác như Meanshift và CAMShift mình sẽ giới thiệu ở seminar sau. 

 

# Tham khảo: 

- Learning OpenCV3 Computer Vision with Python 

- https://docs.opencv.org/3.4/db/df6/tutorial_erosion_dilatation.html 

- https://docs.opencv.org/3.4.3/d1/dc5/tutorial_background_subtraction.html 

- Background subtraction 

![](https://trello-attachments.s3.amazonaws.com/5c0df06a03d0655cec61bb42/5c0df68ae0014e58bfba13c6/33439803552a8b84ddb35efdbaab14b5/GetImage_(3).png)