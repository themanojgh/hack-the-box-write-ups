# 📝 Penetration Testing Write-up: EyeWitness Web Reconnaissance
**Target**: 10.129.239.74

**Objective**: Use EyeWitness to perform automated reconnaissance on multiple vHosts and identify accessible web services.

## 🔍 Scope & Host Configuration
```bash
app.inlanefreight.local  
dev.inlanefreight.local  
drupal-dev.inlanefreight.local  
drupal-qa.inlanefreight.local  
drupal-acc.inlanefreight.local  
drupal.inlanefreight.local  
blog.inlanefreight.local  
```
These were mapped to the target IP using the following command:
```bash
IP=10.129.239.74
printf "%s\t%s\n\n" "$IP" "app.inlanefreight.local dev.inlanefreight.local drupal-dev.inlanefreight.local drupal-qa.inlanefreight.local drupal-acc.inlanefreight.local drupal.inlanefreight.local blog.inlanefreight.local" | sudo tee -a /etc/hosts
```
A scope_list file was also created to keep track of the in-scope domains and IP.
![image](https://github.com/user-attachments/assets/31cf5292-c523-4e9d-bae9-580ecd57c6d9)

## ⚙️ Tool Setup: EyeWitness
Installed EyeWitness with:
```bash
sudo apt install eyewitness
```

Then executed the reconnaissance using:

```bash
eyewitness --web -x web_discovery.xml -d inlanefreight_eyewitness
```
- `--web`: Tells EyeWitness to target web services
- `-x`: Specifies an XML input file (such as from nmap or other tools)
- `-d`: Sets the output directory for reports and data

## 📁 Output
After execution, EyeWitness generated several files in the inlanefreight_eyewitness/ folder, including:
![image](https://github.com/user-attachments/assets/f09e99cb-85c2-4633-b426-7184784886e5)
