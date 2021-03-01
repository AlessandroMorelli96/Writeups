# Juice Shop
Juice Shop is an vulnerable application, that you can use to test your capabilities.

# Set up of Juice Shop
To run this application you need [Node.js](http://nodejs.org/), then you need to clone the repository and start the application by this commands:

```
git clone https://github.com/bkimminich/juice-shop.git
cd juice-shop
npm install
npm start
```

Now with your broswere, go to:

```
http://localhost:3000
```

![Home](https://github.com/AlessandroMorelli96/Writeups/blob/master/juice-shop/images/00_01.png)

# Score Board
Now we need to find the score board to retrive the list of all the vulnerability and some hints.
To find the score-board you need to inspect the page.
You will find main-es2015.js, you can find it or by reading the html or in the network tab of your broswere (Firefox in my case).
Then you need to make readeble the file with a tool like [this](https://beautifier.io).
You will find this:

![score-board](https://github.com/AlessandroMorelli96/Writeups/blob/master/juice-shop/images/01_04.png)

Now visit the page:
```
http://127.0.0.1:3000/#/score-board
```

![Score Board](https://github.com/AlessandroMorelli96/Writeups/blob/master/juice-shop/images/00_02.png)
