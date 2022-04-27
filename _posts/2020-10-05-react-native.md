---
title: React Native tổng quan
categories:
    - Technology
tags:
    - library
    - reactnative
---

Nhân tiện đợt này tham gia khóa học React Native nên mình sẽ viết note này để tổng hợp lại kiến thức. 

React Native là một công nghệ, mà đã là công nghệ thì sẽ có lúc nó lỗi thời. Công nghệ nó có tồn tại được hay không là do bản chất đứng đằng sau nó.

## React Native là framework hay library ?
RN là một framework, khác với ReactJS, vốn là một UI library cho web. Cả hai đều được backed bởi Facebook, có community khá lớn. Tuy hướng đến những nền tảng khác nhau, nhưng chúng có chung một nguyên lý thiết kế.


## Tại sao người ta dùng React Native ?
RN sinh ra để những JS dev có thể tham gia vào viết native app, chủ yếu là cho 2 hệ điều Android và iOS.

Trên thị trường cũng có nhiều giải pháp công nghệ khác hỗ trợ cross-platform development. Tuy nhiên RN được sử dụng khá nhiều là do nó nhanh, các component trong RN sẽ được map sang native component trong các nền tảng di động.

Còn tại sao map được, thì là do có một thằng "bridge" ở trung gian thực hiện việc mapping này. Cái này không quá quan trọng, bạn biết là có thằng này nó thực hiện là được.



## Nhược điểm của React Native ?

Nhược điểm của nó có thể thấy rõ rằng là nó vẫn khá mới, chỉ mới xuất hiện từ 2015. 

Theo kinh nghiệm của mình cũng như chính lời của anh lecturer thì việc cài đặt RN thôi cũng có thể sinh ra rất nhiều lỗi, chủ yếu là về xung đột dependencies. Cái này thì cũng có thể khắc phục được bằng cách sử dụng Expo (một công cụ giúp quản lý, deploy, cài đặt RN project và hơn thế nữa...). Dùng lệnh `expo install` thì sẽ giảm thiểu nguy cơ xung hơn là sử dụng `npm install`.

Nếu ai đã làm quen với Python thì có thể thấy rằng việc install bằng Expo cũng giống như dùng Anaconda để cài.

Nhược điểm thứ 2 đó là khó để debug, có nhiều lỗi nó báo không rõ ràng (hoặc có thể là do mình không hiểu). Nhưng đại loại là tìm kiếm cách debug trên mạng khá khoai vì sẽ có nhiều kiểu có thể dẫn đến lỗi đấy. (You know what I'm talking about right)

Tương lai của RN cũng không quá chắc chắn, cũng không biết lúc nào thì FB cho dừng dự án này chẳng hạn. Có thể chưa phải lúc này, khi mà nó vẫn được rất nhiều người sử dụng.

Có một số công nghệ cũng mới nổi lên, ví dụ như Flutter do Google đứng sau. Mà Google thì đứng sau Android, thế nên bạn biết rồi đấy :). 

Anw, RN có đáng học tại thời điểm này không ? Thì thực ra là có, RN vẫn được nhiều công ty startup tuyển. Vì nó giúp người ta giảm chi phí xây dựng UI, vì nó vẫn được việc, vẫn tạo được những app hiển thị đơn giản (chủ yếu xử lý trên cloud).


## Bắt đầu với React Native
Để bắt đầu với React Native, thì đầu tiên nên biết căn bản về html, javascript, css, nhất là javascript. Ở phần này thì mình sẽ bám theo nội dung chương trình mà mình đang tham gia vì mình thấy nó khá là ổn.

Sau khi biết cơ bản rồi thì có thể bắt tay vào cài đặt React Native. Phần này chính ra khá là lằng nhằng đấy. Có thể bám theo hướng dẫn cài đặt trong trang chủ của RN. Thì về cơ bản là nên sử dụng expo để cài đặt môi trường. 

Nếu không có máy thật thì có thể sử dụng emulator. Với Android thì cài Android Studio rồi tạo emulator. (Với Mac thì chịu, mình chưa có Mac + iphone để mà test...). Anw, cài app Expo vào máy là nó có thể kết nối được.

Nhược điểm của thằng Expo này đó là nó chỉ hỗ trợ project React Native thuần JS. Với trường hợp muốn inject một số native component vào project thì sẽ phải "eject from Expo". Nói chung thì tốt nhất là cố gắng tìm mọi cách để không phải rời Expo. Tránh xa các cái job sử dụng RN để làm mấy cái quét ảnh, dùng camera nhận diện các thứ vì những app như thế sẽ phải sử dụng các cảm biến của thiết bị. 

Cài đặt xong thì bắt tay vào học tiếp thôi. Nguồn tham khảo tốt nhất chính là docs, vì RN còn mới, thay đổi rất nhanh. Các nguồn tham khảo có thể nhanh chóng bị outdated. 

Các vấn đề của RN thì phải bắt tay vào code thì mới rõ được. Trong chương trình mình học thì có một số assignment hỗ trợ việc học:
- Clone Instagram profile screen: học về cách layout trong RN, chủ yếu là sử dụng flex. Học cách sử dụng các components cơ bản. 
- Oẳn tù tì: học về props và states. Học về cách tạo  custom components.
- Todolist: học về react-navigation.

Ở assignment todolist thì mình gặp phải vấn đề state và prop, về cách chia sẻ data giữa các screen. Thì trong hướng dẫn trong docs, người ta cũng có hẳn một mục về Lift up states, đại loại là đẩy state của child components thành state của parent components, biến nó thành ground-truth data. 

Tuy nhiên là với một dự án phức tạp thì việc nâng state lên như kia sẽ rất phức tạp, khó kiểm soát. Thế nên người ta sẽ phải dùng Redux. 

Phần Redux mình chưa học chi tiết, thế nên không chém tiếp được :v. Mấy tuần nữa học xong thì mình viết tiếp.

To be continued...

## Tham khảo
- [React Native: What is it and why is it used](https://medium.com/@thinkwik/react-native-what-is-it-and-why-is-it-used-b132c3581df#:~:text=React%20Native%20is%20a%20framework,library%20to%20create%20user%20interfaces.)
- [React Native docs](https://reactnative.dev/docs/getting-started)





## React Native là gì ?
React Native là một JavaScript framework để viết một ứng dụng *native* cho iOS và Android (cả Web được nữa). Nó dựa trên React (thư viện JavaScript do Facebook tạo ra), nhưng thay vì hướng đến nền tảng trình duyệt web thì nó hướng đến nền tảng di động. 

Xu hướng sử dụng React Native để phát triển ứng dụng di động đang ngày càng phát triển.

![React Native Trend][react-native-trend]
Source: https://recro.io/blog/wp-content/uploads/2017/07/Javascript-Native-Google-Trends.png

## Ứng dụng nào được viết bằng RN ?
Các ứng dụng nổi tiếng sử dụng React Native có thể kể đến là:
- Facebook
- Instagram
- SoundCloud Pulse
- Bloomberg
- Walmart
- Wix


## Nó hoạt động như thế nào ?
React Native "bridge" dùng native rendering APIs ở trong Objective-C (cho iOS) hay Java(cho Android). Thế nên chương trình của bạn sẽ được render với các mobile UI components, chứ ko phải webviews(như trong web app). 

Ngoài ra, nó cũng truy cập được các platform API như camera, định vị,... nhờ sử dụng JavaScript.

Nhìn một cách tổng quan, có 3 phần chính từ RN platform:
1. **Native Code/Modules**: Đôi khi một ứng dụng cần truy cập các platform API và RN không có các module tương ứng. Có thể sẽ cần phải tái sử dụng native code hoặc các đoạn code yêu cầu performance cao. Tìm hiểu thêm tại [đây](https://facebook.github.io/react-native/docs/native-modules-android.html)
2. **Javascript VM**: JS VM là nơi thực thi phần JS code. 
3. **React Native Bridge**: là cầu nối có trách nhiệm giao tiếp giữa các *native thread* và *JS thread*.



## Điểm mạnh
- Sử dụng platform's standard rendering APIs: đạt hiểu suất cao hơn so với các phương pháp khác(VD: Cordova, Ionic vốn sử dụng webviews).
- Sử dụng JavaScript: 
  + tiết kiệm thời gian, không phải build lại toàn bộ ứng dụng trong quá trình phát triển.
  + không cần IDE cồng kềnh như Android Studio, bất cứ text editor nào cũng được.
  + Tận dụng được Chrome's developer tools trong quá trình debug.
- Cross-platform: Share Knowledge, reuse code dễ dàng.


Giờ ta bắt tay vào phần technical một chút. Sẽ rất dễ dàng nếu bạn biến một chút về html, css, javascript.

## Một số thuật ngữ 
**JSX**: cú pháp dùng để nhúng XML vào trong JavaScript. (và ta cũng có thể nhúng các JavaScript expression vào trong JSX)

**Component's Lifetime**: vòng đời của một component. 


## Cấu trúc chương trình 

Một phần mềm React Native được tạo bởi các *(React) component*. Các component sẽ đại diện cho các Native View và component tương ứng. Ví dụ: 

![An example of a UI component][facebook_inputfield]

Ex: Một `TextInput` trong RN sẽ tương ứng với một native `EditText` trong Android.

Có hai loại dữ liệu để kiểm soát một component (giống cơ chế của React): 
- `props`: Thường thì các component sẽ được tạo ra cùng các thông số. Các thông số này được gọi là `props`, nó là thông số cố định suốt component's lifetime.  
- `state`: dữ liệu có thể thay đổi. Thường thì state sẽ được khởi tạo ở trong constructor, khi muốn thay đổi thì gọi hàm `setState`. 

Bất cứ khi nào một trong hai cái trên thay đổi thì chương trình sẽ tự động rerender lại UI.

Điểm qua một số các component đơn giản thì có:
- Text
- Image
- View
- TextInput

## Style
Kết hợp với nhau thì ta sẽ có được khung chương trình. Để làm chương trình có UI đẹp mắt thì ta dùng Style. Mỗi component đều chấp nhận prop có tên là `style`. Khi số lượng các component t
ăng lên, và trở nên phức tạp hơn thì ta cần tách biệt phần style và component. -> Sử dụng `StyleSheet.create`. Điểm này khá giống css và html. 

### Fixed Dimensions
Có một lưu ý nữa là style trong React Native đều là **unitless**, nên sẽ có kích cỡ giống nhau kể cả với màn hình như thế nào. -> [Device-independent pixel](https://en.wikipedia.org/wiki/Device-independent_pixel)

### Flex Dimensions
Dùng `flex` trong style để khiến component co dãn dựa trên khoảng trống cho phép. Thường ta sử dụng `flex: 1` để khiến component chiếm toàn bộ khoảng trống. Giá trị được đặt của `flex` càng lớn thì tỉ lệ chiếm càng lớn.

Kết hợp với các thuộc tính khác như: `flexDirection`, `alignItems`, `justifyContent`,... là có thể tạo được layout như ý muốn. Tìm hiểu thêm tại [đây](https://facebook.github.io/react-native/docs/flexbox)


## Ví dụ về BlinkApp
Source: https://facebook.github.io/react-native/docs/state#docsNav
```javascript
import React, { Component } from 'react';
import { AppRegistry, Text, View } from 'react-native';

class Blink extends Component {

    componentDidMount(){
    // Toggle the state every second
        setInterval(() => (
          this.setState(previousState => (
            { isShowingText: !previousState.isShowingText }
          ))
        ), 1000);
     }

    //state object
    state = { isShowingText: true };

  render() {
    if (!this.state.isShowingText) {
      return null;
    }

    return (
      <Text>{this.props.text}</Text>
    );
  }
}

export default class BlinkApp extends Component {
  render() {
    return (
      <View>
        <Blink text='I love to blink' />
        <Blink text='Yes blinking is so great' />
        <Blink text='Why did they ever take this out of HTML' />
        <Blink text='Look at me look at me look at me' />
      </View>
    );
  }
}

// skip this line if using Create React Native App
AppRegistry.registerComponent('AwesomeProject', () => BlinkApp);

```

Để tạo một text có hiệu ứng blink thì có một cách là khởi tạo một component mới là `Blink` như trên.

- `componentDidMount()`: hàm này sẽ chạy khi component này load xong.
- `setInterval(functionA, interval)`: hàm này để `functionA` chạy theo chu kỳ là `interval`.



## Tham khảo
- [https://www.oreilly.com/library/view/learning-react-native/9781491929049/ch01.html](https://www.oreilly.com/library/view/learning-react-native/9781491929049/ch01.html)

[facebook_inputfield]: /images/2019-06-27-react-native/facebook_inputfield.png
[react-native-trend]: /images/2019-06-27-react-native/react-native-trend.png


## Getting Started

Có 2 cách để tạo một project RN: thông qua Expo hoặc kiểu traditional. Đọc thêm tại [đây](https://facebook.github.io/react-native/docs/getting-started)

### Lỗi trong quá trình cài đặt:
```
Couldn't adb reverse: cannot bind listener: Operation not permitted.
```
Khắc phục: 
```
adb kill-server
adb root
```

### Expo
- Cài đặt đơn giản, nhanh chóng
- Không include đc native code, tuy nhiên vẫn là điểm khởi đầu tốt. Về sau có thể eject.

## Learn the Basics



## Reference
- https://facebook.github.io/react-native/docs/getting-started
- 