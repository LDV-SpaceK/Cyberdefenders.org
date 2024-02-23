## Instructions:

  ### Uncompress the lab (pass: cyberdefenders.org)

## Scenario:

### During your shift as a tier-2 SOC analyst, you receive an escalation from a tier-1 analyst regarding a public-facing server. This server has been flagged for making outbound connections to multiple suspicious IPs. In response, you initiate the standard incident response protocol, which includes isolating the server from the network to prevent potential lateral movement or data exfiltration and obtaining a packet capture from the NSM utility for analysis. Your task is to analyze the pcap and assess for signs of malicious activity.

## File:
### File: [File](LabFile/c119-OpenWire.pcap)

## Web tham khảo:
  

## Tools:

  ### Wireshark

## Q1: By identifying the C2 IP, we can block traffic to and from this IP, helping to contain the breach and prevent further data exfiltration or command execution. Can you provide the IP of the C2 server that communicated with our server?
### Solution
* Mình mở file và thấy có 2 giao thức lạ nên đã search về Openwire protocol và biết được là đó là giao thức của bên ActiveMQ được kết nối với port 61616
* có hai địa chỉ ip giao tiếp với nhau dùng dạng protocol này nên mình đã thử và tìm được đáp án

![Q1](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/f94f36af-6830-4f89-9378-6f4427d35b9c)
`Answer: 146.190.21.92`

## Q2: Initial entry points are critical to trace back the attack vector. What is the port number of the service the adversary exploited?
### Solution
* Như mình đã tìm hiểu ở trên, giao thức này kết nối ở port 61616

`Answer: 61616`

## Q3: Following up on the previous question, what is the name of the service found to be vulnerable?
### Solution
* Có một packet tên là Malformed packet và service của nó là Openwire
* Tuy nhiên không hề giống với answer format nên mình đã thử Apache ActiveMQ là bên tạo ra giao thức Openwire
  ![Q3](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/523b766a-b1ef-490c-9a72-314c12c3b09c)
  `Answer: Apache ActiveMQ`

## Q4: The attacker's infrastructure often involves multiple components. What is the IP of the second C2 server?
### Solution
* Trong câu hỏi có nhắc đến còn có một địa chỉ ip của server nữa, tuy nhiên mình đã xem hầu hết cả file pcap chỉ có hai địa chỉ ip là của bên tấn công và ip của C2 server đã tìm được ở trên
* Nên mình đã sử dụng tính năng xem tất cả các địa chỉ IPv4 và tìm thấy thêm hai địa chỉ IP nữa là 84.239.49.16 và 128.199.52.72
* Tuy nhiên địa chỉ 84.239.49.16 có vẻ không liên quan nên mình đã thử địa chỉ còn lại
  ![Q4](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/b7c02544-9d2a-48d9-8b9c-2e9b30d8c44a)
`Answer: 128.199.52.72`

## Q5: Attackers usually leave traces on the disk. What is the name of the reverse shell executable dropped on the server?
### Solution
* Mình tìm thấy packet có trạng thái **HTTP /1.0 200 OK** nghĩa là yêu cầu được xử lí thành công và trả về dữ liệu
* Trong đó có đoạn code xml trong đó có sử dụng bash command để tải về, thay đổi quyền và chạy tệp docker
  ![Q5](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/e9ce77d1-c516-47fc-bf0d-9dec6ef30cd8)
`Answer: docker`

## Q6: What Java class was invoked by the XML file to run the exploit?
### Solution
* Cũng trong packet đó thì có lớp Java đã được khai báo để sử dụng
  **class="java.lang.ProcessBuilder**

`Answer: java.lang.ProcessBuilder`

## Q7: To better understand the specific security flaw exploited, can you identify the CVE identifier associated with this vulnerability?
### Solution
* Câu hỏi có đề cập đến CVE nên mình đã search về CVE thì nó chính là viết tắt của Common Vulnerabilities and Exposures, ngắn gọn thì đây là một danh sách các lỗ hổng bảo mật
* Mình đã dựa vào các đặc điểm của đoạn mã để search về loại CVE này
  **CVE reverse shell xml**
  ![Q7](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/fcc80085-6cac-4891-b59a-ca1f1edc1aa0)

* Mình đã tìm thấy github [link](https://github.com/rootsecdev/CVE-2023-46604 "Link Github") và [link](https://github.com/X1r0z/ActiveMQ-RCE "ActiveMQ-RCE")
* Và mình đã tìm thấy lỗ hổng đó là **CVE-2023-46604** RCE reverse shell Apache ActiveMQ
* Đây là một số link mình tìm thấy trên github giải thích về CVE này

  https://exp10it.io/2023/10/apache-activemq-版本-5.18.3-rce-分析

  https://attackerkb.com/topics/IHsgZDE3tS/cve-2023-46604/rapid7-analysis

`Answer: CVE-2023-46604`

## Q8: What is the vulnerable Java method and class that allows an attacker to run arbitrary code? (Format: Class.Method)
### Solution 
* Mình đã vào đọc các link ở trong github đó và tìm thấy thông tin class java được sử dụng
![Q8](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/fc5db21c-d302-44aa-9eee-1fea34f4a1bd)


   `Answer: BaseDataStreamMarshaller.createThrowable`
