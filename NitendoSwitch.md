## Scenario:

### You have been hired as a soc analyst to investigate a potential security breach at a company. The company has recently noticed unusual network activity and suspects that there may be a malicious process running on one of their computers. Your task is identifying the malicious process and gathering information about its activity.

## Tools:

  ### Volatility 2

## Q1: What is the process ID of the currently running malicious process?
* Mình sử dụng tool Volatility 3 để phân tích file mem đề bài cho
* Sử dụng plugin pslist để xem list các process
  ``python3 vol.py -f /mnt/c/Users/XIII/Downloads/c82-NintendoHunt/memdump.mem  windows.pslist > /mnt/c/Users/XIII/Downloads/c82-NintendoHunt/pslist.txt``
* Mình có một file các process

![Screenshot 2024-03-11 214624](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/f8740a3b-da29-4e7e-9c22-db688ea7ef3f)

* Một số thông tin process ID, parent process ID, image file name, create time, exit time
* Mình đã lướt xuống cả file thì thấy có một số file có extension khá kì lạ .exe.ex

![Screenshot 2024-03-11 214943](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/a34b5635-cbee-4b26-acb1-fe07f8e3c98c)

* process gì mà lại có extension kì lạ vậy, mình thử một số các PID nhưng có vẻ không phải process cần tìm
* Mình chú ý là các file này có chung parent PID là 4828, nên mình đã dùng grep để tìm các process có ID là 4824

![Screenshot 2024-03-11 215409](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/0abab44e-97cb-4e3e-bf02-36ee0223d720)

* Chú ý lại đề bài thì process cần tìm là running process nên không thể có exit time được nên mình đã grep cả N/A

![Screenshot 2024-03-11 215954](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/3969e8bf-fe81-491c-a7bc-e735ff529ef2)

* Mình đã hỏi GPT cách scan malware trong các process thì có lệnh plugin malfind tuy nhiên không tìm được gì
* Mình nghĩ phải export từng process ra và tìm xem process nào chứa malware, mình xem vid youtube hướng dẫn sử dụng volatility phân tích memory
https://www.youtube.com/watch?v=Uk3DEgY5Ue8
* sau khi dump ra các file thì mình đưa lên kiểm tra bằng các trang web check malware như là virustotal và mình thu được kết quả, file dump của process có id 8560 có 2 lỗ hổng bảo mật

 ![Screenshot 2024-03-15 131608](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/b45b287b-9931-4791-b5f5-9f897370726e)

 `Answer: 8560`

 ## Q2: What is the md5 hash hidden in the malicious process memory?
* Câu hỏi là mã md5 bị giấu trong memory của process nên mình đã sử dụng plugin memmap để dump ra file chứa memory của process 8560
* lúc trên mình dùng pslist dump thì chỉ thu được các thông tin như là tên process, pid, parent pid, còn memmap dump sẽ trả về những thông tin về bộ nhớ của các process

`python .\volatility3\vol.py -f .\memdump.mem windows.memmap.Memmap --pid 8560 --dump`

* Đoạn này mình dùng volatility trên powershell vì mình thấy chạy nhanh hơn và mình phải chạy đi chạy lại nhiều lần để thử câu lệnh 
* Sau khi dump ra thì mình được một file dump `pid.8560.dmp`, mình dùng strings trong linux để đọc file này, sau khi lướt một lúc thì mình tìm thấy một thông tin khả nghi là t.h.e fl.ag.is và một chuỗi có vẻ là base64, mình decode thì ra chuỗi `3a19697f29095bc289a96e4504679680`, mình dùng cipher identifier thì đúng là md5

![Screenshot 2024-03-15 134801](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/fd6ef444-13dd-4398-9bc5-ae163ff45814)

![Screenshot 2024-03-15 135112](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/a50d8d7a-8398-424d-a733-baf894f64c52)

`Answer: 3a19697f29095bc289a96e4504679680`

## Q3: What is the process name of the malicious process parent?
* Ở Q1 mình đã tìm ra được malicious process có pid là 8560 và parent pid của nó là 4824, tên của process có pid 4824 là explorer.exe

![Screenshot 2024-03-15 135601](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/77c88ab4-fe18-42c4-a444-32d71c207fe2)

`Answer: explorer.exe`

## Q4: What is the MAC address of this machine's default gateway?
* Mình đã dùng plugin registry.hivelist để kiểm tra list các hive
* Registry là một cơ sở dữ liệu hệ thống được sử dụng trong hệ điều hành Windows để lưu trữ cấu hình, cài đặt và thông tin khác về hệ thống và các ứng dụng. Registry thường được lưu trữ trên đĩa cứng trong các tập tin gọi là hive

`python .\volatility3\vol.py -f .\memdump.mem windows.registry.hivelist`

![Screenshot 2024-03-15 141309](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/e3d9b41d-465b-4875-a89d-eb0f40b224d1)

* Mình search được là địa chỉ mac của default gateway được lưu ở SOFTWARE\Microsoft\Windows NT\CurrentVersion\NetworkList\Signatures\Unmanaged\

![Screenshot 2024-03-15 141703](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/38da79c2-9fa9-4885-bbb5-c26c448bd411)

* Mình đã dùng plugin registry.printkey xem nội dung của key đó

`python3 volatility3/vol.py -f memdump.mem windows.registry.printkey --key "\SystemRoot\System32\Config\SOFTWARE\Microsoft\Windows NT\CurrentVersion\NetworkList\Signatures\Unmanaged\"`

* mình đã thử chạy rất nhiều lần, thử các cách fix nhưng kết quả vẫn không trả về nội dung
* mình đọc được trong comment của các bạn trong discord của cyberdefender là có vẻ như việc mình dùng volatility3 là không phù hợp, vì lab nitendo switch này được dựng nên phù hợp với vol2 như detail của lab đã recommend, nhưng mình không thể nào cài đặt được vol2 nên mình đã tìm giải pháp khác

![Screenshot 2024-03-16 000733](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/0efe7297-b7d7-4788-8734-42ee226cfbe0)

* trong discord những người đã hoàn thành lab recommend sử dụng regripper

`sudo apt install regripper`

* sau khi dump các hive ra file bằng lệnh `python .\volatility3\vol.py -f memdump.mem windows.registry.hivelist.HiveList --dump`, mình đã sử dụng regripper trên linux để xem network info tại file dump của \SystemRoot\System32\Config\SOFTWARE là registry.SOFTWARE.0xd38985eb3000.hive và tìm được DefaultGatewayMac

`regripper -r registry.SOFTWARE.0xd38985eb3000.hive -p networklist`

![Screenshot 2024-03-16 141848](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/1dde3031-8439-44e7-b987-6d80351dea08)

`Answer: 00:50:56:FE:D8:07`

## Q5: What is the name of the file that is hidden in the alternative data stream?
* Mình search alternative data stream
https://owasp.org/www-community/attacks/Windows_alternate_data_stream

![Screenshot 2024-03-16 144818](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/fe506970-d0e5-445c-8a20-89a51dabd2d5)

* Một "alternative data stream" (ADS) là một cơ chế trong hệ thống tệp NTFS của Windows, cho phép bạn gắn kết dữ liệu với một tệp nhất định mà không làm thay đổi kích thước hoặc thông tin về tệp chính
* đây cũng là một lỗ hổng bảo mật khi mà kẻ tấn công có thể che giấu malware bằng ads vì ads không thể xem bằng các trình xem file thông thường như window explorer
* vì ads có dạng :$DATA nên mình đã strings file memdump.mem và grep ":" và "txt" như answer format
* Mình đã tìm thấy đó là file yes.txt là ads của test.txt

![Screenshot 2024-03-16 151304](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/9b539f72-4f61-4e86-90ca-1a6e7037e03c)

`Answer: yes.txt`

## Q6: What is the full path of the browser cache created when the user visited "www.13cubed.com" ?
* Mình sử dụng symlink của vol3 nhưng không tìm được bất kì thứ gì, vậy nên mình đã phải tìm cách cài vol2 và sử dụng mftparser vì ở vol2 thông tin được hiển thị đầy đủ hơn
* sau khi tải được tool vol2 thì mọi thứ trở nên clear hơn rất nhiều

`vol.py -f memdump.mem --profile=Win10x64_17134 mftparser | grep 13cubed`

![Screenshot 2024-03-16 231518](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/8a7aab6c-991f-4186-9a1e-f3617c112439)

`Answer: C:\Users\CTF\AppData\Local\Packages\MICROS~1.MIC\AC\#!001\MICROS~1\Cache\AHF2COV9\13cubed[1].htm`



 





