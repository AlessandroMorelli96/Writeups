# Level 2

## Admin Section
We need to go to "http://127.0.0.1:3000/#/administration", as the challenge suggest us, but we need to be admin to see the page.
To log as admin we need to do a sql injection on the login form:

```
user = 'or 1=1--
password = 111111111
```

To resolve the challenge visit the page: "http://127.0.0.1:3000/#/administration"

## Classic Stored XSS
to resolve this log as a normal user, not admin, and go to the page to traking order. As you see the id order is inthe url and also displayed.
You need to modify the code given to you to match:

```
<iframe src="javascript:alert(`xss`)">
```

Then go to the page:

```
http://127.0.0.1:3000/#/track-result?id=%3Ciframe%20src%3D%22javascript:alert(%60xss%60)%22%3E
```

## Deprecated Interface

## Five Star Feedback
To resolve this challenge you need to resolve the challenge [Admin Section](https://github.com/AlessandroMorelli96/Juice-Shop/blob/master/02_Level2.md#admin-section)

Then go to the administration panel: "http://127.0.0.1:3000/#/administration"

And use the trash button to delete all the 5 star comments.


## Login Admin
Try to login but use as user:

```
'or 1=1 --
```

and anithing you want as password. This is a simple SQL Injection.

## Login MC SafeSearch
This is a OSINT challenge, after googling "MC SafeSearch" you will find this [video](https://imvdb.com/video/mc-safesearch/protect-ya-passwordz), listen the song and you will understand that the user used as password the name of his dog with 0 instead of o.
Login as:

```
user = mc.safesearch@juice-sh.op
password = Mr. N00dles
```

## Password Strength
To resolve this i first resolved [User Credentials](https://github.com/AlessandroMorelli96/Juice-Shop/blob/master/04_Level4.md#User-Credentials), then with [crackstation](https://crackstation.net), i retrieved the password (admin123). Then you need to login as admin@juice-sh.op and admin123.

## Security Policy

## View Basket
This challenge was risolved by other challenges

## Weird Crypto
