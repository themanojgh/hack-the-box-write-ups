# 🕵️‍♂️ Enumeration – Discovering the User in `/home`
In this task, the goal was to identify the user present in the `/home` directory by exploiting command injection in a vulnerable web application.

Initially, I confirmed the injection point using basic payloads such as:

```bash
127.0.0.1&whoami
```
![image](https://github.com/user-attachments/assets/d967aa5a-70c0-45a2-b7bd-6ff533336ff3)

This successfully executed a command on the server, proving that the ampersand (`&`) operator was not blacklisted.

After confirming injection, I attempted to list the contents of the `/home` directory with:
```bash
127.0.0.1&ls /home
```
![image](https://github.com/user-attachments/assets/3068c3a7-4805-4bd8-8d0a-802478310b5d)

However, this returned a response with nothing listed in home directory. To bypass this, I tried various encodings and command separators. What finally worked was the payload:
```bash
127.0.0.1${LS_COLORS:10:1}%0a{ls,-la}${IFS}${PATH:0:1}home
```
![image](https://github.com/user-attachments/assets/9ec60d9e-a34e-44b5-93bc-5fdb717a6c24)

### Let's break this down:

- `${LS_COLORS:10:1}` extracts a semicolon (`;`) from an environment variable.
- `%0a` represents a newline, helping inject a clean command line.
- `{ls,-la}` is a clever way of executing `ls -la` to avoid filters.
- `${IFS}` injects a space using the shell’s Internal Field Separator.
- `${PATH:0:1}` extracts `/`, making the full path `/home`.
