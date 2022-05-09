---
title:  "Tổng quan và hướng dẫn cài đặt Prometheus"
header:
    caption: Introduction to Prometheus
categories: 
    - Technology
tags:
    - system-design
    - infrastructure
---

# Prometheus

> "Mỗi cuốn sách đều là một bậc thang nhỏ mà khi bước lên tôi tách khỏi con thú để lên tới gần con người." - Maxim Gorky

## I. Một số kiến thức cũ
Nhắc lại kiến thức cũ: Monitoring system là một hệ thống theo dõi, ghi lại các trạng thái, hoạt động của máy tính hay ứng dụng một cách liên tục.

Các đặc điểm của một hệ thống monitoring:
- Xử lí real time
- Có hệ thống cảnh báo
- Visualization
- Có khả năng tạo reports
- Có khả năng cài cắm các plugins

Để biết chi tiết hơn chúng ta có thể quay lại đọc các bài viết về monitoring và logging của Yaoxu Tuấn Anh.

Các thành phần cơ bản của một hệ thống giám sát có thể mô tả như sau:

![MonitorSystem.png](/images/2022-05-09-prometheus/1.png)

Các thành phần chính sẽ là:

- Collector: Được cài trên các máy agent (các máy muốn monitor), có nhiệm vụ collect metrics của host và gửi về database. Ví dụ: Cadvisor, Telegraf, Beat, ...
- Database: Lưu trữ các metrics mà colletor thu thập được, thường thì chúng ta sẽ sử dụng các time series database. Ví dụ ElasticSearch, InfluxDB, Prometheus, Graphite (Whisper)
- Visualizer: Có nhiệm vụ trực quan hóa các metrics thu thập được qua các biểu đồ, bảng, .... Ví dụ: Kibana, Grafana, Chronograf
- Alerter: Gửi thống báo đến cho sysadmin khi có sự cố xảy ra

Có rất nhiều stack phổ biến như:

- Logstash - Elasticsearch - Kibana

- Prometheus - Node Exporter - Grafana

- Telegraf - InfluxDB - Grafana

Trong loạt bài này chúng ta chủ yếu tìm hiểu về Stack: Prometheus - Node Exporter - Grafana

## Prometheus

### 1. Tổng quan

Prometheus phần mềm mã nguồn mở được sử dụng rộng rãi trong giám sát (monitoring), đánh giá độ ổn định và cảnh báo sớm khi một hoặc nhiều mục tiêu trong hệ thống gặp sự cố. Mục tiêu này (dưới đây được gọi chung là node) có thể là một máy chủ server Linux, Database, HAProxy (Nginx), ... Có thể cài Prometheus trên một node, hoặc trên nhiều node để tạo thành cluster (cụm/hệ thống).

Một số tính tính năng:
- Mô hình dữ liệu đa chiều với chuỗi dữ liệu theo thời gian.

- Cung cấp ngôn ngữ truy vấn dữ liệu - PromQL: tổng hợp dữ liệu chuỗi thời gian trong thời gian thực.

- Triển khai đơn giản: bắt đầu từ 1 server duy nhất, có thể mở rộng theo nhu cầu.

- Dữ liệu chuỗi thời gian được thu thập qua giao thức HTTP.

- Hỗ trợ đẩy dữ liệu thông qua bên thứ 3 trung gian.

- Tự động nhận diện mục tiêu hoặc cấu hình tĩnh.

- Nhiều chế độ đồ họa và hỗ trợ dashboard

- Quản lý được trên Cloud và máy chủ vật lý. Được dùng phổ biến để giám sát các hệ thống container và microservices. 

### 2. Các thành phần của Prometheus

Hệ sinh thái của Prometheus khá đa dạng, được tạo nên từ nhiều thành phần độc lập khác nhau như:

- Prometheus server: phục vụ việc tổng hợp (scraped/pull) và lưu trữ dữ liệu time series

- Client libraries:

- Push Gateway: tạo ra các metrics từ short-lived job cho Prometheus, được coi như là metric cache, ..

- GUI-based dashboarch được code bằng Rails và lấy dữ liệu từ SQL

- Exporter giúp cho việc tạo ra các metrics từ hệ thống bên thứ 3 như Database, Storage, Application Server, Hardware, ...

- Alertmanager đưa ra các cảnh báo như mail

- Command-line query tool.

Hầu hết các thành phần được viết trên ngôn ngữ lập trình Go.

### 3. Kiến trúc hệ thống

![prometheus-architecture.png](/images/2022-05-09-prometheus/2.png)

Cách hoạt động:

- Các jobs được phân chia thành short-lived và long-lived jobs/Exporter:

    - Short-lived là những job sẽ tồn tại trong thời gian ngắn và prometheus-server sẽ không kịp scrapes metrics của các jobs này. Do đó, những short-lived jobs sẽ push (đẩy) các metrics đến một nơi gọi là Pushgateway. Pushgateway sẽ là nơi sẽ phơi bày metrics ra và prometheus-server sẽ lấy được metrics của short-lived thông qua Pushgateway.

    - Long-lived jobs/Exporter: Là những job sẽ tồn tại lâu dài. Các Exporter sẽ được chạy như dưới dạng 1 service. Exporter sẽ có nhiệm vụ thu thập metrics và phơi bày metrics đấy ra. Prometheus-server sẽ scrapes được metrics thông qua hành động pull (kéo).

- Prometheus-server scrapes metrics từ các jobs. Sau đó, nó sẽ lưu trữ các metrics này vào Database. (Lưu trữ trong thư mục data). Prometheus sử dụng kiểu time-series database (arrays of numbers indexed by time). Dựa vào các rules mà ta quy định, (ví dụ như khi cpu xử lý hơn 80%) thì prometheus-server sẽ push (đẩy) cảnh báo đến thành phần Alertmanager.

- PromDash, Grafana,.. dùng câu lệnh querying (PromQL - Prometheus Query Language) để lấy được thông tin metrics lưu trữ ở Prometheus-server và trình diễn.

- Alertmanager sẽ được cấu hình các thông tin cần thiết để có thể gửi cảnh bảo đến email, slack,.... Sau khi prometheus-server push alerts đến alertmanager, alertmanager sẽ gửi cảnh báo đến người dùng.

### 4. Chi tiết các thành phần

#### 4.1 Exporter 

- Exporter về cơ bản như là 1 loại hình dịch vụ (Services) có khả năng thu thập các chỉ số từ đối tượng cần giám sát, sau đó chuyển đổi thành định dạng mà Prometheus có thể hiểu. Exporter cũng cung cấp 1 địa chỉ Endpoint đúng theo yêu cầu mà Prometheus cần, dùng để cào dữ liệu.

- Hiện tại, danh sách exporter trải dài từ cơ sở dữ liệu (MongoDB, MySql, PostgreSQL,...), phần cứng (NVIDIA GPU, Fortigate, Netgear Router, IoT Edison,...), lưu trữ (Hadoop HDFS FSImage, ScaleIO, GPFS, Gluster,...), máy chủ web (Apache, Nginx, HAProxy,...), hoặc là các nền tảng đám mây như AWS, Cloudflare, DigitalOcean,...

Danh sách các Exporter được dựng sẵn có [ở đây](https://prometheus.io/docs/instrumenting/exporters/)

- Có thể tự viết các exporter dựa trên các client libraries mà Prometheus cung cấp. Có htrên hầu hết các ngôn ngữ phổ biến: Python, Go, NodeJS, ... (nếu cần sẽ có bài viết chi tiết khác)

#### 4.2 Push Gateway

- Pushgateway được sử dụng trong trường hợp mà Promethes server không thể scrape metrics một cách trực tiếp. Có thể là các job chỉ tồn tại trontg thời gian ngắn mà Promethes server chưa kịp scrape metrics.

- Để giải quyết vấn đề này, thì Pushgateway được ra đời. Pushgateway sẽ đóng vai trò trung gian giữa promethes server và targets cần monitor. Lúc này, metrics sẽ được phơi bay ở pushgateway, chứ không pải là ở targets nữa.

- Trên targets, short-live job sẽ được cấu hình để push metrics đến Pushgateway (có thể sử dụng bằng lệnh curl để push metrics). Rồi từ đó, Prometheus server sẽ scrape (pull) metrics ở Pushgateway về và lưu trữ trên server.

- Pushgateway sẽ không lưu trữ metrics lâu dài. Nó chỉ lưu trữ tạm thời mà thôi. Khi metrics có values mới, nó sẽ thay thế values cũ.

- All pushes are done via HTTP. The interface is vaguely REST-like.

#### 4.3 Prometheus server

<!-- <br><br>
<p align="center">
![prometheus-architecture.png](/images/2022-05-09-prometheus/3.png)
</p>
<br> -->

![prometheus-architecture.png](/images/2022-05-09-prometheus/3.png)


Máy chủ Prometheus (node master) bao gồm 3 thành phần chính:
- Retrieval: thu thập thông tin từ các nguồn cần giám sát (có thể coi là node slave) như máy chủ application, máy chủ db, ...

- Storage: lưu trữ dữ liệu time series

- HTTP server: cung cấp API truy vấn duex liệu 


### 5. Thực hành

Do phạm vi bài viết và thời gian tìm hiểu cũng không quá nhiều, mình chủ yếu thực nghiệm trên 2 node và sử dụng exporter có sẵn của Prometheus:

- Node master: Cài Prometheus server, trên chính máy tính cá nhân

- Node slave (exporter): Cài đặt trên server Sakura của công ty với exporter có sẵn là Node_Exporter

Tiếp theo, mình cũng cài đặt qua các nén chứ không cài đặt dựa trên docker

#### 5.1 Cài đặt Node Exporter

Chúng ta cùng nhau cài đặt Node Exporter như một service chạy trên server. Đầu tiên chúng ta cần tải phiên bản mới nhất của Node Exporter về. Các bạn có thể tìm kiếm phiên bản của Node Exporter tại https://prometheus.io/download/.

```bash
wget https://github.com/prometheus/node_exporter/releases/download/v1.3.1/node_exporter-1.3.1.linux-amd64.tar.gz
```

Giải nén mục mới tải về:

```bash
tar -xvzf node_exporter-1.3.1.linux-amd64.tar.gz
```

Đổi tên thư mục

```bash
mv node_exporter-1.3.1.linux-amd64.tar.gz node_exporter
```
Tạo một user cho việc quản lý exporter:

```bash
sudo useradd -rs /bin/false node_exporter
```

Copy binary file trong thư mục đã giải nén tới địa chỉ /usr/local/bin:

```bash
cd node_exporter
cp node_exporter /usr/local/bin
```

Thiết lập quyền cho binary file:

```bash
chown node_exporter:node_exporter /usr/local/bin/node_exporter
```

Tạo một service cho việc chạy Node Exporter

```bash
cd /lib/systemd/system
sudo nano node_exporter.service
```
Thêm nội dung như sau vào service file:
```bash
[Unit]
Description=Node Exporter
After=network-online.target

[Service]
User=node_exporter
Group=node_exporter
Type=simple
ExecStart=/usr/local/bin/node_exporter

[Install]
WantedBy=multi-user.target
```
Khởi chạy service chúng ta vừa mới tạo:

```bash
sudo systemctl daemon-reload
sudo systemctl start node_exporter
```

Mặc định exporter sẽ chạy trên cổng 9100

Bạn có thể kiếm tra bằng cách truy vập vào địa chỉ: http://localhost:9100. Như dưới, là mình kiểm tra qua địa chỉ ip của server.

![Screenshot from 2021-12-15 17-56-00.png](/images/2022-05-09-prometheus/4.png)


#### 5.2 Cài đặt Prometheus


Tải phiên bản ổn định nhất (LTS) của Prometheus

```bash
wget https://github.com/prometheus/prometheus/releases/download/v2.32.0/prometheus-2.32.0.linux-amd64.tar.gz
```
Giải nén thư mục 

```bash
tar -xvzf prometheus-2.32.0.linux-amd64.tar.gz
```
Đổi tên thư mục cho dễ nhìn :D

```bash
mv prometheus-2.32.0.linux-amd64.tar.gz prometheus
```

Tạo một user và group để quản lý Prometheus server cũng như folder lưu data (dùng root cũng đc)

```bash
sudo useradd -rs /bin/false prometheus
```
Tiếp theo chúng ta di chuyển các file binary trong thư mục đã giải nén về /usr/local/bin:

```bash
sudo chown prometheus:prometheus /usr/local/bin/prometheus
```

Tạo 1 thư mục prometheus trong /etc và di chuyển các tài nguyên cần thiết:

```bash
sudo mkdir /etc/prometheus
sudo cp -R consoles/ console_libraries/ prometheus.yml /etc/prometheus
```

Chỉnh sửa file prometheus.yml 

```yml
# my global config
global:
  scrape_interval: 15s # Set the scrape interval to every 15 seconds. Default is every 1 minute.
  evaluation_interval: 15s # Evaluate rules every 15 seconds. The default is every 1 minute.
  # scrape_timeout is set to the global default (10s).

# Alertmanager configuration
alerting:
  alertmanagers:
    - static_configs:
        - targets:
          # - alertmanager:9093

# Load rules once and periodically evaluate them according to the global 'evaluation_interval'.
rule_files:
  # - "first_rules.yml"
  # - "second_rules.yml"

# A scrape configuration containing exactly one endpoint to scrape:
# Here it's Prometheus itself.
scrape_configs:
  # The job name is added as a label `job=<job_name>` to any timeseries scraped from this config.
  - job_name: "sakura_node_exporter"

    # metrics_path defaults to '/metrics'
    # scheme defaults to 'http'.
    scrape_interval: 5s
    static_configs:
      - targets: ["153.126.136.5:9100"]
  - job_name: "prometheus_exporter"

    # metrics_path defaults to '/metrics'
    # scheme defaults to 'http'.
    scrape_interval: 5s
    static_configs:
      - targets: ["localhost:9090"]
```


Tạo một thư mục để lưu trữ dữ liệu cho Prometheus:
```bash
sudo mkdir -p data/prometheus
```

Cấp quyền:

```bash
sudo chown -R prometheus:prometheus data/prometheus /etc/prometheus/*
```

Tiếp theo, tương tự node_exporter, ta tạo một daemon job cho prometheus:

```bash
cd /lib/systemd/system
sudo touch prometheus.service
```
Điền các thông tin sau:
```bash
[Unit]
Description=Prometheus
Wants=network-online.target
After=network-online.target

[Service]
Type=simple
User=prometheus
Group=prometheus
ExecStart=/usr/local/bin/prometheus \
 --config.file=/etc/prometheus/prometheus.yml \
 --storage.tsdb.path=/home/namnd/Downloads/prometheus/data/prometheus \
 --web.console.templates=/etc/prometheus/consoles \
 --web.console.libraries=/etc/prometheus/console_libraries \
 --web.listen-address=0.0.0.0:9090 \
 --web.enable-admin-api

Restart=always

[Install]
WantedBy=multi-user.target
```
Mặc định Prometheus server sẽ chạy ở cổng 9090

Như vậy khi service start chương trình prometheus trong thư mục /usr/local/bin sẽ được thực thi với các param tương ứng đã được định nghĩa. Ta lưu lại file và thực hiện start service:

```bash
sudo systemctl enable prometheus
sudo systemctl start prometheus
```
 Truy cập địa chỉ: http://localhost:9090. Dưới đây là hình mình họa:

![Screenshot from 2021-12-17 13-59-54.png](/images/2022-05-09-prometheus/5.png)

Thử một truy vấn: 

![Screenshot from 2021-12-17 14-00-46.png](/images/2022-05-09-prometheus/7.png)


![Screenshot from 2021-12-17 14-00-56.png](/images/2022-05-09-prometheus/6.png)


### 6. Lời kết

Cảm ơn mọi người đã đón đọc, hẹn gặp lại trong bài tới về Grafana