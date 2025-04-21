# 🏴 HTB Write-Up: Discovering the User and Reading the Flag File

After confirming the web application was vulnerable to command injection, I began exploring different input encodings and operators to bypass filters. Basic payloads like:
```bash
127.0.0.1&ls /home
```
executed successfully, but returned no visible user output — possibly due to hidden directories or permission constraints.

To dig deeper, I crafted an obfuscated payload using bash brace expansion and environmental variables:
```bash
127.0.0.1${LS_COLORS:10:1}%0a{ls,-la}${IFS}${PATH:0:1}home
```
This broke down as:

- `${LS_COLORS:10:1}` → injected a semicolon `;`
- `%0a` → URL-encoded newline to separate commands
- `{ls,-la}` → brace expansion for `ls -la`
- `${IFS}` → resolved to a space
- `${PATH:0:1}` → resolved to `/`, helping build the path `/home`

This clever combination revealed hidden files and directories in /home, finally exposing the username: `1nj3c70r`.

To retrieve the flag inside `/home/1nj3c70r/flag.txt`, I had to bypass a filter blocking the `cat` command. To achieve this, I used command substitution and reverse logic:

```bash
127.0.0.1${LS_COLORS:10:1}%0a$(rev<<<'tac')${IFS}${PATH:0:1}home${PATH:0:1}1nj3c70r${PATH:0:1}flag.txt
```
This worked because:

`$(rev<<<'tac')` → reversed `tac` to `cat`, dynamically generating the command.

Other environment variables and encoded characters helped avoid detection and build valid paths

With this payload, I successfully retrieved the contents of the `flag` file.
