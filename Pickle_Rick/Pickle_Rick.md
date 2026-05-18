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
- While checking these directories, I came across `/robots.txt`(it is usually a file of interest during enumeration) and opened it : `http://10.67.179.30/robots.txt`.
- It had a text `Wubbalubbadubdub`, which Im assuming is the password for the username we got earlier.
- But the gobuster was still not able to find any login link while searching, so maybe the login page wasnt an html file, perhaps a .php file.
- I again ran gobuster `gobuster dir -u http://10.67.179.30 -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -x php`.
- There was a lot of files to check, but I got `/login.php` and `/portal.php` early on, so I didnt have to continue the search.
- I opened `/portal.php` which led me to `/login.php` itself. Anyways then I entered the details and `Wubbalubbadubdub` was indeed the password.
- Then there was a command box. The first thing that I did was check what files/directories are actually present:
```
Sup3rS3cretPickl3Ingred.txt
assets
clue.txt
denied.php
index.html
login.php
portal.php
robots.txt
```
- I opened each file in the web.
- `clue.txt` Gave me this text *Look around the file system for the other ingredient.* , `index.html` just led us back to the main page.
-  `Sup3rS3cretPickl3Ingred.txt` gave the text - *mr. meeseek hair* which actually was the first ingredient!
-  `denied.php` didnt give any info even in the source code.
-  Then I decided dive deeper by using `cd` command to go to the parent directories but it didnt work, maybe because it doesnt support switching directories in this.
-  So I just tried `ls` in **home** and **/** directories by trial and error. I found `/home/rick/second ingredients`, but I still wasnt able to open it as as I was doing previously(http://10.67.179.30/....php way).
-  Even `cat` didnt work. It said `cat` was disabled to make our work harder.
-  I tried `less`. It was not disabled so it worked - `less /home/rick/"second ingredients"`. I forgot to put "" for "second ingredients" so it didnt work the first time for me.
-  This gave me the second ingrediant - `jerry tear`.
-  After searching in folders for a long time, I didnt get anything, also the `/root` folder was empty which was weird.
-  Then I tried using `sudo` in hoping to uncover any extra files. Turns out there were actually some hiddent files in the `/root` dir.
```
3rd.txt
snap
```
- Opened `3rd.txt` using `less` command. This displayed the final ingrediant- **3rd ingredients: fleeb juice**!!!
- Since Im new to solving these problems I couldnt have done it without other resources and getting some hints from the writeups.

## What i Learned
- Using gobuster to bruteforcing directories from a target url
- `less` command.
- The wordlist argument after `-w` for gobuster is just a folder on my Kali machine that comes pre-loaded with text files full of common words,just text files.
- It starts searching for files with these words. `-x` is to search for specific extentions.

### References
- https://infosecwriteups.com/picklerick-tryhackme-writeup-74c6fedae081
- Google search for commands to use instead of `cat`, that is `less`.
