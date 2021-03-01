# Set up of Zap and WebGoat with Vagrant
WebGoat is an insecure web Application designed to teach web security’s lessons. It’s developed and maintained by OWASP.

The OWASP Zed Attack Proxy (ZAP) can help you to find security vulnerabilities in your web applications.

Firstly, we need to install Virtual Box, Vagrant and ZAP. Secondly, we have to open the terminal and install the software with this command:
```
Ubuntu: sudo apt install virtualbox vagrant
MacOS: brew cask install virtualbox vagrant owasp-zap
```
On Ubuntu and other Linux distribution to install ZAP you can follow this [document](http://techdiscussionforum.blogspot.com/2017/08/how-to-install-zap-zed-attack-proxy-in.html).

The next step is cloning the repo of WebGoat:
```
git clone https://github.com/WebGoat/WebGoat.git
```
Later we will open the Vagrant directory of the repository and start the machine:
```
cd WebGoat/webgoat-images/vagrant-training
vagrant up
```
Now we can chose our favorite web browser, personally I prefer Firefox, and go to:
```
http://127.0.0.1:8080/WebGoat/login
```

![Login page of WebGoat](https://github.com/AlessandroMorelli96/Writeups/blob/master/webgoat/images/00_01.png)

The next passage is to setup ZAP to proxy. To do this we need to install a proxy controller for your Browser, if you have chosen Firefox install [FoxyProxy](https://addons.mozilla.org/it/firefox/addon/foxyproxy-standard/).

Click on the icon of the extension -> Options -> Add, and follow like this step :

![FoxyProxy set up](https://github.com/AlessandroMorelli96/Writeups/blob/master/webgoat/images/00_02.png)

Secondly, we should open Zap and go to -> Preferences -> Local Proxies, and complete like this:

![ZAP Local Proxies set up](https://github.com/AlessandroMorelli96/Writeups/blob/master/webgoat/images/00_03.png)

Go to Zap -> Preferences -> Dynamic SSL Certificate and click on Generate if you have never done it and finally click Save.

At this point open your Browser and add the certificate we have just saved. If you have Firefox open Preferences, go to Certificates section and click on View Certificate; at the end import and select the Certificate we just generate.

Finally to check if it works well select: “Use proxy Zap for all URLs…” from the FoxyProxy icon and go to the login page of WebGoat. You will see something like this:

![Login Page and ZAP](https://github.com/AlessandroMorelli96/Writeups/blob/master/webgoat/images/00_04.png)
