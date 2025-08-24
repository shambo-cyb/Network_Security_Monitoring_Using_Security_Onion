# Network_Security_Monitoring_Using_Security_Onion
In this cybersecurity lab, a simulated network environment is used to demonstrate two common cyber attacks: SQL injection and DNS data exfiltration. The network includes several virtual machines, such as a Kali Linux attacker machine and a vulnerable Metasploitable server. The first part of the lab involves the attacker using a web browser on the Kali machine to perform an SQL injection attack against the Metasploitable server's web application, successfully extracting sensitive credit card information from its MySQL database. Network logs from a Security Onion VM are then analyzed to identify and investigate the attack traffic.


In this cybersecurity lab, a simulated network environment is used to demonstrate two common cyber attacks: SQL injection and DNS data exfiltration. The network includes several virtual machines, such as a Kali Linux attacker machine and a vulnerable Metasploitable server. The first part of the lab involves the attacker using a web browser on the Kali machine to perform an SQL injection attack against the Metasploitable server's web application, successfully extracting sensitive credit card information from its MySQL database. Network logs from a Security Onion VM are then analyzed to identify and investigate the attack traffic.

The second part of the lab focuses on data exfiltration. The attacker, having gained access to a CyberOps Workstation, converts a text file (

confidential.txt) into a hexadecimal format. This hexadecimal data is then embedded within DNS queries and sent to the Metasploitable server, which is configured as a DNS server. The attacker uses a shell script to automate this process, effectively using the DNS service as a covert channel to leak the file's contents. The exfiltrated data is then retrieved, reassembled, and verified on the Kali machine.

## Environment of the setup 
<img width="600" height="310" alt="image" src="https://github.com/user-attachments/assets/ef74a7f1-7e99-47af-a9f3-c42866aacb21" />

