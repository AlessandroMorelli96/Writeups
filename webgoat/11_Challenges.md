# Challenges
## Admin lost password
### Number 2

![N.2](https://github.com/AlessandroMorelli96/WebGoat/blob/master/images/11_01.png)

We need to download the image:

```
wget http://host:port/WebGoat/images/webgoat2.png
```

then with strings we will see the password:

```
strings webgoat2.png
```

![Strings](https://github.com/AlessandroMorelli96/WebGoat/blob/master/images/11_02.png)

## Without password
### Number 1

![N.1](https://github.com/AlessandroMorelli96/WebGoat/blob/master/images/11_03.png)

we need to do a SQL Injection, with:

```
Username: Larry
Password: ' or 1=1 --
```

## Admin password reset
### Number 1

![N.1](https://github.com/AlessandroMorelli96/WebGoat/blob/master/images/11_04.png)

Send an email to WebWolf *username*@webwolf.com

![Send email](https://github.com/AlessandroMorelli96/WebGoat/blob/master/images/11_05.png)

![WebWolf](https://github.com/AlessandroMorelli96/WebGoat/blob/master/images/11_06.png)

In the link we see a hash.

We see in the code of the page this comment:

![Comment](https://github.com/AlessandroMorelli96/WebGoat/blob/master/images/11_07.png)

We clone the repo with:

```
git clone https://github.com/WebGoat/WebGoat.git
cd WebGoat/webgoat-lessons/challenge/src/main/resources/challenge7/
unzip git.zip
git reset --hard f94008f801fceb8833a30fe56a8b26976347edcf
```

Now we have the code from that version. To get the hash for *admin* we need to run:

```
java PasswordResetLink admin
```

![java PasswordResetLink.java admin](https://github.com/AlessandroMorelli96/WebGoat/blob/master/images/11_08.png)

Now send a request to admin@webgoat.org, then go to *WebGoat/challenge/7/reset-password/375afe1104f4a487a73823c50a9292a2* use the hash get from the last command.

![Flag](https://github.com/AlessandroMorelli96/WebGoat/blob/master/images/11_09.png)


## Without account
### Number 1

![N.1](https://github.com/AlessandroMorelli96/WebGoat/blob/master/images/11_10.png)

With Burp you need to intercept the request to *http://localhost:8080/WebGoat/challenge/8/vote/* and change the method from *GET* to *HEAD*

![Original](https://github.com/AlessandroMorelli96/WebGoat/blob/master/images/11_11.png)

![Edited](https://github.com/AlessandroMorelli96/WebGoat/blob/master/images/11_12.png)
