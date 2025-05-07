# Attacking Common Applications - Skills Assessment III

During our penetration test our team found a Windows host running on the network and the corresponding credentials for the Administrator. 
It is required that we connect to the host and find the `hardcoded password` for the MSSQL service.

## Connect to the Target via RDP
After identifying the target machine `(IP: 10.129.95.200)` as a Windows host and obtaining valid credentials for the Administrator account, 
the first step in the assessment was to establish a **Remote Desktop Protocol (RDP)** connection. Using the built-in Remote Desktop Connection client on my local machine, 
I entered the IP address of the target and authenticated with the username `Administrator` and the password `xcyj8izxNVzhf4z`. Upon successful login, I gained access to the Windows environment, 
which allowed me to begin investigating the file system and installed services in search of the `MultimasterAPI.dll` file, where the `hardcoded MSSQL` credentials were expected to be located.

## Search for the File: MultimasterAPI.dll

Once inside the RDP session as the Administrator, I used the Windows search functionality to locate the `MultimasterAPI.dll` file. 
By navigating to "This PC" and initiating a search from the root directory, I was able to efficiently search the file system for the target file. 
The search results revealed that `MultimasterAPI.dll` was located in the `C:\inetpub\wwwroot\bin` directory. This path suggested that the file is part of a web application hosted on the server, 
likely served through IIS (Internet Information Services). With the file located, the next step was to examine its contents to extract the hardcoded MSSQL password.

![image](https://github.com/user-attachments/assets/4660db00-c8f8-4843-a106-2bb6eb1b2b11)

## Inspect the .dll File for Hardcoded Credentials

After locating the `MultimasterAPI.dll` file, I navigated to the `C:\TOOLS\` directory, where I found a reverse engineering tool named `dnSpy`—a popular .NET decompiler and debugger. 
I launched dnSpy and loaded the MultimasterAPI.dll file into the tool to begin the inspection process.

## Open the File in dnSpy

Using `dnSpy`’s intuitive interface, I browsed through the namespaces and class definitions until I came across a class named `ColleagueController`. 
Upon examining the code within this controller, I discovered a section responsible for database connectivity. This section contained a connection string with a hardcoded password for the MSSQL service embedded 
directly in the source.

![image](https://github.com/user-attachments/assets/5aaab0eb-08bc-4d82-9c7e-cea86f772e61)

