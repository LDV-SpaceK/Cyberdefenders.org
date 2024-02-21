# Instructions:

   ## Uncompress the lab (pass: cyberdefenders.org)

# Scenario:

## Your organization's security team has detected a surge in suspicious network activity. There are concerns that LLMNR (Link-Local Multicast Name Resolution) and NBT-NS (NetBIOS Name Service) poisoning attacks may be occurring within your network. These attacks are known for exploiting these protocols to intercept network traffic and potentially compromise user credentials. Your task is to investigate the network logs and examine captured network traffic.


# Tools:

   ## Wireshark

# File:
File: [File](LabFile/poisoned_credentials.pcap)

## Q1: In the context of the incident described in the scenario, the attacker initiated their actions by taking advantage of benign network traffic from legitimate machines. Can you identify the specific mistyped query made by the machine with the IP address 192.168.232.162?
### Solution
* Mình lướt tìm thì thấy packet Name query NB được gửi từ IP 192.168.232.162
* Điều này sử dụng NBNS, một giao thức cũ có mục đích thu thập thông tin về tên NetBIOS trên mạng và được sử dụng ở đây vì máy tính cần gửi yêu cầu phát sóng để nhận được phản hồi cho yêu cầu sai của nó
![Q1](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/e2c8b565-5426-4940-ad67-d4817c75f3ff)

`Answer: FILESHAARE`

## Q2: We are investigating a network security incident. For a thorough investigation, we need to determine the IP address of the rogue machine. What is the IP address of the machine acting as the rogue entity?
### Solution
* mình filter llmnr, tìm được ip của 192.168.232.215 đang cố gắng giả fileshaare
* Ở đây kẻ tấn công đầu độc LLMNR 
![Q2](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/9a0d01da-8a69-4db2-b7b6-fc708cabd3a2)
`Answer: 192.168.232.215`

## Q3: During our investigation, it's crucial to identify all affected machines. What is the IP address of the second machine that received poisoned responses from the rogue machine?
### Solution
* Mình tiếp tục tìm thấy địa chỉ ip bị tấn công thứ 2 là 192.168.232.176

![Q3](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/781efe5b-5d54-4500-8012-70d8526e0698)
`Answer: 192.168.232.176`

## Q4: We suspect that user accounts may have been compromised. To assess this, we must determine the username associated with the compromised account. What is the username of the account that the attacker compromised?
### Solution
* Mình dùng filter string user và tìm thấy janesmith

![Q4](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/d0678616-b090-4ee1-8cb9-d963b2ab0772)
`Answer: janesmith`

## Q5: As part of our investigation, we aim to understand the extent of the attacker's activities. What is the hostname of the machine that the attacker accessed via SMB?
### Solution
* Vì đề bài có nhắc đến SMB nên mình đã cho filter SMB nhưng không tìm thấy tên của máy bị tấn công
* Lướt thấy có giao thức SMB2 nên mình đã filter SMB2 và tìm kiếm được Targer info
* Trong Target info có NetBIOS Computer Name: ACCOUNTINGPC
![Q5](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/99e60ae9-9e30-452a-9daa-fcf39448501f)

  `Answer: ACCOUNTINGPC`
