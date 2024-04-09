![ảnh](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/a319c80d-af38-4881-b525-2bed80c64edf)## Instructions:

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















