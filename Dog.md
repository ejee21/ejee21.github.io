---
share: true
layout: post
title: HTB Dog Walkthrough
date: 2026-02-03 01:00:00 -0500
categories:
  - hackthebox
  - ctf
  - linux
  - web
  - privesc
tags:
  - backdrop-cms
  - git-leak
  - rce
  - sudo
  - oscp-like
---


sleep deprived dont judge ik its bad but please use this if you are stuck :)

git-dumper & ./extractor.sh on .git directory

**[https://exploit-notes.hdks.org/exploit/web/dump-git-repository-from-website/](https://exploit-notes.hdks.org/exploit/web/dump-git-repository-from-website/)**

mysql://root:BackDropJ2024DS2024@127.0.0.1/backdrop

![Pasted image 20250311022244.png](./assets/images/screenshots/Pasted%20image%2020250311022244.png)
![Pasted image 20250311022529.png](./assets/images/screenshots/Pasted%20image%2020250311022529.png)
found tiffany first before i extracted and dumped but holy moly this was a pain. in the past i did not have to extract after dumping, but with this one i needed to. this may be due to the size of the .git but i am not certain
![Pasted image 20250311022631.png](./assets/images/screenshots/Pasted%20image%2020250311022631.png)


we can login as tiffany with the root db password!! Hooray!

now, i think i can do an authenticated RCE via the CMS for backdrop 1.27.1

https://www.exploit-db.com/exploits/52021

lets see
![Pasted image 20250311023036.png](./assets/images/screenshots/Pasted%20image%2020250311023036.png)

the url does not work, but here is a workaround to get there
mysql://root:BackDropJ2024DS2024@127.0.0.1/backdrop


![Pasted image 20250311023221.png](./assets/images/screenshots/Pasted%20image%2020250311023221.png)
![Pasted image 20250311023354.png](./assets/images/screenshots/Pasted%20image%2020250311023354.png)



![Pasted image 20250311030658.png](./assets/images/screenshots/Pasted%20image%2020250311030658.png)
tar cf shell.tar shell

okay this command worked by archiving the whole directory not just the 2 files inside of it. i spent 5 hours finding another vector in because of this
![Pasted image 20250311054127.png](./assets/images/screenshots/Pasted%20image%2020250311054127.png)
Originally, I messed up by only copying the two files and not the entire directory. mistake made, lesson learned.

on the bright side: cat /etc/passwd OR ls /home to see two users. johncusack aligns with the JohnC user in BackDrop CMS who has uid=1. hes og and likes to recycle... passwords.


sudo bee scr /tmp/ejee.php -d

-d is critical afaik otherwise it wont run

