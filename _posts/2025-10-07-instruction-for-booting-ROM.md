---
title: Hướng dẫn boot ROM
Date: 2025-10-07
categories: [Pentest, Android]
tags:
  - Guidelines
  - Tricks
permalink: /pentest/android/instruction-for-booting-ROM/
---
# Download ROM
Trong bài viết này mình sẽ boot ROM Evolution-X nên mình sẽ tải ROM này [tại đây.](https://evolution-x.org/).
# Boot ROM
Để boot được ROM chúng ta cần chuẩn bị những thứ sau:
1. ROM để boot (đã được tải ở trên).
2. Đảm bảo bootloader của thiết bị được unlock (tham khảo [bài viết sau](https://d7cky.github.io/pentest/android/instruction-unlock-bootloader-for-Pixel/) để unlock bootloader cho các dòng pixel).
3. Bộ công cụ SDK của android (bạn có thể tải [tại đây](https://developer.android.com/tools/releases/platform-tools)).
Sau khi mọi thứ đã sẵn sàng chúng ta sẽ thực hiện tuần tự các bước sau để boot ROM nhé.
- Đảm bảo thiết bị di động đang ở trong bootloader.
- Tiến hành kết nối điện thoại vào máy tính và chạy tuần tự các lệnh sau ở trên máy tính:
```
fastboot flash boot boot.img
fastboot flash dtbo dtbo.img
fastboot flash vendor_kernel_boot vendor_kernel_boot.img
fastboot flash vendor_boot vendor_boot.img
fastboot reboot recovery
```

![Screenshot](/assets/Pentest/Android/Screenshot 2025-10-07 at 23.57.45.png)

- Sau khi máy đã vào được **mode recovery** thì hãy chọn **Factory Reset** ⟹ **Format Data/Factory Reset** ⟹ **Format data**.
- Sau đó trở về lại menu chính của **mode recovery** và chọn **Apply Update** ⟹ **Apply from ADB**.
- Quay lại máy tính và chạy lệnh dưới đây:
```
adb sideload EvolutionX-16.0-20250725-panther-11.0-Official.zip
```
*Lưu ý: File zip là file ROM tương ứng của các bạn nhé.*

![Screenshot](/assets/Pentest/Android/Screenshot 2025-10-08 at 00.01.05.png)

- Mặc dù trên máy tính chưa chạy đủ 100% nhưng trên điện thoại các bạn có thể bấm được các nút để điều khiển con trỏ thì có thể là đã boot xong rồi nhé.
- Khúc này trên điện thoại google pixel 7 của mình sẽ có 2 lựa chọn **Yes** và **No** 
	- Nếu chọn **Yes** nghĩa là bạn đồng ý cài thêm các gói hỗ trợ.
	- Nếu chọn **No** nghĩa là bạn không đồng ý cài thêm các gói hỗ trợ.
	⟹ Ở đây mình sẽ chọn **No** nhé.
- Sau đó máy sẽ vào lại **mode recovery** bạn sẽ chọn **Reboot system now** luôn nhé.
- Bạn chờ cho máy khởi động và vào ROM, sau đó hãy cài đặt máy và thưởng thức ROM mới nhé.
# Kết luận
Chúc các bạn thành công 😋😋😋
