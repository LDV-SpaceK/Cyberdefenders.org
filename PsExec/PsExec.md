## Instructions:

  ### Uncompress the lab (pass: cyberdefenders.org)

## Scenario:

### Our Intrusion Detection System (IDS) has raised an alert, indicating suspicious lateral movement activity involving the use of PsExec. To effectively respond to this incident, your role as a SOC Analyst is to analyze the captured network traffic stored in a PCAP file.


## Tools:

  ### Wireshark

## File: 
[File](psexec-hun.pcapng)

## Q1: In order to effectively trace the attacker's activities within our network, can you determine the IP address of the machine where the attacker initially gained access?
### Solution
* Đầu tiên mình tìm tất cả các địa chỉ ip và tìm địa chỉ ip có số lần truy cập nhiều nhất

![Screenshot 2024-03-01 131756](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/cf914a43-2e4c-4831-b68d-d63b03af5698)

`Answer: 10.0.0.130`

## Q2: To fully comprehend the extent of the breach, can you determine the machine's hostname to which the attacker first pivoted?
### Solution
* Xem conversation của các địa chỉ ip

![Screenshot 2024-03-01 135856](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/a0d987d8-9bba-4d30-b9cb-e13c25edca78)

  
* Tìm ip máy tấn công đầu tiên là 10.0.0.133 và gắn vào filter
* Sau một hồi tìm kiếm thì mình đã tìm thấy Target name

![Screenshot 2024-03-01 133044](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/04b4bc99-a774-4d52-bfc1-f49409ee6955)

`Answer: SALES-PC`

## Q3: After identifying the initial entry point, it's crucial to understand how far the attacker has moved laterally within our network. Knowing the username of the account the attacker used for authentication will give us insights into the extent of the breach. What is the username utilized by the attacker for authentication?
### Solution
* Sau khi kiểm tra được tên máy chủ ở trên mình đã tiếp tục kiểm tra các packet ở dưới và tìm thấy username được sử dụng để tấn công

![Screenshot 2024-03-01 133100](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/17270f8f-eb84-48fe-a499-dbd40243deca)

`Answer: ssales`

## Q4: After figuring out how the attacker moved within our network, we need to know what they did on the target machine. What's the name of the service executable the attacker set up on the target?
### Solution
*Nhìn ở giao thức SMB2 thì còn một số packet gần đó có PSEXESVC.exe là dịch vụ dùng để thực thi từ xa

![Screenshot 2024-03-01 135124](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/479a5072-143f-4bba-ad21-17e808804290)

`Answer: psexsvc`

## Q5: We need to know how the attacker installed the service on the compromised machine to understand the attacker's lateral movement tactics. This can help identify other affected systems. Which network share was used by PsExec to install the service on the target machine?




