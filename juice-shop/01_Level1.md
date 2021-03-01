# Level 1

## Confidential Document
```
wfuzz -c -z file,raft-large-file.txt --hh 1834 http://127.0.0.1:3000/FUZZ
```
We find robots.txt. Reading it we find that there is the page "/ftp", we download "acquisitions.md" from that page.


## DOM XSS
Use the XSS given in the search field.

![DOM XSS](https://github.com/AlessandroMorelli96/Writeups/blob/master/juice-shop/images/01_01.png)

## Error Handling
To cause an error, you can try to do an SQL Injection to the login page, this will pop an error.

```
user = ' or 1=1 --
password = asdasd
```

![Error Handling](https://github.com/AlessandroMorelli96/Writeups/blob/master/juice-shop/images/01_02.png)

## Missing Encoding
We need to go to "http://127.0.0.1:3000/#/photo-wall", as we can see there is an image that is not loaded.
If we see the code of the page we see the location of the resources ("assets/public/images/uploads/😼-#zatschi-#whoneedsfourlegs-1572600969477.jpg"), if we see the image is not loaded. The name of the exercise is a hint the problem is the charter "#" that is not encoded.
To resolve the challenge we need to change "#" to "%23".

## Outdated Whitelist
After i have see a hint on internet i went to the method for the payments and after some research i fount "main-es2015.js".
We can use some online tool like [this](https://beautifier.io) to make the code readable.
We find the "redirect to"

![Redirect to](https://github.com/AlessandroMorelli96/Writeups/blob/master/juice-shop/images/01_03.png)

We just to go on one of this link.

## Privacy Policy
This challenge was solved during the research of the Scoreboard challenge during the crawling.

## Reflected XSS
We just need to perform an XSS with this code:
```
<iframe src="javascript:alert(`xss`)">
```
There are two method:

1. Go to the home page and in the search field copy the code.
2. Go to " http://127.0.0.1:3000/#/track-order" and in the field "id" copy the code.

In the new update of Juicy Shop only the 2 method, make you resolve the challenge.

## Repetitive Registration
DRY = "don't repeat yourself"
We go to "http://127.0.0.1:3000/#/register", and type a the the field username, password and repeat password. Then before click on continue we change only the field password (not "repeat password"). Unlike what we expected, no errors appeared, this is an error, because is controlled only the field repeat and not the password field.

## Score Board
To find the score-board you need to inspect the page. You will find main-es2015.js, you can find it or by reading the html or in the network tab of your broswere (Firefox in my case).
Then you need to make readeble the file with a tool like [this](https://beautifier.io).

You will find this:

![score-board](https://github.com/AlessandroMorelli96/Writeups/blob/master/juice-shop/images/01_04.png)

Now visit the page:
```
http://127.0.0.1:3000/#/score-board
```

![Score Board](https://github.com/AlessandroMorelli96/Writeups/blob/master/juice-shop/images/00_02.png)

## Zero Stars
We can use a tool like BurpSuite to change the request for a feedback and set the value of rating to 0


