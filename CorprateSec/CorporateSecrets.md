![ảnh](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/f560ab5e-01be-4ef8-90b1-f25604238b9e)![Screenshot 2024-04-10 201458](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/da261847-ebd9-4344-ba78-5be8092ea13e)![ảnh](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/a319c80d-af38-4881-b525-2bed80c64edf)## Instructions:

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













