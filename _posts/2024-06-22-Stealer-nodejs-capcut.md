---
layout: post
title: Stealer nodejs capcut
date: 2024-06-22 13:17:24 +0700
categories: malware APTs
---
## I. Tổng quan

Mẫu được lấy và submit lên [VirusTotal - File - d6aee63ffe429ddb9340090bff2127efad340240954364f1c996a8da6b711374](https://www.virustotal.com/gui/file/d6aee63ffe429ddb9340090bff2127efad340240954364f1c996a8da6b711374). Sau khi đưa mẫu vào DIE, ta biết được mẫu được biên dịch bằng C/C++, 64bit. Với thông tin này, ta có thể load mẫu vào trong IDA Pro 64 để xem xét các hành vi khác. Ở đây điều đặc biệt hơn là mẫu có tải trọng ~60MB, khá lớn so với một malware thông thường. Tại thời điểm ban đầu mình cho rằng có thể mẫu được chèn trash byte, null byte hay điều gì đó tương tự nhằm bypass các trình sandbox phát hiện mã độc.

![Pasted image 20240622104819](https://github.com/tsof-smoky/tsof-smoky.github.io/assets/107832241/61309e94-c44c-4c3e-95ce-f77255f0a709)

Sau khi load mẫu và chờ một thời gian, bước đầu tiên mình chọn thực hiện là xem xét các strings xuất hiện trong file. Bước đầu từ thông tin từ VirusTotal, mình cố gắng tìm kiếm các thông tin liên quan đến Registry hay các cơ chế persistance trên máy nhưng có vẻ không có quá nhiều thông tin quan trọng. Ngoài ra mẫu còn thực hiện nhiều hành vi liên quan đến file như đọc, ghi và các hành động khác nhưng chưa có bằng chứng rõ ràng nào về hành vi của mã độc. Sau nhiều lần xem xét, mình quay lại và phát hiện sự xuất hiện của các thư viện, hay đúng hơn là các chức năng liên quan đến pkg-packer, một trình packer được sử dụng trong các bundle liên quan đến js.

![Pasted image 20240622110953](https://github.com/tsof-smoky/tsof-smoky.github.io/assets/107832241/aa8ed7b6-a381-430a-aa14-5ca150468eb2)

## II. Unpack malware

Với thông tin có được như trên, lúc này mình đã chuyển hướng sang tìm kiếm các công cụ để unpack cũng như phân tách mẫu trên. Trong quá trình tìm kiếm thì có 2 công cụ được đề cập đến là pkg-unpacker và [ghidra_nodejs](https://github.com/PositiveTechnologies/ghidra_nodejs). Lúc này khi sử dụng remnux như thông thường để cài đặt và sử dụng pkg-unpacker thì xảy ra lỗi nhiều lần. Cảm ơn @bananNat đã cho mình lời khuyên thử chạy trên hệ điều hành Kali và tuyệt vời là chạy thành công. Bên dưới là command để thực hiện cài đặt và unpack mã độc.

```bash
git clone https://github.com/LockBlock-dev/pkg-unpacker.git
cd pkg-unpacker
npm install
node unpack.js -i ../d6aee63ffe429ddb9340090bff2127efad340240954364f1c996a8da6b711374 -o ./unpacked
```

Sau khi chạy chuỗi lệnh trên mình tạo được một thư mục gồm các file .\*js như bên dưới. Trong đó phần lớn là node_modules, các modules, lib mặc định, file duy nhất đáng nghi là app.js.

![Pasted image 20240622105123](https://github.com/tsof-smoky/tsof-smoky.github.io/assets/107832241/482b444c-5edd-47a5-9c8e-2eb94e323acf)

## III. Deobfuscation Malware

Mở file qua SublimeText thì mình nhận ra rằng nó đã bị obfuscation bằng obfuscation thông dụng được dùng khi viết js. Việc viết script mình đã thử bằng tay để decode nhưng có vẻ nó khá tốn thời gian mặc dù việc hiểu cơ chế obfuscation của nó không hề khó. Thay vào đó mình load nội dung của đoạn code vào các trình deobfuscation online như [Obfuscator.io Deobfuscator](https://obf-io.deobfuscate.io/) hay [JavaScript Deobfuscator](https://deobfuscate.relative.im/). Kết quả của 2 công cụ này khá tương đồng.

![Pasted image 20240622105333](https://github.com/tsof-smoky/tsof-smoky.github.io/assets/107832241/64796bf1-0a68-400a-92e2-8557e776dc31)

Bên dưới là kết quả khi deobfuscation đoạn mã:

![Pasted image 20240622105509](https://github.com/tsof-smoky/tsof-smoky.github.io/assets/107832241/176eac6b-9625-42e6-b181-6d1ee2a8cf83)

![Pasted image 20240622105532](https://github.com/tsof-smoky/tsof-smoky.github.io/assets/107832241/42648084-7d18-441e-aa91-aecd2c205070)

## IV. Decrypt các thông tin bị mã hóa 

Trong đoạn mã của mã độc có một hàm decipher De() dùng để mã hóa cứng các chuỗi được hacker đưa vào. Các chuỗi này bao gồm đường dẫn đến nơi lưu trữ của các browser được dùng phổ biến như Chrome, Opera, Brave, Edge, các đường dẫn đến việc gọi bot telegram phục vụ cho mục đích làm C2 (lưu trữ thông tin đánh cắp), hay các server của hacker dựng lên để truyền lệnh hay thực hiện các hành vi độc hại khác. Đoạn mã này đơn giản chỉ thực hiện chuyển đổi hex và xor với key đã được cung cấp trước, ở đây key hacker cung cấp là `haha123444`.

![Pasted image 20240622120648](https://github.com/tsof-smoky/tsof-smoky.github.io/assets/107832241/1cf40b11-eb70-4405-b5c6-a5b1c4dd1f20)

Ngoài ra thì hacker còn sử dụng bbbse (js-base64) trong việc giải mã để kết nối với các server đã được dựng sẵn.

![Pasted image 20240622112000](https://github.com/tsof-smoky/tsof-smoky.github.io/assets/107832241/4bc11875-fdf6-4a58-9f13-e5653ad7af7a)

![Pasted image 20240622130618](https://github.com/tsof-smoky/tsof-smoky.github.io/assets/107832241/d070fb62-3988-4cd0-a89e-df610cdb97c9)

Các thông tin bị đánh cắp được kẻ tấn công gửi thông qua bot với chức năng `sendMessage`, `senDocument` đến địa chỉ telegram được dựng sẵn. Đây không phải là kỹ thuật mới khi nhiều năm qua các hacker đã tận dụng telegram với tính năng tích hợp bot như là cách để exfiltrate dữ liệu bị đánh cắp.

![Pasted image 20240622130914](https://github.com/tsof-smoky/tsof-smoky.github.io/assets/107832241/a5f1da4f-d20b-4c5f-b523-b3bee8ba959b)

## Kết luận



## IOCs

>Domains
>https://avatarcloud[.]top
>https://cloudimages[.]net
>https://editorimage[.]info
>https://getavatar[.]top
>https://hahaimage[.]info
>https://hahaimage[.]top
>https://hahaimage[.]xyz
>https://heheimage[.]info
>https://heheimage[.]top
>https://heheimage[.]xyz
>https://heyavatar[.]info
>https://heyavatar[.]top
>https://heyimage[.]info
>https://nametoimage[.]com
>https://toimageai[.]top
{: .prompt-info }

>Hash
>`d6aee63ffe429ddb9340090bff2127efad340240954364f1c996a8da6b711374`
{: .prompt-info }

### Những thông tin hacker đánh cắp

```json
{"name":"Chrome","productName":"Google Chrome","pa":"\\AppData\\Local\\Google\\Chrome\\User Data","local":"\\AppData\\Local\\Google\\Chrome\\User Data\\Local State","cookie":"\\AppData\\Local\\Google\\Chrome\\User Data\\Default\\Cookies","login":"\\AppData\\Local\\Google\\Chrome\\User Data\\Default\\Login Data"}
{"name":"Opera","productName":"Opera Browser","pa":"\\AppData\\Roaming\\Opera Software\\Opera GX Stable","local":"\\AppData\\Roaming\\Opera Software\\Opera GX Stable\\Local State","cookie":"\\AppData\\Roaming\\Opera Software\\Opera GX Stable\\Cookies","login":"\\AppData\\Roaming\\Opera Software\\Opera GX Stable\\Login Data"}
{"name":"Opera","productName":"Opera Browser","pa":"\\AppData\\Roaming\\Opera Software\\Opera Stable","local":"\\AppData\\Roaming\\Opera Software\\Opera Stable\\Local State","cookie":"\\AppData\\Roaming\\Opera Software\\Opera Stable\\Cookies","login":"\\AppData\\Roaming\\Opera Software\\Opera Stable\\Login Data"}
{"name":"Edge","productName":"Microsoft Edge","pa":"\\AppData\\Local\\Microsoft\\Edge\\User Data","local":"\\AppData\\Local\\Microsoft\\Edge\\User Data\\Local State","cookie":"\\AppData\\Local\\Microsoft\\Edge\\User Data\\Default\\Cookies","login":"\\AppData\\Local\\Microsoft\\Edge\\User Data\\Default\\Login Data"}
{"name":"Brave","productName":"Brave Browser","pa":"\\AppData\\Local\\BraveSoftware\\Brave-Browser\\User Data","local":"\\AppData\\Local\\BraveSoftware\\Brave-Browser\\User Data\\Local State","cookie":"\\AppData\\Local\\BraveSoftware\\Brave-Browser\\User Data\\Default\\Cookies","login":"\\AppData\\Local\\BraveSoftware\\Brave-Browser\\User Data\\Default\\Login Data"}
```
```graphql
type DuLieu {
  ten: String
  uid: String
  fa: String
  cookie: String
  password: String
  token: String
  ngayLay: DateTime
  cookieGg: String
  email: String
  fanpage: [Fanpage]
  businesses: [Businesses]
  taiKhoanAds: [TaiKhoanAds]
  birthday: String
  location: String
  step: String
  cookieOutlook: String
  usergmail: String
  passgmail: String
  useroutlook: String
  passoutlook: String
  ip: String
  nguongHienTaiCaoNhat: String
  hanhDong: String
  trinhDuyet: String
  ngayReset: DateTime
  getnew: String
  _id: String
  createdAt: DateTime
  updatedAt: DateTime
  maquocgia: String
  trangthaionline: Boolean
  maMay: String
}
```
```graphql
type TaiKhoanAds {
  trangThai: String
  ngayTao: String
  ngayLapHoaDon: String
  adminAn: Int
  loaiTaiKhoan: String
  muiGio: String
  theThanhToan: String
  currency: String
  id: String
  gioiHanChiTieu: String
  nguongHienTai: String
  tongChiTieu: String
  soDu: String
  ten: String
  ngayHetHan: String
  trangThaiThe: String
  radioToUsd: Float
}
```
```graphql
type Fanpage {
  accessToken: String
  id: String
  name: String
  verification_status: String
  fan_count: String
}
```
```graphql
type Businesses {
  ten: String
  gioiHan: Int
  ngayTao: String
  role: String
  trangThai: String
  nguoi: Int
  linkBm: String
  id: String
}
```

### Threat Intelligence
https://x.com/josh_penny/status/1679092742666825731

[Undefined #stealer disguised as @capcutapp installer distributed via @trello 🏴‍☠️ No VT detection](https://x.com/ULTRAFRAUD/status/1678849977336954880)
