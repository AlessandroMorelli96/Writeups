# Ebola Virus
## Challenge
HackTheBox -> Challenges -> Crypto -> Ebola Virus
## Tools
## Description
We get a key and a crypted file. Reading on the forum we get a hint by the Author, that we need to use a frequency analyser to decrypt the file.

I used [this](https://www.cryptool.org/en/cto-cryptanalysis/n-gram-analysis) online tools.

![Cryptool](https://github.com/AlessandroMorelli96/Writeups/blob/master/HackTheBox/images/02_01.png)

![Decryption](https://github.com/AlessandroMorelli96/WriteUps/blob/master/HackTheBox/images/02_02.png)

Now using a [this](https://www.semanticscholar.org/paper/Case-sensitive-letter-and-bigram-frequency-counts-Jones-Mewhort/722e190eed82279552905cb8ff8ac4967bacb04f#extracted) list we can do a sobstitution of all the charater.

I write a Python code that read the *encrypted.bin* and list all byte and for each one count the number of occurency. Then I use the list to sobstitute the most commons letters.
This give an almost understandable text, I did some research on google for words, and complete my dictionary.
Then using my dictionary I translate the file.

This is my python code to read and translate the file.

```
filename = "/path/to/encrypted.bin"

alphabet_new = ['x', '9', 'q', '}', 'W', 'L', 'F', '6', '7', '{', '4', '3', 'Y', 'C', 'O', 'I', 'S', 'B', 'V', 'N', '1', 'P', 'R', 'H', 'D', '-', '2', '(', 'z', '0', ')', '_', 'T', 'k', 'E', '\n', 'y', 'v', '.', 'w', 'g', ',', 'p', 'b', 'f', 'm', 'u', 'c', 'd', 'l', 'h', 'r', 's', 'n', 'a', 'i', 'o', 't', 'e', ' ']
alphabet = []
percentage = [1]*60

# Read the file and count the occurency of every byte
with open(filename, "rb") as file:
    byte = file.read(1)
    while byte:
        if byte not in alphabet:
            alphabet.append(byte)
        else:
            percentage[alphabet.index(byte)] += 1
        byte = file.read(1)

# Order the two array by the number of occurrency
percentage, alphabet = zip(*sorted(zip(percentage, alphabet)))
percentage, alphabet = (list(t) for t in zip(*sorted(zip(percentage, alphabet))))

# Translate the file
with open(filename, "rb") as file:
    byte = file.read(1)
    while byte:
        if alphabet_new[alphabet.index(byte)] == '*':
            print(byte, end='')
            print(alphabet.index(byte), end='')
        else:
            print(alphabet_new[alphabet.index(byte)], end='')
        byte = file.read(1)
```

## FLAG
***HTB{W3_kN0w_hOw_to_c0nTr0l_Eb0l4}***
