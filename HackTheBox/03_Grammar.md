# Grammar

## Intro
HackTheBox -> Challenges -> Web -> Grammar <br>
When we access this page we get a Forbidden error. However we believe that something strange lies behind... Can you find a way in and retrieve the flag? 

## Tools
Tool | Descrizione
------- | ------
Burp Suite | Burp suite è un tool che in questa challenge è usato per analizzare e mdificare le richieste che vengono effetuate dal broswere
[dirSearch](https://github.com/maurosoria/dirsearch.git)| Script che ci permette di scannerizzare un sito, per trovare le pagine piu comuni presenti

## Resurces
- [Type Juggling](https://www.owasp.org/images/6/6b/PHPMagicTricks-TypeJuggling.pdf)

## Description
1. Collegandoci alla pagina ci compare una pagina con accesso negato. Proviamo quindi con dirSearch e troviamo index.php. Quindi con Burp modifichiamo la richiesta modificando la richiesta in una POST alla pagina index.php.
2. A questo punto ci compare una pagina con un campo di testo. Analizzando il sorgente della pagina notiamo il seguente commento:

> HTB hint:really not important...totaly solvable without using it! Just there to fill things and to save you from some trouble you might get into :)
3. Quindi capiamo che il campo di testo è fittizio e non va toccato, guardando il pacchetto notiamo un cookie strano, convertendolo dal URL -> Base64, ci accorgiamo che contiene i 3 seguenti campi:
> {"User":"whocares","Admin":"False","MAC":"ff6d0a568d61e5a03bcdb04509d5885d"}
4. Dopo aver provato alcune combinazioni (Amin:"True"), e leggendo la risorsa linkata, capiamo che sul campo MAC, sono fatti dei controlli. Sfruttando la vulnerabilità del php, il Type Juggling, ipotiziamo che dobbiamo modificare il campo MAC con un 0.
5. Modifichiamo il cookie del pacchetto in modo da avere il seguente cookie, controlliamo che sia ancora una POST e che non ci siano variabili. Troviamo la flag.
> {“User”:”whocares”,”Admin”:”True”,”MAC”:0}<br>eyJVc2VyIjoid2hvY2FyZXMiLCJBZG1pbiI6IlRydWUiLCJNQUMiOjB9

## FLAG 

**HTB{TypejugAlingSOulS}**

## CREATOR

Alessandro Morelli
