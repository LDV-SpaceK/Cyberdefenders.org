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
* 



