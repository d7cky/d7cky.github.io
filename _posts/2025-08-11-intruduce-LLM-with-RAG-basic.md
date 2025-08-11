---
title: Hướng dẫn cấu hình workflow LLM with RAG basic
Date: 2025-08-11
categories: Articial-Intelligence
tags:
  - Guidelines
  - Tricks
  - AI
---
# Description
Đây là bài hướng dẫn sử dụng project [LLM with RAG basic](https://github.com/d7cky/LLM-with-RAG-basic) của mình. Để việc chọn LLM model trở nên linh động nên trong project mình không sử dụng bất kỳ image LLM nào chạy trên **Docker**. Vì vậy nếu muốn sử dụng project các bạn có thể chọn LLM model tuỳ thích, có thể là local (LLM studio, Ollama,....) hoặc cloud (chatGPT, Claude, Grok, Copilot,....). Ngoài ra các bạn nhớ cài đặt **Docker** để có thể sử dụng project nhé.
# Installation
## Docker
Các bạn có thể tải và cài đặt [Docker tại đây](https://www.docker.com/products/docker-desktop/).
## Ollama
Mình chọn **Ollama** bởi vì nó dễ cài đặt và dễ sử dụng. Các bạn có thể cài đặt nó tại [đây](https://ollama.com/download) có đầy đủ cho các OS tương ứng.
## LLM with RAG basic
```
git clone https://github.com/d7cky/LLM-with-RAG-basic.git
cd LLM-with-RAG-basic
docker compose up
```
# Usage
## Config
Hãy sử dụng bất kỳ trình soạn thảo văn bản nào mà bạn quen thuộc và open project với nó. Mình sẽ sử dụng **vscode** vì nó tiện 😅.
### Rename file
Rename file **.env.example** ➝ **.env** và **ngrok.yml.example** ➝ **ngrok.yml**.
### Edit file ngrok.yml
Truy cập vào file **ngrok.yml** và thay đổi **authtoken**, **url** tương ứng với tài khoản ngrok của bạn. Bạn có thể lấy nó ở [đây](https://dashboard.ngrok.com/get-started/setup).
### Edit file .env
Tìm tham số **N8N_URL** và thay đổi giá trị của nó bằng URL của ngrok lúc nãy (nhớ thêm tiền tố https:// vào đầu)
Các tham số **POSTGRES_PASSWORD**, **SERVICE_ROLE_KEY**, **DASHBOARD_USERNAME**, **DASHBOARD_PASSWORD** dùng để login vào dashboard supabase và cấu hình postgres.
### Docker compose
Sau khi cấu hình xong xuôi, mình sẽ tiến hành chạy lệnh `docker compose up` để pull image cũng như tạo container.

![Screenshot](/assets/articial-intelligence/Screenshot 2025-08-11 at 11.22.47.png)
### Supabase
Sau khi các dịch vụ ở docker được run ổn định các bạn có thể truy cập vào [supabase](http://localhost:8000) để cấu hình.

![Screenshot](/assets/articial-intelligence/Screenshot 2025-08-11 at 14.49.45.png)

Thông tin login các bạn có thể xem ở file `.env` đó là 2 tham số **DASHBOARD_USERNAME** và **DASHBOARD_PASSWORD**.
Sau khi login mình sẽ tạo một table để làm nhiệm vụ lưu trữ các vector mà chúng ta sẽ embedding từ workflow.
**SQL Editor** ➠ **Quickstarts** ➠ **LangChain** ➠ **Run** (trong trường hợp bạn đã enable extension **vector** rồi thì khi **run** sẽ gặp lỗi, khi đó bạn nên xoá dòng `create extension vector`).

![Screenshot](/assets/articial-intelligence/Screenshot 2025-08-11 at 14.56.25.png)

☞ Kết quả hiện thị như hình trên thì **Run** thành công. Sau đó bạn có thể vào **Table Editor** để xem lại table **documents** đã được tạo ra hay chưa.

### N8N
Đi theo đường dẫn của tham số **N8N_URL** bên trong file `.env` để truy cập vào n8n.
Sau khi thực hiện đăng ký tài khoản hãy tạo một workflow sau đó import file workflow `RAG.json` bên trong thư mục `n8n/backup/workflows/` của dự án vào.
Mình sẽ hướng dẫn các bạn cấu hình node **postgres** và **supabase** nhé, các node còn lại các bạn có thể tuỳ chỉnh theo ý thích.
#### Postgres Node
Truy cập vào node **postgres** và khởi tạo một credential mới.
Sau đó các bạn config như hình bên dưới:

![Screenshot](/assets/articial-intelligence/Screenshot 2025-08-11 at 15.08.25.png)

Phần `host`, `database`, `port`, `password` các bạn có thể xem ở file `.env`. Phần `user` bạn có thể xem ở phần connect bên **supabase**.
#### Supabase Node
Tương tự **postgres** tạo một credential mới. Và điền thông tin như hình.

![Screenshot](/assets/articial-intelligence/Screenshot 2025-08-11 at 15.12.44.png)

Phần `host` các bạn có thể lấy phần domain từ tham số **addr** ở file `ngrok.yml` (phải thay đổi thành port 8000 như trên hình nữa nhé). Còn **Service Role Secret** thì các bạn lấy ở file `.env` trong tham số **SERVICE_ROLE_KEY**.
# Kết luận
Các node còn lại như *Google Drive* các bạn có thể nghiên cứu thêm ở **Google** hoặc **Youtube**.
Còn node `Ollama` các bạn có thể thay thế bằng các LLM khác hoặc dùng **Ollama** giống mình luôn kkkkk
Chúc các bạn có một trãi nghiệm tốt.