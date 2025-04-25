# ✅ HTB Challenge Write-Up: Privilege Escalation via IDOR and Blind XXE

**Target**: 83.136.254.243:49081
**Authentication**:

Username: `htb-student`

Password: `Academy_student!`
## 🎯 Objective:
Escalate privileges and exploit vulnerabilities to access the content of /flag.php.

## 🧭 Step-by-Step Exploitation:
### 1. Initial Access
- Accessed the target in a web browser and logged in using the provided credentials.

  ![image](https://github.com/user-attachments/assets/b67e0cd6-6f6a-450b-9939-1c4960d48fd2)

- Landed on a User Profile page.

![image](https://github.com/user-attachments/assets/7f79e399-91ee-4ec6-9ed2-ccb131c9747f)

### 2. User Information Disclosure via API
Intercepted the profile loading request with Burp Suite:

![image](https://github.com/user-attachments/assets/ee8e9c1d-700c-4ac5-87ed-511d9cee0739)

This API returned the profile details of the current user (`uid=74`), showing JSON content.

### 3. Insecure Direct Object Reference (IDOR) Exploitation
Using Burp Intruder, brute-forced `uid` values from 1 to 75

Discovered a user with `uid=52`, revealing:

![Screenshot 2025-04-25 111244](https://github.com/user-attachments/assets/8e2fcc85-ce14-44a5-9a25-9263d6de536a)

Edited the uid cookie in the browser to 52 and reloaded — successfully accessed the admin profile.

![Screenshot 2025-04-25 112044](https://github.com/user-attachments/assets/8b8afa18-4fc2-4156-a340-81d4c71c2eca)

### 4. Account Takeover via Password Reset
From the admin profile, reset the password and logged in as the administrator.

### 5. New Functionality Unlocked: Event Creation
Discovered a new "**Add Event**" page available to admin users.

![Screenshot 2025-04-25 112825](https://github.com/user-attachments/assets/b641829a-4c9a-4953-8587-c4215bd71c71)

Intercepted the event submission with Burp:

![Screenshot 2025-04-25 113056](https://github.com/user-attachments/assets/653aa614-67a9-434a-8de0-ea2e0066800e)

### 6. Blind XXE Exploit to Read Flag
Crafted a blind XXE payload to `base64-encode` and read `/flag.php`:
```bash
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE name [
  <!ENTITY thisisprinting SYSTEM "php://filter/convert.base64-encode/resource=/flag.php">
]>
<root>
  <name>&thisisprinting;</name>
  <details>test</details>
  <date>2025-04-25</date>
</root>
```
Server responded:
`Event 'PD9waHAgJGZsYWcgPSAiSFRCe200NTczcl93M2JfNDc3NGNrM3J9IjsgPz4K' has been created.`
![image](https://github.com/user-attachments/assets/dd995359-f0f5-4865-a811-7056170e0f92)

### 7. Flag Extraction
Decoded the base64 payload and got the flag.

### 💡 Vulnerabilities Exploited:
**IDOR (Insecure Direct Object Reference)** – API exposed user data without proper authorization.

**Privilege Escalation via Cookie Tampering** – Modified `uid` to access admin session.

**Account Takeover** – Reset admin credentials once access was gained.

**Blind XXE** – Abused event creation XML parser to extract sensitive file data using `php://filter`.
