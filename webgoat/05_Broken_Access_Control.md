# Broken Access Control
## Insecure Direct Object References
### Number 2
![Quest](https://github.com/AlessandroMorelli96/WebGoat/blob/master/images/05_03.png)

Just use tom and cat as **username** and **password**

### Number 3
![Quest](https://github.com/AlessandroMorelli96/WebGoat/blob/master/images/05_04.png)

Click on View Profile (**WebGoat/IDOR/profile**), and then with Zap or Burp you will see in the list that are presents 2 parameter that are not listed:

```
"role": 3
"userId": 2342384
```

### Number 4
![Quest](https://github.com/AlessandroMorelli96/WebGoat/blob/master/images/05_05.png)

From test 3 you remember **userId** and the profile url 

So in the field you will put:

```
WebGoat/IDOR/profile/2342384
```

### Number 5
![Quest](https://github.com/AlessandroMorelli96/WebGoat/blob/master/images/05_06.png)

We need to Bruteforce to retrive the userId of Buffalo, to do that you can use ZAP Fuzzer function.

Buffalo Bills ID: 2342388

```
{"role":"1", "color":"red", "size":"small", "name":"Buffalo Bob", "userId":"2342388"}
```

Change Request **Content-Type** from **application/x-www-form-urlencoded** to **application/json**


## Missing Function Level Access Control
### Number 2
![Quest](https://github.com/AlessandroMorelli96/WebGoat/blob/master/images/05_07.png)

Just read the source code of the page you will find that after the menu is declare another **hidde** menu called Admin, with 2 items **Users** and **Config**

![Source page](https://github.com/AlessandroMorelli96/WebGoat/blob/master/images/05_01.png)

### Number 3
![Quest](https://github.com/AlessandroMorelli96/WebGoat/blob/master/images/05_08.png)

You need to go to **WebGoat/users**, with a proxy modify the request and add **application/json** to **Content-Type**

You will get as a response the hash of your user.

![User Hash](https://github.com/AlessandroMorelli96/WebGoat/blob/master/images/05_02.png)

