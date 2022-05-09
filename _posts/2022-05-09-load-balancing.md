---
title:  "Tổng quan về Load Balancing"
header:
    caption: Introduction to Load balancing
categories: 
    - Technology
tags:
    - system-design
    - infrastructure
---

## Khái niệm cơ bản
Load Balancing (tạm dịch Cân bằng tải), là một thành phần quan trọng của cơ sở hạ tầng thường được sử dụng để cải thiện hiệu suất và độ tin cậy của các trang web, các ứng dụng, cơ sở dữ liệu và các dịch vụ khác bằng cách phân phối khối lượng công việc trên nhiều máy chủ. Hiểu đơn giản là phân tán các request đến các tài nguyên tính toán ví dụ như các server và database. Ở mỗi trường hợp, load balancer trả về một response từ tài nguyên tính toán đến một client thích hợp.

![1.png](/images/2022-05-09-load-balancing/1.png)

<!-- <p align="center"><img src="images/2022-05-09-load-balancing/1.png"/></p> -->

Cơ bản thì Load Balancing giải quyết giúp chúng ta 2 vấn đề:

- Load Balancing giúp scale hệ thống: scale hệ thống theo chiều ngang (Horizontal scaling) dễ và hiệu quả. Thay vì phải sử dụng single server mạnh, đắt tiền thì chúng ta có thể sử dụng nhiều server nhỏ hơn với giá thành rẻ hơn mà vẫn đáp ứng traffic tương tự.

- Load Balancing giúp giải quyết lỗi:
    - Ngăn các request đến các server đang có vấn đề

    - Ngăn chặn một tài nguyên đang bị quá tải.

    - Giúp loại bỏ điểm chết (single points of failure)

Load balancing có thể cài đặt bằng hardware hoặc software.

## Những giao thức mà Load Balancing có thể xử lý

Load balancing thường hoạt động ở tầng 7 (Application) hoặc tầng 4 (Transport), nên nó quy định chuyển tiếp đối với bốn loại giao thức chính sau:
- HTTP - Chuẩn HTTP balancing chỉ đạo yêu cầu dựa trên cơ chế HTTP chuẩn. Load Balancer đặt X-Forwarded-For, X-Forwarded-Proto, và tiêu đề X-Forwarded-Port để cung cấp cho các thông tin backends về các yêu cầu ban đầu.

- HTTPS - HTTPS balancing với các chức năng tương tự như HTTP balancing, với sự bổ sung của mã hóa. Mã hóa được xử lý theo một trong hai cách: hoặc là với passthrough SSL duy trì mã hóa tất cả con đường đến backend hoặc chấm dứt SSL mà đặt gánh nặng giải mã vào load balancer nhưng gửi lưu lượng được mã hóa đến back end.

- TCP - Đối với các ứng dụng không sử dụng HTTP hoặc HTTPS, lưu lượng TCP cũng có thể được cân bằng. Ví dụ, lượng truy cập vào một cụm cơ sở dữ liệu có thể được lan truyền trên tất cả các máy chủ.

- UDP - Gần đây, một số load balancer đã thêm hỗ trợ cho cân bằng tải giao thức internet lõi như DNS và syslogd sử dụng UDP.

## Phân loại Load Balancing

- Server Load Balancing: Với Server Load Balancing, mục tiêu là phân chia khối lượng công việc ra nhiều máy chủ dựa theo năng lực và tính khả dụng của chúng. Server Load Balancing dựa vào các thông tin ở tầng Application để điều hướng truy cập. Server Load Balancing còn được biết đến như Layer 7 Load Balancing vì chúng sử dụng thông tin của tầng ứng dụng.

 - Layer 7 load balancer xem thông tin tại lớp [application layer] để phân tán request. Nó có thể bao gồm thông tin của header, message, và cookies. Layer 7 load balancer ngắt network traffic, đọc nội dung tin, đưa ra quyết định, sau đó mở một kết nối đến server được lựa chọn. Ví dụ, một layer 7 load balancer có thể chỉ đường video traffic tới các server chứa video trong khi chỉ đường cho các traffic nhạy cảm hơn tới những server có tính bảo mật cao (security-hardened server).
 
- Network Load Balancing: Network Load Balancing phân chia lưu lượng truy cập giữa các địa chỉ IP, switches, routers sử dụng thiết bị một cách hiệu quả và nâng cao tính ổn định. Các cấu hình này sẽ được thực hiện ở tầng Transport, do đó, Network Load Balancing còn được gọi với tên Layer 4 Load Balancing.
    - Layer 4 load balancer nhìn vào thông tin ở lớp transport để quyết định phân tán request. Thông thường, nó bao gồm các địa chỉ IP của nguồn đích và port ở phần header, nhưng không có nội dung của package. Layer 4 load balancer chuyển tiếp các gói tin đến và từ các upstream server, biểu diễn Network Address Translation (NAT).

    - Global Server Load Balancing (GSLB): Trong Global Server Load Balancing, một trung tâm điều hành sẽ xử lý việc cân bằng tải giữa khắp nơi trên toàn thế giới thông qua một loạt những thiết bị câng bằng tải Layer 4 và Layer 7. Trong việc triển khai GSLB, thường sẽ có các thiết bị ADC ở cấp độ toàn cầu lẫn cục bộ, nơi lưu lượng truy cập được phân phối đến (phần bên dưới có đề cập).

- Container Load Balancing: Container Load Balancing cung cấp các phiên bản ảo hóa, riêng biệt. Phổ biến nhất hiện nay là hệ thống Kubernetes orchestration, hệ thống này có thể phân chia load giữa các container pods với nhau để giúp nâng cao tính sẵn sàng.

## Cách chọn máy chủ của Load Balancing

Load Balancer chọn máy chủ chuyển tiếp dựa trên sự kết hợp của 2 yếu tố:

- Đảm bảo rằng máy chủ được chuyển tiếp có thể đáp ứng được yêu cầu

- Dựa trên một số độ đo/thuật toán nhất định

### Health Checks

LB (Load Balancing) chỉ chuyển tiếp lưu lượng đến "healthy" backend server. Để kiểm tra sức khỏe của server thì cần kết nối tới backend server sử dụng cổng và giao thức được định nghĩa, đảm bảo rằng máy chủ đó đang lắng nghe. Nếu một máy chủ không được healcheck hoặc không phục vụ (không sẵn sàng) thì nó sẽ được loại ra khỏi cụm phục vụ, request sẽ không được chuyển tiếp đến nó cho đến khi nó được health check và sẵn sàng đáp ứng yêu cầu.

### Một số độ đo/thụât toán trong LB

- Round Robin: tận dụng tính năng round-robin có sẵn trong phần mềm DNS server, quay vòng/lựa chọn máy chủ theo thứ tự đã cho sẵn. Ví dụ cụm máy có 2 server thì req 1 -> server 1, req 2 -> server 2, req 3 -> server 1, ...

    - Ưu điểm: đơn giản, dễ hiểu dễ thực hiện và chi phí thấp

    - Nhược điểm: Khi có 2 yêu cầu liên tục từ phía người dùng sẽ có thể được gửi vào 2 server khác nhau. Điều này làm tốn thời gian tạo thêm kết nối với server thứ 2 trong khi đó server thứ nhất cũng có thể trả lời được thông tin mà người dùng đang cần. Để giải quyết điều này, round robin thường được cài đặt cùng với các phương pháp duy trì session như sử dụng cookie.

     - Một biến thể của Round Robin là Weighted Round Robin: tư tưởng tương tự như RR nhưng xử lý theo trọng số, tức là đánh trọng số cho máy chủ. Một server có khả năng xử lý mạnh hơn server khác sẽ được đánh trọng số cao hơn. 

- Dynamic Round Robin: gần giống với Weighted Round Robin. Điểm khác biệt là trọng số không cố định mà thay đổi liên tục thông qua sự kiểm tra server thường xuyên (xem con nào khỏe, con nào yếu, con nào đang phục vụ nhiều/ít)

- Least Connections: tư tưởng chính là ai rảnh nhất thì người đó làm, lựa chọn máy chủ ít kết nối nhất trong hệ thống. Thuật toán này phải đếm số kết nối trong hệ thống.
    - Ưu điểm: có khả năng hoạt động tốt, ngay cả khi các tải cảu các kết nối biến thiên trong khoảng lớn.

- Least Response Time (Fastest): thuật toán này dựa trên tính toán thời gian đáp ứng của server mà lựa ra server nhanh nhất. Thời gian đáp ứng được tính là khoảng thời gian giữa thời điểm gửi một gói tin đến server và thời điểm nhận được gói tin trả lời. Thuật toán này thường dùng khi server ở các vị trí địa lý khác nhau)


### Nhược điểm của Load Balancing

- Load balancer có thể trở thành thắt cổ chai nếu không có đủ tài nguyên hoặc không được cấu hình tốt.

- Có thể tăng sự phức tạp

- Load balancing đơn lẻ có thể là một điểm lỗi (khi mà lb bị lỗi), khi đó cần dùng nhiều load balancer (lb active, lb passive) thì khi đó việc cấu hình lại phức tạp hơn nhiều
