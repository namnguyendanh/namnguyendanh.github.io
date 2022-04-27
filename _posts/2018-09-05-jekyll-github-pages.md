---
title:  "Tutorial: Jekyll and Github Pages"
categories: 
    - tool
header:
    image: /images/2018-09-05-jekyll-github-pages/0.png
---
<!-- # Jekyll và Github Pages -->

## Jekyll là gì ?
Jekyll là một công cụ đơn giản và nhanh chóng để chuyển plain text thành trang web tĩnh hay blog.

**Đầu vào**: Mardown, Liquid, HTML&CSS

**Đầu ra**: trang web tĩnh

Ở blog này, mình sẽ tập trung vào việc xây dựng web tĩnh đơn giản bằng Jekyll và Markdown và đẩy trang web tĩnh lên github pages. Hệ điều hành mình sử dụng là Ubuntu 18.04 nên quá trình cài đặt có thể khác chút ở HĐH khác.

Kiến thức nên biết trước: Markdown, Git, Linux


## Hướng dẫn cài đặt Jekyll

Jekyll được xây dựng dựa trên Ruby, nó là một [Ruby Gem](https://jekyllrb.com/docs/ruby-101/#gems). Vì thế, bạn cân cài đặt Ruby Development Environment để Jekyll hoạt động.

Mở terminal và viết các dòng lệnh sau:
### 1. Cài đặt Ruby development environment
```
sudo apt-get install ruby ruby-dev build-essential

echo '# Install Ruby Gems to ~/gems' >> ~/.bashrc
echo 'export GEM_HOME=$HOME/gems' >> ~/.bashrc 
echo 'export PATH=$HOME/gems/bin:$PATH' >> ~/.bashrc
source ~/.bashrc
```

### 2. Cài đặt Jekyll và bundler gems
```
gem install jekyll bundler

```

## Xây dựng trang web


### Tạo một trang web mới 
```
jekyll new ten-blog-cua-ban
```

Vào thư mục `ten-blog-cua-ban` và chạy lệnh: 
```
bundle exec jekyll serve
```

Giờ bạn có vào: [http://localhost:4000/](http://localhost:4000/) để xem trang web của mình

Nếu chỉ build trang web thôi (ko chạy local server, sau này bạn sẽ sử dụng lệnh này thường xuyên): 
```
jekyll build
```

Ok, giờ mình sẽ để ý đến thư mục `ten-blog-cua-ban`.

Có 2 thư mục quan trọng:
- `_site`: jekyll sẽ build mọi thứ vào trong folder này. Để ý dòng destination khi chạy lệnh `jekyll serve`

- `_posts`: những bài viết của bạn sẽ nằm ở đây. Sau khi thiết lập xong trang web thì hầu như bạn sẽ chỉ làm việc với folder này là chính. 

Ngoài ra còn có các thư mục có thể sẽ có:
- `_includes`: chứa những extensions cho trang web. VD: MathJax - viết các công thức toán với Markdown
- `assets`: chứa các file tài nguyên như ảnh, video, js, css,...

### Xây dụng About page
Mở file `about.md` lên:
![][3]

#### Front Matter
Đoạn nằm giữa hai dòng `---` được gọi là Front Matter

**Front Matter**: đoạn mã YAML dùng để đặt giá trị của biến trong trang đó. Ví dụ: `title` là tên biến và giá trị của nó là `About`.

Bạn có thể thay đổi giá trị của biến `title` và nội dụng bên dưới (viết bằng Markdown) rồi build lại để thấy được sự thay đổi.

Những biến này có thể được gọi thông qua [**Liquid**](https://jekyllrb.com/docs/step-by-step/02-liquid/). Ví dụ: biến `title` sẽ được gọi thông qua đối tượng `{{ page.title }}`. 

#### Layout
**Layout**: là bản bố trí trang web được xây dựng để tránh lặp code (nhiều trang trong website có cách bố trí giống nhau). 

Các file layout(.html) sẽ nằm trong thư mục `_layouts`. 

Ví dụ về `./_layouts/default.html`:

![][5]

Liquid được sử dụng chủ yếu trong các layout. Bạn không cần quá bận tâm về các layout này, trên mạng có nhiều [Jekyll theme](https://pages.github.com/themes/) chứa sẵn các file layout rồi. 

### Viết một post mới
Tạo một file .md trong folder `_posts` với định dạng sau: `year-month-day-posttitle.md`. VD: 2018-09-04-welcome-to-jekyll.markdown  

#### Tạo Front Matter
VD:
```yaml
---
layout: post
title:  "Welcome to Jekyll!"
date:   2018-09-04 06:39:10 +0700
categories: jekyll update
---
```

với catergories thì càng về bên phải sẽ các subcategories.

Phần dưới Front Matter chính là phần nội dụng để bạn viết. Đến bước này thì bạn đã có một trang web đơn giản hoản chỉnh. Việc tiếp theo là đưa nó lên internet.

## Github Pages
Github Pages là một dịch vụ hosting các trang web tĩnh. Nó hỗ trợ Jekyll rất tốt, ngoài ra mình có thể dùng git để quản lý trang web.

### 1. Thiết lập một Repository trên Github

Nếu là trang web cá nhân bạn tạo repo có tên theo đinh dạng `username github.io`. VD: minhdq99hp.github.io
Nếu là trang web project thì đường link dẫn đến trang web sẽ có dạng `username.github.io/project-name`. VD: minhdq99hp.github.io/my-project

### 2. Đẩy trang web lên Github 
Sau khi đã có repo, giả sử của mình là repo `myblog` bạn clone nó về máy:

```
git clone https://github.com/minhdq99hp/myblog.git
```
Chuyển toàn bộ file trong project `ten-blog-cua-ban` vào `myblog`:

```
mv ten-blog-cua-ban/* myblog/
```
Commit lên Github:

```
git add --all
git commit -m "add new files
git push origin
```

### 3. Thiết lập Github Pages
Bạn vào Settings của repo và thiết lập như dưới đây:
![][4]

Ok, giờ chỉ cần chờ một lúc là trang web của bạn đã có thể truy cập được :D

## Thảo luận 
Jekyll là một công cụ giúp bạn viết blog rất nhanh chóng và mạnh mẽ. Trang [machinelearningcoban.com](www.machinelearningcoban.com) có sử dụng Jekyll kết hợp một số các extensions khác (VD: MathJax, Disqus). Việc của bạn chỉ là chọn một Theme và các extension phù hợp nhu cầu. Trang web của mình sử dụng theme Lanyon-plus, mình dùng Sublime Text để viết.

Ngoài ra, cũng có thể sử dụng Jupyter Notebook (quen thuộc với các bạn team DS) để viết blog do có hỗ trợ xuất ra file .markdown. Có lẽ mình sẽ viết tutorial về cách kết hợp Jekyll với Jupyter Notebook và một số công cụ khác ở blog sau.


## Tài liệu tham khảo
- [Jekyll docs](https://jekyllrb.com/docs/)
- [Markdown Cheatsheet](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet)
- [Blog và các bài viết được tạo như thế nào - Vũ Hữu Tiệp](https://machinelearningcoban.com/2017/02/02/howdoIcreatethisblog/)

[3]: /images/2018-09-05-jekyll-github-pages/3.png
[4]: /images/2018-09-05-jekyll-github-pages/4.png
[5]: /images/2018-09-05-jekyll-github-pages/5.png
