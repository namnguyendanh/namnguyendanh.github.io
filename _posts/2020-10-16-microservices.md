---
title: (Draft) Microservices
toc: true
categories:
    - Technology
tags:
    - architecture
    - experience
---
Note này sẽ bao gồm 2 phần: tóm tắt cuốn Building Microservices - Sam Newman và kinh nghiệm làm việc với Microservice của mình. 

# Building Microservices - Sam Newman
Đây là cuốn sách thuộc hàng top 1 về chủ đề Microservices. Và quả đúng là thế thật, chỉ cần đọc vài trang đầu thôi thì cũng có thể thấy được sự cô đọng của kiến thức lẫn kinh nghiệm mà tác giả gửi gắm vào cuốn sách. Tuy đã xuất bản từ 2015 nhưng các kiến thức trong sách vẫn không hề bị outdate tới thời điểm hiện tại.

Dưới đây là một số tóm tắt, ghi chú của mình sau khi đọc cuốn sách. Có thể nó sẽ không đâ:

## Giới thiệu Microservices 

### Định nghĩa
- A microservice is a single purpose software application
- small, autonomous services that work together (Sam Newman)
- Từng microservice là các entity riêng biệt. Nó nên là độc lập lẫn nhau.
- Tất cả các giao tiếp giữa các service đều thông qua network calls -> tránh tight coupling.

**Cohesion** — the drive to have related code grouped together — is an important concept when we think about microservices. This is reinforced by Robert C. Martin’s definition of the Single Responsibility Principle, which states “Gather together those things that change for the same reason, and separate those things that change for different reasons.”

Small and focused on doing one thing well
"Microservice is small, and focused on doing one thing well"

"The smaller the service, the more you maximize the benefits and downsides of microservice architecture. As you get smaller, the benefits around interdependence increase"
Và đương nhiên là khi chia nhỏ như thế thì sẽ khiến hệ thống trở nên phức tạp. 

**Autonomous**:
Để đạt được điều này thì phải đảm bảo tính độc lập của các service. 

"To do decoupling well, you'll need to model your services right and get the APIs right."

### Key Benefits
"The benefits of microservices are many and varied. Many of these benefits can be laid at the door of any distributed system. Microservices, however, tend to achieve these benefits to a greater degree primarily due to how far they take the concepts behind distributed systems and service-oriented architecture."

Sử dụng Microservice giúp các công ty áp dụng công nghệ mới nhanh hơn, chịu rủi ro thấp hơn nhiều so với Monolithic model.

**Resilience**: (khả năng phục hồi)
"A key concept in resilience engineering is the bulkhead. If one component of a system fails, but that failure doesn't cascade, you can isolate the problem and the rest the system can carry on working".
Với mô hình monolithic, ta cũng có thể xây dựng hệ thống với nhiều machine để giảm thiểu nguy cơ lỗi. 

**Scaling**:
Đặc tính tiếp theo của Microservice đó là scaling. Thay vì phải scale toàn bộ, thì chỉ scale những service cần scale. 
-> Có thể sử dụng những hệ thống on-demand scaling như AWS để cắt giảm chi phí.

**Ease of Deployment**:
Một chút thay đổi nhỏ ở mô hình monolithic cũng sẽ có ảnh hưởng lớn tới toàn bộ hệ thống. Thế nên sẽ cần nhiều thời gian để phát triển từ ver này sang ver khác. Tuy nhiên, càng để lâu thì càng có nhiều rủi ro.

Sử dụng microservice sẽ rút ngắn quá trình phát triển từ ver này sang ver khác. Nếu có lỗi xảy ra thì có thể cô lập lỗi nhanh chóng. Thế nên sure là các hệ thống lớn như Amazon hay Netflix đều sử dụng mô hình microservice.

**Organizational Alignment**
Giảm khối lượng team và khối lượng codebase. -> smaller team working on smaller codebase -> more productive.

**Composability** 
Khả năng kết hợp được nâng cao. Mô hình microservice giúp mở rộng tư duy phát triển sản phẩm. Thay vì bó hẹp lại desktop website, mobile app thì có thể hướng đến những nền tảng khác như đồng hồ thông minh, tivi thông minh,... 

**Optimizing for Replaceability**
Codebase nhỏ -> dễ kill, không nuối tiếc, chi phí thấp.

## The evolutionary architect
Chương này chủ yếu nói về các kỹ năng cần thiết của một software architect phải có khi áp dụng mô hình microservices.

### A pricipled approach
"Making decisions in system design is all about trade-offs, and microservice architectures give us lots of trade-offs to make! When picking a datastore, do we pick a platform that we have less experience with, but that gives us better scaling? Is it OK for us to have two different technology stacks in our system? What about three? Some decisions can be made completely on the spot with information available to us, and these are the easiest to make.
But what about those decisions that might have to be made on incomplete information?"

**Strategic Goals**: Thiết kế hệ thống phải dựa trên mục tiêu chiến lược của công ty.
**Principles**: các nguyên tắc phải có để định hướng theo mục tiêu. Nên hạn chế số nguyên tắc nhỏ lại, 10 là một con số tốt. VD: Heroku's 12 Factors.
**Practices**: cụ thể hóa các nguyên tắc thành các tiêu chuẩn code, technology-specific. VD: coding guidelines, all log data needs to be captured centrally, or HTTP/REST is the standard integration style. 

"Practices should underpin our principles."

![Principles and Practices](/images/2020-10-16-microservices/principles_and_practices.png)


**The required standard**: Bên cạnh các nguyên tắc, thực hành trên thì cần xác định rõ các tiêu chuẩn cho service. Thế nào là service tốt, nó cần có khả năng gì để đảm bảo rằng hệ thống có thể kiểm soát được.

"It needs to be a cohesive system made of many small parts with autonomous lifecycles but all coming together." - Ben Christensen.

**Monitoring**
Để kiểm soát được hệ thống, ta phải có cái nhìn tổng thể, chứ không chỉ ở từng service. Một hướng tiếp cận đó là các service push health data về trung tâm. Có một số tool hỗ trợ monitoring: Graphite, Nagios, dù là cái nào thì cũng nên standardize nó trong hệ thống của mình.

**Interface**
Thống nhất về Interface, nếu chọn HTTP/REST thì sẽ sử dụng verbs hay nouns? Xử lý versioning of end points như thế nào ?

**Architectural Safety**
Cần đảm vảo từng service có thể tự bảo vệ trước unhealthy, downstream calls.



## How to Model Service
### What make a good service ?
Nguyên tắc Loose coupling và High cohesion.

### Loose Coupling
"When services are loosely coupled, a change to one service should not require a change to another"

Loosely coupled service "biết" về service khác càng ít càng tốt. Điều này có nghĩa là nên limit loại call từ service này sang service khác.

### High Cohension
"Making changes in lots of different places is slower, and deploying lots services at once is risky - both of which we want to avoid."

"Prematurely decomposing a system into microservices can be costly, especially if you are
new to the domain."





























# Kinh nghiệm làm việc với Microservice

Với một hệ thống microservice thì việc kết nối giữa các service rất quan trọng. Nó cần ổn định, tin cậy, failure tolerable. Phải đảm bảo tối thiểu rằng không một kết nối lỗi nào có thể break cả service.

Các framework đã giải quyết cho mình vấn đề đấy rồi, tuy nhiên nếu như task bị lỗi thì phải có cách thông báo lại, hoặc cách handle nó. Mình lấy ví dụ cụ thể ở đây là TTS service và backend AutoVid.

Backend service sẽ gửi thông tin đến TTS service bao gồm: text, voice, callback_url.
TTS sẽ xử lý và sẽ gửi metadata của kết quả về callback_url (metadata ở đây bao gồm: link lấy file mp3, subtitle). Phía backend sẽ dựa vào đó để lấy dữ liệu về. Cơ chế callback_url ở đây mình học được từ Vbee API. Nó cũng hoạt động theo cơ chế này. Các service trong mô hình microservice sẽ không thằng nào phải đợi thằng nào cả.

Nếu quá trình diễn ra suôn sẻ thì không sao, trường hợp xấu có thể xảy ra là:
	* 
Backend không gửi thông tin task cho TTS service: trường hợp này thì đơn giản rồi, mình try catch lỗi ở backend luôn và nếu TTS service trả về status_code >= 300 (hiện tại là thế, sau sẽ phải specific hơn) là lỗi.
	* 
Backend gửi thành công nhưng TTS service bị lỗi: trong trường hợp này thì bên phía TTS service sẽ phải try/catch toàn bộ lỗi có thể xảy ra và gửi thông báo rằng TTS xử lý lỗi về phía Backend.
	* 
TTS lỗi nhưng không gửi về Backend được: cái này một là do TTS không connect được đến backend, hai là backend cung cấp callback_url sai. Ở trường hợp này thì bên phía Backend buộc phải có cơ thế timeout.Vấn đề bây giờ là đặt timeout như nào khi mà backend sẽ không có process nào đợi ? -> Sử dụng scheduler để tính timeout. Khi Backend gửi task thành công thì sẽ cập nhật 2 field: created_datetime và status. Khi scheduler đến thời điểm kích hoạt (created_datetime + max_timeout), nó sẽ kiểm tra field status.




Vậy kinh nghiệm rút ra ở đây là:

Ở cả 2 phía sender và receiver:
	* 
Phải đảm bảo code chạy không xảy ra lỗi, phải có try/catch toàn bộ trường hợp. Nếu không thì sẽ rất khó để debug.



Ở phía sender:
	* 
Khi gửi task về receiver thì bản thân cũng phải set timeout đề phòng trường hợp task bị drop không rõ nguyên nhân.
	* 
Phải cung cấp callback_url chuẩn xác, đảm bảo đường link đấy hoạt động ổn định.



Ở phía receiver:
	* 
Trạng thái của task như thế nào, thành công hay thất bại phải gửi thông tin đấy về sender bằng mọi cách.
	* 
Nên check callback_url mà sender gửi xem có mở không, Để tránh trường hợp phải xử lý task nặng xong gửi lại thì không được, rất tốn resource.

# References
- Book: Building Microservices - Sam Newman (2015)




## Definition
Microservices architecture has emerged as one of the best tools to control big software systemsm, enabled by new tools such as containers and orchestrators.

### The traditional monolith architecture
#### Disadvantages:
- The code will increase in size
- Inefficient utilization of resources
- Issues with development scalability
- Interdependency of technologies

A system following a microservices architecture is a *collection of loosely coupled specialized services that work in unison to provide a comprehensive service.*

#### Advantages of microservices
- Each microservice can be programmed in different languages, the communication between microservices is done through a standard protocol.
- Better resource utilization
- Better deal with dependencies.
- Some services can be hidden from external access -> improve security
- Allow for parallel development and deployment.

The microservices architecture is similar to the UNIX philosophy, applied to web services: write each program (service) to do one thing and do it well, write programs (services) to work together and write programs (services) to handle text streams (HTTP calls), because that is a universal interface.



## Some terms
- Spaghetti code: a common way of referring to code that lacks any structure and is difficult to read and follow.
## Resources

## References
- Book: Hands-On Docker for Microservices with Python
