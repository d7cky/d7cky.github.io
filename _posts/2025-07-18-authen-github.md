---
title: Hướng dẫn Authentication với Github khi làm việc trên thiết bị mới
Date: 2025-07-18
categories: Guidelines
tags:
  - Guidelines
  - Tricks
---
# Mô tả
Chuyện là mình mới mua một thiết bị mới và clone project trên git của mình về làm việc cơ mà lại bị dính Authentication, thế là loay hoay cả buổi vẫn không tìm được cách fix. Nhờ chatGPT mà mình cũng fix được, vì vậy mình viết bài này mục đích chính là để lưu lại cách Authen trên thiết bị mới đối với git để sau này tìm lại cho nhanh. Bạn nào thấy phù hợp có thể làm theo giống mình nhé.
# Kiểm tra đã có sẵn key SSH trên máy chưa
```
ls -la ~/.ssh
```
Nếu có rồi bạn có thể bỏ qua bước **tạo cặp key SSH**
# Tạo cặp key SSH
```
ssh-keygen -t rsa -b 4096 -C "youremail@gexample.com"
```
Cặp key sẽ được sinh ra trong thư mục *~/.ssh/*.
# Cấu hình Github
Bạn truy cập vào Github của mình. Vào phần **Setings** ➟ **SSH and GPG keys** ➟ **New SSH key** ➟ **Paste nội dung public key (file id_rsa.pub) vào input key**

Sau đó bạn quay lại repository mà bạn muốn Authen. Ở phần clone code thay vì chọn *HTTPS* thì bạn hãy chọn *SSH* và copy đường dẫn paste vào lệnh sau:
```
git remote set-url origin git@github.com:username/myrepository
```
Bạn chạy lệnh này trên cửa sổ dòng lệnh.

# Kết luận
Sau khi setup xong bạn có thể `clone`, `commit`, `push` code một cách bình thường mà không gặp lỗi Authentication fail nữa. Chúc các bạn thành công.