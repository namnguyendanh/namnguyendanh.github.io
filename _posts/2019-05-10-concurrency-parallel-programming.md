---
title:  "Concurrency & Parallel Programming"
categories: 
    - programming
header:
    image: /images/2019-05-10-concurrency-parallel-programming/header-image.png
---

# Concurrency & Parallel Programming

## Concurrency (đồng thời)
**Concurrency**: Đồng thời là khi hai hoặc nhiều nhiệm vụ chạy có thể bắt đầu , chạy và hoàn thành trong khoảng thời gian chồng chéo. Hiểu đơn giản thì concurrency = Two queues and one coffee machine.(đa nhiệm trên bộ vi xử lý đơn lõi). 1 CPU thì chỉ có thể chạy một thread, tuy vậy chúng ta có thể xử lý đa luồng dựa vào cơ chế time-slicing để chuyển ngữ cảnh (rất nhanh, nên ta cảm giác nhiều thread đang chạy tại một thời điểm).

### Tại sao app cần xử lý đồng thời ?
- Để giữ cho UI luôn trong trạng thái được đáp ứng.
- Tăng tốc độ xử lý. Tận dụng tối đa vi xử lý có trên máy tính.

![](/images/2019-05-10-concurrency-parallel-programming/sync.png)
![](/images/2019-05-10-concurrency-parallel-programming/async.png)

### Race condition 
Race condition là một tình huống xảy ra khi nhiều threads cùng truy cập và cùng lúc muốn thay đổi dữ liệu (có thể là 1 biến, 1 vùng memory,...) Vì thuật toán chuyển đổi việc thực thi giữa các threas có thể xảy ra bất cứ lúc nào nên không thể biết được thứ tự của các threas tuy cập và thay đổi dữ liệu đó sẽ dẫn đến giá trị của data sẽ không như mong muốn. 

## Parallelism (song song)
**Parallelism**: Song song là khi các nhiệm vụ chạy cùng một lúc.

Parallel = Two queues and two coffee machines.(đa nhiệm trên bộ vi xử lý đa lõi)

> "Concurrency is not Parallelism"

## Khi nào thì Concurrency có ích ?
Concurrency giải quyết hai vấn đề:
- CPU-bound: chậm do CPU phải tính toán nhiều 
![CPU Bound](/images/2019-05-10-concurrency-parallel-programming/cpu-bound.webp)
-> giải quyết bằng cách tìm cách để tính toán được nhiều hơn trong cùng một khoảng thời gian.
![Multiprocessing CPU Bound](/images/2019-05-10-concurrency-parallel-programming/multiprocessing-cpu-bound.webp)


- I/O-bound: khiến program chậm do phải đợi i/o từ nguồn ngoài (slower than CPU). Ex: làm việc với file system và network connections. 
![IO Bound](/images/2019-05-10-concurrency-parallel-programming/io-bound.webp)
-> giải quyết bằng cách chèn vào thời gian chờ đợi.
![threading](/images/2019-05-10-concurrency-parallel-programming/threading.webp)


Thêm concurrency vào program có thể khiến nó trở nên phức tạp, thế nên cân nhắc nếu nó thực sự cần thiết.

## Concurrency Programming with Python 
Có vài cách để chạy "đồng thời" ở trong Python.
- Với `multiprocessing`, Python tạo ra processes mới. Mỗi process được hiểu như một chương trình khác nhau. -> Có thể chạy được đồng thời trên đa nhân. 
- `threading` và `asyncio` đều chạy trên một processor, chỉ là có cách để đẩy nhanh quá trình hơn, dù vậy vẫn được gọi là concurrency. Cả hai đều chạy trên 1 nhân. Với `threading`, sử dụng *pre-emptive multitasking* (phủ đầu) -> OS biết từng thread một và có thể **dừng chúng bất cứ lúc nào** để chạy thread khác. Với `asyncio`, sử dụng *cooperative multitasking*. Các task phải hợp tác với nhau bằng cách thông báo sẵn sàng để switch. 

## Ví dụ
Phần code mình đã lược bỏ để tập trung vào kết quả khi sử dụng các phương pháp xử lý đồng thời. Nếu quan tâm về phần code thì các bạn hãy đọc tại [Đây](https://realpython.com/python-concurrency/) hoặc trong file Jupyter Notebook của mình đính ở dưới bài.

### Ví dụ IO-Bound
Bài toán: dùng Python để tạo script download nhiều website về máy tính. Với bài toán này, vấn đề chính là IO-bound gây ra bởi quá trình máy tính kết nối với internet.

Kết quả:
- Synchronous Version: 64.94599461555481 seconds
- `threading` Version: 14.244598388671875 seconds
- `multiprocessing` Version: 16.825629949569702 seconds

Kết quả cho thấy rằng multiprocessing hoạt động không hiệu quả bằng threading.

### Ví dụ CPU-Bound

Bài toán: dùng Python để tạo script tính toán heavy task. Với bài toán này, vấn đề chính là CPU-bound, chúng ta cần tận dụng tối đa hệ thống đa nhân của CPU.

Kết quả:
- Synchronous Version: 9.767674684524536 seconds
- `threading` Versions: 10.26465916633606 seconds
- `multiprocessing` Version: 5.953038454055786 seconds

Kết quả cho thấy rằng multiprocessing hoạt động hiệu quả hơn threading. Phương pháp threading thậm chí còn tệ hơn synchronous do tốn thời gian switch giữa các thread.

## Kết luận
Cần xác định xem có nhất thiết phải dùng concurrency hay không. Nếu có thì xác định là CPU-bound hay I/O Bound.
- Với CPU-bound thì dùng các biện pháp đa nhân.
- Với I/O-bound thì dùng các biện pháp đa luồng.

## Tham khảo
- [Speed Up Your Python Program with Concurrency](https://realpython.com/python-concurrency/)
- https://realpython.com/learning-paths/python-concurrency-parallel-programming/
- https://text.relipasoft.com/2016/12/dong-thoi-khong-phai-la-song-song-concurrency-is-not-parallelism/
- https://viblo.asia/p/concurrency-programming-guide-63vKjpYdl2R
- https://engineering.grokking.org/race-condition-la-gi/

## Jupyter Notebook
[Đây](/images/2019-05-10-concurrency-parallel-programming/concurrency-parallel-programming.ipynb)