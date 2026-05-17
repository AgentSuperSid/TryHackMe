This Rick and Morty-themed challenge requires you to exploit a web server and find three ingredients to help Rick make his potion and transform himself back into a human from a pickle.
Deploy the virtual machine on this task and explore the web application: 10.67.179.30

### Solution:
- As they said, i first searched the IP address in the web which led me to a website.
- I checked the source code and found the username to logon Rick's computer but not sure what the password is.
  ```

  <!--

    Note to self, remember username!

    Username: R1ckRul3s

  -->

  ```
- Then I wasnt quite sure how to proceed next(since Im new to solving these types of rooms).
- I read one of the writeups and did some research. This is where I found out a tool called gobuster which could be used to bruteforce and find different directories in a given url using common wordlists.
- The command here used was:
`gobuster dir -u http://10.67.179.30 -w /usr/share/wordlists/dirb/common.txt`
- Then after waiting for some time this is what I got:
```
                                                                             
┌──(root㉿kali)-[~]
└─# gobuster dir -u http://10.67.179.30 -w /usr/share/wordlists/dirb/common.txt
===============================================================
Gobuster v3.8
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://10.67.179.30
[+] Method:                  GET
[+] Threads:                 10
[+] Wordlist:                /usr/share/wordlists/dirb/common.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.8
[+] Timeout:                 10s
===============================================================
Starting gobuster in directory enumeration mode
===============================================================
/.hta                 (Status: 403) [Size: 277]
/.htaccess            (Status: 403) [Size: 277]
/.htpasswd            (Status: 403) [Size: 277]
/assets               (Status: 301) [Size: 313] [--> http://10.67.179.30/assets/]                                                                         
/index.html           (Status: 200) [Size: 1062]
/robots.txt           (Status: 200) [Size: 17]
/server-status        (Status: 403) [Size: 277]
Progress: 4613 / 4613 (100.00%)
===============================================================
Finished
===============================================================

```
- While checking these directories, I came across `/robots.txt` and opened it.

## What i Learned
- Using gobuster to bruteforcing directories from a target url

### References
https://infosecwriteups.com/picklerick-tryhackme-writeup-74c6fedae081
