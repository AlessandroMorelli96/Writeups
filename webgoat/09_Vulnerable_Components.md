# Vulnerable Components
## Vulnerable Components
### Number 5

![Exercise](09_01.png)

This is not a realy exercise, just an example of two librery 1 vulnerable to xss, while in the second the vulnerability is patched.

To resolve just click the Go buttons

### Number 12

Int this exercise there is a box that use XStream.fromXML(xml), that is vulnerable. We can run commands, to resolve this we just need to use somthing like this:
```
<contact>
    <java.lang.Integer>1</java.lang.Integer>
</contact>
```
