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
* Nhìn ở giao thức SMB2 thì còn một số packet gần đó có PSEXESVC.exe là dịch vụ dùng để thực thi từ xa

![Screenshot 2024-03-01 135124](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/479a5072-143f-4bba-ad21-17e808804290)

`Answer: psexsvc`

## Q5: We need to know how the attacker installed the service on the compromised machine to understand the attacker's lateral movement tactics. This can help identify other affected systems. Which network share was used by PsExec to install the service on the target machine?
### Solution
* Mình tìm thấy packet này Tree Connect Request Tree: \\10.0.0.133\ADMIN$, gói packet sau đó đã trả response
* Mình thấy kể từ khi tạo file PSEXESVC.exe thì nó được gửi đi bằng cách share file qua địa chỉ IP 10.0.0.133\ADMIN$

![Screenshot 2024-03-01 142343](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/ba6f5745-e966-49b6-8d96-50f03e62ca6b)


*  gói tin này đang thực hiện một yêu cầu ghi đến tài nguyên chia sẻ ADMIN$ trên máy có địa chỉ IP là 10.0.0.133, có thể liên quan đến việc truy cập hoặc thao tác với tệp tin PSEXESVC.exe.

`Answer: ADMIN$`

## Q6: We must identify the network share used to communicate between the two machines. Which network share did PsExec use for communication?
* Mình thấy trước khi kết nối với 10.0.0.133\ADMIN$ thì đã kết nối với 10.0.0.133\IPC$

![Screenshot 2024-03-01 143229](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/5af6a181-e890-4fd6-b1de-df1b0ae5950f)

`Answer: IPC$`

## Q7: Now that we have a clearer picture of the attacker's activities on the compromised machine, it's important to identify any further lateral movement. What is the machine's hostname to which the attacker attempted to pivot within our network?
### Solution
* MÌnh tiếp tục tìm số lượng packet giao tiếp giữa các địa chỉ ip và đúng thứ hai là 130 và 131
* Gắn filter source và destination ip là 2 địa chỉ 130 và 131 và mình tìm thấy
![Screenshot 2024-03-01 145320](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/5746b4d4-8a3d-42ca-89a6-f9948800114d)

![Screenshot 2024-03-01 150600](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/3f634844-c365-4a83-bddf-ba986e3ace21)

`Answer: MARKETING-PC`


