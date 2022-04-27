---
title:  "Raspberry Pi"
categories: 
    - technology
header:
    image: /images/2019-06-25-raspberry-pi/raspberry-pi.png
---

## Raspberry Pi là gì ? 

Raspberry Pi là một chiếc máy tính mini, có thể được ứng dụng trong nhiều công việc.  

## Điểm mạnh của RP: 
- khả năng kết nối mạng của nó (rất cần trong IoT) 
- các chân GPIO (tận dụng được các module trong các dự án về phần cứng) 
- nguồn lực hỗ trợ lớn (có OS debian-based riêng, có nhiều tutorial, thiết lập cũng khá dễ dàng) 
- giá thành rẻ 

Việc sử dụng Raspberry thật ra rất dễ dàng. Nếu sử dụng OS Raspbian thì việc config SSH, camera, screen resolution,... trở nên rất dễ dàng với raspi-config 

Raspberry Pi 
 

Nguồn: Cuốn Raspberry Pi Cookbook 

## Setup and Management 

Model B > A 

Nên kiếm case cho RP để tránh bị chập mạch 

5V – 2A is good enough (>700mA) 

Choose Raspbian for hardware projects 

4GB SD Card 

Writing an SD Card with NOOBS 

Chỉnh resolution: /boot/config.txt 

#framebuffer_width = 320 

#framebuffer_height = 240 

raspi-config tool 

$ sudo raspi-config 

-> adjust the picture size: text extend off the screen -> turn off overscan 

-> overclock 

-> change password (default: raspberry) or $ sudo passwd 

-> boot straight 

Note: shutdown RP doesn’t actually turn off the power 

Installing the RP Camera Module 

Connect -> 

If the camera section is not listed in rasp-config -> $ sudo apt-get update && sudo apt-get upgrade 

Manipulating the Camera 

Output image: $ raspistill –o output.jpg 

Output video: $ raspivid –o video.h264 -t 10000 (milliseconds) 

Networking 

Tìm IP của máy (để có thể chạy SSH hoặc VNC): $ sudo ifconfig 

-> eth0 (wire connection) & wlan0 (wireless connection) 

Cài IP tĩnh 

Chỉnh sửa file /etc/network/interfaces 

Chỉnh wlan0 nếu là wireless và eth0 nếu là wire, ví dụ: 

 
iface eth0 inet static 
 
address 192.168.1.16 
 
netmask 255.255.255.0 
 
gateway 192.168.1.1 
 
iface wlan0 inet  dhcp 
 
wpa-ssid "ssid" 
 
wpa-psk "12345678" 
 
 

Thay network name 

Sửa file /etc/hostname. Thay raspberrypi thay tên mình muốn. 

Sử dụng Console Cable để điều khiển RP bằng máy khác 

### SSH 
enable SSH option trong raspi-config 

### VNC 

$ sudo apt-get update 
$ sudo apt-get install tightvncserver 

Run VNC server by: $ vncserver :1 

Nếu muốn VNC tự động chạy khi khởi động thì: 

``` 
cd /home/pi  
cd .config 
mkdir  autostart 
cd autostart 
nano  tightvnc.desktop 
 ```
 

Rồi thêm dòng này: 

```
[Desktop Entry] 
Type=Application 
Name=TightVNC 
Exec=vncserver :1 
StartupNotify=false 
```
 

NAS (Network Attached Storage) 

Network Printing 


Streaming Video  

 

Official way:  

https://www.raspberrypi.org/blog/camera-board-available-for-sale/ 

 

https://blog.miguelgrinberg.com/post/stream-video-from-the-raspberry-pi-camera-to-web-browsers-even-on-ios-and-android 

 
### TorrentBox with Transmission
https://pimylifeup.com/raspberry-pi-transmission/

### Media Server
https://pimylifeup.com/raspberry-pi-plex-server/
 

LD_LIBRARY_PATH=./:./plugins/:./plugins/input_file/:./plugins/output_http/ ./mjpg_streamer -i "input_file.so -f /tmp/stream/" -o "output_http.so -w ./www" 

## Cool Projects
- https://tinhte.vn/threads/cach-dung-raspberry-pi-pi-hole-de-chan-quang-cao-cho-tat-ca-thiet-bi-trong-nha.3000792/