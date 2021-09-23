# BitLab
## Challenge
HackTheBox -> Machine -> BitLab
## Tools
Tool | Description
------- | -------
 | 
## Description
We scan with NMAP the host.

```
nmap -A -sC -sV 10.10.10.14 > nmap.txt
```

![NMAP](13_01.png)

Open the broswere and go to the page

![NMAP](13_02.png)

Discover with Hidra

```
hydra
```

Go to help page 

Open Bookmarks

Open link GitLabLogin, is javascript using biutifier

Login as **clave** **11des0081x**

Go to Administrator Deployer

Index.php Auto pull the request project

You cann see that it do **..** so it deploy on 10.10.10.14/profile

Do a merge request in master profile

I crate a new file named shell.php

and copy a [shell.php](https://raw.githubusercontent.com/flozz/p0wny-shell/master/shell.php)

We got a password in a database.

we can connect **c3NoLXN0cjBuZy1wQHNz==**

ssh clave@10.10.10.14

## FLAG
***HTB{DonTRuNAsRoOt!MESsEdUpMarket}***
