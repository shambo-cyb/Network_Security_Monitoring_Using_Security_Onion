# Network_Security_Monitoring_Using_Security_Onion
In this cybersecurity lab, a simulated network environment is used to demonstrate two common cyber attacks: SQL injection and DNS data exfiltration. The network includes several virtual machines, such as a Kali Linux attacker machine and a vulnerable Metasploitable server. The first part of the lab involves the attacker using a web browser on the Kali machine to perform an SQL injection attack against the Metasploitable server's web application, successfully extracting sensitive credit card information from its MySQL database. Network logs from a Security Onion VM are then analyzed to identify and investigate the attack traffic.


In this cybersecurity lab, a simulated network environment is used to demonstrate two common cyber attacks: SQL injection and DNS data exfiltration. The network includes several virtual machines, such as a Kali Linux attacker machine and a vulnerable Metasploitable server. The first part of the lab involves the attacker using a web browser on the Kali machine to perform an SQL injection attack against the Metasploitable server's web application, successfully extracting sensitive credit card information from its MySQL database. Network logs from a Security Onion VM are then analyzed to identify and investigate the attack traffic.

The second part of the lab focuses on data exfiltration. The attacker, having gained access to a CyberOps Workstation, converts a text file (

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




