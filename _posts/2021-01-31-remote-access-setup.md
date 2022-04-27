---
title: Cài đặt hệ thống để truy cập PC từ xa
toc: false
categories:
    - Technology
tags:
    - network
    - security
    - experience
---
Về vấn đề này, mình xin chia sẻ lại bài viết của mình trong hội Overcoded:


## Share kinh nghiệm cài đặt hệ thống để access từ xa (trường hợp về quê ăn Tết như t chẳng hạn 🙂)

Đầu tiên là về vấn đề mở máy từ xa (vì để máy chạy 247 xót máy). Có vài phương án, có cả phần cứng và phần mềm, để đơn giản nhất thì t chọn WoL. Setup phần này thì cứ đọc tut ở trên mạng thôi, cơ bản là chỉnh BIOS và OS settings. Để access thì dùng ssh hay remote gì thì tùy. 

Tiếp theo là port forwarding, thường thì kết nối từ ngoài vào sẽ thông qua router, sẽ cần forward về cổng ssh trong máy. Nên sử dụng cổng khác cho an toàn. T dùng NAT để chuyển từ xxxxx -> local ip:22.

Ok, giờ thì cần mở ip của router. Thường thì dải ip ban đầu bên nhà mạng cấp cho là đi qua proxy server, thế nên kết nối từ ngoài vào ko có được. Cái này thì phải liên hệ với nhà mạng để nó mở cho thôi. T dùng FPT thì service khá nhanh, đặt yêu cầu qua app hoặc gọi thẳng tổng đài cũng đc. Cơ mà chả hiểu sao lần trc nó reset lại về dải mạng ban đầu, hôm nay (CN) phải kích hoạt lại.

Đến bước này thì coi như thông được toàn bộ hệ thống. Giờ dùng DDNS để thiết lập tên miền tĩnh cho ip cùa router. T dùng gói free của noip.com (khoảng 30 ngày thì sẽ cần review lại một lần để giữ domain đấy). Vào trong cài đặt của router rồi điền tài khoản noip vào -> mỗi lần router bị reset (mất điện chẳng hạn) thì nó sẽ tự cập nhật lại IP cho thằng noip. Tóm lại, cái domain sẽ không thay đổi.

Giờ thì xảy ra một vấn đề lớn ở WoL. nếu t nhớ không nhầm thì ở lớp Network có được dạy là Internet có cơ chế chống broadcast, thì đúng thế thật. Thế nên là không gửi được wake-up packet thông qua internet, chính xác hơn thì router cũng không cho forward từ ngoài đến broadcast ip luôn. Đến đây thì cần sự hỗ trợ của tay trong 🙂. T bật raspberry pi 247 nên chỉ cần ssh vào rồi gửi wake-up packet cho PC là ok.

## Note:
Với một số máy không có WoL thì sao ? trường hợp này thì hiếm, thường là máy cũ mới không có. Cơ mà nếu thì chia sẻ luôn là cần tiếp cận phần cứng. Trong mainboard nó có pin-out PW, thì cần ngắn mạch 2 cái đó (tương tự với việc ấn nút power thôi). Hoặc là dùng servo motor để ấn power button nếu không muốn động đến mainboard. Mạch thì có thể điều khiển bằng arduino + rpi.

Với trường hợp không truy cập được router hoặc liên hệ nhà mạng (ví dụ như dùng chung mạng nhà trọ chẳng hạn) thì sẽ khó khăn hơn, cơ mà vẫn có cách giải quyết. Dùng rpi chạy 247 làm tay trong trong hệ thống, rồi dùng reverse ssh tunneling để truy cập vào bình thường. Phần WoL thì như những cách đã trình bày ở trên.

Với trường hợp không có rpi, không truy cập được router luôn thì khá vl 🙂. Có một cách là schedule PC bật vào những khung giờ cố định và tự động chạy reverse ssh tunneling. À, tuy nhiên là không phải bao giờ cũng thiết lập được reverse ssh tunneling dễ dàng vì nó vẫn có những yêu cầu khó có thể thực hiện được. Có thể thay reverse ssh tunneling bằng ngrok chẳng hạn, dùng bản trả phí thì hơi đắt, dùng free thì có thể viết script để tự động restart ngrok. Hồi trc t có làm theo cách này, rồi để nó thông báo link ngrok thông qua Telegram.

Đấy, tóm lại là như thế, không lại lan man quá rồi, về quê đây. Peace🙂 