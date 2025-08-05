---
title: Hướng dẫn xây dựng Burp extension cơ bản
Date: 2025-07-18
categories: Guidelines
tags:
  - Guidelines
  - Tricks
---
# Mô tả
Vì nhu cầu sử dụng muốn sử dụng Montoya API của Burp nên buộc mình phải dev và build Burp extension với *Java* thay vì *Python* trước đó mình đã từng build. Bởi vì đụng tới *Java* nên mình cảm thấy mọi thứ trở nên phức tạp, mình viết bài này mục tiêu chính chỉ để sau này cần thì xem lại cho nhanh và cũng giúp bạn nào chưa biết cách tạo ra Burp extension bằng *Java (Maven)* có thể tiếp cận nó một cách đơn giản nhất.
# Download và cài đặt IntelliJ IDEA
Các bạn truy cập vào [trang chủ IntelliJ](https://www.jetbrains.com/idea/download/?section=mac) để download phần mềm. Nếu chỉ là Burp extension thì mình nghĩ dùng bảng community là đã đáp ứng được và còn free nữa 😅

Chú ý: Các bạn kéo xuống và chọn đúng cấu hình cần download như hình dưới nhé, nhớ là kéo xuống nhìn kỹ, lúc đầu mình cũng nhầm nó là footer của web nên không tìm được bảng Community á kkkkkk.

![Screenshot](/assets/tips-tricks/Screenshot 2025-07-18 at 15.05.01.png)
# Tạo project Maven
Sau khi cài đặt xong chúng ta sẽ tạo project, nhớ là chọn **Maven** ở mục *Build system* nhé.

![Screenshot](/assets/tips-tricks/Screenshot 2025-07-18 at 15.09.37.png)

![Screenshot](/assets/tips-tricks/Screenshot 2025-07-18 at 15.12.08.png)

Sau khi tạo xong project **Maven** chúng ta sẽ có cấu trúc thư mục như hình trên.
## Cấu hình thư viện Burp
Ở file **pom.xml** mình sẽ thêm đoạn code dưới đây để IntelliJ tải về các thư viện của Burp Motoya.
```
<!-- Burp Suite Extender API Dependency -->  
<dependency>  
    <groupId>net.portswigger.burp.extensions</groupId>  
    <artifactId>montoya-api</artifactId>  
    <version>2025.7</version>  
</dependency>
```
**Chú ý:** Tìm cách load lại đến khi nào file *pom.xml* không còn báo lỗi đỏ nữa thì thư viện mới tải thành công, có thể kiểm tra trong phần *External Libraries*.
