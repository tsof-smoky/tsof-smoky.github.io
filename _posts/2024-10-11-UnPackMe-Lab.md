---
layout: post
title: UnPackMe Lab - CyberDefenders
date: 2024-10-11 13:45:00 +0700
categories: ctf malware CyberDefenders
---

>**Introduction**
>```
>Instructions:
>
>Uncompress the lab (pass: cyberdefenders.org)
>Zip sha256: 4934714622f28589e48bfbab2f1a9b29c70eb16b
>Zip size: 450 KB
>Scenario:
>
>Welcome to the cybersecurity team at CyberSolutions Inc. Recently, our threat intelligence network has uncovered a digital espionage effort targeting organizations worldwide. At the heart of this campaign is a sophisticated and hitherto unknown piece of malware, which we've codenamed "ShadowSteal." This malware is designed with advanced capabilities to infiltrate systems, evade detection, and exfiltrate sensitive information without leaving a trace.
>
>"ShadowSteal" has been identified in a preliminary cybersecurity sweep and flagged due to its anomalous behavior patterns and the potential threat it poses to our operations and confidential data. Initial findings suggest that "ShadowSteal" is not just another piece of malware; it represents a significant threat due to its ability to steal a wide array of sensitive information. Additionally, "ShadowSteal" is designed to self-delete after completing its data exfiltration process, complicating post-incident analysis and forensics.
>
>As a key member of our cybersecurity response team, your task is to analyze the "ShadowSteal" malware sample thoroughly. You must dissect its components, understand its operational mechanics, and uncover its functionalities. Your analysis will directly inform our defensive strategies, helping us to fortify our defenses, develop detection signatures, and ultimately neutralize the threat posed by "ShadowSteal."
>
>Tools:
>
>IDA
>Ghidra
>x64dbg
>```
{: .prompt-info }

<a href="https://cyberdefenders.org/blueteam-ctf-challenges/unpackme/">CyberDefenders: Blue team CTF Challenges | UnPackMe</a>

Q1

Upon initiating your analysis of `ShadowSteal`, understanding its obfuscation techniques is critical. One tell-tale sign is the file's entropy. Can you determine its value, which hints at whether the malware is packed?

PEStudio, DIE, hoặc bất cứ công cụ nào khác có thể phát hiện entropy.

```
7.389
```

Q2

Before `ShadowSteal` begins its data theft, it cleverly checks for a mutex to avoid duplicative data collection. This mutex includes a unique string combined with the computer's name. What is the hardcoded string that `ShadowSteal` uses for this purpose?

Trước khi phân tích và trả lời các câu hỏi tiếp theo, để dễ dàng hãy tìm kiếm bản đã được unpack trên `unpac.me`

Chúng ta bắt đầu với hàm đầu tiên `sub_432df7`. 

![Pasted image 20241011100111](https://github.com/user-attachments/assets/77dbdad0-bf9c-4d64-98fc-de34e53784e7)

Trong quá trình phân tích, nhận thấy rằng mã độc thực hiện cùng một cơ chế làm xáo trộn, do đó mình đã viết đoạn python script ngắn để phục vụ mục đích giải quyết nhanh những đoạn bị làm rối này. Về cơ chế thì không quá khó. Lấy đoạn làm rối `mutex` làm ví dụ. Mã độc đầu tiên khởi tạo 2 chuỗi hex `v5[0]` và `v5[1]` và để ý chút đến `v6`. Trong vòng lặp while, bắt đầu với byte thứ 2 (tức `v5[0][1]=0xB8`, chỗ này là Little-endian), từng byte sẽ được XOR với `~v1` (`v1=0x32=0011 0010` => `~v1=1100 1101=0xCD`). `v2` ở đây là biến đếm, điều kiện thoát vòng lặp là `v2>=9` tức có 9 byte sẽ được XOR, ở đây gồm 3 byte ở `v5[0]` (trừ byte đầu tiên (để ý byte đầu tiên cuối vòng lặp đầu tiên quay lại gán cho v1), 4 byte ở `v5[1]` và 2 byte còn lại ở `v6` (recheck bằng cách quay ngược lên kiểm tra vị trí trong stack so với v5).

```python
def little(value):
    return value.to_bytes(4, byteorder='little')

v5 = [0xACA4B832, 0xBABCABAF]
v6 = 0xB8AB
v1 = 0x32
tmp = bytearray()  

tmp.extend(little(v5[0]))
tmp.extend(little(v5[1]))
tmp.extend(little(v6))

count = 1 
while True:
    tmp[count] ^= ~v1 & 0xFF 
    count += 1
    if count > 9:
        break
    v1 = little(v5[0])[0]

mutex = ''.join(chr(tmp[i]) for i in range(1, len(tmp)))
print(mutex)
```

```
uiabfqwfu
```

Tương tự ở các spawn tiến trình explorer mới `sub_435B8A`, và các đoạn check nhỏ kiểm tra cài đặt ngôn ngữ mặc định và thoát khỏi chương trình nếu nó được đặt thành ngôn ngữ của các quốc gia CIS, bao gồm cả Nga.

![Pasted image 20241011100510](https://github.com/user-attachments/assets/37e8d46d-c36e-425e-ad78-97c248bd3027)

![Pasted image 20241011100824](https://github.com/user-attachments/assets/97d3426d-5333-4691-be4f-e34bdd493ee7)

Q3

`ShadowSteal` employs base64 and RC4 algorithms to safeguard the stolen data and its communication with the control server. Dive into its code to find the RC4 encryption key. What is it?

![Pasted image 20241011101044](https://github.com/user-attachments/assets/8fe91769-74f8-49ee-9986-eaa2fc0f5aaa)

Tìm kiếm trong tab `strings`, mình bắt gặp một số chuỗi Base64 không thể đọc được khi giải mã, cho thấy rằng chúng đã được mã hóa. Dò theo `xRef`, ngay phía trên các đoạn này mình gặp phải một loại mã hóa XOR sử dụng giá trị từ `xmmword_47D3E0`, chứa `9498D7AB8CAEB2808B9A8E9DDCB4CA11` ở dạng thập lục phân. Đầu tiên, số `11` ở cuối giá trị này được thêm vào thanh ghi `cl` và sau đó bị xóa khỏi giá trị ban đầu. Tiếp theo, `BC` được thêm vào đầu giá trị, tạo thành `BC9498D7AB8CAEB2808B9A8E9DDCB4CA`. Sau đó, `cl` được đảo ngược (NOT) từ `11` thành `EE` và giá trị được sửa đổi được XOR với `CAB4DC9D8E9A8B80B2AE8CABD79894BC`, vì nó sử dụng định dạng little-endian. Kết quả mình có được key mã hóa RC4.

![Pasted image 20241011101419](https://github.com/user-attachments/assets/6f4aa82d-6b20-4c51-8bda-844082ea33cb)

```
$Z2s`ten\@bE9vzR
```

Sử dụng key này để giải mã đoạn mã hóa đầu tiên. Tại thời điểm viết bài domain trên đã không còn tồn tại. Recheck với kết quả trên `virustotal` cũng sẽ cho ra domain tương tự.

![Pasted image 20241011101727](https://github.com/user-attachments/assets/0127150d-ffca-45ac-9df7-90a33924c01f)

Q4

In its data collection phase, `ShadowSteal` compiles a report which, among other things, reveals its version to the attacker. This detail is crucial for our threat intelligence. What version of `ShadowSteal` are you analyzing?

```
1.7.3
```

Q5

Part of `ShadowSteal`'s reconnaissance involves scanning for installed software, a task it accomplishes by querying a specific registry key. Identify the registry key it uses for this purpose.

Thời điểm này, có vẻ như tôi chỉ tìm được 2 đoạn mã hóa thú vị trên để chia sẻ, việc tìm kiếm các manh mối còn lại để trả lời có vẻ khá tốn sức. Do đó từ đâu tôi sử dụng các kết quả từ VirusTotal và từ kinh nghiệm của chính mình. 

![Pasted image 20241011114726](https://github.com/user-attachments/assets/4f714b0d-428c-4462-826b-ea887a2ff94b)

```
SOFTWARE\Microsoft\Windows\CurrentVersion\Uninstall
```

Q6

`ShadowSteal` captures a screenshot of the current operating window as part of its information-gathering process. Which function address is responsible for this action?

Khi không biết phải bắt đầu từ đâu, việc quay lại tab `strings` có vẻ là một lựa chọn khả dĩ. 

![Pasted image 20241011114925](https://github.com/user-attachments/assets/8ad20da0-52d4-40cb-92da-574328c3c7a7)

![Pasted image 20241011115001](https://github.com/user-attachments/assets/ca7197fb-4af3-46e1-a7ad-dc7aa01dbbc3)

```
0042951b
```

Q7

Once `ShadowSteal` has fulfilled its mission, it removes all traces of its existence from the infected system. What command does it execute to delete itself?

![Pasted image 20241011115911](https://github.com/user-attachments/assets/a3c07bf5-c484-428a-b67e-26f72eb247a3)

```
cmd.exe /C timeout /T 10 /NOBREAK > Nul & Del /f /q
```

Q8

For a comprehensive threat analysis, knowing where `ShadowSteal` originates from is key. This includes the full path to its build folder on the attacker's computer. Can you provide this path?

Sử dụng strings tìm kiếm với từ khóa `build`.

![Pasted image 20241011120044](https://github.com/user-attachments/assets/ad360cbf-3335-40a0-bb66-b4ee1116e581)

Tìm đến từ khóa này và thử show một chút về `xRef Graph`. Với việc import `json.hpp`, có vẻ JSON được sử dụng cho việc định dạng dữ liệu để gửi thông tin nạn nhân về C2.

![Pasted image 20241011120247](https://github.com/user-attachments/assets/baca9df9-a31a-4ae9-8ef3-93b8a8d398c4)

```
A:\_Work\rc-build-v1-exe\
```

