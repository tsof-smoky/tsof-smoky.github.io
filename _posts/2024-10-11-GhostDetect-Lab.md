---
layout: post
title: GhostDetect Lab - CyberDefenders
date: 2024-10-11 16:02:00 +0700
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
>One of the employees reported receiving an email with a suspicious-looking attachment. Suspecting that a known Threat Actor may be attempting to use phishing to gain a foothold in the organization, you need to analyze the provided file and identify the threat.
>
>Tools:
>
>CyberChef
>Wireshark
>Strings
>VS Code
>LECmd
>ProcMon
>```
{: .prompt-info }

<a href="https://cyberdefenders.org/blueteam-ctf-challenges/ghostdetect/">CyberDefenders: Blue team CTF Challenges | GhostDetect</a>

Q1

>In analyzing the malware's behavior after the initial intrusion, it's crucial to understand where it attempts to establish persistence or further infection. Where were the files dropped by the malware located within the system's file structure?

CyberDefenders đã cung cấp môi trường sandbox tuyệt vời. Ngoài ra trong phần giới thiệu cũng đề cập đến việc sử dụng ProcMon và WireShark như là công cụ để thực hiện bài lab. Do đó công việc của mình chỉ là bật ProcMon và WireShark (thời điểm mình làm thì gần như không phụ thuộc vào các công cụ này), đồng thời thực thi mã độc được cung cấp.

![Pasted image 20241011154600](https://github.com/user-attachments/assets/cb183716-e1f5-4a90-a195-3d52abd8645c)

```
C:\Users\Administrator\AppData\Local\Temp
```

Q2

>The malware's communication with external servers is key to its operation. What is the URL that was used by the malware to download a secondary payload?

Ngay bên trong thư mục chứa pdf được bật lên, nhận thấy sự xuất hiện kì lạ của tệp `.js`. Bạn có thể tùy chọn mở bằng Notepad hoặc VSC ngay trên lab. 

![Pasted image 20241011154646](https://github.com/user-attachments/assets/00e2b7a6-ea92-4da3-b706-ff5e05b2f063)

Sau đó copy nội dung vào https://deobfuscate.relative.im/, một công cụ online được mình sử dụng nhiều trong việc deobfuscator js, theo đó tìm kiếm chuỗi *http*.

![Pasted image 20241011154828](https://github.com/user-attachments/assets/371ff3a8-73b6-4ad5-bdc1-ac0b41f7c473)

```
https://windacarmelita.pw/picdir/big/113-1131910-clipart.svg
```

Q3

>Understanding the malware's defense evasion techniques is essential for developing effective detection strategies. What encryption technique is employed by the malware to conceal its activities or payloads?

Lúc đầu mình đã pass qua câu hỏi này vì chưa thực sự có tìm hiểu cũng như kiến thức nhiều về các loại thuật toán mã hóa. Nhưng sau khi đọc được bài báo cáo của [CERT-UA](https://cert.gov.ua/article/5661411)thì mình đã thực hiện tìm kiếm lại trong file js.

![Pasted image 20241011155022](https://github.com/user-attachments/assets/0a5484f0-3af5-4fb8-9802-8913bbcab055)

```
Rabbit
```

Q4

>Decrypting payloads is a common technique used by malware to evade initial analysis. What is the decryption key used to unlock the second stage of the malware?

Ngay bên dưới url để download payload 2, hàng loạt thông tin có giá trị được tìm thấy.

![Pasted image 20241011155233](https://github.com/user-attachments/assets/5f373a20-e574-4dc9-a64e-8391387bd3b7)

```
dfshji349jg843059utli
```

Q5

>Malware analysis often involves tracking how it interacts with the filesystem. What is the name of the file created by the malware to store decrypted data?

```
mokpp9342jsOUth.dll
```

Q6

>Analyzing the malware's execution flow is crucial for understanding its impact and behavior. What function does the malware execute within the DLL to perform its malicious activities?

Thời điểm mình thực hiện lab thì domain không còn, do đó không thể tải được file đã dll như mong muốn. Do đó mình đã thực hiện OSINT theo domain phía trên đã tìm được nhằm tìm các file dll liên quan. 

[VirusTotal - File - mokpp9342jktihh.dll](https://www.virustotal.com/gui/file/1d7a2003ad66fb13e8ed71ec6e9e90f6314723ce4c723b9bfb8167d3ca19094f/community)

[VirusTotal - File - sdfhui2kjd.js](https://www.virustotal.com/gui/file/f3d8c34443457d32de3c2687619037015e12bd2222c0e457e8c79fda2906d424/community)

Ở file `sdfhui2kjd.js`, phần Processes tree đề cập đến việc sử dụng rundll32 để chạy hàm được export từ dll mã độc.

![Pasted image 20241011155631](https://github.com/user-attachments/assets/6551ed4f-9d0d-4637-97ba-bce668f6b641)

```
NormalizeF
```

Q7

>Investigating related artifacts can provide insights into the broader campaign. What is the name of another JavaScript file that utilizes the domain identified during the investigation?

```
sdfhui2kjd.js
```

Q8

>Attribution is a critical aspect of threat intelligence. Can you identify which Advanced Persistent Threat (APT) group is likely behind this attack?

Theo thông tin từ báo cáo của CERT-UA, dề cập đến thông tin về nhóm tin tặc đằng sau chiến dịch.

![Pasted image 20241011155801](https://github.com/user-attachments/assets/ce3abff2-684a-4c43-8aea-c4e01461bbd5)

```
UAC-0057
```

Q9

>What is the country of origin associated with the APT group identified in this investigation?

```
Belarus
```