![ảnh](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/d1eefa02-ae6c-4996-8fe9-4ba7b9a8f3f0)## Instructions:
  
  ### Uncompress the lab (pass: cyberdefenders.org)

## Scenario:

### You are part of the incident response team at FinTrust Bank. This morning, the network monitoring system flagged unusual outbound traffic patterns from several workstations. Preliminary analysis by the IT department has identified a potential compromise linked to an exploited vulnerability in WinRAR software.

### As an incident responder, your task is to investigate this compromised workstation to understand the scope of the breach, identify the malware, and trace its activities within the network.

## Tools:

  ### Arsenal Image Mounter
  ### SQLite Viewer
  ###  Eric Zimmerman Tools
  ###  NTFS Log Tracker
  ###  Registry Explorer
  ###  Event Log Explorer
  ###  Strings
  ###  CyberChef

## Q1: In your investigation into the FinTrust Bank breach, you found an application that was the entry point for the attack. Which application was used to download the malicious file?
### Solution
* Đầu tiên mình cũng chưa biết làm sao để mở file này nên mình đã mở bằng WinZip
* mở file vhd
* export nó ra thành một thư mục
* dùng ftk imager mở, tìm trong các user xem có gì không
* Tại Download của user administrator có telegram và trong thư mục đó có một file rar lạ

![Screenshot 2024-03-02 195620](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/6e86414e-faf9-4c99-9185-f1d624fae598)

`Answer: Telegram`

## Q2: Finding out when the attack started is critical. What is the UTC timestamp for when the suspicious file was first downloaded?
### Solution
* Tìm thời gian file đó được tải về
* Mình đã tìm được một file **$RHRTGI2.csv** trong đó có lịch sử hoạt động của máy và mình filter file rar thì tìm thấy thời gian file đó được tải về

![Screenshot 2024-03-08 130839](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/4a4f2ea1-5a83-49b1-971b-9290fb303728)

`Answer: 2024-02-03 07:33:20`

## Q3: Knowing which vulnerability was exploited is key to improving security. What is the CVE identifier of the vulnerability used in this attack?
### Solution
* Mình đã search SAN SEC401 CVE và tìm được đây là CVE-2023-38831
* RARLAB WinRAR Code Execution Vulnerability đây là một lỗ hổng được các hacker khai thác từ tháng 4 đến tháng 10 năm 2023
* CVE này hoạt động dựa trên việc giả mạo extension của file, khi người dùng mở file thì mã độc được chạy dưới dạng như một bức ảnh trông có vẻ như vô hại
* Một số nguồn về CVE này mình tìm được:

https://tinnhiemmang.vn/lo-hong-bao-mat-winrar-da-bi-khai-thac-de-nham-muc-tieu-vao-cac-nha-giao-dich
https://nvd.nist.gov/vuln/detail/CVE-2023-38831

## Q4: In examining the downloaded archive, you noticed a file in with an odd extension indicating it might be malicious. What is the name of this file?
### Solution
* Mình mở file rar lên thì trong đó có một tệp có extension là pdf và một tệp là SANS SEC401.pdf .cmd, mình thấy tệp này khá kì lạ và có vẻ nó chính là file chúng ta cần tìm

![Screenshot 2024-03-11 135620](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/f5f5fda9-6b22-4f27-b253-dbc8ec33facb)

`Answer: SANS SEC401.pdf .cmd`

## Q5: Uncovering the methods of payload delivery helps in understanding the attack vectors used. What is the URL used by the attacker to download the second stage of the malware?
### Solution
* mình view file bằng winrar thì thấy có một đường link tải xuống một tệp ảnh

![Screenshot 2024-03-11 141109](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/3cd7afd8-f8d4-49a3-8118-6886d3ab8a19)

`Answer: http://172.18.35.10:8000/amanwhogetsnorest.jpg`

## Q6: To further understand how attackers cover their tracks, identify the script they used to tamper with the event logs. What is the script name?
### Solution
* Cũng trong file đấy có một file powershell khả nghi là Evenlogs.ps1, có vẻ file này sẽ làm giả log

![Screenshot 2024-03-11 142054](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/a114e911-c95d-4c04-b007-2ea073934c57)

`Answer: Eventlogs.ps1`

## Q7: Knowing when unauthorized actions happened helps in understanding the attack. What is the UTC timestamp for when the script that tampered with event logs was run?
* Vì nó là file powershell nên mình đã check log powershell bằng Event Viewer, tại thời điểm chỉ sau khi tệp SANS SEC401 được tải về, mình đã tìm thấy thông tin về file Eventlogs này

![Screenshot 2024-03-11 143630](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/64026480-1aa6-4ebd-9961-17892e217750)

* Lệnh powershell -NOP -EP Bypass C:\Windows\Temp\Eventlogs.ps1
  Option -NOP là noprofile sẽ không tải lên profile người dùng
  Option -EP là ExcutionPolicy tham số Bypass cho phép bỏ qua việc xác nhận
* cả lệnh đó sẽ thực thi ngầm tệp Eventlogs.ps1 mà không cần sự xác nhận của người dùng
* Tệp này được thực thi theo system time là 2024-02-03T07:38:01
`Answer: 2024-02-03 07:38:01`

## Q8: We need to identify if the attacker maintained access to the machine. What is the command used by the attacker for persistence?
### Solution
* persistence là cách kẻ tấn công muốn thâm nhập vào máy nạn nhân
* schtasks /create /sc minute /mo 3 /tn "whoisthebaba" /tr C:\Windows\Temp\run.bat /RL HIGHEST
* Ở trên là câu lệnh được thông báo ra màn hình khi mình thử chạy file mã độc và dừng lại ngay lập tức bằng Ctrl C

![Screenshot 2024-03-11 145548](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/786e7acb-abf8-4aeb-8ce2-daffd3918d8a)

* schtasks là lập lịch trình cho cứ mỗi 1p thì công việc sẽ thực hiện trong vòng 3p tên công việc là whoisthebaba tệp được thực thi là run.bat với quyền Highest
`Answer: schtasks /create /sc minute /mo 3 /tn "whoisthebaba" /tr C:\Windows\Temp\run.bat /RL HIGHEST`

## Q9: To understand the attacker's data exfiltration strategy, we need to locate where they stored their harvested data. What is the full path of the file storing the data collected by one of the attacker's tools in preparation for data exfiltration?
### Solution
* Các dữ liệu tạm thời của của các chương trình được lưu vào AppData/Local/Temp nên mình đã vào đây xem và tìm được một file có chứa một list các port
* Có vẻ như kẻ tấn công đã thử các giá trị ở cuối dãy của địa chỉ ip để xem địa chỉ nào đang online

![Screenshot 2024-03-11 163816](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/37666c4d-e159-4f4c-961c-fe73ec1ebc5a)

`Answer: BL4356.txt`









