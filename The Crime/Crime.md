## Instructions:

  ### Uncompress the lab (pass: cyberdefenders.org)

## Scenario:

### We're currently in the midst of a murder investigation, and we've obtained the victim's phone as a key piece of evidence. After conducting interviews with witnesses and those in the victim's inner circle, your objective is to meticulously analyze the information we've gathered and diligently trace the evidence to piece together the sequence of events leading up to the incident.

## Tools:

  ### ALEAPP 
  ### sqlitebrowser

## Resources:

  ### Android-Forensics-References

### Mình sử dụng hoàn toàn bằng ALEAPP

![Screenshot 2024-03-02 173310](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/23c4a3d4-77ac-4c4e-9052-c556f41bd406)

* Mình đã học cách sử dụng ALEAPP ở https://www.youtube.com/watch?v=_cm1n0stVrA

## Q1: Based on the accounts of the witnesses and individuals close to the victim, it has become clear that the victim was interested in trading. This has led him to invest all of his money and acquire debt. Can you identify which trading application the victim primarily used on his phone?

* Câu hỏi hỏi app trade nào, mình check ngay app trên máy và tìm thấy olymp trade

![Screenshot 2024-03-02 164051](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/454cf91a-8f3c-4ce2-8d88-8a97e1d257db)

`Answer: Olymp Trade`

## Q2: According to the testimony of the victim's best friend, he said, "While we were together, my friend got several calls he avoided. He said he owed the caller a lot of money but couldn't repay now". How much does the victim owe this person?
* Có vẻ đây là một đoạn hội thoại nên mình đã check discord chat, tuy nhiên không có gì, chuyển sang SMS messages thì tìm thấy có vẻ anh này đang đi trốn nợ, khoảng tiền là 250000 EGP(Tiền ai cập thì phải)

![Screenshot 2024-03-02 164206](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/7d83ad05-dae3-446d-8477-1075f54345d1)

`Answer: 250000`

## Q3: What is the name of the person to whom the victim owes money?
* Hỏi tên nạn nhân
* nãy mình lướt qua contact thì thấy có danh sách các liên lạc
* SMS messages cũng có thông tin số điện thoại nên mình đã copy và so sánh với trong contact

![Screenshot 2024-03-02 164728](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/8c8ebcef-4ad3-47d1-a285-38b3ca65230a)

`Answer: Shady Wahab`

## Q4: Based on the statement from the victim's family, they said that on September 20, 2023, he departed from his residence without informing anyone of his destination. Where was the victim located at that moment?
* Mình check recent activity thì có một bức ảnh vào 20-09-2023 và địa điểm được ping là

![Screenshot 2024-03-02 165040](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/04ca1563-3c8f-48c9-b0d0-21d465977130)

`Answer: The Nile Ritz-Carlton`

## Q5: The detective continued his investigation by questioning the hotel lobby. She informed him that the victim had reserved the room for 10 days and had a flight scheduled thereafter. The investigator believes that the victim may have stored his ticket information on his phone. Look for where the victim intended to travel.
* Mình đọc các web history thì thấy người này đã hỏi cách để đổi lại lịch máy bay, tuy nhiên không có thông tin gì
* Mình đã quay lại discord thì thấy có đoạn message khả nghi nói về The Mob Museum

![Screenshot 2024-03-02 171003](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/c9882822-4191-41e6-917d-7e334e400252)

* Mình đã search địa điểm này và biết được nó ở Las Vegas

![Screenshot 2024-03-02 171024](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/521a3e14-c465-471f-b9ca-d267bf277b6c)

`Answer: Las Vegas`

## Q6: After examining the victim's Discord conversations, we discovered he had arranged to meet a friend at a specific location. Can you determine where this meeting was supposed to occur?
* Trong cuộc nói chuyện ở discord, mặc dù đã định đổi lịch phút chót nhưng trước đó 2 người đã hẹn nhau ở bảo tàng Mob

![Screenshot 2024-03-02 171003](https://github.com/LDV-SpaceK/CTF-Learning/assets/151914246/ae417ec2-8f9a-4dc8-a1a3-809ee6bacc37)

`Answer: The Mob Museum`




