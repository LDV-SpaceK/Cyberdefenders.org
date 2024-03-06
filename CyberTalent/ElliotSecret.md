## What is Elliot’s way of hiding his secret data?
### File: [elliot_secrets.bin](elliot_secrets.bin)

* Cảm nhận đầu tiên khi mình đọc challenge này là không có một chút thông tin hay hint gì :))
* Sau khi tải file về thì mình có exiftool thì đây là file elf, mình chạy thử file

   ``./elliot_secrets.bin``

  ![Screenshot 2024-03-06 184757](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/7ad52625-82bc-4566-a891-ee090af91190)

* mình đã thử mở file đó bằng IDA nhưng đọc không được thông tin gì
* Thử strings

  ``strings elliot_secrets.bin``

  ![Screenshot 2024-03-06 185552](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/9eaa653e-afaf-42e6-b331-9152318723e0)

* Mình phát hiện trong này có một file wave, sau đó mình thử binwalk nhưng không có gì, thử foremost thì ra được một tệp output, trong đó có một file 00000039.wav
* Mở lên nghe thử thì toàn thấy tiếng hú và vỗ tay
* Sau đó thì mình dùng Audacity và thử chỉnh các phổ âm thì có vẻ là không có gì

  ![Screenshot 2024-03-06 171414](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/d3c277db-f121-40a3-be58-a729fee92bf4)

* Mình đã search các tool dùng để xem data ẩn trong file audio và tìm được DeepSound

  ![Screenshot 2024-03-06 181831](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/fc2f0298-7d73-4a8b-83a9-c9af2549fa64)

* Sau khi đưa file đó vào, ấn giải nén thì có ô nhập password hiện ra, đến lúc này mình đã quay lại các bước trước xem còn thông tin gì không thì tất cả đều không được, mình đã searh `crack password in deepsound` thì hiện ra một github: https://github.com/openwall/john/blob/bleeding-jumbo/run/deepsound2john.py
* Mình git clone về và dùng với file wav

  ![Screenshot 2024-03-06 190637](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/0384cc66-40a3-4350-9aa3-61c254f10102)

* Có vẻ mật khẩu này đã bị mã hóa, mình thử dùng CyberChef nhưng không thu được gì
* Mình có thấy trong github kia còn có tool john để giải mã mật khẩu

![Screenshot 2024-03-06 175302](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/7e328ad8-2666-4c83-bd7f-0b544622f56c)

* Tìm hiểu ra thì tool này đưa vào một dictionary các hash password rồi so sánh với password bị hash của mình
* Mình đã đi tìm các diction hay được dùng và tìm được github này https://github.com/josuamarcelc/common-password-list

  ![Screenshot 2024-03-06 181728](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/956e7b85-5b5c-4030-b9d8-2c5dd0bac8cc)

* Sau khi clone tất cả các thư viện đấy về thì mình đã thử lệnh `john`

![Screenshot 2024-03-06 181743](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/7f8ec39a-f800-4e8b-b725-5cdc3fa657c5)

* Password tìm được là `ragerocks123`

![Screenshot 2024-03-06 191804](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/fd0e8a96-bafa-4490-9c46-b75c75c62b40)

* Nhập mật khẩu vào thì mình được một file 2312.pdf, mở lên thì được bức ảnh

![Screenshot 2024-03-06 191929](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/709e8a38-0869-4968-8b47-aacdc4693940)

* Mình lại thử strings và ngồi đọc lướt thì thấy có đoạn code JS

![Screenshot 2024-03-06 182018](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/1c7c1b75-8edd-4f00-a241-eccd4824d390)

* Mình đã thử chạy nó bằng web online nhưng không thể được, vậy nên mình đã ngồi sắp xếp lại code

![Screenshot 2024-03-06 182638](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/ae86e8a4-1776-4522-a608-a09855a38ae7)

* Sau khi sắp xếp lại thì mình thấy có các kí tự ở cuối các dòng thì ghép thử lại

`IZWGCZ33IFZGKVKMN5ZXIP3?`

* Có vẻ nó bị mã hóa nên mình đã lên CyberChef thử decode bằng base64, tuy nhiên không thu được gì, thử các base khác thì đến base32 mình đã tìm được flag

![Screenshot 2024-03-06 183017](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/41f8d2df-2c16-4436-aaa2-7d515846efd7)

``Answer: Flag{AreULost?}``
