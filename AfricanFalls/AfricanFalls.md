## Q1

![ảnh](https://github.com/LDV-SpaceK/Cyberdefenders.org/assets/151914246/fb4d35ce-ef8f-46f3-ae51-3cd6c954eb75)

* mở file ad1.text

![ảnh](https://github.com/LDV-SpaceK/Cyberdefenders.org/assets/151914246/9c6c075e-27ac-4e7a-995a-071629ff1c11)

* có đoạn computed hashes:

`9471e69c95d8909ae60ddff30d50ffa1`

## Q2

![ảnh](https://github.com/LDV-SpaceK/Cyberdefenders.org/assets/151914246/8b769087-2280-474d-a2a7-9d696229be51)

* đề bài có nhắc đến lịch sử tìm kiếm đáng ngờ nên mình đã nghĩ đến file History của trình duyệt Chrome

![ảnh](https://github.com/LDV-SpaceK/Cyberdefenders.org/assets/151914246/0ele57cfc8-91ce-4f54-a743-4ce1e5d4335c)


* đây là file SQLite nên mình mở bằng DB browser và tìm thấy tìm kiếm đáng ngờ

![ảnh](https://github.com/LDV-SpaceK/Cyberdefenders.org/assets/151914246/141545aa-4e68-4e3e-824e-40fe9fdbe1d2)

* có vẻ người dùng này định crack password gì đó bằng cách sử dụng dictionary, ở đây là 10-million-password-list-top-100.txt

`password cracking lists`

## Q3

![ảnh](https://github.com/LDV-SpaceK/Cyberdefenders.org/assets/151914246/2d0d7e33-6a97-454f-a52c-eb1c5c30ecda)

* mình search được rằng FileZila sử dụng FTP nên mình đã đào sâu vào AppData/Roaming/FileZilla và tìm được file filezilla.xml và recentserver.xml đều connect đến một host

![ảnh](https://github.com/LDV-SpaceK/Cyberdefenders.org/assets/151914246/c06a314a-cdfd-4b27-a5d3-0e4565a3b043)

`192.168.1.20`

## Q4

![ảnh](https://github.com/LDV-SpaceK/Cyberdefenders.org/assets/151914246/9681c33e-37b0-4b7c-8df1-b9b60285c8ab)

* đề bài đề cập đến file password list đã bị xóa nên mình kiểm tra $Recycle.bin

![ảnh](https://github.com/LDV-SpaceK/Cyberdefenders.org/assets/151914246/cfe1ae95-8eab-4e1a-b4e3-1b53f220d844)

* file đã bị xóa là 10-million-password-list-top-100.txt vào thời gian

`2021-04-29 18:22:17 UTC`

## Q5

![ảnh](https://github.com/LDV-SpaceK/Cyberdefenders.org/assets/151914246/27fb5a15-4882-41a6-97fc-a2fadff85fa3)

* mình xem folder Prefetch của Windows vì thường file Prefetch sẽ lưu lại các lần chạy của chương trình

![ảnh](https://github.com/LDV-SpaceK/Cyberdefenders.org/assets/151914246/4b940725-2e81-4821-8ac2-001cf881b755)

* Tuy nhiên, chỉ có file prefetch của file setup tor browser nên có lẽ tor đã được cài đặt nhưng chưa khởi động lần nào

`0`

## Q6

![ảnh](https://github.com/LDV-SpaceK/Cyberdefenders.org/assets/151914246/8c2ea98a-e9aa-4c84-9cbf-94d02cff35bc)

* mình thấy trong lịch sử duyệt web của user John Doe có một địa chỉ mail

![ảnh](https://github.com/LDV-SpaceK/Cyberdefenders.org/assets/151914246/aa68631a-eb9c-481b-95e7-79656fe5361f)

`dreammaker82@protonmail.com`

## Q7

![ảnh](https://github.com/LDV-SpaceK/Cyberdefenders.org/assets/151914246/b736eb8e-88a7-4189-a72e-5f50f889014f)

* what is FQDN: https://www.techtarget.com/whatis/definition/fully-qualified-domain-name-FQDN
* vì máy này đang thực hiện hành vi quét port nên rất có khả năng sử dụng một số tool như nmap, ipscan... nên mình thử tìm xem có tool nào đã được chạy gần đây

![ảnh](https://github.com/LDV-SpaceK/Cyberdefenders.org/assets/151914246/ea0e74c2-2fc3-4d82-800a-d1a6f992bbcd)

* nmap đã được thực thi và mình sẽ đi xem các lệnh PS nào đã đc chạy

![ảnh](https://github.com/LDV-SpaceK/Cyberdefenders.org/assets/151914246/d85d6ca2-1533-491e-bcf9-f3bd641d263c)

* nmap và ping tới dfir.science

`dfir.science`

## Q8

![ảnh](https://github.com/LDV-SpaceK/Cyberdefenders.org/assets/151914246/f55431d4-dfef-4be1-8d1c-ae0b309a42f1)

* mình thử tìm bằng gg lens nhưng không khả quan lắm vì bức ảnh chỉ là một con đường với 2 bờ bên là cỏ nên mình đã thử xem metadata của bức ảnh

![ảnh](https://github.com/LDV-SpaceK/Cyberdefenders.org/assets/151914246/85a579bb-5ca3-414d-a74a-4eb3f2f2be96)

* mình có tọa độ của bức ảnh này được chụp nên đã thử search và tìm được đây thuộc lãnh thổ của Zambia

`Zambia`

## Q9

![ảnh](https://github.com/LDV-SpaceK/Cyberdefenders.org/assets/151914246/707b292c-4ce4-46d3-bcba-092846f3123e)

* mình check metadata của bức ảnh thì có vẻ bức ảnh này được chụp trực tiếp bằng điện thoại LG

![ảnh](https://github.com/LDV-SpaceK/Cyberdefenders.org/assets/151914246/88852c7b-09db-4cfe-bd47-7642ccb3b6c5)

* theo mình search được thì ảnh chụp bằng camera trên điện thoại sẽ được lưu ở /DCIM/Camera/

`Camera`

## Q10

![ảnh](https://github.com/LDV-SpaceK/Cyberdefenders.org/assets/151914246/d4194162-5544-4427-8290-e78475595d8b)

* `Anon:1001:aad3b435b51404eeaad3b435b51404ee:3DE1A36F6DDB8E036DFD75E8E20C4AF4:::`
* Giải thích:
    Username: Anon
    UserID: 1001
    LM Hash: aad3b435b51404eeaad3b435b51404ee
    NT Hash: 3DE1A36F6DDB8E036DFD75E8E20C4AF4

`This is the NT (New Technology) hash of the password. It is derived from the user's password and is used for authentication in modern Windows environments.`

* mình sử dụng tool online để decrypt mã hash này https://md5decrypt.net/en/Ntlm/

![ảnh](https://github.com/LDV-SpaceK/Cyberdefenders.org/assets/151914246/a482b387-b7d8-4d24-bd47-c499ecf0cc0c)

`AFR1CA!`

## Q11

![ảnh](https://github.com/LDV-SpaceK/Cyberdefenders.org/assets/151914246/b3e69283-5f59-4a12-ba76-477de6a705a1)

* mình tìm hiểu được là password login windows của user được lưu ở trong registry SAM và SYSTEM nên mình đã thử tìm bằng Registry Explorer và Autospy nhưng không thể xem được
* đến đây mình sử dụng một cách khác là dùng tool mimikatz để lấy mật khẩu hash

`lsadump::sam /system:C:\Users\XIII\Downloads\66-Africanfalls\DiskDigger\config\SYSTEM /sam:C:\Users\XIII\Down
loads\66-Africanfalls\DiskDigger\config\SAM`

![ảnh](https://github.com/LDV-SpaceK/Cyberdefenders.org/assets/151914246/a1fc1b3a-74b8-4bee-87a8-412042fee083)

* NT hash: `ecf53750b76cc9a62057ca85ff4c850e`

![ảnh](https://github.com/LDV-SpaceK/Cyberdefenders.org/assets/151914246/44c3a416-03e7-4569-9819-4378976e73c8)

`ctf2021`












