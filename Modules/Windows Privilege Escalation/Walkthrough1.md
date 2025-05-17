## Situational Awareness
What is the IP address of the other NIC attached to the target host?
```bash
ipconfig /all
```
![image](https://github.com/user-attachments/assets/26cdad1b-236a-473e-82ae-f41dcf8cb1b6)

What executable other than cmd.exe is blocked by AppLocker?
```bash
Get-AppLockerPolicy -Effective | select -ExpandProperty RuleCollections
```
![image](https://github.com/user-attachments/assets/96d197df-0e0b-41c9-8a1a-666803104a78)

## Initial Enumeration
What non-default privilege does the htb-student user have?
![image](https://github.com/user-attachments/assets/160484cc-3379-4ac9-b05e-c868d60fc714)

Who is a member of the Backup Operators group?
![image](https://github.com/user-attachments/assets/378109a5-9bc3-436e-8a14-44d38f2f5048)

![image](https://github.com/user-attachments/assets/7dfa0584-42a7-4864-8239-1f9da4be8983)

What service is listening on port 8080 (service name not the executable)?
![image](https://github.com/user-attachments/assets/2189cc24-c959-4bb2-bd3e-2314fa994898)

![image](https://github.com/user-attachments/assets/7de6243b-c79d-435f-bafb-18d24a061729)

What user is logged in to the target host?
![image](https://github.com/user-attachments/assets/8c88529d-7684-4b45-a2b2-8a5914d531da)

What type of session does this user have?
(Answer in above screenshot)
