## Challenge Description

### When you deal with an attacker, don’t always trust what you see

## Solution
* Đầu tiên mình dùng exiftool để kiểm tra xem file này có thông tin gì đặc biệt, thì không phát hiện có gì đặc biệt

![Screenshot 2024-03-19 101741](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/f81df155-e0df-44ca-a826-3b06eaf2d0e2)

* Sau đó mình binwalk thì thấy có rất nhiều file, trong đó có rất nhiều file như xml, html, pe, lzma,...

![Screenshot 2024-03-19 102932](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/b98bccec-b90d-4600-be50-e0ca0ec930fe)

* Nên mình đã nghĩ đến dùng mftparser trong tool volatility để xem cấu trúc các file trong đó một cách rõ ràng hơn
* Trước hết kiểm tra info của file `imageinfo`

![Screenshot 2024-03-19 101801](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/25fe2d47-d72d-488a-9d66-c22f1c5526f9)

* tiếp đến là sử dụng plugin mftparser

![Screenshot 2024-03-19 103423](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/1ad36f44-daeb-439a-a437-b7a94816236a)

* Tuy nhiên do quá nhiều, file nên mình đã thử đoán bằng cách grep flag xem có gì không vì ở bài elliot secret, flag format là flag{}

![Screenshot 2024-03-19 103702](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/481541a3-6f07-4c5f-a251-a7ff2cdf554f)

* Mình thấy có vài hàm void flag() nên mình đã thử mở rộng 50 dòng quanh output xem sao

`vol.py -f chall.raw --profile=Win7SP1x86_23418 mftparser | grep -C 50 flag`

![Screenshot 2024-03-19 104932](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/3de25ca3-2d88-44c8-9b0f-daab33d91749)

* Mình được đoạn hex này thì có vẻ như hàm này đang làm gì đó, có include <iostream>, đây là một đoạn code C++
* Mình cho vào notepad và chỉnh sửa để nhìn cho dễ thì thu được đoạn code

![Screenshot 2024-03-19 104855](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/18ba9e77-b2f7-4dd3-b3d2-8c3110120236)

* Đây chỉ là một nửa phần sau của flag nên mình nghĩ có thể đoạn flag còn lại cũng nằm trong một đoạn hexdump nào đó trong file này nên mình thử grep part1
* với từ khóa part1 thì không cho ra kết quả gì

`vol.py -f chall.raw --profile=Win7SP1x86_23418 mftparser | grep -C 30 part1`

* vì từ khóa tìm được là flag2 nên mình tiếp tục thử flag1, cũng không có kết quả nào được trả ra

`vol.py -f chall.raw --profile=Win7SP1x86_23418 mftparser | grep -C 30 flag1`

* mình thử tiếp từ khóa part và may mắn tìm được đoạn flag đầu

`vol.py -f chall.raw --profile=Win7SP1x86_23418 mftparser | grep -C 30 part`

![Screenshot 2024-03-19 105658](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/43bd2c76-92b0-4b4a-80dc-09b6f77887d0)

* Tổng hợp 2 flag thì chúng ta được:

`Answer: flag{Don0t_alw4ys_TRust_SYS_Prce33_4t4LL}`

## Toi da luyen duoc doi mat dai bang khi doc dong hexdump =))))))




















