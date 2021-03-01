# Client side
## Bypass Front-end restriction
### Number 2

![N.2](https://github.com/AlessandroMorelli96/Writeups/blob/master/webgoat/images/10_01.png)

We need to change the request to:

```
select=option3&radio=option3&checkbox=a&shortInput=123456
```

![Original](https://github.com/AlessandroMorelli96/Writeups/blob/master/webgoat/images/10_03.png)

![Edited](https://github.com/AlessandroMorelli96/Writeups/blob/master/webgoat/images/10_03.png)

### Number 3

![N.3](https://github.com/AlessandroMorelli96/Writeups/blob/master/webgoat/images/10_04.png)

We need to change the request to:

```
field1=abcd&field2=1234&field3=abc+123+ABC@&field4=sevenA&field5=01101A&field6=90210-1111A&field7=301-604-4882A&error=0
```

![Original](https://github.com/AlessandroMorelli96/Writeups/blob/master/webgoat/images/10_05.png)

![Edited](https://github.com/AlessandroMorelli96/Writeups/blob/master/webgoat/images/10_06.png)

## Client side filtering
### Number 2

![N.2](https://github.com/AlessandroMorelli96/Writeups/blob/master/webgoat/images/10_07.png)

We need to inspect the html of the dropdown menu and we will see a table with all the employee.

![Table](https://github.com/AlessandroMorelli96/Writeups/blob/master/webgoat/images/10_08.png)

### Number 3

![N.3](https://github.com/AlessandroMorelli96/Writeups/blob/master/webgoat/images/10_09.png)

We need to see the request with a void coupon, read the request and response we will see that there is a code *get_it_for_free* with 100% discount.

![Coupon](https://github.com/AlessandroMorelli96/Writeups/blob/master/webgoat/images/10_10.png)

## Client side filtering
### Number 2

![N.2](https://github.com/AlessandroMorelli96/Writeups/blob/master/webgoat/images/10_11.png)

Change the request *task* to:

```
QTY=1&Total=0
```

![Original](https://github.com/AlessandroMorelli96/Writeups/blob/master/webgoat/images/10_12.png)

![Edited](https://github.com/AlessandroMorelli96/Writeups/blob/master/webgoat/images/10_13.png)