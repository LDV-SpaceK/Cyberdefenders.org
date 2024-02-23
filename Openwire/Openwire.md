## Instructions:

  ### Uncompress the lab (pass: cyberdefenders.org)

## Scenario:

### During your shift as a tier-2 SOC analyst, you receive an escalation from a tier-1 analyst regarding a public-facing server. This server has been flagged for making outbound connections to multiple suspicious IPs. In response, you initiate the standard incident response protocol, which includes isolating the server from the network to prevent potential lateral movement or data exfiltration and obtaining a packet capture from the NSM utility for analysis. Your task is to analyze the pcap and assess for signs of malicious activity.

## File:
### File: [File](LabFile/c119-OpenWire.pcap)

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
