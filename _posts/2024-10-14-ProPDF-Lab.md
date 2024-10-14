---
layout: post
title: ProPDF Lab - CyberDefenders
date: 2024-10-14 11:55:00 +0700
categories: ctf malware CyberDefenders
---
>**Introduction**
>```
>Instructions:
>
>Ensure that there are no blockers, such as Adblock extensions, that might prevent the lab from opening in a new tab or affect lab’s functionality.
>All the lab-related files and tools are on the desktop in 'Start here' directory.
>Scenario:
>
>Recent alerts highlight targeted attacks using suspicious PDF files, potentially orchestrated by groups associated with North Korea. Documents, largely themed around North-South Korea relations, suggest they are targeting specific geopolitical stakeholders.
>
>Your job is to analyze one of these suspicious PDFs. Initial hints suggest that the known Kimsuky or Thallium APT groups might be involved. Your findings will help confirm this and prevent more attacks.
>
>Tools:
>
>PDFwalker
>VScode
>HexEditor
>CyberChef
>Ghidra
>```
{: .prompt-info }

<a href="https://cyberdefenders.org/blueteam-ctf-challenges/propdf/">CyberDefenders: Blue team CTF Challenges | ProPDF</a>

Q1

>This PDF seems to trigger unexpected system actions when opened. Could you provide the object number that contains the malicious code?

Thực hiện load file `pdf` được cung cấp vào `pdfwalker`. Nhận thấy rằng bên trong file pdf được nhúng đoạn `JavaScript`, đây là dấu hiệu cho thấy sự đáng ngờ xuất hiện.

![Pasted image 20241014110149](https://github.com/user-attachments/assets/34bac595-7e7b-420e-904b-8560a2a9b6d9)

```
89
```

Q2

>The analysis of the extracted malicious code reveals an additional procedure within the PDF, What specific API is utilized to embed this secondary malicious code?

Thực hiện decode Stream, đoạn code đã dễ đọc hơn. Thực hiện dump đoạn mã được decode này ra một file js khác và mở nó bằng VSC.

![Pasted image 20241014110218](https://github.com/user-attachments/assets/c6be1c98-944f-4a36-907e-f2aec88f4742)

Cái nhìn đầu tiên cho thấy mặc dù xác định nó là file js, nhưng đoạn mã bên trong vẫn thực sự khó đọc, dấu hiệu cho thấy nó được làm rối. Ngoài ra bên trong còn khai báo biến `pst` lưu trữ đoạn string được mã hóa bằng base64.

![Pasted image 20241014111326](https://github.com/user-attachments/assets/bf7c3f38-8080-4c4c-aebc-c936793c3108)

Cuối đoạn script thấy rằng có vẻ đây mới chỉ là stage1 khi nó sử dụng function `addScript` để import đoạn mã base64 trên.

![Pasted image 20241014111421](https://github.com/user-attachments/assets/c885880a-7f10-43f3-84c1-00e48b107d50)

```
addScript
```

Q3

>Upon analyzing the scripts, it appears that it actively carrying out malicious operations. The script uses a method to alter memory permissions. What is the Windows API function name?

Copy đoạn base64 vào CyberChef, thực hiện decode ta được một script mới. Lưu đoạn đã được giải mã vào một file js khác (`stage2.js`). Biến s được khởi tạo ban đầu khá chắc là `exe` mã độc khi có đoạn byte mở đầu là `0x4d5a`.

![Pasted image 20241014112159](https://github.com/user-attachments/assets/1a4364b0-5166-49e3-a3e0-cd72d7a4fd16)

Tiếp tục sử dụng [JavaScript Deobfuscator (relative.im)](https://deobfuscate.relative.im/), điều này giúp việc đọc phần code phía sau dễ dàng hơn.

![Pasted image 20241014112336](https://github.com/user-attachments/assets/0ce1d6bb-1743-4fc0-ba0b-fa7cf91ec7ff)

Nhận thấy rằng ở biến fN lưu trữ đoạn decimal đã được mã hóa, thực hiện giải mã qua CyberChef ta nhận được Windows API cần tìm.

![Pasted image 20241014112418](https://github.com/user-attachments/assets/34f61caf-f634-43ff-8288-6df18bd08e04)

```
VirtualProtect
```

Q4

>We are attempting to identify the specific malicious payload. Could you provide the SHA256 hash of the code injected into the memory generated from the second stage? To determine its origins and any potential connections to other known threats.

Thực hiện dump đoạn hex bên trong biến s ra file và tính hash của nó.

```
6F5068784FC1635DADDCFA447082098FA960E32B00906898BC0C4ED921D72B32
```

Q5

>The malicious executable connects with a C2 server to download another stage. What is the C2 server name?

![Pasted image 20241014112555](https://github.com/user-attachments/assets/5d182ceb-59d5-4a69-90df-41160c354cf0)

Ở đây mình đã search và tải đoạn payload trên về máy và phân tích bằng IDA, điều này có thể được phân tích ngay trên lab bằng Ghidra. Bắt đầu bằng việc lướt qua tab strings như mọi khi để ý ngay đến xuất hiện địa chỉ url. Tham chiếu đến nơi gọi địa chỉ này nhận thấy rằng có vẻ mã độc thực hiện tạo request đến máy chủ C2 của attacker.

```
tksRpdl.atwebpages.com
```

Q6

>After the malware connects with the C2 server, it downloads a file onto the disk. What is the name of the downloaded file on the disk?

Ban đầu mình đã nhầm lẫn với file download.php và có kết luận vội vàng. File php này nằm trên server C2. Để tìm kiếm file thực sự được lưu, tìm kiếm đến API [SHGetSpecialFolderPathA](https://learn.microsoft.com/en-us/windows/win32/api/shlobj_core/nf-shlobj_core-shgetspecialfolderpatha).

![Pasted image 20241014112617](https://github.com/user-attachments/assets/91dfdde8-c23a-47f6-a8a5-1a2b7c28394b)

```
AdobeAdv.dll
```