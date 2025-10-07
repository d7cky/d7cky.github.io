---
title: Hướng dẫn unlock bootloader cho Google Pixel
Date: 2025-10-06
categories: Pentest Android
tags:
  - Guidelines
  - Tricks
---
# Truy cập vào bootloader.
Trên thiết bị google pixel bạn hãy tắt nguồn và sau đó nhấn, giữ tổ hợp phím **nguồn + giảm âm lượng** để truy cập vào bootloader.
# Unlock bootloader
Sau khi đã truy cập vào bootloader, các bạn hãy kết nối điện thoại với máy tính sau đó chạy lệnh sau trên máy tính của bạn:
```
fastboot fashing unlock
```
Sau đó quay lại điện thoại và chọn **Unlock the bootloader**.
Nếu bạn nhận được kết quả như hình dưới đây thì bạn đã unlock bootloader thành công.

![Screenshot](/assets/Pentest/Android/IMG_7099.jpeg)