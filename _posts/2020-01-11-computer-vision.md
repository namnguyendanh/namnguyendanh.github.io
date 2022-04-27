---
title:  "Computer Vision"
categories: 
    - programming 
header:
    image: /images/2020-01-11-computer-vision/header.png
---


## Topics
- [Biến đổi hình học - geometric transformation](https://www.vietcs.org/bien-doi-hinh-hoc-geometric-transformation/?fbclid=IwAR1YTwYU5HTd1LIHbKFP6KtU7Tf7Y3zb1FKJ2rbCsSr70PLOCOCXw1sZF3Y): phép biến đổi hình học là các phép ánh xạ tọa độ điểm hay vector thành tọa độ hay vector khác.


## Deblur image
Đây là một problem khá cần thiết, dùng để cải thiện độ chính xác cho các mô hình nhận diện vật thể.

Các phương pháp được đề xuất:
- Autoencoder
- [DeblurGAN](https://github.com/dongheehand/DeblurGAN-tf/blob/master/readme.md?fbclid=IwAR0OuFDywEhzigECt4-yIiuzt94FbAQRtkH6tT90_36ITDtRK1Hu46iFtZk)
- [Deep Image Prior](https://dmitryulyanov.github.io/deep_image_prior?fbclid=IwAR1d1W3nKoXZyaTzYjg62sPYbX_U6YLaL996FdKdMK9q-ELDYhQ0-uSG_d8): Tái tạo ảnh
- Tăng shutter speed (đối với motion blur)


Tham khảo:
- https://www.facebook.com/groups/machinelearningcoban/permalink/898999907224084/

## Tools:
- [labelImg](https://github.com/tzutalin/labelImg?fbclid=IwAR3byWTHJ9CL8U2EFc363qY_gcNZZ8zebpyTw5b5ooDqm86v3rTkhoOJGy8): use for labeling images. `pip3 install labelImg`
- [Image manipulation tools](https://towardsdatascience.com/image-manipulation-tools-for-python-6eb0908ed61f)
