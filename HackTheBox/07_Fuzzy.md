# MarketDump
## Challenge
HackTheBox -> Challenges -> Web -> Fuzzy
## Tools
Tool | Description
------- | -------
[Wfuzz](https://wfuzz.readthedocs.io/en/latest/) | WFuzz is a web application security fuzzer tool and library for Python.
## Description
![Page](https://github.com/AlessandroMorelli96/WebGoat/blob/master/images/07_00.png)

We use WFuzz to discovery all the directory with this command:
```
wfuzz -c -w wordlist/Discovery/Web-Content/raft-large-directories.txt --hc 404 "http://docker.hackthebox.eu:39133/FUZZ"
```
![api](https://github.com/AlessandroMorelli96/WebGoat/blob/master/images/07_01.png)

Then we fuzz for files
```
wfuzz -c -w wordlist/Discovery/Web-Content/raft-large-files-lowercase.txt --hc 404 "http://docker.hackthebox.eu:39133/api/FUZZ"
```
![action.php](https://github.com/AlessandroMorelli96/WebGoat/blob/master/images/07_02.png)

We see the page and get the error:

![parameter error](https://github.com/AlessandroMorelli96/WebGoat/blob/master/images/07_03.png)

We use this command to retrive the parameter:
```
wfuzz -c -w wordlist/Discovery/Web-Content/raft-large-words-lowercase.txt --hh 24 "http://docker.hackthebox.eu:39133/api/action.php?FUZZ=test"
```
![reset](https://github.com/AlessandroMorelli96/WebGoat/blob/master/images/07_04.png)

In the page we get the error:

![error id](https://github.com/AlessandroMorelli96/WebGoat/blob/master/images/07_05.png)

With this command we get the id:
```
wfuzz -c -w wordlist/Discovery/Web-Content/raft-large-words-lowercase.txt --hh 27 "http://docker.hackthebox.eu:39133/api/action.php?reset=FUZZ"
```
![20](https://github.com/AlessandroMorelli96/WebGoat/blob/master/images/07_06.png)

Then at this page we get the flag

![FLAG](https://github.com/AlessandroMorelli96/WebGoat/blob/master/images/07_07.png)

## FLAG
***HTB{h0t_fuzz3r}***
