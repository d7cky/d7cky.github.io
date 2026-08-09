---
title: Hướng dẫn cấu hình proxy đi qua burpsuite cho iOS
Date: 2025-08-05
categories: [Pentest, iOS]
tags:
  - Guidelines
  - Tricks
permalink: /pentest/android/config-proxy-from-iphone-to-burpsuite/
---
# Cách 1
## Trên iPhone
1. Truy cập vào **cài đặt** ➱ **Wi-Fi** như hình bên dưới

![Screenshot](/assets/Pentest/iOS/IMG_0270.png)

2. Chọn vào chữ **i** như phần đánh dấu ở hình trên sau đó cuộn xuống phần **Configure Proxy** và chuyển qua mode **Manual** như hình dưới.

![Screenshot](/assets/Pentest/iOS/IMG_0272.png)

3. Bên trong màn hình **Configure Proxy** các bạn sẽ input những thông tin như sau:
- Server: *IP hoặc domain đến thiết bị đang sử dụng burpsuite mà bạn muốn intercept (trong trường hợp này là laptop của mình).*
- Port: *port kết nối mà bạn đã cấu hình ở phần proxy settings bên trong burpsuite.*
- Dưới đây là hình minh hoạ thông tin mình đã cấu hình proxy qua laptop của mình.

![Screenshot](/assets/Pentest/iOS/IMG_0271.png)

## Trên thiết bị chạy BurpSuite
1. Vào phần **Proxy settings** ➱ chọn **Edit** như hình bên dưới.

![Screenshot](/assets/Pentest/iOS/Screenshot%202025-08-05%20at%2010.51.14.png)

2. Ở phần này sẽ chọn option **All interfaces**.

![Screenshot](/assets/Pentest/iOS/Screenshot%202025-08-05%20at%2010.28.47.png)

3. Chọn qua tab **Request handling** và tick vào **Support invisible proxying**.

![Screenshot](/assets/Pentest/iOS/Screenshot%202025-08-05%20at%2010.51.58.png)

# Cách 2
## Trên thiết bị chạy Burpsuite
### Cài đặt **iproxy**
Các bạn phải đảm bảo có **iproxy** và **ssh**. Do mình sử dụng macOS nên **ssh** có sẵn, dưới đây mình sẽ cài đặt thêm **iproxy**. Bạn có thể truy cập vào [đây](https://command-not-found.com/iproxy) để cài đặt **iproxy** cho nền tảng phù hợp với OS bạn đang dùng.
Trên macOS mình sử dụng **homebrew** để quản lý một vài công cụ, bạn có thể cài đặt [brew](https://brew.sh/) giống mình.
Sau đó chạy lệnh dưới đây để cài đặt **iproxy** nhé.
```
brew install usbmuxd
```
**Lưu ý:** *Sau khi cài đặt xong hãy khởi động lại terminal của bạn hoặc mở một tab mới thì mới thấy được **iproxy** nhé*.
### Start **iproxy**
```
iproxy 2222 22
```
### Start **SSH**
```
ssh -R 8080:localhost:8080 root@localhost -p 2222
```
Mật khẩu mặc định của user **root** và **mobile** là `alpine`.
**Lưu ý:** *Phải đảm bảo phần config proxy trên **burpsuite** phải giống như cách 1 đã hướng dẫn*.
## Trên iPhone
Các bạn phải cài đặt **OpenSSH** để device đóng vai trò như một server ssh. Để cài được **OpenSSH** thì các bạn phải jailbreak iPhone của bạn. Các cách jailbreak máy bạn có thể xem thêm tại [đây]().
Trong trường hợp này mình dùng **Sileo** để cài đặt **OpenSSH**.
### Cài đặt **OpenSSH**
Hãy thực hiện các bước như những hình dưới đây để cài đặt.
- Bước 1: Truy cập vào ứng dụng **Sileo**.

![Screenshot](/assets/Pentest/iOS/IMG_0285.png)

- Bước 2: Search từ khoá **OpenSSH**.

![Screenshot](/assets/Pentest/iOS/IMG_0286.png)

- Bước 3: Get ứng dụng

![Screenshot](/assets/Pentest/iOS/IMG_0287.png)

- Bước 4: Queue nó luôn 😅

![Screenshot](/assets/Pentest/iOS/IMG_0288.png)

- Bước 5: Confirm để tiến hành download và cài đặt.

![Screenshot](/assets/Pentest/iOS/IMG_0289.png)

- Bước 6: Quá trình cài đặt đã thành công.

![Screenshot](/assets/Pentest/iOS/IMG_0290.png)
### Config proxy
Các bạn làm giống như phần 3 ở cách 1 là có thể intercept được rồi. Ngoài ra, nếu lười tìm ip của thiết bị đang sử dụng burp thì ở cách 2 này các bạn có thể thay địa chỉ ip ở mục **Server** thành `127.0.0.1` cũng có thể intercept được nhé.
# Download và cài đặt chứng chỉ BurpSuite cho thiết bị iPhone.
1. Trên iPhone truy cập vào đường dẫn *http://burp* và bạn sẽ thấy được giao diện như hình bên dưới.

![Screenshot](/assets/Pentest/iOS/IMG_0273.png)

2. Click chọn vào **CA Certificate** ➱ **Allow** ➱ **Close**.

![Screenshot](/assets/Pentest/iOS/IMG_0274.png)
![Screenshot](/assets/Pentest/iOS/IMG_0275.png)

3. Sau đó truy cập vào **Cài đặt** ➱ **Profile Downloaded** ➱ **Install**.

![Screenshot](/assets/Pentest/iOS/IMG_0276.png)
![Screenshot](/assets/Pentest/iOS/IMG_0277.png)
![Screenshot](/assets/Pentest/iOS/IMG_0278.png)

4. Sau khi cài đặt xong hãy quay trở lại màn hình **Settings** ➱ **General** ➱ **About** ➱ cuộn xuống cuối cùng và chọn **Certificate Trust Settings** ➱ enable **PortSwigger** ➱ **Continue**.

![Screenshot](/assets/Pentest/iOS/IMG_0279.png)
![Screenshot](/assets/Pentest/iOS/IMG_0280.png)

## Kết luận
Sau khi hoàn thành các bước trên bạn có mở một trình duyệt bất kỳ và truy cập vào một website bất kỳ nào đó rồi theo dõi ở tab **HTTP history** trên burpsuite xem đã intercept được requests thành công hay chưa.
Chúc các bạn thành công 😋😋😋