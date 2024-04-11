## Instructions:

* Uncompress the lab (pass: cyberdefenders.org)

* A windows forensics challenge prepared by Champlain College Digital Forensics Association for their yearly CTF.

* Windows Image Forensics Case created By AccessData® FTK® Imager 4.2.1.4 

* Acquired using: ADI4.2.1.4  

--------------------------------------------------------------

* Information for F:\DFA_Windows\DFA_SP2020_Windows:

  Physical Evidentiary Item (Source) Information:

    [Device Info]  
        Source Type: Physical
    [Drive Geometry]  
        Cylinders: 6,527  
        Heads: 255  
        Sectors per Track: 63  
        Bytes per Sector: 512  
        Sector Count: 104,857,600

  [Physical Drive Information]  

    Drive Interface Type: lsilogic [Image]  
    Image Type: VMWare Virtual Disk  
    Source data size: 51200 MB  
    Sector count:    104857600

 

  [Computed Hashes]  

    MD5 checksum:    e5fe043aa84454237438cdb2b78d08b3  
    SHA1 checksum:   ada83cd44e294ab840fa7acd77cf77e81c3431b3

 

* Image Information: 

    Segment list:  
        F:\DFA_Windows\DFA_SP2020_Windows.E01  
        F:\DFA_Windows\DFA_SP2020_Windows.E02  
        F:\DFA_Windows\DFA_SP2020_Windows.E03  
        F:\DFA_Windows\DFA_SP2020_Windows.E04  
        F:\DFA_Windows\DFA_SP2020_Windows.E05  
        F:\DFA_Windows\DFA_SP2020_Windows.E06  
        F:\DFA_Windows\DFA_SP2020_Windows.E07  
        F:\DFA_Windows\DFA_SP2020_Windows.E08  
        F:\DFA_Windows\DFA_SP2020_Windows.E09

* Your objective as a soc analyst is to analyze the image and answer the question.

## Tools: 

   * FTK Imager
   * Registry Explorer
   * RegRipper
   * HxD
   * DB Browser for SQLite
   * HindSight
   * Event Log Explorer
   * MFTDump

### Q1: What is the current build number on the system?
* Mình sử dụng FTK Imager 4.7.1.2 để phân tích file ad1, câu hỏi là phiên bản của hệ thống này là con số gì, thì mình đã tìm đến tệp cấu hình hệ thống Windows/System32/config và export file SYSTEM để phân tích
* Mình sử dụng Registry explorer v2.0.0.0 để phân tích các hive trong bài lab này
* Tại folder Software/Microsoft thì mình đã tìm thấy value BuildLab là 16299.rs3_release_svc.180808-1748

![Screenshot 2024-04-10 051444](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/c8aed16f-cf67-45cb-badc-16ab4ef8b97c)

`16299`

### Q2: How many users are there?
* folder Users chứa các người dùng 

![Screenshot 2024-04-10 051830](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/817f0d5b-3531-4e29-9a92-7d84dad1eda5)

`6`

### Q3: What is the CRC64 hash of the file "fruit_apricot.jpg"?
* mình tìm thấy ảnh này trong folder Pictures của user hansel.apricot, export rồi sử dụng tool online để tìm mã băm CRC-64 của ảnh
  https://toolkitbay.com/tkb/tool/CRC-64

![Screenshot 2024-04-10 052641](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/a54bde05-b4bf-4793-aa01-265ef8d44272)

`ED865AA6DFD756BF`

### Q4: What is the logical size of the file "strawberry.jpg" in bytes?
* file ảnh này nằm trong Pictues của user suzy.strawberry, export rồi xem properties của ảnh
* logical size của ảnh là size hiện lên cho chúng ta thấy, còn physical size là dung lượng file chiếm trong ổ đĩa

![Screenshot 2024-04-10 053444](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/f9b8ac7d-2a6b-4f6c-adde-1ff943939595)

`72448`

### Q5: What is the processor architecture of the system? (one word)
* Theo mình search thì thông tin của bộ xử lí thường được lưu trong HKEY_LOCAL_MACHINE\HARDWARE\DESCRIPTION\System\CentralProcessor, tuy nhiên trong đây mình không tìm thấy CentralProcessor
* quay lại thì đề bài cho hint là HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\Session Manager\Environment nên mình đã vào tìm, currentcontrolset chỉ có duy nhất là ControlSet001
* tại đó chứa các biến môi trường của hệ thống, value PROCESSOR_ARCHITECTURE chứa đáp án

![ảnh](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/1acd84c9-c4a5-455e-943f-183212a2fd9e)

`AMD64`

### Q6: Which user has a photo of a dog in their recycling bin?
* Tìm trong Recycle Bin tại folder S-1-5-21-2446097003-76624807-2828106174-1005 mình tìm thấy 2 bức ảnh

![ảnh](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/1848ed5f-a945-4fad-91e4-01643aed5f32)

* Export cả hai ảnh ra thì thấy $RGETALS.jpg là ảnh một chú chó

![$RGETALS](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/1eba6f21-6075-4e17-af41-102cbc6c9ccd)

* Đối chiếu tên folder với folder của user trong ProfileList thì đó là user hansel.apricot

![ảnh](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/f63e6f6e-eed5-41a2-9d0f-d6f9594fc881)

`hansel.apricot`

### Q7: What type of file is "vegetable"? Provide the extension without a dot.
* trong Pictures của user miriam.grapes có file vegetable không có extension, xem hex của nó thì là file 7z

![Screenshot 2024-04-10 065428](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/6bd89acf-ffe2-42a1-907d-6ba2a0fa863e)

`7z`

### Q8: What type of girls does Miriam Grapes design phones for (Target audience)?
* đầu tiên mình export file thisIsMyDesign.jpg thì thấy có vẻ đây là một thiết kế bảng mạch gì đấy

![thisIsMyDesign](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/f93cdd57-38e1-4091-b0a5-e9aefaa5a9fa)

* tuy nhiên điều này không nói lên điều gì cả nên mình đã xem còn chi tiết nào không
* mình thấy rằng user này sử dụng firefox nên mình đã thử xem lịch sử duyệt web cũng như có thể có một cuộc trò chuyện gì đấy liên quan đến mục tiêu

![Screenshot 2024-04-10 071156](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/f06f32fe-9650-4ef9-b948-24c7bd4d77a9)

* mình đã export places.sqlite để xem và tìm thấy lịch sử duyệt web của user này

![Screenshot 2024-04-10 070531](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/cf1ffc97-e03c-42e1-ad2d-69c884f7c131)

* what kind of phones do vsco girls enjoy - Google Search, có vẻ như đối tượng ở đây là vsco girl

`VSCO`

### Q9: What is the name of the device?
* Mình search được là tên của máy tính được lưu ở Control\ComputerName

![Screenshot 2024-04-10 201458](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/4a634dae-0ef9-4bc7-8ee2-d96d15641c25)

* Tại folder ComputerName mình tìm thấy tên của máy tính là DESKTOP-3A4NLVQ

![Screenshot 2024-04-10 201517](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/1bfec8a0-0d52-4609-9bd6-951b4717b7ac)

`DESKTOP-3A4NLVQ`

### Q10: What is the SID of the machine?
* Mình search được trên wiki thì SID của machine là S-1-5-21
 
![ảnh](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/43467e4a-b1c8-40f7-a55b-6948808f3316)

* Đối chiếu lại với sid của user thì mình tìm được
![Screenshot 2024-04-10 222001](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/01d403d4-cefb-44a6-ad95-d19cd497ff6a)

`S-1-5-21-2446097003-76624807-2828106174`

### Q11: How many web browsers are present?
* Mình tìm trong AppData của các user thì mình tìm thấy 5 loại trình duyệt là Tor, Chrome, Firefox, Edge và Internet explorer đã được cài đặt từ đầu

![Screenshot 2024-04-10 224214](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/1b3fa68e-793c-461b-b7a3-26ce1eda8c8a)
![Screenshot 2024-04-10 224206](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/baeb2e44-25b4-44cc-aef1-26ef0d39d5e8)
![Screenshot 2024-04-10 224156](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/17825a72-c3c9-4934-a49c-8eb3cda3ce49)
![Screenshot 2024-04-10 224147](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/b443ae0c-12c9-4d64-839f-c86e4c26cd19)
![Screenshot 2024-04-10 224131](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/d83c812a-c7a5-4f1b-9f5d-5565f3fdc81f)

`5`

### Q12: How many super secret CEO plans does Tim have? (Dr. Doofenshmirtz Type Beat)
* Mình tìm trong user tim thì thấy có file secret.odt sau khi mở lên thì có 3 kế hoạch

![Screenshot 2024-04-11 184342](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/a7392e4c-50bd-4398-bfbc-4b79f79809e7)

* Tuy nhiên, submit đáp án bị sai, nên mình đã thử bôi đen hết các kí tự trong trang xem có kí tự nào bị ẩn không, thì ở dòng dưới kế hoạch thứ 3 mình thấy có khoảng trống bị bôi đen, mình đổi màu chữ và được thêm 1 plan

![Screenshot 2024-04-11 184359](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/455a4b48-f2bd-45cf-8301-eee2a2acc1b3)

`4`

### Q13: Which employee does Tim plan to fire? (He's Dead, Tim. Enter the full name - two words - space separated)
* Trong file secret.odt có nhắc đến sa thải Jim Tomato

![Screenshot 2024-04-11 184359](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/eee4d38f-8754-44c1-a8eb-39af39290bb7)

`Jim Tomato`

### Q14: What was the last used username? (I didn't start this conversation, but I'm ending it!)
* Mình phải tìm một user khác ngoài những user trên, Windows NT\Winlogon là nơi quản lí những tác vụ khi đăng nhập của user, trong đó có lưu các user đã từng đăng nhập và user cuối cùng đăng nhập

![Screenshot 2024-04-11 185041](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/807761c0-58e6-4318-a55f-255662bad721)

`jim.tomato`

### Q15: What was the role of the employee Tim was flirting with?
* Mình xem lịch sử trình duyệt firefox của Tim thì thấy Tim search: `is it ok to flirt with my secretary`

![Screenshot 2024-04-11 190250](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/ad974a01-0f5f-4ef5-b74c-29d2e549bad1)

`secretary`

### Q16: What is the SID of the user "suzy.strawberry"?
* mình tìm thấy trong ProfileList thì sid của suzy là 1004

![ảnh](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/c73e4282-2dd7-4046-a84a-e74529b0ae11)

`1004`

### Q17: List the file path for the install location of the Tor Browser.
* check trong folder Program1 có folder TorBrowser

![ảnh](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/69d81edb-17f5-4ea0-bd7a-a22c0797ea8e)

* Mà root chứa folder Windows và thường thì Windows được đặt trong ổ C

`C:\Program1`

### Q18: What was the URL for the Youtube video watched by Jim?
* hướng đi là tìm link youtube nên mình nghĩ là phải tìm một file sqlite để xem lịch sử tìm kiếm của Jim
* Mình tìm thấy một file History, xem hex thì đúng là file sqlite

![ảnh](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/9a55449d-53ec-419e-b4bb-db7f24d099de)

* Mở file sqlite và mình thấy link youtube với nội dung là `How To Hack Into a Computer`

![Screenshot 2024-04-11 193002](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/699418fd-63eb-4433-b7c4-523191ff5c1c)

`https://www.youtube.com/watch?v=Y-CsIqTFEyY`

### Q19: Which user installed LibreCAD on the system?
* Mình tìm thấy trong folder Download của miriam

![ảnh](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/7cdfd5e6-2554-427a-b8bb-1408996d723c)

`miriam.grapes`

### Q20: How many times "admin" logged into the system?
* file SAM chứa dữ liệu người dùng nên mình đã xem và tìm thấy số lần đăng nhập của admin

![Screenshot 2024-04-11 203718](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/9b7fd343-0825-42a6-bf42-3609f6ae1cb1)

`10`

### Q21: What is the name of the DHCP domain the device was connected to?
* DHCP là viết tắt của "Dynamic Host Configuration Protocol" (Giao thức Cấu hình Máy Chủ Động). Đây là một giao thức mạng được sử dụng để tự động cấu hình các thiết bị mạng với các thông tin cần thiết để kết nối và hoạt động trên mạng
* thì mình check NetworkList trong Windows NT thì tìm thấy địa chỉ ip và domain của thiết bị

![Screenshot 2024-04-11 204248](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/0fece3d3-f1ea-4318-adad-4c54c677535e)

`fruitinc.xyz`

### Q22: What time did Tim download his background image? (Oh Boy 3AM . Answer in MM/DD/YYYY HH:MM format (UTC).)
* Mình tìm trong folder Pictures của Tim thì thấy một bức ảnh hqdefault.jpg

![ảnh](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/a6bbdf77-2750-4f3d-9d64-f52d14497d38)

`04/05/2020 03:49`

### Q23: How many times did Jim launch the Tor Browser?
* search `evidence of program execution`

![ảnh](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/555b1c7b-b9f6-4790-8d73-e92195b206b6)

* mình tìm thấy đường dẫn C:\Program1\Browser\firefox.exe và nhớ đến là Tor đã được cài đặt ở Program1 này

![ảnh](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/d722ad3c-bbb3-4fbc-b3ed-91d8d8c31db3)

`2`

### Q24: There is a png photo of an iPhone in Grapes's files. Find it and provide the SHA-1 hash.
* mình tìm thấy bức ảnh samplePhone.jpg của user Grapes, export và mở lên xem thì đó là một chiếc điện thoại gập, mình đã thử sha1sum rồi submit nhưng bị sai
* và đọc kĩ lại đề thì đây phải là ảnh của một chiếc Iphone nên mình đã dùng và foremost để lấy ra bức ảnh bị ẩn giấu trong samplePhone.jpg

![00000011](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/241c58de-1c95-4f04-b565-76b13edd16db)


![Screenshot 2024-04-11 215929](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/4fdcef7d-22de-4023-829a-497d4b47b5e6)

`537fe19a560ba3578d2f9095dc2f591489ff2cde`

### Q25: When was the last time a docx file was opened on the device? (An apple a day keeps the docx away. Answer in UTC, YYYY-MM-DD HH:MM:SS)
* search `what is hive of last opened doc files forensics`

![ảnh](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/bc85a9c4-149c-4be3-811b-dc9c99766ae6)

* mình tìm được recent doc

![ảnh](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/56cb8fc9-fb1b-4bcc-9dd7-ebadaa2cee77)

`2020-04-11 23:23:36`

### Q26: How many entries does the MFT of the filesystem have?
* mình sử dụng mftdump.exe để xem các entry của file hệ thống

![ảnh](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/e0eac4ae-51b2-4cd5-80c1-4fa96e54ed60)

* vì record bắt đầu từ 0 nên tổng số record sẽ là 219904

`219904`

### Q27: Tim wanted to fire an employee because they were ......?(Be careful what you wish for)
* mình nghĩ phải xem lịch sử web của Tim một lần nữa, lần trước là firefox, lần này có thể là chrome hoặc edge, mình đã thử file History của Chrome và tìm thấy lịch sử duyệt web khá đáng ngờ

![ảnh](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/4409d806-a6c3-407e-b08f-fc6c38765455)

* có vẻ vì nhân viên này quá bốc mùi nên Tim muốn sa thải

`stinky`

### Q28: What cloud service was a Startup item for the user admin?
* mình search autostart registry

![ảnh](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/df5730f8-b272-4866-a266-09b75713251c)

* xem file Run thì thấy data duy nhất là OneDrive

![ảnh](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/a89cae86-2acf-4694-8121-f29999211f8f)

`OneDrive`

### Q29: Which Firefox prefetch file has the most runtimes? (Flag format is <filename/#oftimesrun>)
*


























