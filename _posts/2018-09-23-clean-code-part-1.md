---
title:  "Clean Code - Part I: Names & Functions"
categories: 
    - programming
header:
    image: /images/2018-09-23-clean-code-part-1/clean_code.jpg
---
# Clean Code - Part I: Names & Functions

Việc ngồi đọc lại code tốn rất nhiều thời gian so với việc viết code. Bad code còn là mỗi nguy hại đến sản phẩm, đến team khi mà sản phẩm càng phát triển. Muốn là lập trình viên giỏi thì phải biết viết những dòng code chuẩn - clean. Cuốn sách Clean Code - Robert Cecil Martin là một cuốn sách kinh điển mà bất cứ lập trình viên nào cũng phải đọc.

Muốn viết clean code thì phải có sự lành nghề nhất định (craftmanship). Để được điều đó thì cần hai thứ: kiến thức và luyện tập. Bạn có thể biết xe đạp hoạt động, cách sử dụng xe đạp nhưng điều đó không có nghĩa bạn sẽ biết đi xe đạp mà thiếu đi luyện tập. 

Ở Blog này, mình sẽ tóm tắt những điều mình học được từ cuốn Clean Code, trước hết là những kiến thức, khuôn mẫu, nguyên tắc chuẩn để viết clean code.


## 1. Clean Code là gì ?
Có thể có nhiều định nghĩa bởi nhiều người khác nhau. Dưới đây là quan điểm của những lập trình viên có kinh nghiệm dày dặn:

> "I like my code to be elegant and efficient. The logic should be straightforward to make it hard for bugs to hide, the dependencies minimal to ease maintenance, error handling complete according to an articulated strategy, and performance close to optimal so as not to tempt people to make the code messy with unprincipled optimizations. Clean code does one thing well." - Bjarne Stroustrup, inventor of C++

> "Clean code is simple and direct. Clean code
reads like well-written prose. Clean code never
obscures the designer’s intent but rather is full
of crisp abstractions and straightforward lines
of control." - Grady Booch

Điểm qua có thể thấy rằng, clean code phải có những yếu tố như:
- Dễ nhìn, dễ đọc (bởi con người): Rõ ràng, gọn gẽ, có thể hiểu được nhanh chóng và dễ phát triển
- Hiệu quả: Tối ưu, nhanh chóng 
- Tập trung: Giải quyết một vấn đề nhất định
- Tối giản hoá: tối giản sự phụ thuộc, tối giản chức năng.

Tiếp theo, chúng ta sẽ đến với phần chính - những nguyên tắc, khuôn mẫu trong việc viết clean code.

## 2. Đặt tên
Việc đặt tên cho biến, cho hàm hay lớp là việc làm thường xuyên khi viết code. Dưới đây là một số nguyên tắc cơ bản để có những cái tên tốt:

### Sử dụng tên bao hàm ý định
Chọn một cái tên tốt có thể mất nhiều thời gian nhưng nó sẽ tiết kiệm cả khối thời gian trong quá trình phát triển phần mềm.

- Tên của biến, hàm hay lớp nên trả lời cho những câu hỏi lớn, ví dụ như: tại sao nó tồn tại? nó làm gì? làm thế nào để sử dụng nó?
- Một cái tên tốt sẽ không cần đến comment đi kèm để giải thích.

VD:

- Bad: `int d; // elapsed time in days`
- Good: `int elapsedTimeInDays;` 

VD:

- Bad: `c[0] == 4`
- Good: `cell[STATUS_VALUE] == FLAGGGED`
- Better: `cell.isFlagged()`

### Tránh sai lệch thông tin
Khi đặt tên, tránh sử dụng những từ không nằm trong ý định. Cũng như tránh viết tắt vì đôi khi người khác nhìn vào có thể hiểu nhầm ý.

VD: 

- Bad: `accountList` từ List ở đây không nên dùng nếu type không phải là list
- Good: `accounts` hoặc `accountGroup`

Tránh dùng `i, j, o, l` khi đặt tên biến vì nó khó để phân biệt.

VD: 
- Bad: `l = 1`
- Bad: `o = 0`
- Bad: `a[i][j] = b[j][i]`

### Phân biệt tên biến rõ ràng
- Không nên đánh số cho tến biến (`a1, a2,...`)
- Không nên phân biệt biến một cách mập mờ. VD: `ProductData | ProductInfo`, `Customer | CustomerObject`

### Sử dụng tên phát âm được
- Hay chính xác ra đó là đừng dùng từ viết tắt. 

VD: 

- Bad: `genymdhms (generation date, year, month, day, hour, minute, and second)` 
- Good: `generationTimestamp`

### Sử dụng tên biết dễ tìm kiếm
- Tên chỉ có 1 ký tự (a,b,...) hay hằng số  (1,2,...) rất khó để tìm trong đoạn code. VD: MAX_CLASSES_PER_STUDENT dễ tìm hơn số 7 nhiều. -> nên define constants.
- Những cái tên 1 ký tự thì chỉ nên sử dụng làm biến địa phương (local variable). VD: `for(int i=0; i<MAX; i++){`
>  The length of a name should correspond to the size of its scope

### Đặt tên lớp
- Class hay object nên sử dụng danh từ hoặc cụm danh từ. Tránh sử dụng `Manager`, `Processor`, `Data`, `Info`
### Đặt tên hàm chức năng
- Method nên sử dụng động từ hoặc cụm động từ. VD: `deletePage`, `save`.

- Accessors, mutators, và predicates nên đặt tên theo giá trị và có prefix như `get`, `set`, `is` (Javabean standard)

Nên sử dụng từ formal, không slang hay joke các thứ. VD: `HolyHandGrenade | deleteItems`

Sử dụng 1 từ cho một khái niệm trong suốt quá trình code. VD: chọn 1 trong 3 cái `fetch`, `retrieve`, `get` ,`controller`, `manager`, `driver`

Và phân biệt giữa `add`, `insert`, `append`

### Có bối cảnh phù hợp
Giả sử như có các biến `firstName`, `lastName`, `street`, `houseNumber` thì có thể thấy rằng nó thuộc về địa chỉ (address). 

Có thể đặt prefix để thể hiện bối cảnh: `addrLastName`, `addrState` hoặc tạo một class `Address`

### Tóm gọn
Điều khó nhất ở việc chọn tên tốt là bạn phải có descriptive skill tốt và shared cultural background(??).

Giải quyết được vấn đề đặt tên thì code sẽ dễ đọc hơn rất nhiều và có thể dành thời gian để tập trung vào những thứ quan trọng hơn.

## 3. Hàm
Quy tắc đầu tiên và quan trọng nhất:
### Hàm phải tối giản
Càng nhỏ càng tốt và chỉ làm một thứ duy nhất (one level of abstraction). Lý do chúng ta viết hàm là để biến một cái concept lớn thành nhiều những lớp trừu tượng khác nhau.

Nếu hàm mà có thể chia thành section thì chắc chắn là nó có thể chia nhỏ thành các function con nữa.


> FUNCTIONS SHOULD DO ONE THING. THEY SHOULD DO IT WELL.
THEY SHOULD DO IT ONLY.
### Blocks và Indenting
Code nên có những khoảng cách và xuống dòng hợp lý để dễ đọc. VD: trong Python có chuẩn PEP8 
### Chọn tên tốt
Giống như những nguyên tắc ở chương 2. Việc chọn tên cho hàm phải bao hàm ý định, có tính mô tả tốt. Nếu hàm càng nhỏ và tập trung (focused) thì càng dễ để chọn tên. 

Ngoài ra, cần tránh side effects, tên như nào thì làm đúng như thế . VD: `checkPassword()` thì chỉ check password và không động tới các tác vụ khác.


### Tối giản đối số
Lý tưởng nhất là không có đối số nào cả(niladic). Rồi đến 1 đối số (monadic), và 2 đối số (dyadic). 3 Đối số (triadic) thì nên tránh khi có thể.

#### Dạng monadic
Có nhiều trường hợp sử dụng dạng monadic. Ví dụ như:
- Hỏi về đối số. `boolean fileExists("MyFile")`
- Biến đổi và trả về giá trị: `InputStream fileOpen("My File")`

Còn một trường hợp nữa đó là event, có đối số đầu vào nhưng không có đầu ra. VD: `void passwordAttemptFailedNtimes(int attempts)`

Biến đổi đối số mà không trả về giá trị sẽ gây confused. VD:
- Bad: `void transform(StringBuffer out)`
- Good: `StringBuffer transform(StringBuffer in)`

Ngoài ra, không nên truyền Flag argument(boolean) vào hàm. Nếu flag trả về True thì làm một thứ, trả về False thì lại làm thứ khác. Như thế là đi ngược lại quy tắc "do 1 thing".

Nếu mà hàm nhận nhiều hơn 2 hay 3 đối số thì có thể những đối số đấy có thể được đóng gói trong một Object nào đấy.VD:
- Bad: `Circle makeCircle(double x, double y, double radius);`
- Good: `Circle makeCircle(Point center, double radius);`

### Phân tách các lệnh truy vấn
- Bad: `public boolean set(String attribute, String value);`
- Bad: `if (set("username", "unclebob"))...`
- Good: `if (attributeExists("username")) {setAttribute("username", "unclebob");...}`

### Xử lý lỗi
- Sử dụng exception (sử dụng try catch) thay vì trả về error code. 
- Quá trình xử lý lỗi là "1 thing" thế nên nó nên có hàm riêng, tách biệt khỏi các quá trình khác.

### Tóm gọn
Để viết được hàm một cách clean thì cần nhiều thời gian và kinh nghiệm. Từ một hàm phức tạp phải trả qua nhiều công đoạn như: chia nhỏ hàm, đặt lại tên, xoá trùng lặp, tối giản hoá, sắp xếp trật tự thì mới có thể phù hợp với các nguyên tắc trên.

Nghệ thuật lập trình cũng giống như nghệ thuật ngôn ngữ. Hàm có thể ví như động từ, lớp có thể ví như danh từ. Những lập trình viên đạt đến trình độ master sẽ thấy việc viết phần mềm giống như kể một câu chuyện.

