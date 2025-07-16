---
title: Làm đẹp terminal
Date: 2025-07-16
categories: Tips - Trick
tags:
---
# Mô tả
Khi đã quá chán nản với giao diện cơ bản của terminal, mình đã tìm cách trang trí làm sao để nó trông ổn hơn và tiện lợi hơn cho mình. Bài viết này có mục đích chính là sau này mình thay đổi máy mới có thể xem lại cách đã từng làm đẹp terminal của mình và tiện thể chia sẻ thêm cho các bạn nào chưa biết trang trí cho terminal.
# Cài đặt Homebrew
Homebrew là một hệ thống quản lý các gói phần mềm mã nguồn mở và miễn phí giúp đơn giản hoá việc cài đặt phầm mềm trên macOS. Trước khi cài đặt Homebrew, chúng ta cần cài đặt công cụ CLI cho Xcode.

Mở **Terminal** và chạy lệnh:
```
xcode-select —-install
```
Nếu bạn gặp lỗi, hãy chạy `xcode-select -r` để reset `xcode-select`.

![Screenshot](/assets/tips-tricks/Screenshot 2025-07-16 at 16.42.44.png)

Nếu được thông báo như hình nghĩa là `xcode-select` của bạn đã được cài đặt ⟹ tiếp tục bước tiếp theo.

Sau đó cài đặt **Homebrew**, với lệnh:
```
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```
# Cài đặt iTerm2
[iTerm2](https://iterm2.com/) là sự thay thế cho **Terminal** mặc định và là sự kế thừa cho **iTerm**. Hầu hết dân IT thích iTerm2 hơn Terminal mặc định do các [tính năng](https://iterm2.com/features.html) thú vị của nó.

Để cài đặt iTerm2, hãy chạy lệnh:
```
brew install iterm2
```
Từ bước này các bạn hãy sử dụng **iTerm2 thay cho Terminal mặc định** nhé.
# Cài đặt ZSH
**ZSH** là một shell được thiết kế để người dùng tương tác với OS, mặc dù nó cũng là một ngôn ngữ kịch bản mạnh mẽ.

Theo mặc định, macOS lưu Zsh nằm trong **/bin/zsh**. Cài đặt zsh bằng brew và sử dụng iTerm2, chạy lệnh:
```
brew install zsh
```
# Cài đặt Oh My Zsh
**Oh My Zsh** là một mã nguồn mở, hướng đến cộng đồng để quản lý cấu hình zsh của bạn. Nó chạy trên Zsh để cung cấp các tính năng thú vị có thể trực tiếp cấu hình trong tệp config ~/.zshrc

Cài đặt Oh My Zsh bằng cách chạy lệnh:
```
sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
```
Check version đã cài đặt:
```
zsh --version
```
Upgrade để có được những tính năng mới nhất:
```
upgrade_oh_my_zsh
```
Khởi động lại iTerm2 để sử dụng Zsh. Welcome to the “Oh My Zsh” world. Nhưng chưa đủ, cài thêm dependencies và extension để trông pro hơn nhé.
# Cấu hình Theme
Để có UI ngầu hơn, tự config theo sở thích cá nhân, chúng ta sẽ dùng theme **Powerlevel10k**, run lệnh:
```
git clone --depth=1 https://github.com/romkatv/powerlevel10k.git ${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}/themes/powerlevel10k
```
Mở file cấu hình **.zshrc** để sửa theme đã tải xuống, run lệnh:
```
open ~/.zshrc
```

![Screenshot](/assets/tips-tricks/Screenshot 2025-07-16 at 17.07.22.png)

Khi file cấu hình **.zshrc** đã hiển thị, bạn thêm dòng `ZSH_THEME="powerlevel10k/powerlevel10k"` vào vị trí như trên, dùng tổ hợp phím **Command + S** để save lại thay đổi. Nếu muốn huỷ config nào có thể thay tên theme hoặc thêm kí tự `#` vào đầu dòng và thêm config mới nhé.

Quay trở lại iTerm2, bạn Quit iTerm2 (Command + Q) và open lại hoặc run lệnh sau để reset lại cấu hình zsh:
```
source ~/.zshrc
```

![Screenshot](/assets/tips-tricks/Screenshot 2025-07-16 at 17.10.19.png)

Theme p10k sẽ hiển thị cấu hình cho lần đầu tiên, hoặc bạn có thể chạy lệnh sau để hiển thị cấu hình như trên
```
p10k configure
```
Làm theo hướng dẫn để dễ dàng cá nhân hoá theo sở thích của bạn. Như hình trên mình chọn **Yes** và cài đặt font **Meslo Nerd Font**.

![Screenshot](/assets/tips-tricks/Screenshot 2025-07-16 at 17.14.36.png)

Nếu nhìn thấy hình kim cương giống mình thì chọn **Yes** nhé. Còn nếu chưa thấy thì bạn hãy kiểm tra và cài lại **Powerlevel10k** hoặc font nhé.

![Screenshot](/assets/tips-tricks/Screenshot 2025-07-16 at 17.16.35.png)

Tương tự ổ khoá cũng vậy. Tiếp tục cho các lựa chọn nhận dạng như vậy.
Từ giờ bạn có thể chọn theme tuỳ vào sở thích của bạn.

# Cài đặt Plugins
Oh My ZSH được cài sẵn plugin git. Để thêm nhiều thứ khác như docker, tự động đề xuất (auto-suggestion), tô sáng cú pháp (syntax highlighting), ... bạn có thể cài đặt theo các bước sau đây:
- Cài auto-suggestion, run lệnh sau:
```
git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
```
- Cài syntax highlighting, run lệnh sau:
```
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
```
Thêm plugin vào phần plugin của tệp config **~/.zshrc** được hiển thị bên dưới

![Screenshot](/assets/tips-tricks/Screenshot 2025-07-16 at 17.37.38.png)

Save file và chạy lại lệnh `source ~/.zshrc` để apply cài đặt mới nhất.
Và đây là giao diện mới cho terminal của tôi.

![Screenshot](/assets/tips-tricks/Screenshot 2025-07-16 at 17.39.33.png)

Thế là xong rồi đó, với giao diện mới và các tính năng tiện ích, chắc chắn bạn sẽ có nhiều động lực hơn để vọc vạch bash và gõ phím như dân chuyên nghiệp rồi. See you!