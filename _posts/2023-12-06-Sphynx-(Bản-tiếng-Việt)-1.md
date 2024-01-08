---
layout: post
title: "BlackCat Sphynx: Mối đe dọa mới trong bối cảnh an ninh mạng"
date: 2023-12-06 07:01:24 +0700
categories: malware APTs
---
# BlackCat Sphynx: Mối đe dọa mới trong bối cảnh an ninh mạng

Thế giới tội phạm mạng không ngừng phát triển và đặt ra những thách thức mới cho các chuyên gia và tổ chức bảo mật. Một trong những mối đe dọa mới nhất là họ ransomware có tên Sphynx, được phát triển và phân phối bởi một nhóm tin tặc khét tiếng trong giới bảo mật có tên BlackCat. Trong bài đăng trên blog này, chúng tôi sẽ cung cấp thông tin tổng quan về BlackCat và Sphynx, đồng thời thảo luận về các tính năng, kỹ thuật và tác động của chúng.

## BlackCat là ai?

BlackCat, còn được gọi là ALPHV và Noberus, là một nhóm ransomware-as-a-service (RaaS) xuất hiện vào tháng 11 năm 2021. RaaS là một mô hình kinh doanh trong đó các nhà phát triển ransomware cung cấp phần mềm độc hại của họ để các chi nhánh, những người chịu trách nhiệm lây nhiễm cho nạn nhân, sử dụng và thu tiền chuộc. Các nhà phát triển sau đó sẽ lấy phần trăm lợi nhuận. Theo FBI, nhiều nhà phát triển và kẻ rửa tiền cho BlackCat có liên quan đến DarkSide và BlackMatter, hai nhóm RaaS không còn tồn tại đứng sau một số vụ tấn công ransomware khét tiếng nhất trong lịch sử, chẳng hạn như sự cố Colonial Pipeline và JBS.

BlackCat hoạt động theo chiến thuật tống tiền gấp đôi hoặc gấp ba, có nghĩa là chúng không chỉ mã hóa dữ liệu của nạn nhân mà còn lấy cắp dữ liệu đó và đe dọa xuất bản dữ liệu đó lên trang web rò rỉ dữ liệu công khai của chúng hoặc khởi động các cuộc tấn công từ chối dịch vụ phân tán (DDoS) trên cơ sở hạ tầng của nạn nhân, nếu tiền chuộc không được trả.

BlackCat đã nhắm mục tiêu vào hàng trăm tổ chức trên toàn thế giới, bao gồm các trường đại học, cơ quan chính phủ và các công ty trong nhiều lĩnh vực khác nhau như năng lượng, công nghệ, sản xuất và vận tải. Một số nạn nhân nổi tiếng của họ bao gồm Reddit, Fidelity National Financial và Mr. Cooper. [BlackCat thường yêu cầu thanh toán tiền chuộc vài triệu đô la bằng Bitcoin hoặc Monero và được biết là thương lượng hoặc hạ thấp yêu cầu ban đầu của họ](https://en.wikipedia.org/wiki/BlackCat_%28cyber_gang%29)
## Sphynx là gì?

Sphynx là tên của họ ransomware mà BlackCat sử dụng để mã hóa và tống tiền nạn nhân của chúng. Sphynx được viết bằng Rust, một ngôn ngữ lập trình tương đối phổ biến để phát triển phần mềm độc hại nhưng cung cấp một số ưu điểm, chẳng hạn như an toàn bộ nhớ, hiệu suất và khả năng tương thích đa nền tảng. Sphynx mã hóa các tệp của nạn nhân bằng thuật toán AES-256 mạnh và gắn thêm phần mở rộng ngẫu nhiên vào chúng. [Sphynx cũng đưa ra một thông báo đòi tiền chuộc có tên chẳng hạn như HOW_TO_RECOVER_FILES.txt, hướng dẫn nạn nhân liên hệ với những kẻ tấn công qua email hoặc Tor để nhận khóa giải mã và hướng dẫn thanh toán](https://finance.yahoo.com/news/alphv-blackcat-gang-allegedly-behind-183954187.html)

Sphynx dựa vào nhiều kỹ thuật khác nhau để xâm nhập và lây lan trên mạng của nạn nhân, chẳng hạn như khai thác VPN và tường lửa lỗi thời và chưa được vá, sử dụng thông tin đăng nhập bị đánh cắp thu được từ các nhà môi giới truy cập ban đầu hoặc tận dụng các công cụ kiểm tra thâm nhập hợp pháp, chẳng hạn như Cobalt Strike hoặc BloodHound, để thực hiện các hoạt động di chuyển ngang và leo thang đặc quyền. [Sphynx cũng cố gắng tránh bị phát hiện và phân tích bằng cách sử dụng các kỹ thuật che giấu, mã hóa, chống gỡ lỗi và chống VM, cũng như xóa các bản sao ẩn và vô hiệu hóa phần mềm bảo mật](https://www.zdnet.com/article/fbi-this-ransomware-written-in-the-rust-programming-language-has-hit-at-least-60-targets/)
## Tại sao Sphynx là mối đe dọa nghiêm trọng?

Sphynx là một mối đe dọa nghiêm trọng vì nhiều lý do. Đầu tiên, nó được hỗ trợ bởi một nhóm tin tặc mạng tinh vi và giàu kinh nghiệm, có quyền truy cập vào một mạng lưới chi nhánh và tài nguyên rộng lớn. Thứ hai, nó sử dụng ngôn ngữ lập trình mạnh mẽ và linh hoạt cho phép nó nhắm mục tiêu vào nhiều nền tảng và hệ thống. Thứ ba, nó sử dụng sự kết hợp của các cuộc tấn công mã hóa, trích xuất dữ liệu và DDoS để tối đa hóa thiệt hại và áp lực lên nạn nhân. Thứ tư, nó khai thác các lỗ hổng và công cụ khác nhau để có được quyền truy cập ban đầu và khả năng tồn tại (persistence) lâu dài trên mạng của nạn nhân. Thứ năm, nó thực hiện nhiều cơ chế chống phát hiện và chống phân tích khác nhau để trốn tránh các giải pháp bảo mật và các nhà nghiên cứu.

--------------------

## Identification

|   |   |
|---|---|
|Size|32.97 MB|
|SHA256|40278f86caa9ceb72a5e5d272b28cbafa01865ecbaec3a8eead11093f1237714|
|MD5|5b39661efe8a8f49623b3088d88acdc4|
|File Type|Win32 EXE|
|SHA1|209f8748ff00c994968ba9748428f7e165c44383|
|Ssdeep|786432:idxIB6uWeTrTms1darsJYY4L80yfEt7qY04wjejBsnktO:idxIB6yTusjarrnlyfEqoBsniO|
|VirusTotal|[Report (41 / 72 Detections)](https://www.virustotal.com/gui/file/40278f86caa9ceb72a5e5d272b28cbafa01865ecbaec3a8eead11093f1237714)|
|Target Machine|Intel 386 or later, and compatibles|
|Compilation Timestamp|21 Nov 2023 06:59:48|

Trước khi đi vào phân tích chi tiết, tôi sẽ sử dụng công cụ DIE để phân tích sơ bộ và xem xét entropy của mẫu mã độc mà chúng ta sẽ phân tích.

![Pasted image 20231203150107](https://github.com/tsof-smoky/writeup-CTF-challenge/assets/107832241/d5df05e0-e7fa-4774-900b-2be3f1b508c4)

Metadata of sample

![Pasted image 20231203150238](https://github.com/tsof-smoky/writeup-CTF-challenge/assets/107832241/d02199ce-6aa2-4b08-881e-5a6c78740d4e)
Entropy of sample

![Pasted image 20231203152103](https://github.com/tsof-smoky/writeup-CTF-challenge/assets/107832241/64743eb3-ecca-4502-a64e-54148f51b060)

Sphynx được viết bằng Rust, một ngôn ngữ lập trình khá phổ biến để phát triển phần mềm độc hại và mang lại một số lợi ích về An toàn bộ nhớ, Hiệu suất và Khả năng tương thích đa nền tảng.

![Pasted image 20231203205325](https://github.com/tsof-smoky/writeup-CTF-challenge/assets/107832241/8903e687-f23d-45a3-a566-b8178cfb51d9)

`GetCommandLineW` là một hàm API của Windows trả về chuỗi dòng lệnh cho tiến trình hiện tại. Nó có thể được sử dụng bởi các ứng dụng console và GUI để truy cập các đối số dòng lệnh được truyền cho các hàm main hoặc wmain. Sphynx sử dụng `GetCommandLineW` để trực tiếp nhận các lệnh do tin tặc thực thi.

![Pasted image 20231203210527](https://github.com/tsof-smoky/writeup-CTF-challenge/assets/107832241/86a31cea-9981-46c7-ad3a-975e17e25cf1)

Bằng cách sử dụng hàm `Sleep`, mã độc có thể trì hoãn việc thực thi và khiến trình gỡ lỗi mất nhiều thời gian hơn để xử lý nó. Đây là một kỹ thuật cản trở kỹ thuật đảo ngược bằng cách làm cho quá trình tháo gỡ trở nên khó khăn hơn.

![Pasted image 20231203214721](https://github.com/tsof-smoky/writeup-CTF-challenge/assets/107832241/fef02489-90ba-4cff-81da-ed32271ad2dc)

`sub_784d50` là một hàm quan trọng. Trong quá trình phân tích mẫu phần mềm độc hại, tôi gặp phải rất nhiều trường hợp nó xuất hiện. Ở đây, nó đóng vai trò chính trong việc nhận được đường dẫn của các tài nguyên mà Sphynx sẽ mã hóa, sao chép, xóa.

![Pasted image 20231203212342](https://github.com/tsof-smoky/writeup-CTF-challenge/assets/107832241/2de02a48-ecb0-49d4-ab14-9f8f9850a098)

Kẻ tấn công sử dụng `CoCreateGuid` để tạo GUID ngẫu nhiên và tạo registry key có tên đó trong subkey `HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Image File Execution Options` (IFEO). Subkey này cho phép hệ thống thực thi chương trình gỡ lỗi trước khi khởi chạy chương trình đích. Sau đó, kẻ tấn công có thể đặt giá trị của mục Trình gỡ lỗi trong subkey mới được tạo để trỏ đến một tệp thực thi độc hại sẽ chạy với cùng đặc quyền như chương trình đích.

![Pasted image 20231206102309](https://github.com/tsof-smoky/writeup-CTF-challenge/assets/107832241/fc9a4fed-86c2-4b8d-ab22-4a056e27123a)

Sau đó, kẻ tấn công có thể đợi chương trình mục tiêu được khởi chạy bởi một người dùng có đặc quyền, chẳng hạn như quản trị viên hoặc dịch vụ hệ thống. Khi chương trình mục tiêu được khởi chạy, trước tiên hệ thống sẽ kiểm tra subkey IFEO để tìm registry key phù hợp có cùng tên với chương trình mục tiêu. Nếu tìm thấy, nó sẽ thực thi chương trình gỡ lỗi được chỉ định bởi mục Trình gỡ lỗi thay vì chương trình đích. Bằng cách này, kẻ tấn công có thể chạy tệp thực thi độc hại với các đặc quyền nâng cao.

Sphynx sử dụng `CoCreateGuid` để tạo GUID ngẫu nhiên mà bất kỳ chương trình hợp pháp nào khó có thể sử dụng. Sau đó, kẻ tấn công có thể tạo registry key có tên đó trong subkey IFEO và đặt mục nhập Trình gỡ lỗi để trỏ đến tệp thực thi độc hại.

Bằng cách này, kẻ tấn công có thể tạo registry key tạm thời, registry key chỉ tồn tại trong một khoảng thời gian ngắn, trong thời gian đó chương trình mục tiêu được khởi chạy và tệp thực thi độc hại được thực thi. Registry key sau đó sẽ tự động bị xóa, không để lại dấu vết của cuộc tấn công. 

![Pasted image 20231203212944](https://github.com/tsof-smoky/writeup-CTF-challenge/assets/107832241/194213a2-2ab2-4ece-bae3-4bd8f1395088)

![Pasted image 20231203213206](https://github.com/tsof-smoky/writeup-CTF-challenge/assets/107832241/4ca44aec-e7e5-44b8-8b13-52b5d89d3189)

`CreateNamedPipeW` được sử dụng để tạo kênh liên lạc duy nhất giữa phần mềm độc hại và máy chủ từ xa. `CreateNamedPipeW` cũng cho phép phần mềm độc hại thiết lập các thuộc tính bảo mật của đường ống, chẳng hạn như cấp quyền truy cập đầy đủ cho người tạo và từ chối quyền truy cập của người khác. Bằng cách này, phần mềm độc hại có thể ngăn chặn truy cập trái phép hoặc giả mạo đường ống.

![Pasted image 20231203213951](https://github.com/tsof-smoky/writeup-CTF-challenge/assets/107832241/d262a38f-18e1-4b13-b2c5-c75e81901280)

![Pasted image 20231203214000](https://github.com/tsof-smoky/writeup-CTF-challenge/assets/107832241/7716e1cc-0774-4983-8b31-872681fc6783)

Bằng cách sử dụng `BCryptGenRandom`, phần mềm tống tiền tính toán một khóa `AES` ngẫu nhiên. Hàm `BCryptGenRandom` được quan sát thấy được gọi nhiều lần cho quá trình mã hóa.

![Pasted image 20231205005310](https://github.com/tsof-smoky/writeup-CTF-challenge/assets/107832241/30851d49-0511-41db-b92b-ae9c1fc13da5)

![Pasted image 20231205005357](https://github.com/tsof-smoky/writeup-CTF-challenge/assets/107832241/701f47f5-392b-4904-8b04-4eaaa6a532a0)

Để tìm tất cả các tệp trên hệ thống, trước tiên BlackCat sử dụng vòng lặp các chức năng `FindFirstFileW` và `FindNextFileW` để quét hệ thống.

![Pasted image 20231203214237](https://github.com/tsof-smoky/writeup-CTF-challenge/assets/107832241/28a96a32-c3dd-4867-a3ca-017fb4c7f865)

`CopyFileExW` được sử dụng để tạo bản sao của tệp gốc trước khi mã hóa và xóa sạch nó.

`CreateFileW`, `WriteFileEx`, `DeleteFileW`, `GetTempPathW` được sử dụng trong quá trình mã hóa với chức năng tạo file, ghi file mã hóa cũng như ghi chú tiền chuộc vào thư mục bị mã hóa, xóa file gốc trên hệ thống, lấy đường dẫn thư mục TEMP phục vụ cho việc thực thi tải trọng độc hại.

![Pasted image 20231203215446](https://github.com/tsof-smoky/writeup-CTF-challenge/assets/107832241/1c965778-dd59-4d18-bf8a-a6baa14065e6)

![Pasted image 20231203214431](https://github.com/tsof-smoky/writeup-CTF-challenge/assets/107832241/80079754-dd78-4842-88ba-f7b20eb029c2)

![Pasted image 20231203214551](https://github.com/tsof-smoky/writeup-CTF-challenge/assets/107832241/12b4f3ad-1b22-4573-a5a0-a7e054417ac4)

![Pasted image 20231203215021](https://github.com/tsof-smoky/writeup-CTF-challenge/assets/107832241/4292b37f-7e1f-46b7-9e96-eb965fe4c5b8)

Ngoài ra, Sphynx còn tắt các tiến trình có thể ảnh hưởng đến quá trình mã hóa bằng cách gọi hàm `TerminateProcess`.

![Pasted image 20231203215338](https://github.com/tsof-smoky/writeup-CTF-challenge/assets/107832241/97a4f82a-180b-451a-94e1-31b7f049ee9b)

| SHA256                                                           | Path                                                             | Type      | Classification |
| ---------------------------------------------------------------- | ---------------------------------------------------------------- | --------- | -------------- |
| 40278f86caa9ceb72a5e5d272b28cbafa01865ecbaec3a8eead11093f1237714 | 40278f86caa9ceb72a5e5d272b28cbafa01865ecbaec3a8eead11093f1237714 | Main file | Malicious      |

----------


## Làm thế nào để bảo vệ khỏi Sphynx?

Để bảo vệ chống lại Sphynx và các mối đe dọa ransomware khác, các tổ chức nên áp dụng cách tiếp cận nhiều lớp bao gồm các biện pháp sau:

- Thực hiện chiến lược sao lưu và phục hồi mạnh mẽ để đảm bảo tính sẵn sàng và tính toàn vẹn của dữ liệu và hệ thống quan trọng.
- Áp dụng các bản vá và cập nhật bảo mật mới nhất cho tất cả các thiết bị và ứng dụng, đặc biệt là VPN và tường lửa, để ngăn chặn việc khai thác các lỗ hổng đã biết.
- Thực thi các chính sách mật khẩu mạnh và sử dụng xác thực đa yếu tố để ngăn chặn hành vi trộm cắp và sử dụng sai thông tin xác thực.
- Giáo dục và đào tạo nhân viên cũng như người dùng cách nhận biết và tránh email lừa đảo cũng như các tệp đính kèm hoặc liên kết độc hại, vốn là những phương tiện phổ biến để phát tán phần mềm tống tiền.
- Triển khai và cập nhật các phần mềm và công cụ bảo mật như chống vi-rút, tường lửa, EDR và giám sát mạng để phát hiện và ngăn chặn các hoạt động độc hại và các hoạt động bất thường.
- Cách ly và ngắt kết nối các thiết bị bị nhiễm khỏi mạng càng sớm càng tốt để ngăn chặn sự lây lan và thiệt hại thêm.
- Liên hệ với các chuyên gia thực thi pháp luật và bảo mật trong trường hợp bị tấn công bằng ransomware và tránh trả tiền chuộc vì điều đó không đảm bảo việc khôi phục dữ liệu và khuyến khích các cuộc tấn công tiếp theo.
---

Tôi hy vọng bạn thích bài đăng trên blog này và học được điều gì đó mới về BlackCat và Sphynx. Hãy theo dõi để biết thêm thông tin chi tiết và cập nhật về an ninh mạng từ nhóm của chúng tôi.

[Malicious 40278f86caa9ceb72a5e5d272b28cbafa01865ecbaec3a8eead11093f1237714 - Intezer](https://analyze.intezer.com/analyses/e2434d3e-3abd-47fb-8ca3-30c86b9f406d)
[BlackCat (ALPHV) Ransomware Levels Up for Stealth, Speed and Exfiltration (securityintelligence.com)](https://securityintelligence.com/x-force/blackcat-ransomware-levels-up-stealth-speed-exfiltration/)