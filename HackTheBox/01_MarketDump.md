# MarketDump
Open the file you download with Wireshark.
![Wireshark](https://github.com/AlessandroMorelli96/Writeups/blob/master/HackTheBox/images/01_01.png)
There are different protocols, we will focus on the http stream, as we read from the description of the challenge.
We will see there are a stream of http, named "/x-sql". We click on a pocket and then with the right click Follow -> TCP Stream.
![http x-sql](https://github.com/AlessandroMorelli96/Writeups/blob/master/HackTheBox/images/01_02.png)
![Stream](https://github.com/AlessandroMorelli96/Writeups/blob/master/HackTheBox/images/01_03.png)
As we can see in the stream there is a particular string. We can use CyberChef to decode it with a base 58 decoder, and retrive the FLAG.
![CyberChef](https://github.com/AlessandroMorelli96/Writeups/blob/master/HackTheBox/images/01_04.png)
```
HTB{DonTRuNAsRoOt!MESsEdUpMarket}
```
