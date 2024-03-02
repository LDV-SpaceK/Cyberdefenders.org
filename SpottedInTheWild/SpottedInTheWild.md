## Instructions:
  
  ### Uncompress the lab (pass: cyberdefenders.org)

## Scenario:

### You are part of the incident response team at FinTrust Bank. This morning, the network monitoring system flagged unusual outbound traffic patterns from several workstations. Preliminary analysis by the IT department has identified a potential compromise linked to an exploited vulnerability in WinRAR software.

### As an incident responder, your task is to investigate this compromised workstation to understand the scope of the breach, identify the malware, and trace its activities within the network.

## Tools:

  ### Arsenal Image Mounter
  ### SQLite Viewer
  ###  Eric Zimmerman Tools
  ###  NTFS Log Tracker
  ###  Registry Explorer
  ###  Event Log Explorer
  ###  Strings
  ###  CyberChef

## Q1: In your investigation into the FinTrust Bank breach, you found an application that was the entry point for the attack. Which application was used to download the malicious file?
### Solution
* Đầu tiên mình cũng chưa biết làm sao để mở file này nên mình đã mở bằng WinZip
* mở file vhd
* export nó ra thành một thư mục
* dùng ftk imager mở, tìm trong các user xem có gì không
* Tại Download của user administrator có telegram và trong thư mục đó có một file rar lạ

![Screenshot 2024-03-02 195620](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/6e86414e-faf9-4c99-9185-f1d624fae598)

`Answer: Telegram`
