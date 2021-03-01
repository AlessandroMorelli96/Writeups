# WebGoat Cross Site Scripting (XSS)
## Cross Site Scripting
### Number 2
![Number 2](https://github.com/AlessandroMorelli96/Writeups/blob/master/webgoat/images/07_01.png)

As said you need to open a new tab with the same address of the first page and then type "javascript:alert(document.cookie);" in the url, if you proxy the page the two page will have the same COOKIE, so the answer will be "Yes". You will see that when the first page is reload also the second change, because the two page has teh same cookies.
### Number 7
![Number 7](https://github.com/AlessandroMorelli96/Writeups/blob/master/webgoat/images/07_02_01.png)

You just need to try in every field to input the string given to you. You will find that the credit card number field is injectable.
```
<script>alert('test');</script>
```
### Number 10
![Number 10](https://github.com/AlessandroMorelli96/Writeups/blob/master/webgoat/images/07_03.png)

We just need to try and click on the Submit button. It will suggest us to see "GoatRouter.js" if we search in the file we wil see that the word "test" is in the "routes" function. So the response is "start.mvc#test/".

![GoatRouter.js](https://github.com/AlessandroMorelli96/Writeups/blob/master/webgoat/images/07_04.png)
### Number 11
![Number 11](https://github.com/AlessandroMorelli96/Writeups/blob/master/webgoat/images/07_05.png)
We go in the page , that we found in the exercise 10, and attach the script:
```
localhost:8080/WebGoat/start.mvc#test/<script>webgoat.customjs.phoneHome()</script>
```
We need to proxy it with burp and we will see in the response the number.
![Response](https://github.com/AlessandroMorelli96/Writeups/blob/master/webgoat/images/07_06.png)
