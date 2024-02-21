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
![image](Image/Q1.png)
`Answer: FILESHAARE`

## Q2: We are investigating a network security incident. For a thorough investigation, we need to determine the IP address of the rogue machine. What is the IP address of the machine acting as the rogue entity?
### Solution
* mình filter llmnr, tìm được response của ip 192.168.232.215
![Q2](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/9a0d01da-8a69-4db2-b7b6-fc708cabd3a2)


## Q5: As part of our investigation, we aim to understand the extent of the attacker's activities. What is the hostname of the machine that the attacker accessed via SMB?
### Solution
* Vì đề bài có nhắc đến SMB nên mình đã cho filter SMB nhưng không tìm thấy tên của máy bị tấn công
* Lướt thấy có giao thức SMB2 nên mình đã filter SMB2 và tìm kiếm được Targer info
* Trong Target info có NetBIOS Computer Name: ACCOUNTINGPC
  ![image](Image/Q5.png)
  `Answer: ACCOUNTINGPC`
