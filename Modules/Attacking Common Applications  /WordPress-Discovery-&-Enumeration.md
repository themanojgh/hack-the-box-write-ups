# WordPress - Discovery & Enumeration
## 🔍 Target Info
**IP Address**: 10.129.36.157

**Host (vHost)**: blog.inlanefreight.local

**Goal**: Find the flag.txt file through enumeration

## 🧭 Initial Setup
Add the following entry to `/etc/hosts` to resolve the virtual host locally:
```bash
echo "10.129.36.157 blog.inlanefreight.local" | sudo tee -a /etc/hosts
```

## 🧰 Tool Used: WPScan
Logged into `wpscan.com` and generated a free API token for scanning.

```bash
sudo wpscan --url http://blog.inlanefreight.local --enumerate --api-token CKFtS1LOOsbzSIcmHRy3aUjdLmuOXa0l22CjGthJwAc
_______________________________________________________________
         __          _______   _____
         \ \        / /  __ \ / ____|
          \ \  /\  / /| |__) | (___   ___  __ _ _ __ ®
           \ \/  \/ / |  ___/ \___ \ / __|/ _` | '_ \
            \  /\  /  | |     ____) | (__| (_| | | | |
             \/  \/   |_|    |_____/ \___|\__,_|_| |_|

         WordPress Security Scanner by the WPScan Team
                         Version 3.8.27
                               
       @_WPScan_, @ethicalhack3r, @erwan_lr, @firefart
_______________________________________________________________

[i] Updating the Database ...
[i] Update completed.

[+] URL: http://blog.inlanefreight.local/ [10.129.36.157]
[+] Started: Fri Apr 25 23:19:19 2025

Interesting Finding(s):

[+] Headers
 | Interesting Entry: Server: Apache/2.4.41 (Ubuntu)
 | Found By: Headers (Passive Detection)
 | Confidence: 100%

[+] XML-RPC seems to be enabled: http://blog.inlanefreight.local/xmlrpc.php
 | Found By: Direct Access (Aggressive Detection)
 | Confidence: 100%
 | References:
 |  - http://codex.wordpress.org/XML-RPC_Pingback_API
 |  - https://www.rapid7.com/db/modules/auxiliary/scanner/http/wordpress_ghost_scanner/
 |  - https://www.rapid7.com/db/modules/auxiliary/dos/http/wordpress_xmlrpc_dos/
 |  - https://www.rapid7.com/db/modules/auxiliary/scanner/http/wordpress_xmlrpc_login/
 |  - https://www.rapid7.com/db/modules/auxiliary/scanner/http/wordpress_pingback_access/

[+] WordPress readme found: http://blog.inlanefreight.local/readme.html
 | Found By: Direct Access (Aggressive Detection)
 | Confidence: 100%

[+] Upload directory has listing enabled: http://blog.inlanefreight.local/wp-content/uploads/
 | Found By: Direct Access (Aggressive Detection)
 | Confidence: 100%

[+] The external WP-Cron seems to be enabled: http://blog.inlanefreight.local/wp-cron.php
 | Found By: Direct Access (Aggressive Detection)
 | Confidence: 60%
 | References:
 |  - https://www.iplocation.net/defend-wordpress-from-ddos
 |  - https://github.com/wpscanteam/wpscan/issues/1299

[+] WordPress version 5.8 identified (Insecure, released on 2021-07-20).
 | Found By: Rss Generator (Passive Detection)
 |  - http://blog.inlanefreight.local/?feed=rss2, <generator>https://wordpress.org/?v=5.8</generator>
 |  - http://blog.inlanefreight.local/?feed=comments-rss2, <generator>https://wordpress.org/?v=5.8</generator>
 |
 | [!] 41 vulnerabilities identified:
 |
 | [!] Title: WordPress 5.4 to 5.8 - Data Exposure via REST API
 |     Fixed in: 5.8.1
 |     References:
 |      - https://wpscan.com/vulnerability/38dd7e87-9a22-48e2-bab1-dc79448ecdfb
 |      - https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2021-39200
 |      - https://wordpress.org/news/2021/09/wordpress-5-8-1-security-and-maintenance-release/
 |      - https://github.com/WordPress/wordpress-develop/commit/ca4765c62c65acb732b574a6761bf5fd84595706
 |      - https://github.com/WordPress/wordpress-develop/security/advisories/GHSA-m9hc-7v5q-x8q5
 |
 | [!] Title: WordPress 5.4 to 5.8 - Authenticated XSS in Block Editor
 |     Fixed in: 5.8.1
 |     References:
 |      - https://wpscan.com/vulnerability/5b754676-20f5-4478-8fd3-6bc383145811
 |      - https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2021-39201
 |      - https://wordpress.org/news/2021/09/wordpress-5-8-1-security-and-maintenance-release/
 |      - https://github.com/WordPress/wordpress-develop/security/advisories/GHSA-wh69-25hr-h94v
 |
 | [!] Title: WordPress 5.4 to 5.8 -  Lodash Library Update
 |     Fixed in: 5.8.1
 |     References:
 |      - https://wpscan.com/vulnerability/5d6789db-e320-494b-81bb-e678674f4199
 |      - https://wordpress.org/news/2021/09/wordpress-5-8-1-security-and-maintenance-release/
 |      - https://github.com/lodash/lodash/wiki/Changelog
 |      - https://github.com/WordPress/wordpress-develop/commit/fb7ecd92acef6c813c1fde6d9d24a21e02340689
 |
 | [!] Title: WordPress < 5.8.2 - Expired DST Root CA X3 Certificate
 |     Fixed in: 5.8.2
 |     References:
 |      - https://wpscan.com/vulnerability/cc23344a-5c91-414a-91e3-c46db614da8d
 |      - https://wordpress.org/news/2021/11/wordpress-5-8-2-security-and-maintenance-release/
 |      - https://core.trac.wordpress.org/ticket/54207
 |
 | [!] Title: WordPress < 5.8.3 - SQL Injection via WP_Query
 |     Fixed in: 5.8.3
 |     References:
 |      - https://wpscan.com/vulnerability/7f768bcf-ed33-4b22-b432-d1e7f95c1317
 |      - https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2022-21661
 |      - https://github.com/WordPress/wordpress-develop/security/advisories/GHSA-6676-cqfm-gw84
 |      - https://hackerone.com/reports/1378209
 |
 | [!] Title: WordPress < 5.8.3 - Author+ Stored XSS via Post Slugs
 |     Fixed in: 5.8.3
 |     References:
 |      - https://wpscan.com/vulnerability/dc6f04c2-7bf2-4a07-92b5-dd197e4d94c8
 |      - https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2022-21662
 |      - https://github.com/WordPress/wordpress-develop/security/advisories/GHSA-699q-3hj9-889w
 |      - https://hackerone.com/reports/425342
 |      - https://blog.sonarsource.com/wordpress-stored-xss-vulnerability
 |
 | [!] Title: WordPress 4.1-5.8.2 - SQL Injection via WP_Meta_Query
 |     Fixed in: 5.8.3
 |     References:
 |      - https://wpscan.com/vulnerability/24462ac4-7959-4575-97aa-a6dcceeae722
 |      - https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2022-21664
 |      - https://github.com/WordPress/wordpress-develop/security/advisories/GHSA-jp3p-gw8h-6x86
 |
 | [!] Title: WordPress < 5.8.3 - Super Admin Object Injection in Multisites
 |     Fixed in: 5.8.3
 |     References:
 |      - https://wpscan.com/vulnerability/008c21ab-3d7e-4d97-b6c3-db9d83f390a7
 |      - https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2022-21663
 |      - https://github.com/WordPress/wordpress-develop/security/advisories/GHSA-jmmq-m8p8-332h
 |      - https://hackerone.com/reports/541469
 |
 | [!] Title: WordPress < 5.9.2 - Prototype Pollution in jQuery
 |     Fixed in: 5.8.4
 |     References:
 |      - https://wpscan.com/vulnerability/1ac912c1-5e29-41ac-8f76-a062de254c09
 |      - https://wordpress.org/news/2022/03/wordpress-5-9-2-security-maintenance-release/
 |
 | [!] Title: WordPress < 5.9.2 / Gutenberg < 12.7.2 - Prototype Pollution via Gutenberg’s wordpress/url package
 |     Fixed in: 5.8.4
 |     References:
 |      - https://wpscan.com/vulnerability/6e61b246-5af1-4a4f-9ca8-a8c87eb2e499
 |      - https://wordpress.org/news/2022/03/wordpress-5-9-2-security-maintenance-release/
 |      - https://github.com/WordPress/gutenberg/pull/39365/files
 |
 | [!] Title: WP < 6.0.2 - Reflected Cross-Site Scripting
 |     Fixed in: 5.8.5
 |     References:
 |      - https://wpscan.com/vulnerability/622893b0-c2c4-4ee7-9fa1-4cecef6e36be
 |      - https://wordpress.org/news/2022/08/wordpress-6-0-2-security-and-maintenance-release/
 |
 | [!] Title: WP < 6.0.2 - Authenticated Stored Cross-Site Scripting
 |     Fixed in: 5.8.5
 |     References:
 |      - https://wpscan.com/vulnerability/3b1573d4-06b4-442b-bad5-872753118ee0
 |      - https://wordpress.org/news/2022/08/wordpress-6-0-2-security-and-maintenance-release/
 |
 | [!] Title: WP < 6.0.2 - SQLi via Link API
 |     Fixed in: 5.8.5
 |     References:
 |      - https://wpscan.com/vulnerability/601b0bf9-fed2-4675-aec7-fed3156a022f
 |      - https://wordpress.org/news/2022/08/wordpress-6-0-2-security-and-maintenance-release/
 |
 | [!] Title: WP < 6.0.3 - Stored XSS via wp-mail.php
 |     Fixed in: 5.8.6
 |     References:
 |      - https://wpscan.com/vulnerability/713bdc8b-ab7c-46d7-9847-305344a579c4
 |      - https://wordpress.org/news/2022/10/wordpress-6-0-3-security-release/
 |      - https://github.com/WordPress/wordpress-develop/commit/abf236fdaf94455e7bc6e30980cf70401003e283
 |
 | [!] Title: WP < 6.0.3 - Open Redirect via wp_nonce_ays
 |     Fixed in: 5.8.6
 |     References:
 |      - https://wpscan.com/vulnerability/926cd097-b36f-4d26-9c51-0dfab11c301b
 |      - https://wordpress.org/news/2022/10/wordpress-6-0-3-security-release/
 |      - https://github.com/WordPress/wordpress-develop/commit/506eee125953deb658307bb3005417cb83f32095
 |
 | [!] Title: WP < 6.0.3 - Email Address Disclosure via wp-mail.php
 |     Fixed in: 5.8.6
 |     References:
 |      - https://wpscan.com/vulnerability/c5675b59-4b1d-4f64-9876-068e05145431
 |      - https://wordpress.org/news/2022/10/wordpress-6-0-3-security-release/
 |      - https://github.com/WordPress/wordpress-develop/commit/5fcdee1b4d72f1150b7b762ef5fb39ab288c8d44
 |
 | [!] Title: WP < 6.0.3 - Reflected XSS via SQLi in Media Library
 |     Fixed in: 5.8.6
 |     References:
 |      - https://wpscan.com/vulnerability/cfd8b50d-16aa-4319-9c2d-b227365c2156
 |      - https://wordpress.org/news/2022/10/wordpress-6-0-3-security-release/
 |      - https://github.com/WordPress/wordpress-develop/commit/8836d4682264e8030067e07f2f953a0f66cb76cc
 |
 | [!] Title: WP < 6.0.3 - CSRF in wp-trackback.php
 |     Fixed in: 5.8.6
 |     References:
 |      - https://wpscan.com/vulnerability/b60a6557-ae78-465c-95bc-a78cf74a6dd0
 |      - https://wordpress.org/news/2022/10/wordpress-6-0-3-security-release/
 |      - https://github.com/WordPress/wordpress-develop/commit/a4f9ca17fae0b7d97ff807a3c234cf219810fae0
 |
 | [!] Title: WP < 6.0.3 - Stored XSS via the Customizer
 |     Fixed in: 5.8.6
 |     References:
 |      - https://wpscan.com/vulnerability/2787684c-aaef-4171-95b4-ee5048c74218
 |      - https://wordpress.org/news/2022/10/wordpress-6-0-3-security-release/
 |      - https://github.com/WordPress/wordpress-develop/commit/2ca28e49fc489a9bb3c9c9c0d8907a033fe056ef
 |
 | [!] Title: WP < 6.0.3 - Stored XSS via Comment Editing
 |     Fixed in: 5.8.6
 |     References:
 |      - https://wpscan.com/vulnerability/02d76d8e-9558-41a5-bdb6-3957dc31563b
 |      - https://wordpress.org/news/2022/10/wordpress-6-0-3-security-release/
 |      - https://github.com/WordPress/wordpress-develop/commit/89c8f7919460c31c0f259453b4ffb63fde9fa955
 |
 | [!] Title: WP < 6.0.3 - Content from Multipart Emails Leaked
 |     Fixed in: 5.8.6
 |     References:
 |      - https://wpscan.com/vulnerability/3f707e05-25f0-4566-88ed-d8d0aff3a872
 |      - https://wordpress.org/news/2022/10/wordpress-6-0-3-security-release/
 |      - https://github.com/WordPress/wordpress-develop/commit/3765886b4903b319764490d4ad5905bc5c310ef8
 |
 | [!] Title: WP < 6.0.3 - SQLi in WP_Date_Query
 |     Fixed in: 5.8.6
 |     References:
 |      - https://wpscan.com/vulnerability/1da03338-557f-4cb6-9a65-3379df4cce47
 |      - https://wordpress.org/news/2022/10/wordpress-6-0-3-security-release/
 |      - https://github.com/WordPress/wordpress-develop/commit/d815d2e8b2a7c2be6694b49276ba3eee5166c21f
 |
 | [!] Title: WP < 6.0.3 - Stored XSS via RSS Widget
 |     Fixed in: 5.8.6
 |     References:
 |      - https://wpscan.com/vulnerability/58d131f5-f376-4679-b604-2b888de71c5b
 |      - https://wordpress.org/news/2022/10/wordpress-6-0-3-security-release/
 |      - https://github.com/WordPress/wordpress-develop/commit/929cf3cb9580636f1ae3fe944b8faf8cca420492
 |
 | [!] Title: WP < 6.0.3 - Data Exposure via REST Terms/Tags Endpoint
 |     Fixed in: 5.8.6
 |     References:
 |      - https://wpscan.com/vulnerability/b27a8711-a0c0-4996-bd6a-01734702913e
 |      - https://wordpress.org/news/2022/10/wordpress-6-0-3-security-release/
 |      - https://github.com/WordPress/wordpress-develop/commit/ebaac57a9ac0174485c65de3d32ea56de2330d8e
 |
 | [!] Title: WP < 6.0.3 - Multiple Stored XSS via Gutenberg
 |     Fixed in: 5.8.6
 |     References:
 |      - https://wpscan.com/vulnerability/f513c8f6-2e1c-45ae-8a58-36b6518e2aa9
 |      - https://wordpress.org/news/2022/10/wordpress-6-0-3-security-release/
 |      - https://github.com/WordPress/gutenberg/pull/45045/files
 |
 | [!] Title: WP <= 6.2 - Unauthenticated Blind SSRF via DNS Rebinding
 |     References:
 |      - https://wpscan.com/vulnerability/c8814e6e-78b3-4f63-a1d3-6906a84c1f11
 |      - https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2022-3590
 |      - https://blog.sonarsource.com/wordpress-core-unauthenticated-blind-ssrf/
 |
 | [!] Title: WP < 6.2.1 - Directory Traversal via Translation Files
 |     Fixed in: 5.8.7
 |     References:
 |      - https://wpscan.com/vulnerability/2999613a-b8c8-4ec0-9164-5dfe63adf6e6
 |      - https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2023-2745
 |      - https://wordpress.org/news/2023/05/wordpress-6-2-1-maintenance-security-release/
 |
 | [!] Title: WP < 6.2.1 - Thumbnail Image Update via CSRF
 |     Fixed in: 5.8.7
 |     References:
 |      - https://wpscan.com/vulnerability/a03d744a-9839-4167-a356-3e7da0f1d532
 |      - https://wordpress.org/news/2023/05/wordpress-6-2-1-maintenance-security-release/
 |
 | [!] Title: WP < 6.2.1 - Contributor+ Stored XSS via Open Embed Auto Discovery
 |     Fixed in: 5.8.7
 |     References:
 |      - https://wpscan.com/vulnerability/3b574451-2852-4789-bc19-d5cc39948db5
 |      - https://wordpress.org/news/2023/05/wordpress-6-2-1-maintenance-security-release/
 |
 | [!] Title: WP < 6.2.2 - Shortcode Execution in User Generated Data
 |     Fixed in: 5.8.7
 |     References:
 |      - https://wpscan.com/vulnerability/ef289d46-ea83-4fa5-b003-0352c690fd89
 |      - https://wordpress.org/news/2023/05/wordpress-6-2-1-maintenance-security-release/
 |      - https://wordpress.org/news/2023/05/wordpress-6-2-2-security-release/
 |
 | [!] Title: WP < 6.2.1 - Contributor+ Content Injection
 |     Fixed in: 5.8.7
 |     References:
 |      - https://wpscan.com/vulnerability/1527ebdb-18bc-4f9d-9c20-8d729a628670
 |      - https://wordpress.org/news/2023/05/wordpress-6-2-1-maintenance-security-release/
 |
 | [!] Title: WP 5.6-6.3.1 - Reflected XSS via Application Password Requests
 |     Fixed in: 5.8.8
 |     References:
 |      - https://wpscan.com/vulnerability/da1419cc-d821-42d6-b648-bdb3c70d91f2
 |      - https://wordpress.org/news/2023/10/wordpress-6-3-2-maintenance-and-security-release/
 |
 | [!] Title: WP < 6.3.2 - Denial of Service via Cache Poisoning
 |     Fixed in: 5.8.8
 |     References:
 |      - https://wpscan.com/vulnerability/6d80e09d-34d5-4fda-81cb-e703d0e56e4f
 |      - https://wordpress.org/news/2023/10/wordpress-6-3-2-maintenance-and-security-release/
 |
 | [!] Title: WP < 6.3.2 - Subscriber+ Arbitrary Shortcode Execution
 |     Fixed in: 5.8.8
 |     References:
 |      - https://wpscan.com/vulnerability/3615aea0-90aa-4f9a-9792-078a90af7f59
 |      - https://wordpress.org/news/2023/10/wordpress-6-3-2-maintenance-and-security-release/
 |
 | [!] Title: WP < 6.3.2 - Contributor+ Comment Disclosure
 |     Fixed in: 5.8.8
 |     References:
 |      - https://wpscan.com/vulnerability/d35b2a3d-9b41-4b4f-8e87-1b8ccb370b9f
 |      - https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2023-39999
 |      - https://wordpress.org/news/2023/10/wordpress-6-3-2-maintenance-and-security-release/
 |
 | [!] Title: WP < 6.3.2 - Unauthenticated Post Author Email Disclosure
 |     Fixed in: 5.8.8
 |     References:
 |      - https://wpscan.com/vulnerability/19380917-4c27-4095-abf1-eba6f913b441
 |      - https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2023-5561
 |      - https://wpscan.com/blog/email-leak-oracle-vulnerability-addressed-in-wordpress-6-3-2/
 |      - https://wordpress.org/news/2023/10/wordpress-6-3-2-maintenance-and-security-release/
 |
 | [!] Title: WordPress < 6.4.3 - Deserialization of Untrusted Data
 |     Fixed in: 5.8.9
 |     References:
 |      - https://wpscan.com/vulnerability/5e9804e5-bbd4-4836-a5f0-b4388cc39225
 |      - https://wordpress.org/news/2024/01/wordpress-6-4-3-maintenance-and-security-release/
 |
 | [!] Title: WordPress < 6.4.3 - Admin+ PHP File Upload
 |     Fixed in: 5.8.9
 |     References:
 |      - https://wpscan.com/vulnerability/a8e12fbe-c70b-4078-9015-cf57a05bdd4a
 |      - https://wordpress.org/news/2024/01/wordpress-6-4-3-maintenance-and-security-release/
 |
 | [!] Title: WordPress < 6.5.5 - Contributor+ Stored XSS in HTML API
 |     Fixed in: 5.8.10
 |     References:
 |      - https://wpscan.com/vulnerability/2c63f136-4c1f-4093-9a8c-5e51f19eae28
 |      - https://wordpress.org/news/2024/06/wordpress-6-5-5/
 |
 | [!] Title: WordPress < 6.5.5 - Contributor+ Stored XSS in Template-Part Block
 |     Fixed in: 5.8.10
 |     References:
 |      - https://wpscan.com/vulnerability/7c448f6d-4531-4757-bff0-be9e3220bbbb
 |      - https://wordpress.org/news/2024/06/wordpress-6-5-5/
 |
 | [!] Title: WordPress < 6.5.5 - Contributor+ Path Traversal in Template-Part Block
 |     Fixed in: 5.8.10
 |     References:
 |      - https://wpscan.com/vulnerability/36232787-754a-4234-83d6-6ded5e80251c
 |      - https://wordpress.org/news/2024/06/wordpress-6-5-5/

[+] WordPress theme in use: transport-gravity
 | Location: http://blog.inlanefreight.local/wp-content/themes/transport-gravity/
 | Latest Version: 1.0.1 (up to date)
 | Last Updated: 2020-08-02T00:00:00.000Z
 | Readme: http://blog.inlanefreight.local/wp-content/themes/transport-gravity/readme.txt
 | [!] Directory listing is enabled
 | Style URL: http://blog.inlanefreight.local/wp-content/themes/transport-gravity/style.css
 | Style Name: Transport Gravity
 | Style URI: https://keonthemes.com/downloads/transport-gravity/
 | Description: Transport Gravity is an enhanced child theme of Business Gravity. Transport Gravity is made for tran...
 | Author: Keon Themes
 | Author URI: https://keonthemes.com/
 |
 | Found By: Css Style In Homepage (Passive Detection)
 | Confirmed By: Urls In Homepage (Passive Detection)
 |
 | Version: 1.0.1 (80% confidence)
 | Found By: Style (Passive Detection)
 |  - http://blog.inlanefreight.local/wp-content/themes/transport-gravity/style.css, Match: 'Version: 1.0.1'

[+] Enumerating Vulnerable Plugins (via Passive Methods)
[+] Checking Plugin Versions (via Passive and Aggressive Methods)

[i] Plugin(s) Identified:

[+] contact-form-7
 | Location: http://blog.inlanefreight.local/wp-content/plugins/contact-form-7/
 | Last Updated: 2025-04-10T06:47:00.000Z
 | [!] The version is out of date, the latest version is 6.0.6
 |
 | Found By: Urls In Homepage (Passive Detection)
 |
 | [!] 4 vulnerabilities identified:
 |
 | [!] Title: Contact Form 7 < 5.8.4 - Authenticated (Editor+) Arbitrary File Upload
 |     Fixed in: 5.8.4
 |     References:
 |      - https://wpscan.com/vulnerability/70e21d9a-b1e6-4083-bcd3-7c1c13fd5382
 |      - https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2023-6449
 |      - https://www.wordfence.com/threat-intel/vulnerabilities/id/5d7fb020-6acb-445e-a46b-bdb5aaf8f2b6
 |
 | [!] Title: Contact Form 7 < 5.9.2 - Reflected Cross-Site Scripting
 |     Fixed in: 5.9.2
 |     References:
 |      - https://wpscan.com/vulnerability/1c070a2c-2ab0-43bf-b10b-6575709918bc
 |      - https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2024-2242
 |      - https://www.wordfence.com/threat-intel/vulnerabilities/id/d5bf4972-424a-4470-a0bc-7dcc95378e0e
 |
 | [!] Title:  Contact Form 7 < 5.9.5 - Unauthenticated Open Redirect
 |     Fixed in: 5.9.5
 |     References:
 |      - https://wpscan.com/vulnerability/8bdcdb5a-9026-4157-8592-345df8fb1a17
 |      - https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2024-4704
 |
 | [!] Title: Contact Form 7 < 6.0.6 - Order Replay Vulnerability
 |     Fixed in: 6.0.6
 |     References:
 |      - https://wpscan.com/vulnerability/7dbafbe2-abbc-4191-a587-afa89c2f7421
 |      - https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2025-3247
 |      - https://www.wordfence.com/threat-intel/vulnerabilities/id/38257dbf-288e-4028-af65-85f5389888ac
 |
 | Version: 5.4.2 (90% confidence)
 | Found By: Query Parameter (Passive Detection)
 |  - http://blog.inlanefreight.local/wp-content/plugins/contact-form-7/includes/css/styles.css?ver=5.4.2
 | Confirmed By: Readme - Stable Tag (Aggressive Detection)
 |  - http://blog.inlanefreight.local/wp-content/plugins/contact-form-7/readme.txt

[+] mail-masta
 | Location: http://blog.inlanefreight.local/wp-content/plugins/mail-masta/
 | Latest Version: 1.0 (up to date)
 | Last Updated: 2014-09-19T07:52:00.000Z
 |
 | Found By: Urls In Homepage (Passive Detection)
 |
 | [!] 2 vulnerabilities identified:
 |
 | [!] Title: Mail Masta <= 1.0 - Unauthenticated Local File Inclusion (LFI)
 |     References:
 |      - https://wpscan.com/vulnerability/5136d5cf-43c7-4d09-bf14-75ff8b77bb44
 |      - https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2016-10956
 |      - https://www.exploit-db.com/exploits/40290/
 |      - https://www.exploit-db.com/exploits/50226/
 |      - https://cxsecurity.com/issue/WLB-2016080220
 |
 | [!] Title: Mail Masta 1.0 - Multiple SQL Injection
 |     References:
 |      - https://wpscan.com/vulnerability/c992d921-4f5a-403a-9482-3131c69e383a
 |      - https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-6095
 |      - https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-6096
 |      - https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-6097
 |      - https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-6098
 |      - https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-6570
 |      - https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-6571
 |      - https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-6572
 |      - https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-6573
 |      - https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-6574
 |      - https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-6575
 |      - https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-6576
 |      - https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-6577
 |      - https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-6578
 |      - https://www.exploit-db.com/exploits/41438/
 |      - https://github.com/hamkovic/Mail-Masta-Wordpress-Plugin
 |
 | Version: 1.0 (80% confidence)
 | Found By: Readme - Stable Tag (Aggressive Detection)
 |  - http://blog.inlanefreight.local/wp-content/plugins/mail-masta/readme.txt

[+] Enumerating Vulnerable Themes (via Passive and Aggressive Methods)
 Checking Known Locations - Time: 00:00:33 <> (652 / 652) 100.00% Time: 00:00:33
[+] Checking Theme Versions (via Passive and Aggressive Methods)

[i] No themes Found.

[+] Enumerating Timthumbs (via Passive and Aggressive Methods)
 Checking Known Locations - Time: 00:00:51 <> (1001 / 2575) 38.87%  ETA: 00:01:2 Checking Known Locations - Time: 00:00:52 <> (1006 / 2575) 39.06%  ETA: 00:01:2 Checking Known Locations - Time: 00:00:52 <> (1011 / 2575) 39.26%  ETA: 00:01:2 Checking Known Locations - Time: 00:00:52 <> (1016 / 2575) 39.45%  ETA: 00:01:2 Checking Known Locations - Time: 00:00:52 <> (1021 / 2575) 39.65%  ETA: 00:01:2 Checking Known Locations - Time: 00:00:53 <> (1026 / 2575) 39.84%  ETA: 00:01:2 Checking Known Locations - Time: 00:00:53 <> (1031 / 2575) 40.03%  ETA: 00:01:2 Checking Known Locations - Time: 00:00:53 <> (1032 / 2575) 40.07%  ETA: 00:01:2 Checking Known Locations - Time: 00:00:53 <> (1036 / 2575) 40.23%  ETA: 00:01:2 Checking Known Locations - Time: 00:00:53 <> (1041 / 2575) 40.42%  ETA: 00:01:2 Checking Known Locations - Time: 00:00:54 <> (1046 / 2575) 40.62%  ETA: 00:01:1 Checking Known Locations - Time: 00:00:54 <> (1051 / 2575) 40.81%  ETA: 00:01:1 Checking Known Locations - Time: 00:00:54 <> (1056 / 2575) 41.00%  ETA: 00:01:1 Checking Known Locations - Time: 00:00:54 <> (1061 / 2575) 41.20%  ETA: 00:01:1 Checking Known Locations - Time: 00:00:55 <> (1066 / 2575) 41.39%  ETA: 00:01:1 Checking Known Locations - Time: 00:00:55 <> (1071 / 2575) 41.59%  ETA: 00:01:1 Checking Known Locations - Time: 00:00:55 <> (1076 / 2575) 41.78%  ETA: 00:01:1 Checking Known Locations - Time: 00:00:55 <> (1081 / 2575) 41.98%  ETA: 00:01:1 Checking Known Locations - Time: 00:00:56 <> (1086 / 2575) 42.17%  ETA: 00:01:1 Checking Known Locations - Time: 00:00:56 <> (1091 / 2575) 42.36%  ETA: 00:01:1 Checking Known Locations - Time: 00:00:56 <> (1096 / 2575) 42.56%  ETA: 00:01:1 Checking Known Locations - Time: 00:00:56 <> (1101 / 2575) 42.75%  ETA: 00:01:1 Checking Known Locations - Time: 00:00:57 <> (1106 / 2575) 42.95%  ETA: 00:01:1 Checking Known Locations - Time: 00:00:57 <> (1111 / 2575) 43.14%  ETA: 00:01:1 Checking Known Locations - Time: 00:00:57 <> (1116 / 2575) 43.33%  ETA: 00:01:1 Checking Known Locations - Time: 00:00:57 <> (1121 / 2575) 43.53%  ETA: 00:01:1 Checking Known Locations - Time: 00:00:58 <> (1126 / 2575) 43.72%  ETA: 00:01:1 Checking Known Locations - Time: 00:00:58 <> (1131 / 2575) 43.92%  ETA: 00:01:1 Checking Known Locations - Time: 00:00:58 <> (1136 / 2575) 44.11%  ETA: 00:01:1 Checking Known Locations - Time: 00:00:58 <> (1141 / 2575) 44.31%  ETA: 00:01:1 Checking Known Locations - Time: 00:00:59 <> (1146 / 2575) 44.50%  ETA: 00:01:1 Checking Known Locations - Time: 00:00:59 <> (1151 / 2575) 44.69%  ETA: 00:01:1 Checking Known Locations - Time: 00:00:59 <> (1156 / 2575) 44.89%  ETA: 00:01:1 Checking Known Locations - Time: 00:00:59 <> (1161 / 2575) 45.08%  ETA: 00:01:1 Checking Known Locations - Time: 00:01:00 <> (1166 / 2575) 45.28%  ETA: 00:01:1 Checking Known Locations - Time: 00:01:00 <> (1171 / 2575) 45.47%  ETA: 00:01:1 Checking Known Locations - Time: 00:01:00 <> (1176 / 2575) 45.66%  ETA: 00:01:1 Checking Known Locations - Time: 00:01:00 <> (1181 / 2575) 45.86%  ETA: 00:01:1 Checking Known Locations - Time: 00:01:01 <> (1186 / 2575) 46.05%  ETA: 00:01:1 Checking Known Locations - Time: 00:01:01 <> (1191 / 2575) 46.25%  ETA: 00:01:1 Checking Known Locations - Time: 00:01:01 <> (1196 / 2575) 46.44%  ETA: 00:01:1 Checking Known Locations - Time: 00:01:01 <> (1201 / 2575) 46.64%  ETA: 00:01:1 Checking Known Locations - Time: 00:01:02 <> (1206 / 2575) 46.83%  ETA: 00:01:1 Checking Known Locations - Time: 00:01:02 <> (1211 / 2575) 47.02%  ETA: 00:01:1 Checking Known Locations - Time: 00:01:02 <> (1216 / 2575) 47.22%  ETA: 00:01:1 Checking Known Locations - Time: 00:01:02 <> (1221 / 2575) 47.41%  ETA: 00:01:1 Checking Known Locations - Time: 00:01:03 <> (1226 / 2575) 47.61%  ETA: 00:01:1 Checking Known Locations - Time: 00:01:03 <> (1231 / 2575) 47.80%  ETA: 00:01:0 Checking Known Locations - Time: 00:01:03 <> (1236 / 2575) 48.00%  ETA: 00:01:0 Checking Known Locations - Time: 00:01:04 <> (1241 / 2575) 48.19%  ETA: 00:01:0 Checking Known Locations - Time: 00:01:04 <> (1246 / 2575) 48.38%  ETA: 00:01:0 Checking Known Locations - Time: 00:01:04 <> (1251 / 2575) 48.58%  ETA: 00:01:0 Checking Known Locations - Time: 00:01:04 <> (1256 / 2575) 48.77%  ETA: 00:01:0 Checking Known Locations - Time: 00:01:05 <> (1261 / 2575) 48.97%  ETA: 00:01:0 Checking Known Locations - Time: 00:01:05 <> (1266 / 2575) 49.16%  ETA: 00:01:0 Checking Known Locations - Time: 00:01:05 <> (1271 / 2575) 49.35%  ETA: 00:01:0 Checking Known Locations - Time: 00:01:05 <> (1276 / 2575) 49.55%  ETA: 00:01:0 Checking Known Locations - Time: 00:01:06 <> (1281 / 2575) 49.74%  ETA: 00:01:0 Checking Known Locations - Time: 00:01:06 <> (1286 / 2575) 49.94%  ETA: 00:01:0 Checking Known Locations - Time: 00:01:06 <> (1291 / 2575) 50.13%  ETA: 00:01:0 Checking Known Locations - Time: 00:01:06 <> (1296 / 2575) 50.33%  ETA: 00:01:0 Checking Known Locations - Time: 00:01:07 <> (1301 / 2575) 50.52%  ETA: 00:01:0 Checking Known Locations - Time: 00:01:07 <> (1306 / 2575) 50.71%  ETA: 00:01:0 Checking Known Locations - Time: 00:01:07 <> (1311 / 2575) 50.91%  ETA: 00:01:0 Checking Known Locations - Time: 00:01:07 <> (1316 / 2575) 51.10%  ETA: 00:01:0 Checking Known Locations - Time: 00:01:08 <> (1321 / 2575) 51.30%  ETA: 00:01:0 Checking Known Locations - Time: 00:01:08 <> (1326 / 2575) 51.49%  ETA: 00:01:0 Checking Known Locations - Time: 00:01:08 <> (1331 / 2575) 51.68%  ETA: 00:01:0 Checking Known Locations - Time: 00:01:08 <> (1336 / 2575) 51.88%  ETA: 00:01:0 Checking Known Locations - Time: 00:01:09 <> (1341 / 2575) 52.07%  ETA: 00:01:0 Checking Known Locations - Time: 00:01:09 <> (1346 / 2575) 52.27%  ETA: 00:01:0 Checking Known Locations - Time: 00:01:09 <> (1351 / 2575) 52.46%  ETA: 00:01:0 Checking Known Locations - Time: 00:01:09 <> (1356 / 2575) 52.66%  ETA: 00:01:0 Checking Known Locations - Time: 00:01:10 <> (1361 / 2575) 52.85%  ETA: 00:01:0 Checking Known Locations - Time: 00:01:10 <> (1366 / 2575) 53.04%  ETA: 00:01:0 Checking Known Locations - Time: 00:01:10 <> (1369 / 2575) 53.16%  ETA: 00:01:0 Checking Known Locations - Time: 00:01:10 <> (1371 / 2575) 53.24%  ETA: 00:01:0 Checking Known Locations - Time: 00:01:11 <> (1376 / 2575) 53.43%  ETA: 00:01:0 Checking Known Locations - Time: 00:01:11 <> (1381 / 2575) 53.63%  ETA: 00:01:0 Checking Known Locations - Time: 00:01:11 <> (1386 / 2575) 53.82%  ETA: 00:01:0 Checking Known Locations - Time: 00:01:11 <> (1391 / 2575) 54.01%  ETA: 00:01:0 Checking Known Locations - Time: 00:01:12 <> (1396 / 2575) 54.21%  ETA: 00:01:0 Checking Known Locations - Time: 00:01:12 <> (1401 / 2575) 54.40%  ETA: 00:01:0 Checking Known Locations - Time: 00:01:12 <> (1406 / 2575) 54.60%  ETA: 00:01:0 Checking Known Locations - Time: 00:01:12 <> (1411 / 2575) 54.79%  ETA: 00:01:0 Checking Known Locations - Time: 00:01:13 <> (1416 / 2575) 54.99%  ETA: 00:01:0 Checking Known Locations - Time: 00:01:13 <> (1421 / 2575) 55.18%  ETA: 00:01:0 Checking Known Locations - Time: 00:01:13 <> (1426 / 2575) 55.37%  ETA: 00:00:5 Checking Known Locations - Time: 00:01:13 <> (1431 / 2575) 55.57%  ETA: 00:00:5 Checking Known Locations - Time: 00:01:14 <> (1436 / 2575) 55.76%  ETA: 00:00:5 Checking Known Locations - Time: 00:01:14 <> (1441 / 2575) 55.96%  ETA: 00:00:5 Checking Known Locations - Time: 00:01:14 <> (1446 / 2575) 56.15%  ETA: 00:00:5 Checking Known Locations - Time: 00:01:14 <> (1451 / 2575) 56.34%  ETA: 00:00:5 Checking Known Locations - Time: 00:01:15 <> (1456 / 2575) 56.54%  ETA: 00:00:5 Checking Known Locations - Time: 00:01:15 <> (1461 / 2575) 56.73%  ETA: 00:00:5 Checking Known Locations - Time: 00:01:15 <> (1466 / 2575) 56.93%  ETA: 00:00:5 Checking Known Locations - Time: 00:01:15 <> (1471 / 2575) 57.12%  ETA: 00:00:5 Checking Known Locations - Time: 00:01:16 <> (1476 / 2575) 57.32%  ETA: 00:00:5 Checking Known Locations - Time: 00:01:16 <> (1481 / 2575) 57.51%  ETA: 00:00:5 Checking Known Locations - Time: 00:01:16 <> (1486 / 2575) 57.70%  ETA: 00:00:5 Checking Known Locations - Time: 00:01:16 <> (1491 / 2575) 57.90%  ETA: 00:00:5 Checking Known Locations - Time: 00:01:17 <> (1496 / 2575) 58.09%  ETA: 00:00:5 Checking Known Locations - Time: 00:01:17 <> (1501 / 2575) 58.29%  ETA: 00:00:5 Checking Known Locations - Time: 00:01:17 <> (1506 / 2575) 58.48%  ETA: 00:00:5 Checking Known Locations - Time: 00:01:17 <> (1511 / 2575) 58.67%  ETA: 00:00:5 Checking Known Locations - Time: 00:01:18 <> (1516 / 2575) 58.87%  ETA: 00:00:5 Checking Known Locations - Time: 00:01:18 <> (1521 / 2575) 59.06%  ETA: 00:00:5 Checking Known Locations - Time: 00:01:18 <> (1526 / 2575) 59.26%  ETA: 00:00:5 Checking Known Locations - Time: 00:01:18 <> (1531 / 2575) 59.45%  ETA: 00:00:5 Checking Known Locations - Time: 00:01:19 <> (1536 / 2575) 59.65%  ETA: 00:00:5 Checking Known Locations - Time: 00:01:19 <> (1541 / 2575) 59.84%  ETA: 00:00:5 Checking Known Locations - Time: 00:01:19 <> (1546 / 2575) 60.03%  ETA: 00:00:5 Checking Known Locations - Time: 00:01:19 <> (1551 / 2575) 60.23%  ETA: 00:00:5 Checking Known Locations - Time: 00:01:20 <> (1556 / 2575) 60.42%  ETA: 00:00:5 Checking Known Locations - Time: 00:01:20 <> (1561 / 2575) 60.62%  ETA: 00:00:5 Checking Known Locations - Time: 00:01:20 <> (1566 / 2575) 60.81%  ETA: 00:00:5 Checking Known Locations - Time: 00:01:20 <> (1570 / 2575) 60.97%  ETA: 00:00:5 Checking Known Locations - Time: 00:01:20 <> (1571 / 2575) 61.00%  ETA: 00:00:5 Checking Known Locations - Time: 00:01:20 <> (1575 / 2575) 61.16%  ETA: 00:00:5 Checking Known Locations - Time: 00:01:21 <> (1576 / 2575) 61.20%  ETA: 00:00:5 Checking Known Locations - Time: 00:01:21 <> (1581 / 2575) 61.39%  ETA: 00:00:5 Checking Known Locations - Time: 00:01:21 <> (1585 / 2575) 61.55%  ETA: 00:00:5 Checking Known Locations - Time: 00:01:21 <> (1586 / 2575) 61.59%  ETA: 00:00:5 Checking Known Locations - Time: 00:01:21 <> (1590 / 2575) 61.74%  ETA: 00:00:5 Checking Known Locations - Time: 00:01:21 <> (1591 / 2575) 61.78%  ETA: 00:00:5 Checking Known Locations - Time: 00:01:21 <> (1595 / 2575) 61.94%  ETA: 00:00:5 Checking Known Locations - Time: 00:01:22 <> (1596 / 2575) 61.98%  ETA: 00:00:5 Checking Known Locations - Time: 00:01:22 <> (1600 / 2575) 62.13%  ETA: 00:00:5 Checking Known Locations - Time: 00:01:22 <> (1601 / 2575) 62.17%  ETA: 00:00:5 Checking Known Locations - Time: 00:01:22 <> (1606 / 2575) 62.36%  ETA: 00:00:5 Checking Known Locations - Time: 00:01:22 <> (1611 / 2575) 62.56%  ETA: 00:00:5 Checking Known Locations - Time: 00:01:23 <> (1616 / 2575) 62.75%  ETA: 00:00:4 Checking Known Locations - Time: 00:01:23 <> (1621 / 2575) 62.95%  ETA: 00:00:4 Checking Known Locations - Time: 00:01:23 <> (1625 / 2575) 63.10%  ETA: 00:00:4 Checking Known Locations - Time: 00:01:23 <> (1626 / 2575) 63.14%  ETA: 00:00:4 Checking Known Locations - Time: 00:01:23 <> (1630 / 2575) 63.30%  ETA: 00:00:4 Checking Known Locations - Time: 00:01:23 <> (1631 / 2575) 63.33%  ETA: 00:00:4 Checking Known Locations - Time: 00:01:24 <> (1636 / 2575) 63.53%  ETA: 00:00:4 Checking Known Locations - Time: 00:01:24 <> (1641 / 2575) 63.72%  ETA: 00:00:4 Checking Known Locations - Time: 00:01:24 <> (1646 / 2575) 63.92%  ETA: 00:00:4 Checking Known Locations - Time: 00:01:24 <> (1651 / 2575) 64.11%  ETA: 00:00:4 Checking Known Locations - Time: 00:01:25 <> (1656 / 2575) 64.31%  ETA: 00:00:4 Checking Known Locations - Time: 00:01:25 <> (1661 / 2575) 64.50%  ETA: 00:00:4 Checking Known Locations - Time: 00:01:25 <> (1666 / 2575) 64.69%  ETA: 00:00:4 Checking Known Locations - Time: 00:01:25 <> (1671 / 2575) 64.89%  ETA: 00:00:4 Checking Known Locations - Time: 00:01:26 <> (1676 / 2575) 65.08%  ETA: 00:00:4 Checking Known Locations - Time: 00:01:26 <> (1681 / 2575) 65.28%  ETA: 00:00:4 Checking Known Locations - Time: 00:01:26 <> (1686 / 2575) 65.47%  ETA: 00:00:4 Checking Known Locations - Time: 00:01:26 <> (1691 / 2575) 65.66%  ETA: 00:00:4 Checking Known Locations - Time: 00:01:27 <> (1696 / 2575) 65.86%  ETA: 00:00:4 Checking Known Locations - Time: 00:01:27 <> (1701 / 2575) 66.05%  ETA: 00:00:4 Checking Known Locations - Time: 00:01:27 <> (1706 / 2575) 66.25%  ETA: 00:00:4 Checking Known Locations - Time: 00:01:27 <> (1711 / 2575) 66.44%  ETA: 00:00:4 Checking Known Locations - Time: 00:01:28 <> (1716 / 2575) 66.64%  ETA: 00:00:4 Checking Known Locations - Time: 00:01:28 <> (1721 / 2575) 66.83%  ETA: 00:00:4 Checking Known Locations - Time: 00:01:28 <> (1726 / 2575) 67.02%  ETA: 00:00:4 Checking Known Locations - Time: 00:01:28 <> (1731 / 2575) 67.22%  ETA: 00:00:4 Checking Known Locations - Time: 00:01:29 <> (1736 / 2575) 67.41%  ETA: 00:00:4 Checking Known Locations - Time: 00:01:29 <> (1741 / 2575) 67.61%  ETA: 00:00:4 Checking Known Locations - Time: 00:01:29 <> (1746 / 2575) 67.80%  ETA: 00:00:4 Checking Known Locations - Time: 00:01:29 <> (1751 / 2575) 68.00%  ETA: 00:00:4 Checking Known Locations - Time: 00:01:30 <> (1756 / 2575) 68.19%  ETA: 00:00:4 Checking Known Locations - Time: 00:01:30 <> (1761 / 2575) 68.38%  ETA: 00:00:4 Checking Known Locations - Time: 00:01:30 <> (1766 / 2575) 68.58%  ETA: 00:00:4 Checking Known Locations - Time: 00:01:30 <> (1771 / 2575) 68.77%  ETA: 00:00:4 Checking Known Locations - Time: 00:01:31 <> (1776 / 2575) 68.97%  ETA: 00:00:4 Checking Known Locations - Time: 00:01:31 <> (1781 / 2575) 69.16%  ETA: 00:00:4 Checking Known Locations - Time: 00:01:31 <> (1786 / 2575) 69.35%  ETA: 00:00:4 Checking Known Locations - Time: 00:01:31 <> (1791 / 2575) 69.55%  ETA: 00:00:4 Checking Known Locations - Time: 00:01:32 <> (1796 / 2575) 69.74%  ETA: 00:00:4 Checking Known Locations - Time: 00:01:32 <> (1801 / 2575) 69.94%  ETA: 00:00:4 Checking Known Locations - Time: 00:01:32 <> (1806 / 2575) 70.13%  ETA: 00:00:3 Checking Known Locations - Time: 00:01:32 <> (1811 / 2575) 70.33%  ETA: 00:00:3 Checking Known Locations - Time: 00:01:33 <> (1816 / 2575) 70.52%  ETA: 00:00:3 Checking Known Locations - Time: 00:01:33 <> (1821 / 2575) 70.71%  ETA: 00:00:3 Checking Known Locations - Time: 00:01:33 <> (1826 / 2575) 70.91%  ETA: 00:00:3 Checking Known Locations - Time: 00:01:33 <> (1831 / 2575) 71.10%  ETA: 00:00:3 Checking Known Locations - Time: 00:01:34 <> (1836 / 2575) 71.30%  ETA: 00:00:3 Checking Known Locations - Time: 00:01:34 <> (1841 / 2575) 71.49%  ETA: 00:00:3 Checking Known Locations - Time: 00:01:34 <> (1846 / 2575) 71.68%  ETA: 00:00:3 Checking Known Locations - Time: 00:01:34 <> (1851 / 2575) 71.88%  ETA: 00:00:3 Checking Known Locations - Time: 00:01:35 <> (1856 / 2575) 72.07%  ETA: 00:00:3 Checking Known Locations - Time: 00:01:35 <> (1861 / 2575) 72.27%  ETA: 00:00:3 Checking Known Locations - Time: 00:01:35 <> (1866 / 2575) 72.46%  ETA: 00:00:3 Checking Known Locations - Time: 00:01:35 <> (1871 / 2575) 72.66%  ETA: 00:00:3 Checking Known Locations - Time: 00:01:36 <> (1874 / 2575) 72.77%  ETA: 00:00:3 Checking Known Locations - Time: 00:01:36 <> (1876 / 2575) 72.85%  ETA: 00:00:3 Checking Known Locations - Time: 00:01:36 <> (1881 / 2575) 73.04%  ETA: 00:00:3 Checking Known Locations - Time: 00:01:36 <> (1886 / 2575) 73.24%  ETA: 00:00:3 Checking Known Locations - Time: 00:01:37 <> (1891 / 2575) 73.43%  ETA: 00:00:3 Checking Known Locations - Time: 00:01:37 <> (1896 / 2575) 73.63%  ETA: 00:00:3 Checking Known Locations - Time: 00:01:37 <> (1901 / 2575) 73.82%  ETA: 00:00:3 Checking Known Locations - Time: 00:01:37 <> (1906 / 2575) 74.01%  ETA: 00:00:3 Checking Known Locations - Time: 00:01:38 <> (1911 / 2575) 74.21%  ETA: 00:00:3 Checking Known Locations - Time: 00:01:38 <> (1916 / 2575) 74.40%  ETA: 00:00:3 Checking Known Locations - Time: 00:01:39 <> (1921 / 2575) 74.60%  ETA: 00:00:3 Checking Known Locations - Time: 00:01:39 <> (1926 / 2575) 74.79%  ETA: 00:00:3 Checking Known Locations - Time: 00:01:40 <> (1931 / 2575) 74.99%  ETA: 00:00:3 Checking Known Locations - Time: 00:01:40 <> (1936 / 2575) 75.18%  ETA: 00:00:3 Checking Known Locations - Time: 00:01:40 <> (1941 / 2575) 75.37%  ETA: 00:00:3 Checking Known Locations - Time: 00:01:40 <> (1946 / 2575) 75.57%  ETA: 00:00:3 Checking Known Locations - Time: 00:01:41 <> (1951 / 2575) 75.76%  ETA: 00:00:3 Checking Known Locations - Time: 00:01:41 <> (1956 / 2575) 75.96%  ETA: 00:00:3 Checking Known Locations - Time: 00:01:41 <> (1961 / 2575) 76.15%  ETA: 00:00:3 Checking Known Locations - Time: 00:01:41 <> (1966 / 2575) 76.34%  ETA: 00:00:3 Checking Known Locations - Time: 00:01:42 <> (1971 / 2575) 76.54%  ETA: 00:00:3 Checking Known Locations - Time: 00:01:42 <> (1976 / 2575) 76.73%  ETA: 00:00:3 Checking Known Locations - Time: 00:01:42 <> (1981 / 2575) 76.93%  ETA: 00:00:3 Checking Known Locations - Time: 00:01:42 <> (1986 / 2575) 77.12%  ETA: 00:00:3 Checking Known Locations - Time: 00:01:43 <> (1991 / 2575) 77.32%  ETA: 00:00:3 Checking Known Locations - Time: 00:01:43 <> (1996 / 2575) 77.51%  ETA: 00:00:3 Checking Known Locations - Time: 00:01:43 <> (2001 / 2575) 77.70%  ETA: 00:00:3 Checking Known Locations - Time: 00:01:43 <> (2006 / 2575) 77.90%  ETA: 00:00:3 Checking Known Locations - Time: 00:01:44 <> (2011 / 2575) 78.09%  ETA: 00:00:2 Checking Known Locations - Time: 00:01:44 <> (2016 / 2575) 78.29%  ETA: 00:00:2 Checking Known Locations - Time: 00:01:44 <> (2021 / 2575) 78.48%  ETA: 00:00:2 Checking Known Locations - Time: 00:01:44 <> (2026 / 2575) 78.67%  ETA: 00:00:2 Checking Known Locations - Time: 00:01:45 <> (2031 / 2575) 78.87%  ETA: 00:00:2 Checking Known Locations - Time: 00:01:45 <> (2036 / 2575) 79.06%  ETA: 00:00:2 Checking Known Locations - Time: 00:01:45 <> (2041 / 2575) 79.26%  ETA: 00:00:2 Checking Known Locations - Time: 00:01:46 <> (2046 / 2575) 79.45%  ETA: 00:00:2 Checking Known Locations - Time: 00:01:46 <> (2051 / 2575) 79.65%  ETA: 00:00:2 Checking Known Locations - Time: 00:01:46 <> (2056 / 2575) 79.84%  ETA: 00:00:2 Checking Known Locations - Time: 00:01:46 <> (2061 / 2575) 80.03%  ETA: 00:00:2 Checking Known Locations - Time: 00:01:47 <> (2066 / 2575) 80.23%  ETA: 00:00:2 Checking Known Locations - Time: 00:01:47 <> (2071 / 2575) 80.42%  ETA: 00:00:2 Checking Known Locations - Time: 00:01:47 <> (2076 / 2575) 80.62%  ETA: 00:00:2 Checking Known Locations - Time: 00:01:47 <> (2081 / 2575) 80.81%  ETA: 00:00:2 Checking Known Locations - Time: 00:01:48 <> (2086 / 2575) 81.00%  ETA: 00:00:2 Checking Known Locations - Time: 00:01:48 <> (2091 / 2575) 81.20%  ETA: 00:00:2 Checking Known Locations - Time: 00:01:48 <> (2096 / 2575) 81.39%  ETA: 00:00:2 Checking Known Locations - Time: 00:01:48 <> (2101 / 2575) 81.59%  ETA: 00:00:2 Checking Known Locations - Time: 00:01:49 <> (2106 / 2575) 81.78%  ETA: 00:00:2 Checking Known Locations - Time: 00:01:49 <> (2111 / 2575) 81.98%  ETA: 00:00:2 Checking Known Locations - Time: 00:01:49 <> (2116 / 2575) 82.17%  ETA: 00:00:2 Checking Known Locations - Time: 00:01:49 <> (2121 / 2575) 82.36%  ETA: 00:00:2 Checking Known Locations - Time: 00:01:50 <> (2126 / 2575) 82.56%  ETA: 00:00:2 Checking Known Locations - Time: 00:01:50 <> (2131 / 2575) 82.75%  ETA: 00:00:2 Checking Known Locations - Time: 00:01:50 <> (2136 / 2575) 82.95%  ETA: 00:00:2 Checking Known Locations - Time: 00:01:50 <> (2141 / 2575) 83.14%  ETA: 00:00:2 Checking Known Locations - Time: 00:01:51 <> (2146 / 2575) 83.33%  ETA: 00:00:2 Checking Known Locations - Time: 00:01:51 <> (2151 / 2575) 83.53%  ETA: 00:00:2 Checking Known Locations - Time: 00:01:51 <> (2156 / 2575) 83.72%  ETA: 00:00:2 Checking Known Locations - Time: 00:01:51 <> (2161 / 2575) 83.92%  ETA: 00:00:2 Checking Known Locations - Time: 00:01:52 <> (2166 / 2575) 84.11%  ETA: 00:00:2 Checking Known Locations - Time: 00:01:52 <> (2171 / 2575) 84.31%  ETA: 00:00:2 Checking Known Locations - Time: 00:01:52 <> (2176 / 2575) 84.50%  ETA: 00:00:2 Checking Known Locations - Time: 00:01:52 <> (2181 / 2575) 84.69%  ETA: 00:00:2 Checking Known Locations - Time: 00:01:53 <> (2186 / 2575) 84.89%  ETA: 00:00:2 Checking Known Locations - Time: 00:01:53 <> (2191 / 2575) 85.08%  ETA: 00:00:2 Checking Known Locations - Time: 00:01:53 <> (2196 / 2575) 85.28%  ETA: 00:00:2 Checking Known Locations - Time: 00:01:53 <> (2201 / 2575) 85.47%  ETA: 00:00:1 Checking Known Locations - Time: 00:01:54 <> (2206 / 2575) 85.66%  ETA: 00:00:1 Checking Known Locations - Time: 00:01:54 <> (2211 / 2575) 85.86%  ETA: 00:00:1 Checking Known Locations - Time: 00:01:54 <> (2216 / 2575) 86.05%  ETA: 00:00:1 Checking Known Locations - Time: 00:01:54 <> (2221 / 2575) 86.25%  ETA: 00:00:1 Checking Known Locations - Time: 00:01:55 <> (2226 / 2575) 86.44%  ETA: 00:00:1 Checking Known Locations - Time: 00:01:55 <> (2231 / 2575) 86.64%  ETA: 00:00:1 Checking Known Locations - Time: 00:01:55 <> (2236 / 2575) 86.83%  ETA: 00:00:1 Checking Known Locations - Time: 00:01:55 <> (2241 / 2575) 87.02%  ETA: 00:00:1 Checking Known Locations - Time: 00:01:56 <> (2246 / 2575) 87.22%  ETA: 00:00:1 Checking Known Locations - Time: 00:01:56 <> (2251 / 2575) 87.41%  ETA: 00:00:1 Checking Known Locations - Time: 00:01:56 <> (2256 / 2575) 87.61%  ETA: 00:00:1 Checking Known Locations - Time: 00:01:56 <> (2261 / 2575) 87.80%  ETA: 00:00:1 Checking Known Locations - Time: 00:01:57 <> (2266 / 2575) 88.00%  ETA: 00:00:1 Checking Known Locations - Time: 00:01:57 <> (2271 / 2575) 88.19%  ETA: 00:00:1 Checking Known Locations - Time: 00:01:57 <> (2276 / 2575) 88.38%  ETA: 00:00:1 Checking Known Locations - Time: 00:01:57 <> (2281 / 2575) 88.58%  ETA: 00:00:1 Checking Known Locations - Time: 00:01:58 <> (2286 / 2575) 88.77%  ETA: 00:00:1 Checking Known Locations - Time: 00:01:58 <> (2291 / 2575) 88.97%  ETA: 00:00:1 Checking Known Locations - Time: 00:01:58 <> (2296 / 2575) 89.16%  ETA: 00:00:1 Checking Known Locations - Time: 00:01:58 <> (2301 / 2575) 89.35%  ETA: 00:00:1 Checking Known Locations - Time: 00:01:58 <> (2303 / 2575) 89.43%  ETA: 00:00:1 Checking Known Locations - Time: 00:01:59 <> (2306 / 2575) 89.55%  ETA: 00:00:1 Checking Known Locations - Time: 00:01:59 <> (2307 / 2575) 89.59%  ETA: 00:00:1 Checking Known Locations - Time: 00:01:59 <> (2311 / 2575) 89.74%  ETA: 00:00:1 Checking Known Locations - Time: 00:01:59 <> (2312 / 2575) 89.78%  ETA: 00:00:1 Checking Known Locations - Time: 00:01:59 <> (2316 / 2575) 89.94%  ETA: 00:00:1 Checking Known Locations - Time: 00:01:59 <> (2317 / 2575) 89.98%  ETA: 00:00:1 Checking Known Locations - Time: 00:01:59 <> (2321 / 2575) 90.13%  ETA: 00:00:1 Checking Known Locations - Time: 00:01:59 <> (2322 / 2575) 90.17%  ETA: 00:00:1 Checking Known Locations - Time: 00:02:00 <> (2326 / 2575) 90.33%  ETA: 00:00:1 Checking Known Locations - Time: 00:02:00 <> (2327 / 2575) 90.36%  ETA: 00:00:1 Checking Known Locations - Time: 00:02:00 <> (2331 / 2575) 90.52%  ETA: 00:00:1 Checking Known Locations - Time: 00:02:00 <> (2332 / 2575) 90.56%  ETA: 00:00:1 Checking Known Locations - Time: 00:02:00 <> (2336 / 2575) 90.71%  ETA: 00:00:1 Checking Known Locations - Time: 00:02:00 <> (2337 / 2575) 90.75%  ETA: 00:00:1 Checking Known Locations - Time: 00:02:00 <> (2341 / 2575) 90.91%  ETA: 00:00:1 Checking Known Locations - Time: 00:02:00 <> (2342 / 2575) 90.95%  ETA: 00:00:1 Checking Known Locations - Time: 00:02:01 <> (2346 / 2575) 91.10%  ETA: 00:00:1 Checking Known Locations - Time: 00:02:01 <> (2347 / 2575) 91.14%  ETA: 00:00:1 Checking Known Locations - Time: 00:02:01 <> (2351 / 2575) 91.30%  ETA: 00:00:1 Checking Known Locations - Time: 00:02:01 <> (2352 / 2575) 91.33%  ETA: 00:00:1 Checking Known Locations - Time: 00:02:01 <> (2356 / 2575) 91.49%  ETA: 00:00:1 Checking Known Locations - Time: 00:02:01 <> (2357 / 2575) 91.53%  ETA: 00:00:1 Checking Known Locations - Time: 00:02:01 <> (2361 / 2575) 91.68%  ETA: 00:00:1 Checking Known Locations - Time: 00:02:01 <> (2362 / 2575) 91.72%  ETA: 00:00:1 Checking Known Locations - Time: 00:02:02 <> (2366 / 2575) 91.88%  ETA: 00:00:1 Checking Known Locations - Time: 00:02:02 <> (2367 / 2575) 91.92%  ETA: 00:00:1 Checking Known Locations - Time: 00:02:02 <> (2371 / 2575) 92.07%  ETA: 00:00:1 Checking Known Locations - Time: 00:02:02 <> (2372 / 2575) 92.11%  ETA: 00:00:1 Checking Known Locations - Time: 00:02:02 <> (2376 / 2575) 92.27%  ETA: 00:00:1 Checking Known Locations - Time: 00:02:02 <> (2378 / 2575) 92.34%  ETA: 00:00:1 Checking Known Locations - Time: 00:02:02 <> (2379 / 2575) 92.38%  ETA: 00:00:1 Checking Known Locations - Time: 00:02:02 <> (2380 / 2575) 92.42%  ETA: 00:00:1 Checking Known Locations - Time: 00:02:03 <> (2381 / 2575) 92.46%  ETA: 00:00:1 Checking Known Locations - Time: 00:02:03 <> (2382 / 2575) 92.50%  ETA: 00:00:1 Checking Known Locations - Time: 00:02:03 <> (2385 / 2575) 92.62%  ETA: 00:00:1 Checking Known Locations - Time: 00:02:03 <> (2386 / 2575) 92.66%  ETA: 00:00:1 Checking Known Locations - Time: 00:02:03 <> (2389 / 2575) 92.77%  ETA: 00:00:1 Checking Known Locations - Time: 00:02:03 <> (2391 / 2575) 92.85%  ETA: 00:00:1 Checking Known Locations - Time: 00:02:03 <> (2394 / 2575) 92.97%  ETA: 00:00:0 Checking Known Locations - Time: 00:02:03 <> (2396 / 2575) 93.04%  ETA: 00:00:0 Checking Known Locations - Time: 00:02:03 <> (2399 / 2575) 93.16%  ETA: 00:00:0 Checking Known Locations - Time: 00:02:04 <> (2401 / 2575) 93.24%  ETA: 00:00:0 Checking Known Locations - Time: 00:02:04 <> (2404 / 2575) 93.35%  ETA: 00:00:0 Checking Known Locations - Time: 00:02:04 <> (2406 / 2575) 93.43%  ETA: 00:00:0 Checking Known Locations - Time: 00:02:04 <> (2409 / 2575) 93.55%  ETA: 00:00:0 Checking Known Locations - Time: 00:02:04 <> (2411 / 2575) 93.63%  ETA: 00:00:0 Checking Known Locations - Time: 00:02:04 <> (2414 / 2575) 93.74%  ETA: 00:00:0 Checking Known Locations - Time: 00:02:04 <> (2416 / 2575) 93.82%  ETA: 00:00:0 Checking Known Locations - Time: 00:02:04 <> (2419 / 2575) 93.94%  ETA: 00:00:0 Checking Known Locations - Time: 00:02:05 <> (2421 / 2575) 94.01%  ETA: 00:00:0 Checking Known Locations - Time: 00:02:05 <> (2424 / 2575) 94.13%  ETA: 00:00:0 Checking Known Locations - Time: 00:02:05 <> (2426 / 2575) 94.21%  ETA: 00:00:0 Checking Known Locations - Time: 00:02:05 <> (2430 / 2575) 94.36%  ETA: 00:00:0 Checking Known Locations - Time: 00:02:05 <> (2431 / 2575) 94.40%  ETA: 00:00:0 Checking Known Locations - Time: 00:02:05 <> (2436 / 2575) 94.60%  ETA: 00:00:0 Checking Known Locations - Time: 00:02:06 <> (2441 / 2575) 94.79%  ETA: 00:00:0 Checking Known Locations - Time: 00:02:06 <> (2446 / 2575) 94.99%  ETA: 00:00:0 Checking Known Locations - Time: 00:02:06 <> (2451 / 2575) 95.18%  ETA: 00:00:0 Checking Known Locations - Time: 00:02:06 <> (2456 / 2575) 95.37%  ETA: 00:00:0 Checking Known Locations - Time: 00:02:07 <> (2461 / 2575) 95.57%  ETA: 00:00:0 Checking Known Locations - Time: 00:02:07 <> (2466 / 2575) 95.76%  ETA: 00:00:0 Checking Known Locations - Time: 00:02:07 <> (2471 / 2575) 95.96%  ETA: 00:00:0 Checking Known Locations - Time: 00:02:07 <> (2476 / 2575) 96.15%  ETA: 00:00:0 Checking Known Locations - Time: 00:02:08 <> (2481 / 2575) 96.34%  ETA: 00:00:0 Checking Known Locations - Time: 00:02:08 <> (2486 / 2575) 96.54%  ETA: 00:00:0 Checking Known Locations - Time: 00:02:08 <> (2491 / 2575) 96.73%  ETA: 00:00:0 Checking Known Locations - Time: 00:02:08 <> (2496 / 2575) 96.93%  ETA: 00:00:0 Checking Known Locations - Time: 00:02:09 <> (2501 / 2575) 97.12%  ETA: 00:00:0 Checking Known Locations - Time: 00:02:09 <> (2506 / 2575) 97.32%  ETA: 00:00:0 Checking Known Locations - Time: 00:02:09 <> (2511 / 2575) 97.51%  ETA: 00:00:0 Checking Known Locations - Time: 00:02:09 <> (2516 / 2575) 97.70%  ETA: 00:00:0 Checking Known Locations - Time: 00:02:11 <> (2521 / 2575) 97.90%  ETA: 00:00:0 Checking Known Locations - Time: 00:02:11 <> (2526 / 2575) 98.09%  ETA: 00:00:0 Checking Known Locations - Time: 00:02:12 <> (2531 / 2575) 98.29%  ETA: 00:00:0 Checking Known Locations - Time: 00:02:12 <> (2536 / 2575) 98.48%  ETA: 00:00:0 Checking Known Locations - Time: 00:02:12 <> (2541 / 2575) 98.67%  ETA: 00:00:0 Checking Known Locations - Time: 00:02:12 <> (2546 / 2575) 98.87%  ETA: 00:00:0 Checking Known Locations - Time: 00:02:13 <> (2551 / 2575) 99.06%  ETA: 00:00:0 Checking Known Locations - Time: 00:02:13 <> (2556 / 2575) 99.26%  ETA: 00:00:0 Checking Known Locations - Time: 00:02:13 <> (2561 / 2575) 99.45%  ETA: 00:00:0 Checking Known Locations - Time: 00:02:14 <> (2566 / 2575) 99.65%  ETA: 00:00:0 Checking Known Locations - Time: 00:02:14 <> (2571 / 2575) 99.84%  ETA: 00:00:0 Checking Known Locations - Time: 00:02:14 <> (2575 / 2575) 100.00% Time: 00:02:14

[i] No Timthumbs Found.

[+] Enumerating Config Backups (via Passive and Aggressive Methods)
 Checking Config Backups - Time: 00:00:07 <=> (137 / 137) 100.00% Time: 00:00:07

[i] No Config Backups Found.

[+] Enumerating DB Exports (via Passive and Aggressive Methods)
 Checking DB Exports - Time: 00:00:04 <=======> (84 / 84) 100.00% Time: 00:00:04

[i] No DB Exports Found.

[+] Enumerating Medias (via Passive and Aggressive Methods) (Permalink setting must be set to "Plain" for those to be detected)
 Brute Forcing Attachment IDs - Time: 00:00:00 <> (0 / 100)  0.00%  ETA: ??:??:? Brute Forcing Attachment IDs - Time: 00:00:00 <> (1 / 100)  1.00%  ETA: 00:00:3 Brute Forcing Attachment IDs - Time: 00:00:00 <> (2 / 100)  2.00%  ETA: 00:00:1 Brute Forcing Attachment IDs - Time: 00:00:00 <> (6 / 100)  6.00%  ETA: 00:00:0 Brute Forcing Attachment IDs - Time: 00:00:00 <> (9 / 100)  9.00%  ETA: 00:00:0 Brute Forcing Attachment IDs - Time: 00:00:00 <> (11 / 100) 11.00%  ETA: 00:00: Brute Forcing Attachment IDs - Time: 00:00:00 <> (14 / 100) 14.00%  ETA: 00:00: Brute Forcing Attachment IDs - Time: 00:00:01 <> (16 / 100) 16.00%  ETA: 00:00: Brute Forcing Attachment IDs - Time: 00:00:01 <> (19 / 100) 19.00%  ETA: 00:00: Brute Forcing Attachment IDs - Time: 00:00:01 <> (21 / 100) 21.00%  ETA: 00:00: Brute Forcing Attachment IDs - Time: 00:00:01 <> (24 / 100) 24.00%  ETA: 00:00: Brute Forcing Attachment IDs - Time: 00:00:01 <> (26 / 100) 26.00%  ETA: 00:00: Brute Forcing Attachment IDs - Time: 00:00:01 <> (29 / 100) 29.00%  ETA: 00:00: Brute Forcing Attachment IDs - Time: 00:00:01 <> (31 / 100) 31.00%  ETA: 00:00: Brute Forcing Attachment IDs - Time: 00:00:01 <> (33 / 100) 33.00%  ETA: 00:00: Brute Forcing Attachment IDs - Time: 00:00:02 <> (36 / 100) 36.00%  ETA: 00:00: Brute Forcing Attachment IDs - Time: 00:00:02 <> (39 / 100) 39.00%  ETA: 00:00: Brute Forcing Attachment IDs - Time: 00:00:02 <> (41 / 100) 41.00%  ETA: 00:00: Brute Forcing Attachment IDs - Time: 00:00:02 <> (43 / 100) 43.00%  ETA: 00:00: Brute Forcing Attachment IDs - Time: 00:00:02 <> (46 / 100) 46.00%  ETA: 00:00: Brute Forcing Attachment IDs - Time: 00:00:02 <> (48 / 100) 48.00%  ETA: 00:00: Brute Forcing Attachment IDs - Time: 00:00:02 <> (51 / 100) 51.00%  ETA: 00:00: Brute Forcing Attachment IDs - Time: 00:00:02 <> (54 / 100) 54.00%  ETA: 00:00: Brute Forcing Attachment IDs - Time: 00:00:03 <> (56 / 100) 56.00%  ETA: 00:00: Brute Forcing Attachment IDs - Time: 00:00:03 <> (59 / 100) 59.00%  ETA: 00:00: Brute Forcing Attachment IDs - Time: 00:00:03 <> (61 / 100) 61.00%  ETA: 00:00: Brute Forcing Attachment IDs - Time: 00:00:03 <> (64 / 100) 64.00%  ETA: 00:00: Brute Forcing Attachment IDs - Time: 00:00:03 <> (66 / 100) 66.00%  ETA: 00:00: Brute Forcing Attachment IDs - Time: 00:00:03 <> (68 / 100) 68.00%  ETA: 00:00: Brute Forcing Attachment IDs - Time: 00:00:04 <> (71 / 100) 71.00%  ETA: 00:00: Brute Forcing Attachment IDs - Time: 00:00:04 <> (73 / 100) 73.00%  ETA: 00:00: Brute Forcing Attachment IDs - Time: 00:00:04 <> (76 / 100) 76.00%  ETA: 00:00: Brute Forcing Attachment IDs - Time: 00:00:04 <> (79 / 100) 79.00%  ETA: 00:00: Brute Forcing Attachment IDs - Time: 00:00:04 <> (81 / 100) 81.00%  ETA: 00:00: Brute Forcing Attachment IDs - Time: 00:00:04 <> (84 / 100) 84.00%  ETA: 00:00: Brute Forcing Attachment IDs - Time: 00:00:04 <> (85 / 100) 85.00%  ETA: 00:00: Brute Forcing Attachment IDs - Time: 00:00:05 <> (88 / 100) 88.00%  ETA: 00:00: Brute Forcing Attachment IDs - Time: 00:00:05 <> (90 / 100) 90.00%  ETA: 00:00: Brute Forcing Attachment IDs - Time: 00:00:05 <> (92 / 100) 92.00%  ETA: 00:00: Brute Forcing Attachment IDs - Time: 00:00:05 <> (94 / 100) 94.00%  ETA: 00:00: Brute Forcing Attachment IDs - Time: 00:00:05 <> (96 / 100) 96.00%  ETA: 00:00: Brute Forcing Attachment IDs - Time: 00:00:05 <> (97 / 100) 97.00%  ETA: 00:00: Brute Forcing Attachment IDs - Time: 00:00:05 <> (99 / 100) 99.00%  ETA: 00:00: Brute Forcing Attachment IDs - Time: 00:00:05 <> (100 / 100) 100.00% Time: 00:00:05

[i] Medias(s) Identified:

[+] http://blog.inlanefreight.local/?attachment_id=5
 | Found By: Attachment Brute Forcing (Aggressive Detection)

[+] http://blog.inlanefreight.local/?attachment_id=6
 | Found By: Attachment Brute Forcing (Aggressive Detection)

[+] http://blog.inlanefreight.local/?attachment_id=13
 | Found By: Attachment Brute Forcing (Aggressive Detection)

[+] http://blog.inlanefreight.local/?attachment_id=18
 | Found By: Attachment Brute Forcing (Aggressive Detection)

[+] http://blog.inlanefreight.local/?attachment_id=22
 | Found By: Attachment Brute Forcing (Aggressive Detection)

[+] Enumerating Users (via Passive and Aggressive Methods)
 Brute Forcing Author IDs - Time: 00:00:01 <==> (10 / 10) 100.00% Time: 00:00:01

[i] User(s) Identified:

[+] by:
									admin
 | Found By: Author Posts - Display Name (Passive Detection)

[+] admin
 | Found By: Rss Generator (Passive Detection)
 | Confirmed By:
 |  Author Id Brute Forcing - Author Pattern (Aggressive Detection)
 |  Login Error Messages (Aggressive Detection)

[+] doug
 | Found By: Author Id Brute Forcing - Author Pattern (Aggressive Detection)
 | Confirmed By: Login Error Messages (Aggressive Detection)

[+] WPScan DB API OK
 | Plan: free
 | Requests Done (during the scan): 5
 | Requests Remaining: 15

[+] Finished: Fri Apr 25 23:22:46 2025
[+] Requests Done: 3632
[+] Cached Requests: 12
[+] Data Sent: 1.045 MB
[+] Data Received: 23.173 MB
[+] Memory used: 277.684 MB
[+] Elapsed time: 00:03:27

```

## 📋 Findings
The scan revealed:

- Apache/2.4.41 (Ubuntu)
- WordPress version: 5.8 (with 41 known vulnerabilities)
- XML-RPC: Enabled (brute-force vector)
- Directory listing: Enabled on /wp-content/uploads/
- Discovered Plugins:
  - wpDiscuz 7.0.4 — Unauthenticated Remote Code Execution (RCE)
  - mail-masta 1.0.0 — Local File Inclusion + SQL Injection
  - Contact Form 7
- Enumerated users: admin, doug
- readme.txt and other sensitive files accessible

## 🕵️ Manual Exploration
Found image using attachment ID:
```bash
http://blog.inlanefreight.local/?attachment_id=5
```
Image URL resolved to:
```bash
http://blog.inlanefreight.local/wp-content/uploads/2021/08/cropped-header1.png
```
## 🧠 From there, I guessed the directory structure and tried:
```bash
http://blog.inlanefreight.local/wp-content/uploads/2021/08/flag.txt
```
✅ Success! The flag was exposed due to insecure upload permissions and directory structure.

# 📘 WordPress Plugin Enumeration
During the internal penetration test of inlanefreight.local, I performed plugin enumeration on the WordPress instance hosted at http://blog.inlanefreight.local.

## 🔍 Methodology
To enumerate active plugins, I fetched the HTML content of a blog page and filtered it using grep:
```bash
curl -s http://blog.inlanefreight.local/?p=1 | grep plugins
```

## 🔎 Findings
This revealed several WordPress plugins through linked CSS and JavaScript assets:

✅ Detected Plugins:

Plugin Name	| Version	| Path
Contact Form 7	| 5.4.2	| /wp-content/plugins/contact-form-7/
WPDiscuz	| 7.0.4	| /wp-content/plugins/wpdiscuz/
Mail Masta	| 5.8	| /wp-content/plugins/mail-masta/
WP Sitemap Page	| Unknown	Plugin | reference in footer

These plugins can be checked for publicly known vulnerabilities using tools such as:
`wpscan`
`Searchsploit`
`Exploit-DB`

We don't have the version number directly for WP Sitemap Page like other plugins, but try to enumerate more where the plugin might be uploaded.
```bash
http://blog.inlanefreight.local/wp-content/plugins/wp-sitemap-page/readme.txt
```

Let's read the file and get the answer of version number.
