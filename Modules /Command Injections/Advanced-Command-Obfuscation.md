# Decoding and Executing a Command via Base64 Encoding
In this challenge, the task was to execute the command `find /usr/share/ | grep root | grep mysql | tail -n 1` while bypassing certain character restrictions. The given command searches through the `/usr/share/` directory for instances of "root" and "mysql," ultimately displaying the last match using `tail -n 1`.

To tackle this, I first encoded the given command into `base64` to handle potential restrictions on special characters or spaces. The command used for `base64` encoding was:
```bash
echo -n "find /usr/share/ | grep root | grep mysql | tail -n 1" | base64
```
![image](https://github.com/user-attachments/assets/ae99825f-5661-4eae-86da-ba3b3ec2c1c1)

This generated the following base64-encoded string:
` ZmluZCAvdXNyL3NoYXJlLyB8IGdyZXAgcm9vdCB8IGdyZXAgbXlzcWwgfCB0YWlsIC1uIDE= `

Next, I utilized bash to decode and execute this command in a restricted environment where `spaces`, `echo`, and special characters were blocked. The final command used was:

```bash
127.0.0.1%0abash<<<$(base64%09-d<<<ZmluZCAvdXNyL3NoYXJlLyB8IGdyZXAgcm9vdCB8IGdyZXAgbXlzcWwgfCB0YWlsIC1uIDE=)
```

This command works by first decoding the base64 string using `base64 -d` and then passing the decoded command to bash for execution. The `%0a` is used to represent a newline character, and `%09` is a tab character. The decoded command `find /usr/share/ | grep root | grep mysql | tail -n 1` was then successfully executed.

By using this method, I was able to bypass restrictions on certain characters and execute the original command, successfully retrieving the output from the `/usr/share/` directory. This method is a useful technique in environments where specific characters or commands are restricted but still allows for indirect command execution.

![image](https://github.com/user-attachments/assets/7a7b06c4-a604-4fe8-aee0-8fa72aabd220)
