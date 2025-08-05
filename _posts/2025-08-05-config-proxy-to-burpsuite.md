---
title: Hướng dẫn cấu hình proxy đi qua burpsuite
Date: 2025-08-05
categories: Pentest iOS
tags:
  - Guidelines
  - Tricks
---
# Trên iPhone
1. Truy cập vào **cài đặt** ➱ **Wi-Fi** như hình bên dưới

![Screenshot](/assets/Pentest/iOS/IMG_0270.png)

2. Chọn vào chữ **i** như phần đánh dấu ở hình trên sau đó cuộn xuống phần **Configure Proxy** và chuyển qua mode **Manual** như hình dưới.

![Screenshot](/assets/Pentest/iOS/IMG_0272.png)

3. Bên trong màn hình **Configure Proxy** các bạn sẽ input những thông tin như sau:
- Server: *IP hoặc domain đến thiết bị đang sử dụng burpsuite mà bạn muốn intercept (trong trường hợp này là laptop của mình).*
- Port: *port kết nối mà bạn đã cấu hình ở phần proxy settings bên trong burpsuite.*
- Dưới đây là hình minh hoạ thông tin mình đã cấu hình proxy qua laptop của mình.

![Screenshot](/assets/Pentest/iOS/IMG_0271.png)

# Trên BurpSuite
1. Vào phần **Proxy settings** ➱ chọn **Edit** như hình bên dưới.

![Screenshot](/assets/Pentest/iOS/Screenshot 2025-08-05 at 10.51.14.png)

2. Ở phần này sẽ chọn option **All interfaces**.

![Screenshot](/assets/Pentest/iOS/Screenshot 2025-08-05 at 10.28.47.png)

3. Chọn qua tab **Request handling** và tick vào **Support invisible proxying**.

![Screenshot](/assets/Pentest/iOS/Screenshot 2025-08-05 at 10.51.58.png)

# Download và cài đặt chứng chỉ BurpSuite cho thiết bị iPhone.
1. Trên iPhone truy cập vào đường dẫn *http://burp và bạn sẽ thấy được giao diện như hình bên dưới.

![Screenshot](/assets/Pentest/iOS/IMG_0273.png)

2. Click chọn vào **CA Certificate** ➱ **Allow** ➱ **Close.

![Screenshot](/assets/Pentest/iOS/IMG_0274.png)
![Screenshot](/assets/Pentest/iOS/IMG_0275.png)

3. Sau đó truy cập vào **Cài đặt** ➱ **Profile Downloaded** ➱ **Install.

![Screenshot](/assets/Pentest/iOS/IMG_0276.png)
![Screenshot](/assets/Pentest/iOS/IMG_0277.png)
![Screenshot](/assets/Pentest/iOS/IMG_0278.png)

4. Sau khi cài đặt xong hãy quay trở lại màn hình **Settings** ➱ **General** ➱ **About** ➱ cuộn xuống cuối cùng và chọn **Certificate Trust Settings** ➱ enable **PortSwigger** ➱ **Continue**.

![Screenshot](/assets/Pentest/iOS/IMG_0279.png)
![Screenshot](/assets/Pentest/iOS/IMG_0280.png)

# Kết luận
Sau khi hoàn thành các bước trên bạn có mở một trình duyệt bất kỳ và truy cập vào một website bất kỳ nào đó rồi theo dõi ở tab **HTTP history** trên burpsuite xem đã intercept được requests thành công hay chưa.
Chúc các bạn thành công 😋😋😋